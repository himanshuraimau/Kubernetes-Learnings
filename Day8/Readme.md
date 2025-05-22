# Day 8: Kubernetes Controllers (RC, ReplicaSet, Deployment)

This guide covers ReplicationController, ReplicaSet, and Deployment operations in Kubernetes.

## Prerequisites

- Kind cluster installed
- kubectl configured

## 1. ReplicationController (Legacy)

### Apply ReplicationController
```bash
kubectl apply -f rs.yml  # Note: rs.yml contains ReplicationController config
```
Output:
```
replicationcontroller/nginx-rc created
```

### Monitor ReplicationController
```bash
# List ReplicationControllers
kubectl get rc

# Describe ReplicationController
kubectl describe rc nginx-rc

# List pods created by RC
kubectl get po -l env=demo
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

## 3. Deployment (Most Recommended)

### Apply Deployment
```bash
kubectl apply -f rc-dep.yml
```

### Deployment Operations
```bash
# List Deployments
kubectl get deploy

# Describe Deployment
kubectl describe deploy nginx-deploy

# Scale Deployment
kubectl scale deploy/nginx-deploy --replicas=5

# Update image
kubectl set image deploy/nginx-deploy nginx=nginx:1.7.9

# Rollout status
kubectl rollout status deploy/nginx-deploy

# Rollback deployment
kubectl rollout undo deploy/nginx-deploy
```

## 4. Common Pod Operations

### List Pods
```bash
kubectl get po
```
Output:
```
NAME             READY   STATUS              RESTARTS   AGE
nginx-rc-db4wb   0/1     ContainerCreating   0          12s
nginx-rc-fhvpk   0/1     ContainerCreating   0          12s
nginx-rc-zjfz6   0/1     ContainerCreating   0          12s
```

### Describe Pod
```bash
kubectl describe po <pod-name>
```

### Delete Pod (Auto-recreated by controller)
```bash
kubectl delete po <pod-name>
```

## 5. Useful Commands

### Dry Run (Generate YAML)
```bash
# ReplicationController
kubectl run nginx-rc --image=nginx:latest --dry-run=client -o yaml

# Deployment
kubectl create deployment nginx-deploy --image=nginx --dry-run=client -o yaml
```

### Cleanup
```bash
# Delete ReplicationController
kubectl delete rc nginx-rc

# Delete ReplicaSet
kubectl delete rs nginx-rs

# Delete Deployment
kubectl delete deploy nginx-deploy
```

## Key Differences

- **ReplicationController**: Legacy, basic pod replication
- **ReplicaSet**: Improved selector capabilities, used by Deployments
- **Deployment**: Provides rolling updates, rollbacks, and manages ReplicaSets

## Files Used
- `rs.yml` - ReplicationController configuration
- `rc.yml` - ReplicaSet configuration  
- `rc-dep.yml` - Deployment configuration

