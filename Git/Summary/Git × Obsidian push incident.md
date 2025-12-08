### 2025-10-09

#### What went wrong (symptoms)

- Obsidian Git push failed initially (no GUI prompt for credentials), then CLI `git push` was rejected as **non-fast-forward** (remote was ahead).
    
- During conflict resolution you saw **“You are not currently on a branch (detached HEAD)”** because an interactive rebase was in progress.
    

#### Root causes

1. **Auth setup:** The remote URL previously embedded a PAT in the username part, causing prompt/Keychain issues inside Obsidian.
    
2. **History divergence:** `origin/main` had new commits, so your local `main` was **behind**.

3. **Conflict file:** `.obsidian/workspace.json` (Obsidian’s UI layout) is noisy and often conflicts—best not to track it.
    
4. **Detached HEAD:** Rebasing temporarily checks out commits directly (not a branch) until you `--continue`.
    

#### What we changed (the fix)

1. **Clean HTTPS remote + Keychain**
    
    ```bash
    git remote set-url origin https://github.com/JasperZhang121/Vaults.git
    git config --global credential.helper osxkeychain
    printf "protocol=https\nhost=github.com\n" | git credential-osxkeychain erase
    git fetch   # triggers login once; Keychain stores the token
    ```
    
2. **Reconcile histories (rebase)**
    
    ```bash
    git checkout main
    git pull --rebase origin main
    # Resolved conflict in .obsidian/workspace.json (took ours/theirs), then:
    git add .obsidian/workspace.json
    git rebase --continue
    ```
    
3. **Push after rebase**
    
    ```bash
    git push origin main
    # (If needed) git push --force-with-lease origin main
    ```
    

#### Why “detached HEAD” appeared

- During an **interactive rebase**, Git checks out specific commits. Until you stage fixes and run `git rebase --continue` (or `--abort`), you’re **not on a branch**. That’s expected; finishing the rebase reattaches `HEAD` to `main`.
    

#### Final state

- `main` and `origin/main` are aligned.
    
- Credentials are stored in macOS Keychain, so Obsidian’s Git plugin can push/pull without prompts.
    

---

### Prevent it next time

#### 1) Ignore Obsidian’s volatile layout file

(If it’s already tracked, remove it from the index once.)

```bash
echo ".obsidian/workspace.json" >> .gitignore
git rm --cached .obsidian/workspace.json
git add .gitignore
git commit -m "chore: ignore Obsidian workspace layout file"
git push
```

Optional: make merges auto-prefer your local version for that file:

```bash
# .gitattributes
.obsidian/workspace.json merge=ours
```

#### 2) Keep pulls clean

```bash
git config --global pull.rebase true
git config --global rebase.autostash true
```

#### 3) Use stable auth

- **HTTPS + Keychain** (what you have now), or switch to **SSH** to avoid tokens entirely:
    
    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    eval "$(ssh-agent -s)"
    ssh-add --apple-use-keychain ~/.ssh/id_ed25519
    pbcopy < ~/.ssh/id_ed25519.pub   # add at GitHub → Settings → SSH keys
    git remote set-url origin git@github.com:JasperZhang121/Vaults.git
    ```
    

---

### Handy commands (cheat sheet)

- See current branch & remotes:
    
    ```bash
    git rev-parse --abbrev-ref HEAD
    git remote -v
    ```
    
- Resolve rebase conflicts then continue:
    
    ```bash
    git status
    git add <files>
    git rebase --continue
    ```
    
- Safe overwrite after rebase:
    
    ```bash
    git push --force-with-lease origin main
    ```
    

That’s the whole story: auth cleaned up, histories reconciled, conflict resolved, and future conflicts avoided by ignoring `workspace.json`.