# Day 7: Kubernetes Pods

## 🚀 What are Pods?

**Pods** are the smallest deployable units in Kubernetes. They wrap one or more containers with shared storage and network.

## 🎯 Key Concepts

- **Single Pod = One or more containers**
- **Shared network** (same IP, different ports)  
- **Shared storage** volumes
- **Lifecycle** tied together

## 📝 Hands-on Practice

### 1. Imperative Approach

```bash
# Create pod directly
kubectl run nginx-pod --image=nginx:latest

# Get pod info
kubectl get pods
kubectl describe pod nginx-pod
```

### 2. Declarative Approach (Recommended)

Using `pod.yml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

```bash
# Apply YAML configuration
kubectl apply -f pod.yml

# Verify pod is running
kubectl get pods -l env=demo
```

### 3. Pod Management

```bash
# Get detailed pod information
kubectl describe pod nginx-pod

# View pod logs
kubectl logs nginx-pod

# Execute commands inside pod
kubectl exec -it nginx-pod -- /bin/bash

# Port forwarding to access pod locally
kubectl port-forward nginx-pod 8080:80
```

### 4. Pod Outputs & Status

```bash
# Check pod status
kubectl get pods -o wide

# Generate YAML without creating
kubectl run test-pod --image=nginx --dry-run=client -o yaml
```

## 🧹 Cleanup

```bash
# Delete pod
kubectl delete pod nginx-pod

# Delete using YAML file
kubectl delete -f pod.yml
```

## 💡 Key Commands

| Command | Purpose |
|---------|---------|
| `kubectl run` | Create pod imperatively |
| `kubectl apply -f` | Apply YAML configuration |
| `kubectl get pods` | List pods |
| `kubectl describe pod` | Get detailed pod info |
| `kubectl logs` | View pod logs |
| `kubectl exec` | Execute commands in pod |

## 🎯 Best Practices

- Use **declarative YAML** for production
- Always set **resource limits**
- Use meaningful **labels** for organization
- Monitor pod **health** and **logs**

---