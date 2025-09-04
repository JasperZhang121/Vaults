
### Strict Mode & Shebang

```bash
#!/usr/bin/env bash
set -euo pipefail
# optional during debugging:
# set -x
```

- `#!/usr/bin/env bash` finds `bash` via `PATH`, more portable than `/bin/bash`.
    
- `set -e` aborts on any unhandled non-zero exit status (exceptions: `if` tests, `until/while` conditions, the right side of `||`).
    
- `set -u` errors on unset variables—pair with `${VAR:-default}` or `${VAR:?message}`.
    
- `pipefail` makes a pipeline fail if **any** command fails, not just the last.
    
- Prefer `printf` over `echo` for predictable output; never rely on `echo -e`.
    

### Resolving Paths Robustly

```bash
# Directory containing this script (works even when sourced)
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
PROJECT_ROOT="$(dirname "$SCRIPT_DIR")"
cd "$PROJECT_ROOT"
```

- `${BASH_SOURCE[0]}` is safer than `$0` when functions/source are involved.
    
- Normalize to absolute paths early; treat later paths as relative to a known root.
    

### Read-Only Constants & Command Templates

```bash
readonly DOCKER_CMD="docker compose --env-file ${ENV_FILE} -f ${RENDERED}"
```

- `readonly` blocks accidental reassignment.
    
- A **single** command template (`BASE_CMD`) avoids missing flags in later calls.
    

### Importing Environment & Safe Defaults

```bash
set -o allexport
source "$ENV_FILE"
set +o allexport

# required inputs (fail fast)
: "${DB_ROOT_PASSWORD:?DB_ROOT_PASSWORD is required}"

# optional with defaults
SVC_PORT="${SVC_PORT:-8080}"
```

- `allexport` makes sourced names exported to children (e.g., template rendering).
    
- Use parameter expansion:
    
    - `${X:-d}` default if unset/empty
        
    - `${X:=d}` assign default into `X`
        
    - `${X:?msg}` hard fail if unset/empty
        
    - `${X:+y}` substitute `y` if `X` is set
        

### Feature Switches / Fallbacks

```bash
if [[ -z "${REMOTE_USER:-}" || -z "${REMOTE_PASS:-}" ]]; then
  STORAGE_ENDPOINT="http://local-storage:9000"
else
  STORAGE_ENDPOINT="https://${REMOTE_HOST}"
fi
export STORAGE_ENDPOINT
```

- Guard with `:-` when `set -u` is active.
    
- Centralize fallbacks; avoid scattering `if/else` across the file.
    

### Files, Directories & Permissions

```bash
mkdir -p ./data/app ./logs/app ./conf
chmod 755 ./conf
[[ -f ./conf/app.conf ]] && chmod 644 ./conf/app.conf

# beware: 777 is convenient but risky—prefer precise ownership or 775/664
# chown -R svcuser:svcgroup ./data ./logs
```

- `-p` is idempotent.
    
- Use `-f`, `-d`, `-x` to test file/dir/exec presence.
    
- Prefer correct ownership (`chown`) over blanket `chmod 777`.

### Rendering Templates with `envsubst`

```bash
# render a single template
envsubst < "$TEMPLATE" > "$RENDERED"

# render many templates under a tree
for tpl in configs/*/*.template; do
  out="${tpl%.template}"
  mkdir -p "$(dirname "$out")"
  envsubst < "$tpl" > "$out"
done
```

- `envsubst` replaces `$VAR`/`${VAR}` with environment values.
    
- To **whitelist** variables (avoid accidental replacements), pass a list:
    
    ```bash
    envsubst '${PORT} ${HOST}' < in.tmpl > out.conf
    ```
    

### Atomic Writes & Newline Normalization

```bash
tmp="$(mktemp)"
envsubst < "$TEMPLATE" > "$tmp"
tr -d '\r' < "$tmp" > "$tmp.lf" && mv -f "$tmp.lf" "$RENDERED"
rm -f "$tmp"
```

- Write to a temp file, normalize CRLF→LF, then `mv -f` for atomic replacement.
    

