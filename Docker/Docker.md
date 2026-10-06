# 🐳 Docker — Complete DevOps Course

> **Beginner → Advanced Docker Learning Path**
> Learn Docker from fundamentals to production-ready containerization, including Dockerfiles, networking, volumes, Docker Compose, security, troubleshooting, and a complete Frontend + Backend + SQLite project.

---

## 📚 Course Overview

This repository is designed to teach Docker from **zero to job-ready DevOps level**.

By completing this course, you will learn how to:

* Understand Docker architecture
* Create and manage containers
* Work with Docker images
* Write production-quality Dockerfiles
* Build optimized Docker images
* Use multi-stage builds
* Configure containers using environment variables
* Create and manage Docker networks
* Persist data using Docker volumes
* Build multi-container applications using Docker Compose
* Add health checks
* Debug containerized applications
* Apply Docker security best practices
* Push images to Docker Hub
* Containerize frontend and backend applications
* Persist a SQLite database
* Prepare Docker workflows for CI/CD

---

# 🗂️ Course Roadmap

```text
Docker Fundamentals
        ↓
Docker Architecture
        ↓
Docker Commands
        ↓
Images & Containers
        ↓
Dockerfile
        ↓
Image Optimization
        ↓
Environment Variables
        ↓
Container Debugging
        ↓
Docker Networking
        ↓
Docker Volumes
        ↓
Docker Compose
        ↓
Health Checks & Resource Limits
        ↓
Docker Security
        ↓
Docker Registry / Docker Hub
        ↓
Production Best Practices
        ↓
Frontend + Backend + SQLite Project
        ↓
CI/CD
```

---

# 1. 🐳 Docker Fundamentals

## What is Docker?

Docker is a platform used to package an application together with its dependencies into a **container**.

A container provides an isolated runtime environment for an application while sharing the host operating system kernel.

For example:

```text
Application
    +
Dependencies
    +
Configuration
    ↓
Docker Image
    ↓
Docker Container
```

Docker helps solve:

> "It works on my machine."

The same container image can be used across:

```text
Developer Machine
        ↓
Testing
        ↓
Staging
        ↓
Production
        ↓
Cloud
```

---

# 2. 🖥️ Containers vs Virtual Machines

## Virtual Machine

```text
Hardware
   ↓
Host OS
   ↓
Hypervisor
   ├── VM 1
   │    ├── Guest OS
   │    └── Application
   │
   └── VM 2
        ├── Guest OS
        └── Application
```

Each VM normally contains its own operating system.

## Docker

```text
Hardware
   ↓
Host OS
   ↓
Docker Engine
   ├── Container 1
   │    └── Application
   │
   ├── Container 2
   │    └── Application
   │
   └── Container 3
        └── Application
```

Containers share the host kernel.

### Containers are generally:

* Lightweight
* Fast to start
* Portable
* Easy to scale
* Efficient with resources

> Containers are not simply "lightweight virtual machines." They use operating-system-level isolation.

---

# 3. 🏗️ Docker Architecture

The basic architecture:

```text
                 Docker CLI
                     │
                 Docker API
                     │
                     ▼
               Docker Daemon
                 (dockerd)
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
     Images      Containers    Networks
                                   │
                                Volumes
```

## Docker CLI

You interact with Docker using commands such as:

```bash
docker run nginx
```

## Docker Daemon

The Docker daemon manages:

* Images
* Containers
* Networks
* Volumes

## Docker Image

A read-only template used to create containers.

Example:

```text
nginx:latest
```

## Docker Container

A runtime instance of an image.

```text
Image
  ↓
Container
```

---

# 4. ⚙️ Docker Installation Verification

Check Docker:

```bash
docker --version
```

Detailed information:

```bash
docker info
```

Docker version information:

```bash
docker version
```

Test Docker:

```bash
docker run hello-world
```

---

# 5. 🚀 Your First Container

Run Nginx:

```bash
docker run nginx
```

Run in detached mode:

```bash
docker run -d nginx
```

Run with a custom name:

