# Day 10: Kubernetes Namespaces

##  Learning Objectives
By the end of this session, you will be able to:
- Understand what Kubernetes namespaces are and why they're important
- Create, manage, and delete namespaces
- Deploy applications in specific namespaces
- Access services across namespaces using FQDN
- Implement namespace-based resource isolation

##  What are Kubernetes Namespaces?

Namespaces in Kubernetes provide a way to create **virtual clusters** within a single physical cluster. They help in:
- **Resource Organization**: Group related resources together
- **Multi-tenancy**: Isolate resources between teams or environments
- **Access Control**: Apply different RBAC policies per namespace
- **Resource Quotas**: Limit resource usage per namespace

## Default Namespaces

Kubernetes comes with several built-in namespaces:
- **default**: Resources created without specifying a namespace
- **kube-system**: System components and add-ons
- **kube-public**: Publicly accessible resources
- **kube-node-lease**: Node heartbeat information

##  Hands-on Practice

### 1. Creating and Managing Namespaces

#### Create a New Namespace
```bash
# Method 1: Using kubectl create
kubectl create namespace demo

# Method 2: Using short form
kubectl create ns demo

# Method 3: Using YAML file
kubectl apply -f - <<EOF
apiVersion: v1
kind: Namespace
metadata:
  name: demo
  labels:
    environment: development
EOF
```

#### List All Namespaces
```bash
# List all namespaces
kubectl get namespaces

# Get detailed information
kubectl get ns -o wide

# Describe a specific namespace
kubectl describe ns demo
```

### 2. Working with Resources in Namespaces

#### Deploy Applications in Specific Namespaces
```bash
# Create deployment in default namespace
kubectl create deployment nginx-default --image=nginx

# Create deployment in demo namespace
kubectl create deployment nginx-demo --image=nginx --namespace=demo

# Alternative syntax using -n flag
kubectl create deployment nginx-demo --image=nginx -n demo
```

#### Listing Resources by Namespace
```bash
# List deployments in specific namespace
kubectl get deployments -n demo

# List all pods in demo namespace
kubectl get pods -n demo

# Get detailed pod information
kubectl get pods -n demo -o wide

# List resources across all namespaces
kubectl get pods --all-namespaces
# or
kubectl get pods -A
```

### 3. Pod Operations Across Namespaces

#### Execute Commands in Pods
```bash
# Get pod name first
kubectl get pods -n demo

# Execute shell in a pod (replace pod name with actual)
kubectl exec -it <pod-name> -n demo -- /bin/bash

# Example with actual pod name
kubectl exec -it nginx-demo-87cd4cbb7-gt8p6 -n demo -- sh
```

#### Scaling Deployments
```bash
# Scale deployment in demo namespace
kubectl scale deployment nginx-demo --replicas=3 -n demo

# Check scaling status
kubectl get deployments -n demo
```

### 4. Service Discovery and FQDN

#### Creating Services
```bash
# Expose deployment as ClusterIP service
kubectl expose deployment nginx-demo --port=80 --type=ClusterIP -n demo

# Create NodePort service
kubectl expose deployment nginx-demo --port=80 --type=NodePort -n demo --name=nginx-nodeport

# List services in namespace
kubectl get services -n demo
```

#### Understanding FQDN (Fully Qualified Domain Names)

Services in Kubernetes can be accessed using FQDN format:
```
<service-name>.<namespace>.svc.cluster.local
```

**Examples:**
- `nginx-demo.demo.svc.cluster.local` - Full FQDN
- `nginx-demo.demo.svc` - Short form
- `nginx-demo.demo` - Shorter form
- `nginx-demo` - Only works within the same namespace

#### Testing Service Connectivity
```bash
# Create a test pod in demo namespace
kubectl run test-pod --image=busybox --rm -it -n demo -- /bin/sh

# Inside the test pod, test connectivity:
# Same namespace (short name works)
wget -qO- http://nginx-demo

# Cross-namespace access (requires FQDN)
wget -qO- http://nginx-demo.demo.svc.cluster.local

# Test from default namespace
kubectl run test-pod --image=busybox --rm -it -- /bin/sh
# This requires full FQDN
wget -qO- http://nginx-demo.demo.svc.cluster.local
```

### 5. Advanced FQDN Examples

```bash
# Create test pod for connectivity testing
kubectl run debug-pod --image=busybox --rm -it -n demo -- /bin/sh

# Basic service access
wget -qO- http://nginx-demo.demo.svc.cluster.local

# With specific port
wget -qO- http://nginx-demo.demo.svc.cluster.local:80

# Testing different paths (if application supports)
wget -qO- http://nginx-demo.demo.svc.cluster.local:80/

# Using curl for more detailed testing
curl -v http://nginx-demo.demo.svc.cluster.local
```

## 🧹 Cleanup Operations

### Delete Resources
```bash
# Delete specific deployment
kubectl delete deployment nginx-demo -n demo

# Delete service
kubectl delete service nginx-demo -n demo

# Delete all resources in namespace
kubectl delete all --all -n demo

# Delete the namespace (this deletes everything inside)
kubectl delete namespace demo
```

## 📋 Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Namespace** | Virtual cluster for resource organization and isolation |
| **Default Namespace** | Used when no namespace is specified |
| **Resource Isolation** | Separate resources between teams/environments |
| **FQDN** | `<service>.<namespace>.svc.cluster.local` format |
| **Cross-namespace Access** | Requires FQDN for service discovery |

## 🎯 Best Practices

### 1. Namespace Naming Convention
```bash
# Use descriptive names
kubectl create ns dev-frontend
kubectl create ns prod-backend
kubectl create ns staging-database
```

### 2. Always Specify Namespace
```bash
# Good practice - explicit namespace
kubectl get pods -n production

# Avoid relying on default namespace for production
kubectl get pods  # This uses default namespace
```

### 3. Environment Separation
```bash
# Create environment-specific namespaces
kubectl create ns development
kubectl create ns staging  
kubectl create ns production

# Label namespaces for better organization
kubectl label namespace development environment=dev
kubectl label namespace staging environment=stage
kubectl label namespace production environment=prod
```

### 4. Resource Organization
```bash
# Group related resources
kubectl create ns ecommerce-frontend
kubectl create ns ecommerce-backend
kubectl create ns ecommerce-database
```

## 🔧 Troubleshooting

### Common Issues and Solutions

1. **Service Not Accessible Across Namespaces**
   ```bash
   # Problem: Cannot access service from different namespace
   # Solution: Use FQDN
   wget http://service-name.target-namespace.svc.cluster.local
   ```

2. **Namespace Stuck in Terminating State**
   ```bash
   # Check what's preventing deletion
   kubectl get all -n stuck-namespace
   
   # Force delete if needed (use with caution)
   kubectl delete ns stuck-namespace --force --grace-period=0
   ```

3. **Resources Not Found**
   ```bash
   # Always check which namespace you're in
   kubectl config view --minify | grep namespace
   
   # List resources across all namespaces
   kubectl get pods -A | grep your-app
   ```

## 🎓 Practice Exercises

1. Create three namespaces: `frontend`, `backend`, and `database`
2. Deploy nginx in each namespace with different replica counts
3. Create services in each namespace
4. Test cross-namespace connectivity using FQDN
5. Set up a multi-tier application across different namespaces

## 📚 Additional Resources

- [Kubernetes Namespaces Documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
- [Namespace-based Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)
- [Network Policies with Namespaces](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

---
