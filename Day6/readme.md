# Day 6: KIND Cluster Setup

## 🎯 What is KIND?

**KIND** (Kubernetes IN Docker) runs Kubernetes clusters using Docker containers as nodes. Perfect for:
- Local development
- CI/CD testing  
- Learning Kubernetes

## 📋 Task Completion

### 1. Single Node Cluster

```bash
# Install KIND (if not already installed)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create single node cluster with K8s v1.29
kind create cluster --image kindest/node:v1.29.0 --name single-node

# Verify cluster
kubectl get nodes
```

### 2. Delete Single Node Cluster

```bash
kind delete cluster --name single-node
```

### 3. Multi-Node Cluster Setup

Create cluster config:
```yaml
# cluster-config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: cka-cluster2
nodes:
- role: control-plane
- role: worker
- role: worker  
- role: worker
```

```bash
# Create multi-node cluster with K8s v1.30
kind create cluster --config cluster-config.yaml --image kindest/node:v1.30.0

# Set context to new cluster
kubectl cluster-info --context kind-cka-cluster2
```

### 4. Verification Commands

```bash
# Check all nodes are ready
kubectl get nodes -o wide

# Verify nodes as Docker containers
docker ps

# Get cluster info
kubectl cluster-info
```

## ✅ Expected Output

```bash
$ kubectl get nodes
NAME                        STATUS   ROLES           AGE   VERSION
cka-cluster2-control-plane   Ready    control-plane   2m    v1.30.0
cka-cluster2-worker          Ready    <none>          90s   v1.30.0
cka-cluster2-worker2         Ready    <none>          90s   v1.30.0
cka-cluster2-worker3         Ready    <none>          90s   v1.30.0

$ docker ps
CONTAINER ID   IMAGE                  NAMES
abc123def456   kindest/node:v1.30.0   cka-cluster2-control-plane
def456ghi789   kindest/node:v1.30.0   cka-cluster2-worker
ghi789jkl012   kindest/node:v1.30.0   cka-cluster2-worker2
jkl012mno345   kindest/node:v1.30.0   cka-cluster2-worker3
```

## 🔗 Resources

- [KIND Documentation](https://kind.sigs.k8s.io/)
- [Kubectl Installation Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

---