# Pertemuan 1: Introduction to Cloud-Native Principles

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan pertemuan ini, mahasiswa diharapkan mampu:
1. Memahami konsep Cloud-Native architecture
2. Mengenal 12-Factor App principles
3. Memahami perbedaan traditional vs cloud-native applications
4. Setup development environment untuk cloud-native
5. Mengenal microservices architecture pattern

## 📚 Teori Singkat

### Apa itu Cloud-Native?

Cloud-Native adalah pendekatan untuk building dan running applications yang memanfaatkan keuntungan dari cloud computing delivery model.

**Karakteristik:**
- **Containerized**: Packaged dalam containers
- **Dynamically orchestrated**: Automated container lifecycle
- **Microservices-oriented**: Loosely coupled services
- **API-driven**: Communication via APIs

### Cloud-Native Architecture

```
┌─────────────────────────────────────────┐
│     Load Balancer / API Gateway         │
└─────────────────┬───────────────────────┘
                  │
    ┌─────────────┼─────────────┐
    │             │             │
┌───▼────┐  ┌────▼─────┐  ┌───▼────┐
│Service │  │ Service  │  │Service │
│   A    │  │    B     │  │   C    │
│(Docker)│  │ (Docker) │  │(Docker)│
└────┬───┘  └────┬─────┘  └───┬────┘
     │           │            │
     └───────────┼────────────┘
                 │
        ┌────────▼─────────┐
        │    Database      │
        └──────────────────┘
```

### 12-Factor App Principles

1. **Codebase**: One codebase tracked in version control
2. **Dependencies**: Explicitly declare dependencies
3. **Config**: Store config in environment variables
4. **Backing Services**: Treat as attached resources
5. **Build, Release, Run**: Separate stages
6. **Processes**: Stateless processes
7. **Port Binding**: Export services via port binding
8. **Concurrency**: Scale out via process model
9. **Disposability**: Fast startup and graceful shutdown
10. **Dev/Prod Parity**: Keep development and production similar
11. **Logs**: Treat logs as event streams
12. **Admin Processes**: Run as one-off processes

### Traditional vs Cloud-Native

| Aspect | Traditional | Cloud-Native |
|--------|------------|--------------|
| Architecture | Monolithic | Microservices |
| Deployment | Manual | Automated (CI/CD) |
| Scaling | Vertical (bigger servers) | Horizontal (more instances) |
| Infrastructure | Physical/VMs | Containers |
| State | Stateful | Stateless |
| Configuration | Files | Environment variables |
| Updates | Downtime | Rolling updates |

## 🛠️ Setup Environment

### Prerequisites

Install the following tools:

#### 1. Docker
```bash
# Ubuntu/Debian
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Verify
docker --version
docker run hello-world
```

#### 2. Docker Compose
```bash
# Download
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make executable
sudo chmod +x /usr/local/bin/docker-compose

# Verify
docker-compose --version
```

#### 3. kubectl (Kubernetes CLI)
```bash
# Download
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Install
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Verify
kubectl version --client
```

#### 4. Git
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install git

# Verify
git --version
```

#### 5. Code Editor
- Visual Studio Code (recommended)
- Extensions: Docker, Kubernetes, YAML

### Cloud Environment Options

**Option 1: Local Development**
- Docker Desktop (Windows/Mac)
- Minikube (local Kubernetes)

**Option 2: Cloud Providers**
- AWS (EC2, EKS)
- Google Cloud (GCE, GKE)
- Azure (VMs, AKS)
- DigitalOcean (Droplets, DOKS)

**Option 3: Online Playgrounds**
- Play with Docker (https://labs.play-with-docker.com/)
- Katacoda (https://www.katacoda.com/)
- Google Cloud Shell

## 📝 Praktikum

### Langkah 1: Verify Docker Installation

```bash
# Check Docker version
docker --version

# Run test container
docker run hello-world

# Check running containers
docker ps

# Check all containers (including stopped)
docker ps -a

# Check images
docker images
```

### Langkah 2: First Docker Container

```bash
# Run nginx web server
docker run -d -p 8080:80 --name my-nginx nginx

# Check if running
docker ps

# Access in browser
# http://localhost:8080

# View logs
docker logs my-nginx

# Stop container
docker stop my-nginx

# Remove container
docker rm my-nginx
```

### Langkah 3: Simple Web Application

**Create `app.py`:**
```python
from flask import Flask
import os

app = Flask(__name__)

@app.route('/')
def hello():
    return f"""
    <h1>Hello from Cloud-Native App!</h1>
    <p>Hostname: {os.getenv('HOSTNAME', 'unknown')}</p>
    <p>Version: 1.0</p>
    """

@app.route('/health')
def health():
    return {'status': 'healthy'}, 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Create `requirements.txt`:**
```
Flask==3.0.0
```

**Run locally:**
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows

# Install dependencies
pip install -r requirements.txt

# Run application
python app.py

# Test
curl http://localhost:5000
```

### Langkah 4: Cloud-Native Checklist

Create a checklist for your application:

```markdown
## Cloud-Native Readiness Checklist

