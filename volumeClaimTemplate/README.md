### VOLUME CLAIM TEMPLATE

### 1. Create vtc-ns namespace
```bash
kubectl create ns vts-ns
kubectl config set-context --current --namespace=vts-ns
kubectl config get-contexts
```

### 2. Create sts & headless svc
```bash
kubectl apply -f headless.yaml
kubectl apply -f sts.yaml
```

### 3. Test the attached volumes
```bash
# Check mount point in each pod
for i in 0 1; do echo "=== mongodb-$i ==="; kubectl exec "mongodb-$i" -- df -h /data/db; done
```
```bash
# Write data
kubectl exec mongodb-0 -- sh -c 'echo "I survived pod deletion!" > /data/db/persist.txt'
```
```bash
# Delete the pod (StatefulSet will recreate it)
kubectl delete pod mongodb-0
```
```bash
# Verify data survived
kubectl exec mongodb-0 -- cat /data/db/persist.txt
# I survived pod deletion!
```

### CLEAN UP
```bash
kubectl delete -f sts.yaml
kubectl delete -f headless.yaml
kubectl config set-context --current --namespace=default
kubectl delete ns vts-ns
```

