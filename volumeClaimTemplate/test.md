```bash
# Check mount point in each pod
for i in 0 1; do echo "=== mongodb-$i ==="; kubectl exec "mongodb-$i" -- df -h /data/db; done

# Write data
kubectl exec mongodb-0 -- sh -c 'echo "survive me" > /data/db/persist.txt'

# Delete the pod (StatefulSet will recreate it)
kubectl delete pod mongodb-0

# Wait for it to come back
kubectl wait --for=condition=Ready pod/mongodb-0 --timeout=120s

# Verify data survived
kubectl exec mongodb-0 -- cat /data/db/persist.txt
# survive me   
```

