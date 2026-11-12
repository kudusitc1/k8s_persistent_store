### PERSISTENT VOLUME

### 0. Install and configure nfs share on the minikube host
``` bash
sudo apt install nfs-kernel-server -y
sudo mkdir -p /mnt/nfs_share
echo '/mnt/nfs_share 192.168.58.0/24(rw,sync,no_subtree_check,no_root_squash)' | sudo tee -a /etc/exports
sudo exportfs -a && sudo systemctl restart nfs-kernel-server
```

### 1. Create pyapp-ns namespace
```bash
kubectl create ns pyapp-ns
kubectl config set-context --current --namespace=pyapp-ns
kubectl config get-contexts
```

### 2. Create pv & pvc
```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```

### 3. Create deployment & nodeport svc
```bash
kubectl apply -f deploy.yaml
```

### 4. To access the app via wsl-minikube
```bash
minikube service pyapp-svc -n pyapp-ns -p kubesphere
```

### 5. To test - create 2 windows
#### window 1
```bash
kubectl get po -w
```
#### window 2
```bash
POD_NAME=$(kubectl get pods -l app=pyapp -o jsonpath='{.items[0].metadata.name}')
kubectl delete po $POD_NAME
```

### CLEAN UP

```bash
kubectl delete -f deploy.yaml
kubectl delete -f pvc.yaml
kubectl delete -f pv.yaml
kubectl config set-context --current --namespace=default
kubectl delete ns pyapp-ns
```