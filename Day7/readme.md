# Day 7: Kubernetes Pod Management

This guide covers basic pod operations in Kubernetes using both imperative commands and declarative YAML files.

## Prerequisites

- Kind cluster installed
- kubectl configured

## 1. Cluster Setup

### Create a Kind Cluster
```bash
kind create cluster --name kind
```

## 2. Pod Creation

### Imperative Way (Command Line)
```bash
# Create an Nginx pod directly
kubectl run nginx-pod --image=nginx:latest
```

### Declarative Way (YAML File)
```bash
# Create pod from YAML file
kubectl create -f pod.yml

# Apply changes from YAML file (recommended)
kubectl apply -f pod.yml
```

## 3. Pod Information and Monitoring

### List Pods
```bash
# Basic pod listing
kubectl get pods

# Show pod labels
kubectl get pods --show-labels

# Wide output (more details)
kubectl get pods -o wide
```

### Pod Details
```bash
# Get detailed pod information
kubectl describe pod nginx-pod

# Get pod specification help
kubectl explain pod
```

## 4. Pod Interaction

### Edit Running Pod
```bash
kubectl edit pod nginx-pod
```

### Execute Commands in Pod
```bash
# Interactive bash session
kubectl exec -it nginx-pod -- /bin/bash
```

## 5. Dry Run and Testing

### YAML Output
```bash
# Generate YAML without creating the pod
kubectl run nginx-pod --image=nginx:latest --dry-run=client -o yaml
```

### JSON Output
```bash
# Generate JSON without creating the pod
kubectl run nginx-pod --image=nginx:latest --dry-run=client -o json
```

## 6. Cleanup

### Delete Pod
```bash
kubectl delete pod nginx-pod
```

## Common Commands Summary

| Command | Description |
|---------|-------------|
| `kubectl run` | Create pod imperatively |
| `kubectl create -f` | Create resources from file |
| `kubectl apply -f` | Apply configuration from file |
| `kubectl get pods` | List pods |
| `kubectl describe pod` | Get detailed pod info |
| `kubectl edit pod` | Edit running pod |
| `kubectl exec -it` | Execute commands in pod |
| `kubectl delete pod` | Remove pod |

## Best Practices

- Use `kubectl apply` instead of `kubectl create` for better version control
- Always use `--dry-run=client -o yaml` to preview changes
- Use descriptive names for your pods
- Clean up resources when no longer needed