# Pertemuan 8: UTS - Containerized Application Deployment

## 🎯 Tujuan UTS

Evaluasi pemahaman mahasiswa terhadap:
1. Docker containerization
2. Docker Compose orchestration
3. Kubernetes deployment
4. Cloud-native best practices
5. Documentation dan troubleshooting

## 📋 Project Requirements

### Pilihan Project

**Option 1: Multi-Tier Web Application**
- Frontend (React/Vue/Angular)
- Backend API (Node.js/Python/Go)
- Database (PostgreSQL/MongoDB)
- Cache (Redis)

**Option 2: Microservices E-commerce**
- Product Service
- User Service
- Order Service
- Payment Service
- API Gateway

**Option 3: Content Management System**
- CMS Application (WordPress/Strapi)
- Database
- Object Storage
- CDN/Reverse Proxy

## 📝 Project Structure

### Part 1: Dockerization (25 points)

**Requirements:**
- Dockerfile untuk setiap service
- Multi-stage builds
- Optimized image size (< 500MB total)
- Security best practices
- Health checks

**Deliverables:**
```
project/
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── src/
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── app/
└── database/
    └── init-scripts/
```

### Part 2: Docker Compose (25 points)

**Requirements:**
- docker-compose.yml
- Service dependencies
- Network configuration
- Volume management
- Environment variables
- Service scaling

**Example:**
```yaml
version: '3.8'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    environment:
      - API_URL=http://backend:5000

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - database
      - redis
    environment:
      - DB_HOST=database
      - REDIS_HOST=redis

  database:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}

  redis:
    image: redis:7-alpine

volumes:
  db-data:
```

### Part 3: Kubernetes Deployment (30 points)

**Requirements:**
- Deployment manifests
- Service definitions
- ConfigMaps & Secrets
- Resource limits
- Probes (liveness & readiness)
- Horizontal Pod Autoscaling (bonus)

**Manifests:**
```
k8s/
├── namespace.yaml
├── deployments/
│   ├── frontend-deployment.yaml
│   ├── backend-deployment.yaml
│   └── database-deployment.yaml
├── services/
│   ├── frontend-service.yaml
│   ├── backend-service.yaml
│   └── database-service.yaml
├── configmaps/
│   └── app-config.yaml
└── secrets/
    └── db-secrets.yaml
```

### Part 4: CI/CD Pipeline (10 points)

**Requirements:**
- GitHub Actions atau GitLab CI
- Automated build
- Image push to registry
- Basic tests

**Example workflow:**
```yaml
name: Build and Push

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
      
      - name: Push to registry
        run: |
          docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
          docker push myapp:${{ github.sha }}
```

### Part 5: Documentation (10 points)

**Requirements:**
- Architecture diagram
- Setup instructions
- API documentation
- Troubleshooting guide
- Performance considerations

## ✅ Rubrik Penilaian

| Komponen | Points | Kriteria |
|----------|--------|----------|
| **Dockerization** | 25 | Multi-stage, optimized, secure |
| **Docker Compose** | 25 | All services working, proper networking |
| **Kubernetes** | 30 | Deployments, services, scaling |
| **CI/CD** | 10 | Automated pipeline working |
| **Documentation** | 10 | Clear, complete, professional |
| **Code Quality** | 10 | Clean, organized, commented |
| **Demo** | 10 | Live demonstration |
| **TOTAL** | **120** | **Normalized to 100** |

## 📤 Deliverables

### 1. Source Code Structure

```
NIM_Nama_UTS/
├── docker/
│   ├── frontend/Dockerfile
│   ├── backend/Dockerfile
│   └── docker-compose.yml
├── kubernetes/
│   ├── deployments/
│   ├── services/
│   └── configs/
├── .github/workflows/
│   └── ci.yml
├── docs/
│   ├── README.md
│   ├── ARCHITECTURE.md
│   ├── API.md
│   └── DEPLOYMENT.md
└── src/
    ├── frontend/
    └── backend/
```

### 2. Documentation Requirements

**README.md:**
- Project overview
- Architecture diagram
- Prerequisites
- Quick start guide
- Environment variables
- Troubleshooting