```bash
docker run -d --name my-nginx nginx
```

Run with port mapping:

```bash
docker run -d \
  --name my-nginx \
  -p 8080:80 \
  nginx
```

Open:

```text
http://localhost:8080
```

### Port mapping

```text
-p HOST_PORT:CONTAINER_PORT
```

Example:

```text
-p 8080:80
```

means:

```text
localhost:8080
      ↓
Docker Host
      ↓
Container:80
      ↓
Nginx
```

---

# 6. 📦 Container Commands

## List running containers

```bash
docker ps
```

## List all containers

```bash
docker ps -a
```

## Start container

```bash
docker start my-nginx
```

## Stop container

```bash
docker stop my-nginx
```

## Restart container

```bash
docker restart my-nginx
```

## Kill container

```bash
docker kill my-nginx
```

## Remove container

```bash
docker rm my-nginx
```

## Force remove

```bash
docker rm -f my-nginx
```

---

# 7. 📋 Container Logs

View logs:

```bash
docker logs my-nginx
```

Follow logs:

```bash
docker logs -f my-nginx
```

Last 100 lines:

```bash
docker logs --tail 100 my-nginx
```

Logs with timestamps:

```bash
docker logs -t my-nginx
```

---

# 8. 🐚 Execute Commands Inside Containers

Open Bash:

```bash
docker exec -it my-nginx bash
```

If Bash isn't available:

```bash
docker exec -it my-nginx sh
```

Run a single command:

```bash
docker exec my-nginx ls
```

Check working directory:

```bash
docker exec my-nginx pwd
```

Exit:

```bash
exit
```

---

# 9. 🔍 Inspect Containers

Inspect:

```bash
docker inspect my-nginx
```

View resource usage:

```bash
docker stats
```

View processes:

```bash
docker top my-nginx
```

View published ports:

```bash
docker port my-nginx
```

---

# 10. 🖼️ Docker Images

List images:

```bash
docker images
```

or:

```bash
docker image ls
```

Pull an image:

```bash
docker pull nginx
```

Pull a specific version:

```bash
docker pull nginx:1.27
```

Inspect image:

```bash
docker image inspect nginx
```

View image layers:

```bash
docker history nginx
```

Remove image:

```bash
docker rmi nginx
```

---

# 11. 🏷️ Docker Image Tags

Docker image references generally follow:

```text
repository:tag
```

Examples:

```text
nginx:latest
python:3.12
node:22-alpine
ubuntu:24.04
```

Tag your image:

```bash
docker tag myapp:latest sachinborse/myapp:1.0
```

Push:

```bash
docker push sachinborse/myapp:1.0
```

---

# 12. 🐳 Dockerfile

A `Dockerfile` contains instructions used to build a Docker image.

Example:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

Build:

```bash
docker build -t my-python-app:1.0 .
```

Run:

```bash
docker run -d \
  --name my-python-app \
  -p 8000:8000 \
  my-python-app:1.0
```

---

# 13. 📝 Dockerfile Instructions

| Instruction   | Purpose                             |
| ------------- | ----------------------------------- |
| `FROM`        | Select base image                   |
| `RUN`         | Execute build-time commands         |
| `COPY`        | Copy files                          |
| `ADD`         | Copy files with additional behavior |
| `WORKDIR`     | Set working directory               |
| `ENV`         | Set environment variable            |
| `ARG`         | Build-time variable                 |
| `EXPOSE`      | Document container port             |
| `CMD`         | Default command                     |
| `ENTRYPOINT`  | Main executable                     |
| `USER`        | Set container user                  |
| `HEALTHCHECK` | Define health check                 |
| `LABEL`       | Add image metadata                  |

---

# 14. `FROM`

Defines the base image.

```dockerfile
FROM python:3.12-slim
```

Node:

```dockerfile
FROM node:22-alpine
```

Nginx:

```dockerfile
FROM nginx:alpine
```

---

# 15. `WORKDIR`

Sets the working directory.

```dockerfile
WORKDIR /app
```

Instead of:

```dockerfile
RUN cd /app
```

Use:

```dockerfile
WORKDIR /app
```

---

# 16. `COPY`

Copy files from the build context into the image.

```dockerfile
COPY . .
```

Or:

```dockerfile
COPY requirements.txt .
```

---

# 17. `RUN`

Executes commands while building the image.

```dockerfile
RUN pip install -r requirements.txt
```

Example:

```dockerfile
RUN apt-get update && apt-get install -y curl
```

---

# 18. `ENV`

Set runtime environment variables.

```dockerfile
ENV APP_ENV=production
```

Multiple variables:

```dockerfile
ENV APP_ENV=production \
    PORT=8000
```

---

# 19. `ARG`

Build-time variables.

```dockerfile
ARG VERSION=1.0
```

Build:

```bash
docker build \
  --build-arg VERSION=2.0 \
  -t myapp:2.0 .
```

Difference:

```text
ARG → Build time
ENV → Runtime
```

---

# 20. `EXPOSE`

Documents the application's intended port.

```dockerfile
EXPOSE 8000
```

Important:

> `EXPOSE` does NOT publish the port.

You still need:

```bash
docker run -p 8000:8000 myapp
```

---

# 21. `CMD`

Defines the default command.

```dockerfile
CMD ["python", "app.py"]
```

Another example:

```dockerfile
CMD ["npm", "start"]
```

---

# 22. `ENTRYPOINT`

Defines the main executable.

```dockerfile
ENTRYPOINT ["python"]
```

Combined with:

```dockerfile
CMD ["app.py"]
```

Running:

```bash
docker run myimage
```

results in:

```bash
python app.py
```

---

# 23. CMD vs ENTRYPOINT

| CMD                       | ENTRYPOINT                            |
| ------------------------- | ------------------------------------- |
| Default command/arguments | Main executable                       |
| Easy to override          | Usually more fixed                    |
| Often used for defaults   | Often used for application executable |

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Then:

```bash
docker run myimage other.py
```

runs:

```bash
python other.py
```

---

# 24. 👤 USER

Avoid running applications as root where possible.

Example:

```dockerfile
FROM python:3.12-slim

RUN useradd --create-home appuser

WORKDIR /app

COPY --chown=appuser:appuser . .

USER appuser

CMD ["python", "app.py"]
```

---

# 25. 🩺 HEALTHCHECK

A process can be running but the application may still be unhealthy.

Example:

```dockerfile
HEALTHCHECK \
  --interval=30s \
  --timeout=5s \
  --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1
```

Inspect:

```bash
docker inspect CONTAINER
```

---

# 26. 📄 `.dockerignore`

Create:

```text
.dockerignore
```

Example:

```text
.git
.gitignore
.env
node_modules
__pycache__
*.pyc
*.log
.venv
dist
coverage
```

Benefits:

* Smaller build context
* Faster builds
* Prevents unnecessary files from entering images
* Helps avoid accidentally copying sensitive files

---

# 27. ⚡ Docker Image Optimization

Good practices:

* Use minimal base images.
* Use `.dockerignore`.
* Combine appropriate `RUN` commands.
* Order layers carefully.
* Use multi-stage builds.
* Avoid unnecessary packages.
* Don't install development tools in runtime images.

---

# 28. 🏗️ Multi-stage Builds

Multi-stage builds separate build dependencies from runtime dependencies.

Example:

```dockerfile
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build


FROM nginx:alpine

COPY --from=builder \
  /app/dist \
  /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Architecture:

```text
Builder Image
    ↓
Compile / Build
    ↓
Production Files
    ↓
Runtime Image
```

Advantages:

* Smaller production image
* Reduced attack surface
* Faster deployment
* No unnecessary build tools in production

---

# 29. 🌎 Environment Variables

Pass variables during runtime:

```bash
docker run \
  -e APP_ENV=production \
  myapp
```

Using `.env`:

```text
APP_ENV=production
PORT=8000
DATABASE_URL=sqlite:///./app.db
```

Run:

```bash
docker run \
  --env-file .env \
  myapp