### Mode Flags & `case` Dispatch

```bash
MODE="${1:-all}"  # "infra" | "app" | "all"
DO_INFRA=1; DO_APP=1
case "$MODE" in
  infra) DO_APP=0 ;;
  app)   DO_INFRA=0 ;;
  all)   ;;
  *)     printf 'Usage: %s [infra|app|all]\n' "$0"; exit 2 ;;
esac
```

- `case` is clearer than nested `if`.
    
- Keep booleans numeric for easy `-eq` comparisons.
    

### Health Checks & Polling with Timeout

```bash
wait_healthy() {
  local name="$1" timeout="${2:-120}" waited=0
  while :; do
    status="$(docker inspect --format='{{if .State.Health}}{{.State.Health.Status}}{{else}}running{{end}}' "$name" 2>/dev/null || true)"
    [[ "$status" == "healthy" ]] && { echo "OK: $name"; return 0; }
    sleep 2; waited=$(( waited + 2 ))
    (( waited >= timeout )) && { echo "TIMEOUT: $name"; return 1; }
  done
}
```

- Always add a timeout to prevent “infinite wait”.
    
- Use `|| true` to swallow `inspect` errors during startup races.
    

### Conditional Execution of Steps

```bash
if [[ -x "./scripts/init_data.sh" ]]; then
  ./scripts/init_data.sh
else
  echo "init_data.sh missing or not executable—skipping"
fi
```

- `-x` checks executability.
    
- Design init scripts to be **idempotent** (safe to re-run).
    

### Running Commands in Containers (Generic Pattern)

```bash
# super user (e.g., database admin)
db_super() {
  docker exec -i db env DB_PWD="$DB_ROOT_PASSWORD" \
    mydbcli -h 127.0.0.1 -P 5432 -U root --some-flag "$@"
}

# application user
db_as() {
  local user="$1"; shift
  local pwd="$1";  shift
  docker exec -i db env DB_PWD="$pwd" \
    mydbcli -h 127.0.0.1 -P 5432 -U "$user" "$@"
}
```

- Wrap `docker exec` in functions to centralize host/port/encoding flags.
    
- Use `-i` to feed stdin; export sensitive values via env (not CLI args) when possible.
    

### Indirect Expansion & Name Derivation

```bash
to_key_upper()  { tr '[:lower:]-' '[:upper:]_' <<<"$1"; }
role_from_dir() { local b="${1##*/}"; local s="${b#svc-}"; echo "app_${s//-/_}"; }

svc="svc-payments-v2"
key="$(to_key_upper "${svc#svc-}")"           # PAYMENTS_V2
user_var="DB_USER_${key}"                     # e.g., DB_USER_PAYMENTS_V2
user="${!user_var:-$(role_from_dir "$svc")}"  # indirect expansion + fallback
```

- Indirect expansion `${!name}` fetches the value of the variable _named_ by `$name`.
    
- Useful for “convention over configuration” with optional overrides.
    

### Processing Environment in Bulk with `awk`

```bash
# Build (user,password) pairs from DB_PASSWORD_* / DB_USER_* envs
awk '
  BEGIN {
    for (k in ENVIRON) if (k ~ /^DB_PASSWORD_/) {
      suf = substr(k, 13) # after DB_PASSWORD_
      pass = ENVIRON[k]
      userk = "DB_USER_" suf
      user = (userk in ENVIRON) ? ENVIRON[userk] : "app_" tolower(suf)
      print user, pass
    }
  }
' </dev/null
```

- `ENVIRON` exposes the process environment to `awk`.
    
- Keep naming conventions consistent to enable simple derivations.
    

### Executing SQL/Text Safely

```bash
# quote single quotes for SQL string literals
sqlq() { sed "s/'/''/g" <<<"$1"; }

# print file with indentation (for readable logs)
print_file() { sed 's/^/    /' "$1" || true; }
```

- Always sanitize input crossing a language boundary (shell → SQL).
    
- Prefer client flags that increase visibility (`-vvv`, `--show-warnings`) in CI.
    

### Globbing Pitfalls & `nullglob`

