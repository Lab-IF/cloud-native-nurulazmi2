# Pertemuan 7: Pods, Deployments, dan Services

## 🎯 Tujuan Pembelajaran

1. Deep dive into Pods
2. Managing Deployments
3. Service types dan networking
4. Labels dan Selectors
5. Rolling updates dan Rollbacks

## 📚 Teori Singkat

### Pod Lifecycle

```
Pending → Running → Succeeded/Failed
```

### Deployment Strategies

1. **Rolling Update**: Gradual replacement
2. **Recreate**: Stop all, start new
3. **Blue-Green**: Two identical environments
4. **Canary**: Gradual traffic shift

### Service Types

- **ClusterIP**: Internal only (default)
- **NodePort**: Expose on node port
- **LoadBalancer**: Cloud load balancer
- **ExternalName**: DNS CNAME

## 📝 Praktikum

### Multi-Container Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
  
  - name: log-agent
    image: busybox
    command: ['sh', '-c', 'tail -f /var/log/nginx/access.log']
    volumeMounts:
    - name: logs
      mountPath: /var/log/nginx
  
  volumes:
  - name: logs
    emptyDir: {}
```

### Deployment with Probes

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
        version: v1
    spec:
      containers:
      - name: web
        image: myapp:1.0
        ports:
        - containerPort: 8080
        
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
        
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 500m
            memory: 512Mi
```

### Service Examples

```yaml
# ClusterIP (Internal)
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
  - port: 8080
    targetPort: 8080

---

# NodePort (External Access)
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
  - port: 80
    targetPort: 3000
    nodePort: 30080

---

# LoadBalancer (Cloud)
apiVersion: v1
kind: Service
metadata:
  name: public-service
spec:
  type: LoadBalancer
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 8080
```

### Rolling Update

```bash
# Update image
kubectl set image deployment/web-app web=myapp:2.0

# Check rollout status
kubectl rollout status deployment/web-app

# View history
kubectl rollout history deployment/web-app

# Rollback
kubectl rollout undo deployment/web-app

# Rollback to specific revision
kubectl rollout undo deployment/web-app --to-revision=2
```

### Labels & Selectors

```bash
# Add label
kubectl label pod my-pod environment=production

# Filter by label
kubectl get pods -l environment=production
kubectl get pods -l 'environment in (production,staging)'

# Remove label
kubectl label pod my-pod environment-
```

## 💪 Tugas Praktikum

### Tugas 1: Multi-Tier Application (35%)

Deploy 3-tier app:
- Frontend (React) - 3 replicas
- Backend (API) - 5 replicas
- Database (PostgreSQL) - 1 replica

Requirements:
- Proper service connections
- Health checks
- Resource limits
- Rolling update strategy

### Tugas 2: Blue-Green Deployment (25%)

Implement blue-green deployment:
- Deploy version 1 (blue)
- Deploy version 2 (green)
- Switch traffic from blue to green
- Document the process

### Tugas 3: Canary Release (25%)

Implement canary deployment:
- 90% traffic to stable version
- 10% traffic to canary
- Monitor and gradually increase
- Rollback if issues

### Tugas 4: Service Mesh Basics (15%)

Explore service discovery:
- Internal DNS resolution
- Service-to-service communication
- Network policies

## 📚 Referensi

[Kubernetes Workloads](https://kubernetes.io/docs/concepts/workloads/)

---
**Deploy with Confidence! 🚀**