```

> Never hard-code passwords, API keys or cloud credentials inside Dockerfiles.

---

# 30. 🔄 Container Lifecycle

Typical lifecycle:

```text
Created
   ↓
Running
   ↓
Stopped
   ↓
Removed
```

Commands:

```bash
docker create IMAGE
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER
```

---

# 31. 🔁 Restart Policies

Always restart unless explicitly stopped:

```bash
docker run \
  -d \
  --restart unless-stopped \
  nginx
```

Restart on failure:

```bash
docker run \
  -d \
  --restart on-failure:5 \
  myapp
```

---

# 32. 🌐 Docker Networking

List networks:

```bash
docker network ls
```

Create network:

```bash
docker network create app-network
```

Inspect:

```bash
docker network inspect app-network
```

Connect:

```bash
docker network connect app-network CONTAINER
```

Disconnect:

```bash
docker network disconnect app-network CONTAINER
```

Remove:

```bash
docker network rm app-network
```

---

# 33. Docker Network Types

## Bridge

Default networking model for many containers.

```text
Container
    ↓
Docker Bridge
    ↓
Host
```

## Host

Container uses the host network namespace.

```bash
docker run --network host nginx
```

## None

Disable normal networking:

```bash
docker run --network none nginx
```

## Custom Bridge

Recommended for application stacks:

```bash
docker network create app-network
```

---

# 34. 🔗 Container-to-Container Communication

Create network:

```bash
docker network create app-network
```

Backend:

```bash
docker run -d \
  --name backend \
  --network app-network \
  backend:1.0
```

Frontend:

```bash
docker run -d \
  --name frontend \
  --network app-network \
  frontend:1.0
```

The frontend can communicate with the backend using:

```text
backend
```

rather than relying on a hard-coded container IP.

---

# 35. 📡 Port Publishing

Run:

```bash
docker run \
  -d \
  -p 8080:80 \
  nginx
```

Meaning:

```text
Host:8080
     ↓
Container:80
```

Multiple ports:

```bash
docker run \
  -d \
  -p 8080:80 \
  -p 8443:443 \
  nginx
```

---

# 36. 💾 Docker Volumes

Containers are ephemeral.

If a container is removed, data stored only in its writable container layer may disappear.

Named volume:

```bash
docker volume create app-data
```

List:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect app-data
```

Remove:

```bash
docker volume rm app-data
```

---

# 37. Mount a Volume

```bash
docker run \
  -d \
  --name db \
  -v app-data:/data \
  mydatabase
```

Architecture:

```text
Container
    │
    ▼
/data
    │
    ▼
Docker Volume
    │
    ▼
Persistent Storage
```

---

# 38. 📁 Bind Mounts

Bind mounts map a host directory into a container.

Example:

```bash
docker run \
  -it \
  --rm \
  -v "$(pwd)":/app \
  python:3.12-slim \
  sh
```

Useful for:

* Local development
* Live source code
* Configuration files
* Development workflows

---

# 39. SQLite + Docker

SQLite stores data in a file.

Example:

```text
/data/app.db
```

Mount the `/data` directory:

```yaml
volumes:
  - sqlite-data:/data
```

Then:

```text
SQLite
  ↓
/data/app.db
  ↓
Docker Volume
  ↓
Persistent Data
```

This allows the database file to survive container replacement.

> SQLite is excellent for learning and many small applications. For highly concurrent production workloads, PostgreSQL or another server database is often more appropriate.

---

# 40. 💾 Volume Backup

Example:

```bash
docker run --rm \
  -v app-data:/data \
  -v "$(pwd)":/backup \
  alpine \
  tar czf /backup/app-data.tar.gz -C /data .
```

---

# 41. 🎼 Docker Compose

Docker Compose lets you define multi-container applications declaratively.

Modern Compose uses:

```text
compose.yaml
```

Example:

