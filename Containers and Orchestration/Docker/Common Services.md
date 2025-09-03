### Docker ps (list containers)

```bash
# Live containers
docker ps

# All containers (including exited)
docker ps -a
```

---

### MySQL

#### Images

```bash
# Pull
docker pull mysql:latest

# Delete image
docker rmi mysql:latest
```

#### Run

```bash
# Minimal run (no volume)
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d -p 3306:3306 mysql:latest

# With volume (macOS/Linux)
docker run --name mysql \
  -e MYSQL_ROOT_PASSWORD=1234 \
  -v /PATH/TO/mysql-data:/var/lib/mysql \
  -p 3306:3306 -d mysql:latest

# With volume (Windows PowerShell)
docker run --name mysql `
  -e MYSQL_ROOT_PASSWORD=1234 `
  -v C:\PATH\TO\mysql-data:/var/lib/mysql `
  -p 3306:3306 -d mysql:latest
```

> If the host data directory has permission issues (macOS/Linux):

```bash
sudo chmod -R 777 /PATH/TO/mysql-data   # (Quick fix; prefer proper ownership in real setups)
```

#### Container lifecycle

```bash
# Start / Stop
docker start mysql
docker stop mysql

# Remove (after stopping)
docker rm mysql
```

#### Connect (from host)

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
# then enter your password (e.g., 1234 or 123456)
```

#### Exec into container & login

```bash
docker exec -it mysql mysql -uroot -p
# or with inline password:
docker exec -it mysql mysql -uroot -p1234
```

#### Troubleshooting

```bash
docker logs -f mysql
```

---

### Redis (and RediSearch)

#### Images

```bash
# Redis
docker pull redis:latest

# RediSearch
docker pull redislabs/redisearch:latest
```

#### Run

```bash
# Redis with volume (macOS/Linux)
docker run -d --name my-redis -p 6379:6379 -v /PATH/TO/redis-data:/data redis

# RediSearch (macOS/Linux)
docker run -d --name redisearch -p 16379:6379 redislabs/redisearch:latest

# RediSearch (Windows PowerShell)
docker run -d --name redisearch `
  -p 16379:6379 `
  -v D:\Docker\redis\data:/data `
  redislabs/redisearch:latest
```

#### Start/Stop

```bash
docker start my-redis
docker stop  my-redis
```

#### Connect

```bash
# Quick check via telnet
telnet localhost 6379

# Enter container shell (if image has /bin/bash)
docker exec -it my-redis bash

# Redis CLI (inside container or installed on host)
redis-cli -p 6379
```

---

### Nacos

#### Simple (non-Docker) start

```bash
# In a Nacos checkout
sh startup.sh -m standalone
```

#### Pull (Docker)

```bash
docker pull nacos/nacos-server:latest
```

#### Run (standalone, macOS/Linux — Nacos 2.x)

> Open ports 8848, 9848, 9849 if using 2.x

```bash
docker run -d --name nacos-standalone \
  -e MODE=standalone \
  -p 8848:8848 \
  -p 9848:9848 \
  -p 9849:9849 \
  -v /PATH/TO/nacos-data:/home/nacos/data \
  -v /PATH/TO/nacos-conf:/home/nacos/conf \
  nacos/nacos-server
```

#### Pinned version example (2.0.3)

```bash
docker run -d --name nacos-standalone-203 \
  -e MODE=standalone \
  -p 8848:8848 -p 9848:9848 -p 9849:9849 \
  -v /PATH/TO/nacos-data-203:/home/nacos/data \
  nacos/nacos-server:2.0.3
```

#### Nacos v3.0.2 (Windows PowerShell example, **secrets redacted**)

**Generate a Base64 token from your secret** (do **not** hardcode in notes):

```powershell
# Example template (REPLACE with your own strong secret; do not commit):
$raw = '***REDACTED_YOUR_SECRET***'
$b64 = [Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes($raw))
$b64    # Copy the result to use as NACOS_AUTH_TOKEN
```

(macOS/Linux alternative)

```bash
echo -n '***REDACTED_YOUR_SECRET***' | base64
```

Create data/log directories (Windows):

```powershell
mkdir C:\PATH\nacos-data\data -ea 0
mkdir C:\PATH\nacos-data\logs -ea 0
mkdir C:\PATH\nacos-data\conf -ea 0
```

Run:

```powershell
docker rm -f nacos

docker run -d --name nacos --restart unless-stopped `
  -e MODE=standalone `
  -e PREFER_HOST_MODE=ip `
  -e NACOS_AUTH_ENABLE=true `
  -e NACOS_AUTH_IDENTITY_KEY=serverIdentity `
  -e NACOS_AUTH_IDENTITY_VALUE=nacosServer `
  -e NACOS_AUTH_TOKEN=***REDACTED_BASE64_TOKEN*** `
  -e JVM_XMS=256m -e JVM_XMX=512m -e JVM_XMN=128m `
  -p 8848:8848 -p 9848:9848 -p 9849:9849 -p 8080:8080 `
  -v "C:\PATH\nacos-data\data:/home/nacos/data" `
  -v "C:\PATH\nacos-data\logs:/home/nacos/logs" `
  nacos/nacos-server:v3.0.2
```

#### Start/Stop

```bash
docker start nacos-standalone
docker start nacos-standalone-203

docker stop nacos-standalone
docker stop nacos-standalone-203
```

---

### RabbitMQ (management console)

#### Pull

```bash
docker pull rabbitmq:management
```

#### Run

```bash
docker run -d --name rabbitmq \
  -p 5672:5672 -p 15672:15672 \
  -v /PATH/TO/rabbitmq-data:/var/lib/rabbitmq \
  rabbitmq:management
```

> UI: [http://localhost:15672](http://localhost:15672/) (default user/pass often `guest`/`guest` in dev; change in real setups)

---

### Tomcat

#### Run

```bash
# macOS/Linux
docker run -d -p 8080:8080 \
  -v /PATH/TO/tomcat_volume:/usr/local/tomcat/webapps \
  --name my_tomcat tomcat

# Windows PowerShell
docker run -d -p 8080:8080 `
  -v C:\PATH\tomcat_volume:/usr/local/tomcat/webapps `
  --name my_tomcat tomcat
```

#### Start/Stop

```bash
docker start my_tomcat
docker stop  my_tomcat
```

#### Enter container

```bash
docker exec -it my_tomcat /bin/bash
```

---

### General Tips

- Prefer **pinned versions** (e.g., `mysql:8.0.33`, `redis:7`, `nacos/nacos-server:2.3.2`) over `latest` for reproducibility.
    
- Map **host volumes** for persistence; ensure permissions are adequate on the host.
    
- Use `docker logs -f <name>` to tail logs for any container.
    
- Cleanup:
    

```bash
docker stop <name> && docker rm <name>
docker rmi <image[:tag]>
```
