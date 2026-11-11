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