```yaml
services:

  backend:
    build: ./backend
    ports:
      - "8000:8000"
    volumes:
      - sqlite-data:/data
    networks:
      - app-network

  frontend:
    build: ./frontend
    ports:
      - "3000:80"
    depends_on:
      - backend
    networks:
      - app-network

volumes:
  sqlite-data:

networks:
  app-network:
```

---

# 42. Compose Commands

Start:

```bash
docker compose up
```

Detached:

```bash
docker compose up -d
```

Build and start:

```bash
docker compose up --build -d
```

List services:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs
```

Follow backend logs:

```bash
docker compose logs -f backend
```

Execute command:

```bash
docker compose exec backend sh
```

Restart:

```bash
docker compose restart backend
```

Stop:

```bash
docker compose stop
```

Remove containers/network:

```bash
docker compose down
```

Remove containers and volumes:

```bash
docker compose down -v
```

> Be careful with `down -v`: it removes Compose-managed named volumes and can delete persistent data.

Validate configuration:

```bash
docker compose config
```

---

# 43. 🩺 Compose Health Checks

Example:

```yaml
services:

  backend:
    build: ./backend

    healthcheck:
      test:
        [
          "CMD",
          "python",
          "-c",
          "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"
        ]

      interval: 10s
      timeout: 3s
      retries: 5
```

Health checks help distinguish:

```text
Process running
```

from:

```text
Application actually healthy
```

---

# 44. 📊 Resource Limits

Limit memory:

```bash
docker run \
  -d \
  --memory=512m \
  myapp
```

Limit CPU:

```bash
docker run \
  -d \
  --cpus=1.0 \
  myapp
```

Monitor:

```bash
docker stats
```

---

# 45. 🔐 Docker Security

Important Docker security practices:

* Use minimal trusted images.
* Keep Docker and images updated.
* Run applications as non-root.
* Do not put secrets in Dockerfiles.
* Scan images for vulnerabilities.
* Use versioned images.
* Expose only required ports.
* Use isolated networks.
* Use read-only filesystems where practical.
* Drop unnecessary Linux capabilities.
* Keep production images small.

---

# 46. 👤 Non-root Containers

Example:

```dockerfile
FROM python:3.12-slim

RUN useradd --create-home appuser

WORKDIR /app

COPY --chown=appuser:appuser . .

USER appuser

CMD ["python", "app.py"]
```

Check:

```bash
docker exec CONTAINER whoami
```

---

# 47. 🔒 Secrets

Never do this:

```dockerfile
ENV AWS_SECRET_KEY=secret123
```

Never commit:

```text
.env
credentials.json
private-key.pem
```

Use:

* Runtime environment variables
* Docker/Compose secret mechanisms where appropriate
* Cloud secret managers
* CI/CD secret stores

---

# 48. 📦 Docker Registry

A Docker registry stores container images.

Docker Hub is a popular public registry.

Login:

```bash
docker login
```

Build:

```bash
docker build \
  -t sachinborse/myapp:1.0 \
  .
```

Push:

```bash
docker push sachinborse/myapp:1.0
```

Pull:

```bash
docker pull sachinborse/myapp:1.0
```

---

# 49. 🏭 Production Best Practices

## Image

* Use small images.
* Use specific versions.
* Use multi-stage builds.
* Scan images.
* Remove unnecessary dependencies.

## Container

* Run as non-root.
* Add health checks.
* Configure restart policies.
* Set resource limits.
* Keep containers stateless when possible.

## Network

* Use custom networks.
* Avoid hard-coded IP addresses.
* Expose only required ports.

## Storage

* Use volumes for persistent data.
* Back up important data.
* Understand database persistence requirements.

## Configuration

* Use environment variables/configuration.
* Never commit secrets.

## CI/CD

Typical pipeline:

```text
Git Push
   ↓
CI
   ↓
Run Tests
   ↓
Build Docker Image
   ↓
Security Scan
   ↓
Push Image
   ↓
