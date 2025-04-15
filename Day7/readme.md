# Creating an Nginx Pod with kubectl (Imperative Way)

## 1. Create a Cluster

```sh
kind create cluster --name kind
```
<!-- create cluster -->

## 2. Create an Nginx Pod

```sh
kubectl run nginx-pod --image=nginx:latest
```
<!-- create pod -->

## 3. List Pods

```sh
kubectl get pods
```
<!-- list pods -->

## 4. Get Pod Details

```sh
kubectl explain pod
```
<!-- pod details -->

## 5. Delete the Pod

```sh
kubectl delete pod nginx-pod
```
<!-- remove pod -->

## 6. Create Pod from YAML

```sh
kubectl create -f pod.yml
```
<!-- from yaml -->

## 7. Apply Changes from YAML

```sh
kubectl apply -f pod.yml
```

## 8. Describe Pod

```sh
kubectl describe pod nginx-pod
```
<!-- describe pod -->

## 9. Edit Pod

```sh
kubectl edit pod nginx-pod
```
<!-- edit pod -->

## 10. Execute Command in Pod

```sh
kubectl exec -it nginx-pod -- /bin/bash
```

## 11. Dry Run

```sh
kubectl run nginx-pod --image=nginx:latest --dry-run=client -o yaml
```
<!-- dry run -->
```

## 12. Dry Run in JSON

```sh
kubectl run nginx-pod --image=nginx:latest --dry-run=client -o json
```
<!-- dry run json -->

## 13. Get Pods Labels

```sh
kubectl get pods --show-labels
```
<!-- get labels -->

## 14. Get Pods with Wide Output

```sh
kubectl get pods -o wide
```
<!-- get wide output -->