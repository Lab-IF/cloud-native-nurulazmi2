# Pertemuan 2: Docker Fundamentals - Images & Containers

## 🎯 Tujuan Pembelajaran

1. Memahami perbedaan Docker images vs containers
2. Menguasai Docker CLI commands
3. Building custom Docker images
4. Managing containers lifecycle
5. Docker networking basics

## 📚 Teori Singkat

### Docker Architecture

```
┌──────────────────────────────────┐
│       Docker Client (CLI)        │
└────────────┬─────────────────────┘
             │
┌────────────▼─────────────────────┐
│       Docker Daemon              │
│  ┌──────────────────────────┐   │
│  │   Container Runtime      │   │
│  └──────────────────────────┘   │
└──────────────────────────────────┘
             │
┌────────────▼─────────────────────┐
│       Host OS Kernel             │
└──────────────────────────────────┘
```

### Images vs Containers

**Image:**
- Read-only template
- Contains application code + dependencies
- Layered filesystem
- Stored in registry

**Container:**
- Running instance of image
- Writable layer on top
- Isolated process
- Ephemeral

### Docker Lifecycle

```
docker pull  → docker create → docker start → docker stop → docker rm
                                    ↓
                              docker run (create + start)
```

## 📝 Praktikum

### Basic Commands

```bash
# Pull image
docker pull ubuntu:22.04
docker pull nginx:latest

# List images
docker images

# Run container
docker run ubuntu echo "Hello Docker"
docker run -it ubuntu bash  # Interactive
docker run -d nginx        # Detached

# List containers
docker ps       # Running only
docker ps -a    # All

# Container management
docker stop <container-id>
docker start <container-id>
docker restart <container-id>
docker rm <container-id>

# Image management
docker rmi <image-id>
docker image prune  # Remove unused
```

### Create First Dockerfile

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

```bash
# Build image
docker build -t my-python-app:1.0 .

# Run container
docker run -d -p 5000:5000 --name myapp my-python-app:1.0

# View logs
docker logs myapp
docker logs -f myapp  # Follow

# Execute command in running container
docker exec -it myapp bash
```

### Port Mapping & Volumes

```bash
# Port mapping
docker run -d -p 8080:80 nginx
docker run -d -p 3000:3000 -p 8080:8080 myapp

# Volume mounting
docker run -v /host/path:/container/path nginx
docker run -v mydata:/app/data myapp

# Named volumes
docker volume create mydata
docker volume ls
docker volume inspect mydata
```

## 💪 Tugas Praktikum

### Tugas 1: Docker Commands Mastery (20%)
Practice all basic commands, create cheatsheet

### Tugas 2: Build Multi-Stage Image (30%)
Optimize image size dengan multi-stage build

### Tugas 3: Persistent Data (25%)
Implement volume for database container

### Tugas 4: Container Networking (25%)
Connect multiple containers

## 📚 Referensi

[Docker Get Started](https://docs.docker.com/get-started/)

---
**Master Docker! 🐳**
