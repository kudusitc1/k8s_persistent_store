## STORAGE CLASS

### 1. create sclass-ns namespace
```bash
kubectl create ns sclass-ns
kubectl get ns
kubectl config set-context --current --namespace=sclass-ns
kubectl config get-contexts
```
### 2. helm install csi driver package
```bash
# Add the Helm repository
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts

# Update the repo index
helm repo update

helm pull csi-driver-nfs/csi-driver-nfs --untar

# Install the chart into kube-system
helm install csi-driver-nfs -f sc-values.yml csi-driver-nfs/ --namespace kube-system 
```

### 3. create pvc
```bash
kubectl apply -f pvc.yaml
```

### 4.check that a pvc & pv have been created
```bash
kubectl get pvc,pv
```

### 5. deploy pods & svc
```bash
kubectl apply -f deploy.yaml
```

## CLEAN UP
```bash
kubectl delete -f deploy.yaml
kubectl delete -f pvc.yaml
helm uninstall csi-driver-nfs -n kube-system
kubectl config set-context --current --namespace=default
kubectl delete ns sclass-ns
kubectl get ns
```