- [ ] **Containerized**: Can run in Docker?
- [ ] **Stateless**: No local state storage?
- [ ] **Config via ENV**: Configuration from environment variables?
- [ ] **Health Endpoints**: /health endpoint available?
- [ ] **Logging**: Logs to stdout/stderr?
- [ ] **Graceful Shutdown**: Handle SIGTERM signal?
- [ ] **12-Factor**: Follows 12-factor principles?
- [ ] **API-driven**: Communication via REST/gRPC?
- [ ] **Horizontally Scalable**: Can run multiple instances?
- [ ] **Versioned**: Proper version tagging?
```

### Langkah 5: Microservices Example

**Architecture:**
```
Frontend Service (React/Vue)
        ↓
    API Gateway
        ↓
    ┌───┴───┐
    │       │
User Service  Order Service
    │           │
    ↓           ↓
  User DB    Order DB
```

**Simple Microservice:**
```python
# user-service.py
from flask import Flask, jsonify
import os

app = Flask(__name__)

users = [
    {"id": 1, "name": "Alice", "email": "alice@example.com"},
    {"id": 2, "name": "Bob", "email": "bob@example.com"}
]

@app.route('/users')
def get_users():
    return jsonify(users)

@app.route('/users/<int:user_id>')
def get_user(user_id):
    user = next((u for u in users if u['id'] == user_id), None)
    if user:
        return jsonify(user)
    return jsonify({"error": "User not found"}), 404

@app.route('/health')
def health():
    return jsonify({"status": "healthy", "service": "user-service"})

if __name__ == '__main__':
    port = int(os.getenv('PORT', 5001))
    app.run(host='0.0.0.0', port=port)
```

## 💪 Tugas Praktikum

### Tugas 1: Environment Setup (20 poin)

1. Install semua required tools:
   - Docker
   - Docker Compose
   - kubectl
   - Git
2. Verify instalasi dengan screenshot:
   - `docker --version`
   - `docker-compose --version`
   - `kubectl version --client`
   - `git --version`
3. Run hello-world container dan screenshot output
4. Dokumentasikan langkah instalasi yang Anda lakukan

### Tugas 2: 12-Factor App Analysis (25 poin)

Pilih satu aplikasi open-source (contoh: WordPress, GitLab, Nextcloud):

1. Analisis aplikasi berdasarkan 12-Factor principles
2. Buat tabel checklist:
   - Principle name
   - Compliance status (Yes/No/Partial)
   - Evidence/explanation
   - Recommendation for improvement
3. Kesimpulan: Apakah aplikasi ini cloud-native ready?

### Tugas 3: Simple Cloud-Native App (30 poin)

Buat REST API dengan requirements:

1. **Functionality:**
   - Minimal 3 endpoints (GET, POST, DELETE)
   - Health check endpoint
   - CRUD operations

2. **Cloud-Native Features:**
   - Configuration via environment variables
   - Logging to stdout
   - Stateless (no local file storage)
   - Graceful shutdown handling

3. **Documentation:**
   - API documentation (endpoints, methods, responses)
   - Environment variables needed
   - How to run locally

**Example endpoints:**
```
GET    /api/items        # List all items
GET    /api/items/:id    # Get item by ID
POST   /api/items        # Create new item
PUT    /api/items/:id    # Update item
DELETE /api/items/:id    # Delete item
GET    /health           # Health check
```

### Tugas 4: Architecture Design (25 poin)

Design microservices architecture untuk salah satu:

**Option A: E-commerce Platform**
- User Service
- Product Service
- Cart Service
- Order Service
- Payment Service

**Option B: Social Media Platform**
- User Service
- Post Service
- Comment Service
- Like Service
- Notification Service

**Deliverables:**
1. Architecture diagram (gunakan draw.io, Lucidchart, atau tool lain)
2. Deskripsi setiap service:
   - Responsibility
   - APIs exposed
   - Database schema
   - Communication pattern
3. Technology stack recommendation
4. Scalability considerations

## 📤 Cara Mengumpulkan

1. **Tugas 1**: Screenshots + dokumentasi dalam PDF
2. **Tugas 2**: Analysis report dalam PDF/Markdown
3. **Tugas 3**: Source code + README.md
4. **Tugas 4**: Architecture diagram + documentation
5. Compress: `NIM_Nama_Pertemuan01.zip`
6. Upload ke LMS

## ✅ Kriteria Penilaian

| Aspek | Bobot |
|-------|-------|
| Tugas 1: Environment Setup | 20% |
| Tugas 2: 12-Factor Analysis | 25% |
| Tugas 3: Cloud-Native App | 30% |
| Tugas 4: Architecture Design | 25% |
| Documentation quality | 15% |
| Code quality | 10% |

## 📚 Referensi

1. [The Twelve-Factor App](https://12factor.net/)
2. [Cloud Native Computing Foundation](https://www.cncf.io/)
3. [Docker Documentation](https://docs.docker.com/)
4. [Kubernetes Documentation](https://kubernetes.io/docs/)
5. [Microservices Patterns](https://microservices.io/)

## 💡 Tips

- Start small and iterate
- Follow 12-factor principles from day one
- Think stateless for easy scaling
- Use environment variables for configuration
- Implement health checks in every service
- Log everything to stdout/stderr
- Design for failure (retry, timeout, circuit breaker)

## 🔗 Useful Resources

- [Play with Docker](https://labs.play-with-docker.com/)
- [Awesome Cloud Native](https://github.com/rootsongjc/awesome-cloud-native)
- [Cloud Native Landscape](https://landscape.cncf.io/)

---

**Welcome to Cloud-Native! ☁️🚀**

Jika ada pertanyaan, silakan diskusikan di forum kelas atau hubungi asisten praktikum.
