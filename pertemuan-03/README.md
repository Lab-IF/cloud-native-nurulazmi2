# Pertemuan 3: Dockerfile Best Practices

## 🎯 Tujuan Pembelajaran

1. Menulis efficient Dockerfiles
2. Optimasi image size
3. Multi-stage builds
4. Layer caching strategies
5. Security best practices

## 📚 Best Practices

### 1. Official Base Images
```dockerfile
# Good
FROM python:3.11-slim

# Avoid
FROM ubuntu
```

### 2. Minimize Layers
```dockerfile
# Bad
RUN apt-get update
RUN apt-get install curl

# Good
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
```

### 3. Multi-Stage Build
```dockerfile
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### 4. .dockerignore
```
node_modules
.git
*.md
```

### 5. Security
```dockerfile
RUN useradd -m appuser
USER appuser
```

## 💪 Tugas Praktikum

### Tugas 1: Optimization (30%)
Reduce image size by 50%

### Tugas 2: Multi-Stage (35%)
Build production React app

### Tugas 3: Security (20%)
Non-root user, minimal dependencies

### Tugas 4: Linting (15%)
Use hadolint

---
**Optimize! 📦**
