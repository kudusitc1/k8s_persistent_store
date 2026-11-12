### STORAGE CLASS

### 0. Helm install csi driver package
```bash
# Add the Helm repository
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts

# Update the repo index
helm repo update

helm pull csi-driver-nfs/csi-driver-nfs --untar

# Install the chart into kube-system
helm install csi-driver-nfs -f sc-values.yml csi-driver-nfs/ --namespace kube-system 
```

### 1. Create sclass-ns namespace
```bash
kubectl create ns sclass-ns
kubectl config set-context --current --namespace=sclass-ns
kubectl config get-contexts
```

### 2. Create pvc
```bash
kubectl apply -f pvc.yaml
```

### 3. Check that a pvc & pv have been created
```bash
kubectl get pvc,pv
```

### 4. Deploy pods & svc
```bash
kubectl apply -f deploy.yaml
```

### 5. To access the app via wsl-minikube
```bash
minikube service pyapp-svc -n sclass-ns -p kubesphere
```

### 6. To test - create 2 windows
#### window 1
```bash
kubectl get po -w
```
#### window 2
```bash
POD_NAME=$(kubectl get pods -l app=pyapp -o jsonpath='{.items[0].metadata.name}')
kubectl delete $POD_NAME
```

### CLEAN UP
```bash
kubectl delete -f deploy.yaml
kubectl delete -f pvc.yaml
helm uninstall csi-driver-nfs -n kube-system
kubectl config set-context --current --namespace=default
kubectl delete ns sclass-ns
```