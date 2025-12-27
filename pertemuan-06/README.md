# Pertemuan 6: Kubernetes Architecture & Concepts

## 🎯 Tujuan Pembelajaran

1. Memahami Kubernetes architecture
2. Setup Kubernetes cluster (Minikube)
3. Kubernetes components (Master & Worker nodes)
4. kubectl CLI basics
5. Kubernetes objects (Pods, Deployments, Services)

## 📚 Teori Singkat

### Kubernetes Architecture

```
┌─────────────────────────────────────┐
│         Master Node                 │
│  ┌──────────────────────────────┐  │
│  │  API Server                   │  │
│  │  Scheduler                    │  │
│  │  Controller Manager           │  │
│  │  etcd (Key-Value Store)       │  │
│  └──────────────────────────────┘  │
└─────────────────┬───────────────────┘
                  │
    ┌─────────────┼─────────────┐
    │             │             │
┌───▼────┐   ┌───▼────┐   ┌───▼────┐
│ Worker │   │ Worker │   │ Worker │
│ Node 1 │   │ Node 2 │   │ Node 3 │
│┌──────┐│   │┌──────┐│   │┌──────┐│
││Kubelet││   ││Kubelet││   ││Kubelet││
││Kube  ││   ││Kube  ││   ││Kube  ││
││Proxy ││   ││Proxy ││   ││Proxy ││
│└──────┘│   │└──────┘│   │└──────┘│
└────────┘   └────────┘   └────────┘
```

### Core Components

**Control Plane:**
- API Server: Central management
- Scheduler: Pod placement
- Controller Manager: Cluster state
- etcd: Configuration data

**Worker Nodes:**
- Kubelet: Node agent
- Kube-proxy: Network proxy
- Container Runtime: Docker/containerd

### Key Concepts

- **Pod**: Smallest deployable unit
- **Deployment**: Manages ReplicaSets
- **Service**: Network endpoint
- **Namespace**: Virtual clusters
- **ConfigMap**: Configuration data
- **Secret**: Sensitive data

## 🛠️ Setup Minikube

```bash
# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start cluster
minikube start --cpus=2 --memory=4096

# Verify
kubectl cluster-info
kubectl get nodes

# Minikube dashboard
minikube dashboard
```

## 📝 Praktikum

### kubectl Basics

```bash
# Cluster info
kubectl cluster-info
kubectl get nodes
kubectl get namespaces

# Create namespace
kubectl create namespace dev

# Get resources
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get all

# Describe resource
kubectl describe pod <pod-name>
kubectl describe node <node-name>

# Logs
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow

# Execute command
kubectl exec -it <pod-name> -- /bin/bash
```

### First Pod

```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

```bash
# Apply configuration
kubectl apply -f pod.yaml

# Check status
kubectl get pods
kubectl describe pod nginx-pod

# Port forward
kubectl port-forward nginx-pod 8080:80

# Delete pod
kubectl delete pod nginx-pod
```

### First Deployment

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

```bash
# Create deployment
kubectl apply -f deployment.yaml

# Check deployment
kubectl get deployments
kubectl get pods

# Scale deployment
kubectl scale deployment nginx-deployment --replicas=5

# Update image
kubectl set image deployment/nginx-deployment nginx=nginx:1.22
```

### First Service

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

```bash
# Create service
kubectl apply -f service.yaml

# Access service
minikube service nginx-service

# Get service URL
minikube service nginx-service --url
```

## 💪 Tugas Praktikum

### Tugas 1: Minikube Setup (20%)
Install, start cluster, screenshot dashboard

### Tugas 2: kubectl Mastery (25%)
Practice all basic commands, create cheatsheet

### Tugas 3: Deploy Application (30%)
Deploy web app dengan 3 replicas + service

### Tugas 4: Troubleshooting (25%)
Debug failing pods, analyze logs

## 📚 Referensi

1. [Kubernetes Documentation](https://kubernetes.io/docs/)
2. [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
3. [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)

---
**Welcome to Kubernetes! ☸️**