**ARCHITECTURE.md:**
- System design
- Component interactions
- Technology stack
- Scalability considerations

**DEPLOYMENT.md:**
- Docker deployment steps
- Kubernetes deployment steps
- Monitoring setup
- Backup/restore procedures

## 💡 Grading Criteria

### Excellent (90-100)
- All services containerized perfectly
- Kubernetes deployment production-ready
- CI/CD fully automated
- Comprehensive documentation
- Advanced features (monitoring, auto-scaling)

### Good (80-89)
- Services containerized well
- Kubernetes deployment working
- Basic CI/CD
- Good documentation
- Most features implemented

### Satisfactory (70-79)
- Basic containerization
- Kubernetes partially working
- Manual deployment process
- Basic documentation
- Core features only

### Needs Improvement (<70)
- Incomplete containerization
- Kubernetes not working
- No automation
- Poor documentation

## 🚫 Common Mistakes to Avoid

1. ❌ Hardcoded configurations
2. ❌ No health checks
3. ❌ Missing resource limits
4. ❌ Root user in containers
5. ❌ Large image sizes
6. ❌ No documentation
7. ❌ Services can't communicate
8. ❌ No error handling

## ✅ Best Practices Checklist

- [ ] All images < 500MB
- [ ] Non-root users
- [ ] Health checks configured
- [ ] Resource limits set
- [ ] Secrets not in code
- [ ] Services properly networked
- [ ] Documentation complete
- [ ] CI/CD pipeline working
- [ ] Can deploy from scratch
- [ ] Logs accessible

## 📊 Example Projects

### Example 1: Blog Platform
- Frontend: React
- Backend: Node.js + Express
- Database: PostgreSQL
- Cache: Redis
- Features: CRUD, Auth, Comments

### Example 2: Task Manager
- Frontend: Vue.js
- Backend: Python + Flask
- Database: MongoDB
- Queue: RabbitMQ
- Features: Tasks, Users, Notifications

### Example 3: API Gateway
- Gateway: Kong/Traefik
- Service 1: User API
- Service 2: Product API
- Database: MySQL
- Features: Rate limiting, Auth

## 📧 Submission

**Deadline:** [Sesuai jadwal - Pertemuan 8]

**Format:** 
- GitHub repository URL
- Or `NIM_Nama_UTS_CloudNative.zip`

**Upload:** LMS

**Late Submission:**
- 1 day: -10 points
- 2 days: -20 points
- 3+ days: -50 points

## ❓ FAQ

**Q: Boleh pakai existing open-source project?**
A: Boleh, tapi harus ada modifikasi dan improvement significant.

**Q: Harus deploy ke cloud?**
A: Tidak wajib. Minikube sudah cukup.

**Q: Berapa minimal services?**
A: Minimal 3 services (frontend, backend, database).

**Q: Boleh pakai managed databases?**
A: Untuk UTS, sebaiknya containerized semua.

## 🎯 Tips Sukses

### Technical
1. Test Docker compose first
2. Then move to Kubernetes
3. Use version control
4. Implement incrementally
5. Test each component

### Time Management
- Week 1: Dockerization
- Week 2: Kubernetes
- Week 3: CI/CD & Documentation
- Week 4: Testing & Demo prep

### Documentation
1. Start documentation early
2. Screenshot key steps
3. Document errors and solutions
4. Include architecture diagrams

## 🔗 Resources

- [Docker Samples](https://github.com/docker/awesome-compose)
- [Kubernetes Examples](https://github.com/kubernetes/examples)
- [12-Factor App](https://12factor.net/)

---

## 📋 Final Checklist

Before submission:

- [ ] Docker Compose starts all services
- [ ] Kubernetes deploys successfully
- [ ] All health checks passing
- [ ] Services can communicate
- [ ] CI/CD pipeline runs
- [ ] Documentation complete
- [ ] Architecture diagram included
- [ ] Demo video ready (optional)
- [ ] Code clean and organized
- [ ] Repository well-structured

---

**Build Something Amazing! 🚀☁️**

**Remember:** This is your chance to showcase everything learned in pertemuan 1-7. Make it production-quality!

**Questions?** Contact asisten praktikum or discuss in class forum.
