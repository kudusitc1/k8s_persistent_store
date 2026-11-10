## Install and configure nfs share on the minikube host
``` bash
sudo apt install nfs-kernel-server -y
sudo mkdir -p /mnt/nfs_share
echo '/mnt/nfs_share 192.168.58.0/24(rw,sync,no_subtree_check,no_root_squash)' | sudo tee -a /etc/exports
sudo exportfs -a && sudo systemctl restart nfs-kernel-server
```

## Create pyapp-ns namespace
```bash
kubectl create ns pyapp-ns
kubectl get ns
kubectl config set-context --current --namespace=pyapp-ns
kubectl config get-contexts
```

## Create pv
```bash
kubectl apply -f pv.yaml
```