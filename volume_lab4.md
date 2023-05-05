
## Persistent Volumes: 
## a. Create a PersistentVolume named cool-volume backed by a hostPath /tmp/my-cool-vol with size 100Mi in the default namespace, set it's storageClassName to “” 
```
vim pv.yaml
```
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: cool-volume
spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
  hostPath:
    path: /tmp/my-cool-vol
```
```
kubectl apply -f pv.yaml
```
## Create a PersistentVolumeClaim named my-claim that requests a volume of size 100Mi , make sure the storageClassName is “” so Kubernetes will match our Claim to the volume we created earlier. 
```
vim pvc.yaml
```
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
  storageClassName: ""
  ```
  ```
kubectl apply -f pvc.yaml
```
## c. Create a pod named pvc-user in namespace default that mounts your PVC my-claim under /mnt/share/my-pvc . Use the image nginx
```
vim pod.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: pvc-user
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: lab4
      mountPath: /mnt/share/my-pvc
  volumes:
  - name: lab4
    persistentVolumeClaim:
      claimName: my-claim
```
```
kubectl apply -f pod.yaml
```
