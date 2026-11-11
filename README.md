## install and configure nfs share on the minikube host
``` bash
sudo apt install nfs-kernel-server -y
sudo mkdir -p /mnt/nfs_share
echo '/mnt/nfs_share 192.168.58.0/24(rw,sync,no_subtree_check,no_root_squash)' | sudo tee -a /etc/exports
sudo exportfs -a && sudo systemctl restart nfs-kernel-server
```

## create pyapp-ns namespace
```bash
kubectl create ns pyapp-ns
kubectl get ns
kubectl config set-context --current --namespace=pyapp-ns
kubectl config get-contexts
```

## create pv
```bash
kubectl apply -f pv.yaml
```

## create deployment & nodeport svc
```bash
kubectl apply -f deploy.yaml
```

## to access the app via wsl-minikube
```bash
minikube service pyapp-svc -n pyapp-ns -p kubesphere
```
## to delete pods - create 2 windows
### window 1
```bash
kubectl get po -w
```
### window 2
```bash
POD_NAME=$(kubectl get pods -l app=pyapp -o jsonpath='{.items[0].metadata.name}')
kubectl delete $POD_NAME
```

## CLEAN UP

```bash
kubectl delete -f deploy.yaml
kubectl delete -f pvc.yaml
kubectl delete -f pv.yaml
kubectl config set-context --current --namespace=default
kubectl delete ns pyapp-ns
```