Deploy
```

---

# 50. 🚀 Complete Docker Project

We will build:

```text
Frontend + Backend + SQLite
```

## Architecture

```text
                    Browser
                       │
                       ▼
              ┌─────────────────┐
              │    Frontend     │
              │      React      │
              │    Container    │
              └────────┬────────┘
                       │
                 Docker Network
                       │
                       ▼
              ┌─────────────────┐
              │     Backend     │
              │     FastAPI     │
              │    Container    │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │     SQLite      │
              │   /data/app.db  │
              └────────┬────────┘
                       │
                       ▼
                 Docker Volume
```

---

# 51. 📁 Project Structure

```text
docker-devops-project/
│
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── app/
│   │   └── main.py
│   └── data/
│
├── compose.yaml
├── .env
├── .dockerignore
├── .gitignore
└── README.md
```

---

# 52. 🐍 Backend Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app

RUN useradd --create-home appuser && \
    mkdir -p /data && \
    chown -R appuser:appuser /app /data

USER appuser

EXPOSE 8000

CMD [
  "uvicorn",
  "app.main:app",
  "--host",
  "0.0.0.0",
  "--port",
  "8000"
]
```

---

# 53. 🐍 FastAPI Backend

Example:

```python
from fastapi import FastAPI
import sqlite3
from pathlib import Path

app = FastAPI()

DB = Path("/data/app.db")


@app.get("/health")
def health():
    return {"status": "ok"}


@app.get("/items")
def items():

    DB.parent.mkdir(
        parents=True,
        exist_ok=True
    )

    conn = sqlite3.connect(DB)

    conn.execute(
        """
        CREATE TABLE IF NOT EXISTS items (
            id INTEGER PRIMARY KEY,
            name TEXT
        )
        """
    )

    rows = conn.execute(
        "SELECT id, name FROM items"
    ).fetchall()

    conn.close()

    return [
        {
            "id": row[0],
            "name": row[1]
        }
        for row in rows
    ]
```

---

# 54. ⚛️ Frontend Multi-stage Dockerfile

```dockerfile
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build


FROM nginx:alpine

COPY --from=builder \
  /app/dist \
  /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# 55. 🎼 Complete `compose.yaml`

```yaml
services:

  backend:

    build:
      context: ./backend

    ports:
      - "8000:8000"

    volumes:
      - sqlite-data:/data

    networks:
      - app-network

    healthcheck:
      test:
        [
          "CMD",
          "python",
          "-c",
          "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"
        ]

      interval: 10s
      timeout: 3s
      retries: 5


  frontend:

    build:
      context: ./frontend

    ports:
      - "3000:80"

    depends_on:
      - backend

    networks:
      - app-network


volumes:

  sqlite-data:


networks:

  app-network:
    driver: bridge
```

---

# 56. ▶️ Run the Project

From the project root:

```bash
docker compose up --build -d
```

Check containers:

```bash
docker compose ps
```

Expected architecture:

```text
frontend
backend
sqlite-data volume
app-network
```

---

# 57. 📋 View Logs

Frontend:

```bash
docker compose logs -f frontend
```

Backend:

```bash
docker compose logs -f backend
```

All services:

```bash
docker compose logs -f
```

---

# 58. 🌍 Access the Application

Frontend:

```text
http://localhost:3000
```

Backend:

```text
http://localhost:8000
```

Health endpoint:

```text
http://localhost:8000/health
```

Items endpoint:

```text
http://localhost:8000/items
```

---

# 59. 💾 Verify SQLite Persistence

Start the application:

```bash
docker compose up -d
```

Create/use database data.

Then remove containers:

```bash
docker compose down
```

Start again:

```bash
docker compose up -d
```

Because SQLite is stored in:

```text
sqlite-data
```

the data can survive container replacement.

Do **not** use:

```bash
docker compose down -v
```

when you want to preserve the named volume.

---

# 60. 🐛 Docker Troubleshooting

## Container immediately exits

Run:

```bash
docker ps -a
```

Then:

```bash
docker logs CONTAINER
```

Then:

```bash
docker inspect CONTAINER
```

Common causes:

* Application crash
* Incorrect CMD
* Missing dependency
* Missing environment variable
* Permission problem
* Incorrect configuration

---

## Port already in use

Example:

```text
Bind for 0.0.0.0:8080 failed:
port is already allocated
```

Use another host port:

```bash
docker run \
  -p 8081:80 \
  nginx
