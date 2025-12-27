# Pertemuan 5: Container Registry - Docker Hub & Private Registry

## 🎯 Tujuan Pembelajaran

1. Push/pull images dari Docker Hub
2. Setup private Docker registry
3. Image tagging strategies
4. Registry security
5. CI/CD integration

## 📚 Teori Singkat

### Container Registry

**Public Registries:**
- Docker Hub
- GitHub Container Registry
- Google Container Registry
- Amazon ECR

**Private Registry:**
- Harbor
- Nexus
- Self-hosted Docker Registry

## 📝 Praktikum

### Docker Hub

```bash
# Login
docker login

# Tag image
docker tag myapp:latest username/myapp:1.0
docker tag myapp:latest username/myapp:latest

# Push to Docker Hub
docker push username/myapp:1.0
docker push username/myapp:latest

# Pull from Docker Hub
docker pull username/myapp:1.0
```

### Private Registry

```yaml
# docker-compose.yml
version: '3.8'

services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
    volumes:
      - registry-data:/var/lib/registry
    environment:
      REGISTRY_STORAGE_DELETE_ENABLED: "true"

volumes:
  registry-data:
```

```bash
# Tag for private registry
docker tag myapp localhost:5000/myapp:1.0

# Push to private registry
docker push localhost:5000/myapp:1.0

# Pull from private registry
docker pull localhost:5000/myapp:1.0
```

### Image Tagging Strategy

```bash
# Semantic versioning
docker tag myapp:latest myapp:1.0.0
docker tag myapp:latest myapp:1.0
docker tag myapp:latest myapp:1

# Environment tags
docker tag myapp:latest myapp:dev
docker tag myapp:latest myapp:staging
docker tag myapp:latest myapp:prod

# Git commit hash
docker tag myapp:latest myapp:abc1234

# Combined
docker tag myapp:latest myapp:1.0.0-abc1234
```

## 💪 Tugas Praktikum

### Tugas 1: Docker Hub (25%)
Create account, push 3 images dengan proper tags

### Tugas 2: Private Registry (30%)
Setup dengan TLS dan authentication

### Tugas 3: Harbor (25%)
Install Harbor, vulnerability scanning

### Tugas 4: CI/CD Pipeline (20%)
GitHub Actions push to registry

---
**Push Your Images! 📤**
