## STORAGE CLASS

### 1. create sclass-ns namespace
```bash
kubectl create ns sclass-ns
kubectl get ns
kubectl config set-context --current --namespace=sclass-ns
kubectl config get-contexts
```