```

On Windows PowerShell:

```powershell
Get-NetTCPConnection -LocalPort 8080
```

---

## Containers cannot communicate

Check:

```bash
docker network ls
```

Then:

```bash
docker network inspect app-network
```

Verify:

* Both containers are on the same network.
* Correct service/container name is used.
* Application listens on `0.0.0.0`.
* Correct container port is being used.
* Application logs contain no errors.

---

# 61. 🔎 Useful Debugging Commands

```bash
docker ps
docker ps -a
docker logs CONTAINER
docker logs -f CONTAINER
docker inspect CONTAINER
docker stats
docker top CONTAINER
docker exec -it CONTAINER sh
docker network inspect NETWORK
docker volume inspect VOLUME
docker compose ps
docker compose logs
docker compose config
```

---

# 62. 🧪 Beginner Exercises

### Exercise 1

Run Nginx:

```bash
docker run -d nginx
```

### Exercise 2

Run Nginx on port `8080`:

```bash
docker run -d -p 8080:80 nginx
```

### Exercise 3

Give it a name:

```bash
docker run -d \
  --name web-server \
  -p 8080:80 \
  nginx
```

### Exercise 4

Check:

```bash
docker ps
docker logs web-server
docker inspect web-server
```

### Exercise 5

Open a shell:

```bash
docker exec -it web-server sh
```

---

# 63. 🧪 Intermediate Exercises

1. Create a custom Docker network.
2. Start two containers on that network.
3. Test container-to-container communication.
4. Create a named volume.
5. Store a file inside the volume.
6. Remove the container.
7. Create another container using the same volume.
8. Verify the file still exists.
9. Create a Dockerfile for a Python application.
10. Build and run the image.

---

# 64. 🧪 Advanced Exercises

1. Create a multi-stage Node.js Dockerfile.
2. Run your application as a non-root user.
3. Add a health check.
4. Add resource limits.
5. Create a Compose application.
6. Add frontend/backend networking.
7. Add persistent database storage.
8. Scan the image for vulnerabilities.
9. Push the image to Docker Hub.
10. Create a CI/CD pipeline that builds and publishes the image.

---

# 65. 💼 Docker Interview Questions

### 1. What is Docker?

Docker is a platform for packaging and running applications in isolated containers.

### 2. Container vs VM?

Containers share the host kernel, while VMs generally contain a complete guest operating system.

### 3. Image vs container?

An image is a template. A container is a runtime instance of that image.

### 4. What does `-p 8080:80` mean?

It publishes host port `8080` to container port `80`.

### 5. What is Dockerfile?

A Dockerfile contains instructions for building a Docker image.

### 6. CMD vs ENTRYPOINT?

`ENTRYPOINT` defines the main executable, while `CMD` provides defaults or arguments.

### 7. What is a Docker volume?

A Docker volume provides persistent storage independent of the container lifecycle.

### 8. What is Docker Compose?

Compose defines and runs multi-container applications using a YAML configuration.

### 9. Why use multi-stage builds?

To separate build dependencies from runtime dependencies and create smaller production images.

### 10. Why use `.dockerignore`?

To prevent unnecessary files from being included in the Docker build context.

### 11. Does EXPOSE publish a port?

No. `EXPOSE` documents a port. `-p` publishes it.

### 12. How do you debug a container?

Use:

```bash
docker ps -a
docker logs CONTAINER
docker inspect CONTAINER
docker exec -it CONTAINER sh
```

### 13. How do you persist SQLite?

Mount the directory containing the SQLite database file to a Docker volume.

---

# 66. 📋 Docker Command Cheat Sheet

## Containers

```bash
docker run IMAGE

docker run -d IMAGE

docker run -d \
  --name NAME \
  -p HOST:CONTAINER \
  IMAGE

docker ps

docker ps -a

docker start NAME

docker stop NAME

