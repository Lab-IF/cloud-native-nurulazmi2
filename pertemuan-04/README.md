# Pertemuan 4: Docker Compose untuk Multi-Container Apps

## 🎯 Tujuan Pembelajaran

1. Menguasai Docker Compose
2. Orchestrate multiple services
3. Network dan volume management
4. Environment variables
5. Service dependencies

## 📚 Teori Singkat

### Docker Compose

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "8080:80"
    depends_on:
      - db
    environment:
      - DB_HOST=db
      - DB_PORT=5432
    networks:
      - app-network

  db:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: secret
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
```

### Commands

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Scale services
docker-compose up -d --scale web=3
```

## 📝 Praktikum

### Full-Stack Application

```yaml
# docker-compose.yml
version: '3.8'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    environment:
      - REACT_APP_API_URL=http://backend:5000

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - database
      - redis
    environment:
      - DATABASE_URL=postgresql://user:pass@database:5432/mydb
      - REDIS_URL=redis://redis:6379

  database:
    image: postgres:15
    volumes:
      - postgres-data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data

volumes:
  postgres-data:
  redis-data:
```

## 💪 Tugas Praktikum

### Tugas 1: MERN Stack (30%)
MongoDB, Express, React, Node

### Tugas 2: Microservices (35%)
3+ services dengan API Gateway

### Tugas 3: WordPress (20%)
WordPress + MySQL + Redis

### Tugas 4: Monitoring Stack (15%)
Prometheus + Grafana

---
**Compose It! 🎼**