```bash
shopt -s nullglob
for f in sql/*.sql; do
  # loop will skip entirely if no files match
  :
done
shopt -u nullglob
```

- Without `nullglob`, an unmatched pattern is returned _literally_, often causing bugs.
    

### Arrays, Quoting & Safe Loops

```bash
services=(svc-a svc-b "svc with space")
for s in "${services[@]}"; do
  run_service "$s"
done
```

- **Always** quote `"${array[@]}"` to preserve element boundaries.
    
- Avoid `for x in $(cmd)`—use `while IFS= read -r line; do ... done < <(cmd)` for arbitrary text.
    

### Traps, Cleanup & Interrupts

```bash
cleanup() { rm -f -- "$TMPFILE"; }
trap 'echo; echo "Interrupted"; cleanup; exit 1' INT TERM
trap 'rc=$?; cleanup; exit $rc' EXIT
```

- Use `EXIT` to guarantee cleanup on any exit path.
    
- Separate interrupt handling (`INT`, `TERM`) for nicer UX.
    

### Idempotency & “Best-Effort” Patterns

```bash
mkdir -p path || true
some_cli create resource || echo "already exists; skipping"
```

- Make steps safe to re-run; deployments should be re-entrant.
    

### Logging Conventions

```bash
log()  { printf -- ">> %s\n" "$*"; }
info() { printf -- "  • %s\n" "$*"; }
warn() { printf -- "!! %s\n" "$*" >&2; }
die()  { printf -- "xx %s\n" "$*" >&2; exit 1; }
```

- Consistent prefixes improve readability in CI.
    
- Send warnings/errors to `stderr`.
    

### Security Hygiene

- Never echo secrets; redact (`******`) in logs.
    
- Prefer `--password-stdin` or env vars over inline CLI flags that land in history.
    
- Restrict file permissions to the minimum necessary; avoid world-writable mounts.
    

### Timeout Helpers

```bash
wait_with_timeout() {
  local secs="$1"; shift
  local start=$SECONDS
  until "$@"; do
    (( SECONDS - start >= secs )) && return 124
    sleep 2
  done
}
```

- Return `124` on timeout (mimics `timeout(1)` exit code).
    

### Minimal, Reusable Scaffolding

```bash
#!/usr/bin/env bash
set -euo pipefail

main() {
  parse_args "$@"
  load_env
  render_templates
  start_infra
  wait_health "db" 180
  init_data
  start_apps
  echo "All done."
}

parse_args() { MODE="${1:-all}"; }
load_env()   { set -o allexport; source "./.env"; set +o allexport; }
render_templates() { envsubst < app.tmpl > app.yaml; }
start_infra() { :; }   # placeholder
wait_health()  { wait_healthy "$@"; }
init_data()    { [[ -x scripts/init.sh ]] && scripts/init.sh || :; }
start_apps()   { :; }  # placeholder

wait_healthy() {
  local name="$1" timeout="${2:-120}" waited=0
  while :; do
    status="$(docker inspect --format='{{if .State.Health}}{{.State.Health.Status}}{{else}}running{{end}}' "$name" 2>/dev/null || true)"
    [[ "$status" == "healthy" ]] && return 0
    sleep 2; waited=$(( waited + 2 ))
    (( waited >= timeout )) && return 124
  done
}

main "$@"
```

---

### Quick Checklist (Copy Me)

-  `#!/usr/bin/env bash` + `set -euo pipefail`
    
-  Validate required vars with `${VAR:?missing}`
    
-  Provide sane defaults `${VAR:-default}`
    
-  Resolve absolute paths (`${BASH_SOURCE[0]}`)
    
-  Atomic writes (`mktemp` → normalize → `mv -f`)
    
-  Template render via `envsubst` (prefer whitelist)
    
-  Health-check loops include **timeouts**
    
-  Idempotent actions (`mkdir -p`, “already exists → skip”)
    
-  Traps for cleanup and interrupts
    
-  Quote **everything**; use arrays; avoid `for x in $(...)`
    
-  Never log secrets; minimize file permissions