docker restart NAME

docker kill NAME

docker rm NAME

docker rm -f NAME

docker logs NAME

docker logs -f NAME

docker exec -it NAME sh

docker inspect NAME

docker stats
```

## Images

```bash
docker pull IMAGE

docker images

docker build -t NAME:TAG .

docker image inspect IMAGE

docker history IMAGE

docker tag IMAGE USER/IMAGE:TAG

docker push USER/IMAGE:TAG

docker rmi IMAGE
```

## Networks

```bash
docker network ls

docker network create NETWORK

docker network inspect NETWORK

docker network connect NETWORK CONTAINER

docker network disconnect NETWORK CONTAINER

docker network rm NETWORK
```

## Volumes

```bash
docker volume ls

docker volume create VOLUME

docker volume inspect VOLUME

docker volume rm VOLUME
```

## Compose

```bash
docker compose up

docker compose up -d

docker compose up --build -d

docker compose ps

docker compose logs

docker compose logs -f SERVICE

docker compose exec SERVICE sh

docker compose restart SERVICE

docker compose stop

docker compose down

docker compose down -v

docker compose config
```

---

# 67. 🎯 Job-Ready Docker Skills

Before moving to Kubernetes or advanced cloud DevOps, you should be able to confidently do the following:

* [ ] Explain Docker architecture
* [ ] Explain image vs container
* [ ] Run and manage containers
* [ ] Read container logs
* [ ] Debug failed containers
* [ ] Write Dockerfiles
* [ ] Understand every major Dockerfile instruction
* [ ] Build custom images
* [ ] Tag images
* [ ] Push images to a registry
* [ ] Create Docker networks
* [ ] Understand container DNS
* [ ] Use Docker volumes
* [ ] Persist application data
* [ ] Write Compose files
* [ ] Configure environment variables
* [ ] Add health checks
* [ ] Use resource limits
* [ ] Build multi-stage images
* [ ] Run containers as non-root
* [ ] Understand Docker security
* [ ] Containerize frontend applications
* [ ] Containerize backend applications
* [ ] Containerize a database workflow
* [ ] Troubleshoot networking and storage problems
* [ ] Explain Docker decisions during an interview

---

# 68. 🚀 Next Step: DevOps Roadmap

After mastering Docker, continue with:

```text
Docker
  ↓
Git & GitHub
  ↓
Linux
  ↓
Networking
  ↓
CI/CD
  ↓
Jenkins / GitHub Actions
  ↓
Terraform
  ↓
Cloud
  ↓
Kubernetes
  ↓
Monitoring
  ↓
DevSecOps
  ↓
Cloud DevOps Engineer
```

For a cloud-focused path:

```text
Docker
   ↓
AWS / GCP
   ↓
Container Registry
   ↓
CI/CD
   ↓
Terraform
   ↓
Kubernetes
   ↓
Cloud DevOps
```

---

# 🏆 Final Project Goal

By the end of this course, you should be able to take an application like:

```text
React Frontend
      +
FastAPI Backend
      +
SQLite Database
```

and turn it into:

```text
                   Internet
                       │
                       ▼
                ┌─────────────┐
                │   Frontend  │
                │   Container │
                └──────┬──────┘
                       │
                 Docker Network
                       │
                       ▼
                ┌─────────────┐
                │   Backend   │
                │   Container │
                └──────┬──────┘
                       │
                       ▼
                ┌─────────────┐
                │   SQLite    │
                │   Database  │
                └──────┬──────┘
                       │
                       ▼
                Docker Volume
```

Run everything with:

```bash
docker compose up --build -d
```

This is the practical Docker skillset you should be comfortable demonstrating in a **DevOps interview or portfolio project**.

---

## 📌 Author

**Sachin Borse**

Learning path focused on:

* DevOps
* Docker
* Cloud
* CI/CD
* Linux
* Kubernetes
* Infrastructure as Code

---

## ⭐ If this repository helps you

Give the repository a ⭐ and use the exercises to build your own Docker projects.

**🐳**
  
