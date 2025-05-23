# Day 9: Kubernetes Services

This guide provides a comprehensive overview of Kubernetes Services, including ClusterIP, NodePort, LoadBalancer, Ingress, and ExternalName services.

## Table of Contents
- [What is a Service?](#what-is-a-service)
- [Why Use Services?](#why-use-services)
- [Service Types](#service-types)
- [Hands-on Examples](#hands-on-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## What is a Service?

A **Service** in Kubernetes is an abstraction that defines a logical set of Pods and a policy for accessing them. Services enable stable network communication between different components of your application, providing a consistent endpoint even as Pods are created, destroyed, or rescheduled.

## Why Use Services?

- **🔄 Decoupling**: Services decouple application components, allowing them to evolve independently
- **⚖️ Load Balancing**: Automatically distribute traffic across multiple Pods for high availability
- **🔍 Service Discovery**: Provide stable endpoints for accessing Pods through DNS names
- **🛡️ Network Policies**: Control and secure communication between different application components
- **🎯 Abstraction**: Hide the complexity of Pod IPs and lifecycle management

## Service Types

### 1. ClusterIP (Default)
Exposes the service on a cluster-internal IP, accessible only from within the cluster.

**Use Cases:**
- Internal microservice communication
- Database connections
- Internal APIs

### 2. NodePort
Exposes the service on each Node's IP at a static port (30000-32767 range).

**Traffic Flow:**
```
External Client → NodeIP:NodePort → Service:Port → Pod:TargetPort
```

**Use Cases:**
- Development and testing
- Simple external access without load balancer

### 3. LoadBalancer
Exposes the service externally using a cloud provider's load balancer.

**Use Cases:**
- Production external access
- Internet-facing applications
- Automatic cloud integration

### 4. ExternalName
Maps a service to an external DNS name, allowing internal services to access external resources.

**Use Cases:**
- Database hosted outside cluster
- External APIs
- Migration scenarios

### 5. Ingress
Manages external HTTP/HTTPS access to services with advanced routing capabilities.

**Use Cases:**
- HTTP/HTTPS routing
- SSL termination
- Virtual hosting
- Path-based routing

## Hands-on Examples

### Setting Up the Environment

1. **Create Kind Cluster** (for local testing):
```bash
kind create cluster --config kind.yml --name cka
```

2. **Deploy Sample Application**:
```bash
kubectl create deployment nginx --image=nginx --replicas=3
kubectl label deployment nginx env=demo
```

### ClusterIP Service Example

```bash
# Apply ClusterIP service
kubectl apply -f clusterip.yml

# Test internal connectivity
kubectl run test-pod --image=busybox --rm -it -- /bin/sh
# Inside the pod:
wget -qO- http://cluster-svc
```

### NodePort Service Example

```bash
# Apply NodePort service
kubectl apply -f nodeport.yml

# Get service details
kubectl get svc nodeport

# Access from outside cluster
curl http://localhost:30000
```

### LoadBalancer Service Example

```bash
# Apply LoadBalancer service
kubectl apply -f lb.yml

# Check external IP (may take a few minutes)
kubectl get svc lb-svc --watch

# Access via external IP
curl http://<EXTERNAL-IP>
```

### Useful Commands

```bash
# List all services
kubectl get svc

# Describe service details
kubectl describe svc <service-name>

# Get service endpoints
kubectl get endpoints

# Port forward for testing
kubectl port-forward svc/<service-name> 8080:80

# Delete services
kubectl delete svc <service-name>
```

## Best Practices

### Service Configuration
- Use meaningful service names
- Apply consistent labeling strategy
- Document service dependencies
- Use health checks and readiness probes

### Security
- Limit service exposure (prefer ClusterIP when possible)
- Use Network Policies to control traffic
- Implement proper authentication/authorization
- Regular security audits

### Performance
- Monitor service latency and throughput
- Use appropriate service type for use case
- Consider service mesh for complex scenarios
- Implement proper load balancing strategies

## Troubleshooting

### Common Issues

1. **Service Not Accessible**
```bash
# Check service configuration
kubectl get svc <service-name> -o yaml

# Verify endpoints
kubectl get endpoints <service-name>

# Check pod labels match service selector
kubectl get pods --show-labels
```

2. **DNS Resolution Issues**
```bash
# Test DNS from within cluster
kubectl run test-dns --image=busybox --rm -it -- nslookup <service-name>
```

3. **LoadBalancer Pending**
```bash
# Check cloud provider integration
kubectl describe svc <service-name>

# Verify cloud provider configuration
kubectl get nodes -o wide
```

### Debug Commands

```bash
# Check service logs
kubectl logs -l app=<app-name>

# Test connectivity
kubectl run debug --image=nicolaka/netshoot --rm -it -- /bin/bash

# Monitor service traffic
kubectl top pods
```

## File Structure

```
Day9/
├── readme.md          # This comprehensive guide
├── clusterip.yml      # ClusterIP service example
├── nodeport.yml       # NodePort service example  
├── lb.yml            # LoadBalancer service example
└── kind.yml          # Kind cluster configuration
```

## Next Steps

- Explore Ingress controllers
- Learn about service mesh (Istio, Linkerd)
- Practice with different service scenarios
- Study network policies
- Implement monitoring and observability

---

**Note**: Remember to clean up resources after testing to avoid unnecessary costs in cloud environments.