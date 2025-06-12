# Day 8: Kubernetes Controllers

## 🎯 What are Controllers?

Controllers ensure **desired state** matches **actual state** in your cluster. They manage pod lifecycle automatically.

## 📊 Controller Types

### 1. ReplicationController (Legacy)
- Ensures specified number of pod replicas
- Basic scaling and self-healing
- **Deprecated** - use ReplicaSet instead

### 2. ReplicaSet 
- Next-gen ReplicationController
- **Set-based selectors** (more flexible)
- Usually managed by Deployments

### 3. Deployment (Recommended)
- Manages ReplicaSets
- **Rolling updates** & **rollbacks**
- Production-ready scaling

## 🚀 Hands-on Practice

### ReplicaSet Example

Using `rs.yml`:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    matchLabels:
      env: demo
  template:
    metadata:
      labels:
        env: demo
    spec:
      containers:
      - name: nginx
        image: nginx
```

```bash
# Apply ReplicaSet
kubectl apply -f rs.yml

# Check ReplicaSet status
kubectl get rs
kubectl get pods -l env=demo
```

## 2. ReplicaSet (Recommended over RC)

### Apply ReplicaSet
```bash
kubectl apply -f rc.yml  # Note: rc.yml contains ReplicaSet config
```

### ReplicaSet Operations
```bash
# List ReplicaSets
kubectl get rs

# Describe ReplicaSet
kubectl describe rs nginx-rs

# Edit ReplicaSet
kubectl edit rs nginx-rs

# Scale ReplicaSet
kubectl scale rs/nginx-rs --replicas=5

# Update image
kubectl set image rs/nginx-rs nginx=nginx:1.7.9
```

### Deployment Example

Using `rc-dep.yml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
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
        image: nginx:latest
```

```bash
# Apply Deployment
kubectl apply -f rc-dep.yml

# Check Deployment status
kubectl get deployments
kubectl get rs
kubectl get pods
```

## 🔧 Common Operations

### Scaling

```bash
# Scale ReplicaSet
kubectl scale rs nginx-rs --replicas=5

# Scale Deployment
kubectl scale deployment nginx-deploy --replicas=5
```

### Updates & Rollbacks

```bash
# Update image (Deployment only)
kubectl set image deployment/nginx-deploy nginx=nginx:1.21

# Check rollout status
kubectl rollout status deployment/nginx-deploy

# Rollback deployment
kubectl rollout undo deployment/nginx-deploy
```

### Pod Management

```bash
# Delete a pod (auto-recreated by controller)
kubectl delete pod <pod-name>

# View controller events
kubectl describe rs nginx-rs
```

## 🧹 Cleanup

```bash
# Delete ReplicaSet
kubectl delete rs nginx-rs

# Delete Deployment
kubectl delete deployment nginx-deploy
```

## 📊 Key Differences

| Feature | ReplicationController | ReplicaSet | Deployment |
|---------|---------------------|------------|------------|
| **Selectors** | Equality-based | Set-based | Set-based |
| **Rolling Updates** | ❌ | ❌ | ✅ |
| **Rollback** | ❌ | ❌ | ✅ |
| **Status** | Deprecated | Active | **Recommended** |

## 💡 Best Practices

- Use **Deployments** for production workloads
- Set **resource limits** and **requests**
- Use **meaningful labels** for selection
- Monitor **rollout status** during updates

---
