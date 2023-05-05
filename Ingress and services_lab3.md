##  Ingress and services:
## a. Create 2 deployments in world namespace:
## i. asia with 2 replicas and image “husseingalal/africa:latest”
```
kubectl create namespace --dry-run=client -o yaml world >> ~/world.yaml
kubectl apply -f world.yaml
kubectl create deployment asia --image=husseingalal/africa:latest --replicas=2 --dry-run=client -n world -o yaml >> ~/deployment1.yaml
kubectl apply -f deployment1.yaml
```


## ii. europe with 2 replicas and image “husseingalal/europe:latest”

```
kubectl create deployment europe --image=husseingalal/europe:latest --replicas=2 --dry-run=client -n world -o yaml >> ~/deployment2.yaml
kubectl apply -f deployment2.yaml
```
## b. Using kubectl expose create ClusterIP Services for both Deployments for port 8888 and target port 80 . The Services should have the same name as the Deployments.
```
kubectl expose deployment europe --port=8888 --target-port=80 --type=ClusterIP --namespace=world
kubectl expose deployment asia --port=8888 --target-port=80 --type=ClusterIP --namespace=world
```

## c. Create a new Ingress resource called world for domain name world.universe.mine . The domain points to the K8s Node IP via /etc/hosts . The Ingress resource should have two routes pointing to the existing Services:
http://world.universe.mine/europe/
and
http://world.universe.mine/africa/
```
vim Ingress.yaml
```
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: world
  namespace: world
spec:
  rules:
  - host: world.universe.mine
    http:
      paths:
      - path: /europe
        pathType: Prefix
        backend:
          service:
            name: europe
            port:
              number: 8888
      - path: /africa
        pathType: Prefix
        backend:
          service:
            name: asia
            port:
              number: 8888
```
```
kubectl apply -f Ingress.yaml
vim /etc/hosts
192.168.124.174   world.universe.mine
curl http://world.universe.mine/europe/
```
