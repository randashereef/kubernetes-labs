## 1. RBAC
## a. User smoke should be allowed to create and delete Pods, Deployments and StatefulSets in Namespace applications.
```
vim RABC.yaml
```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: applications
  name: lab5
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["delete", "create"]

- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets"]
  verbs: ["delete", "create"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: lab
  namespace: applications
subjects:
- kind: User
  name: smoke
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: lab5
  apiGroup: rbac.authorization.k8s.io

```
```
kubectl apply -f RABC.yaml
```

## b. User smoke should have view permissions (like the permissions of the default ClusterRole named view ) in all Namespaces but not in kube-system .
```
vim clusterRole.yaml
```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sview
  namespace: default
subjects:
- kind: User
  name: smoke
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sview
  namespace: kube-public
subjects:
- kind: User
  name: smoke
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io

---


apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sview
  namespace: kube-node-lease
subjects:
- kind: User
  name: smoke
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sview
  namespace: applications
subjects:
- kind: User
  name: smoke
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
```
```
kubectl apply -f clusterRole.yaml
kubectl auth can-i get pods -n kube-system --as smoke
kubectl auth can-i get pods -n default --as smoke
```

## c. Verify everything using kubectl auth can-i .
```
kubectl auth can-i create pods -n applications --as smoke
kubectl auth can-i delete deployments -n applications --as smoke
kubectl auth can-i get deployments -n kube-system --as smoke
kubectl auth can-i get pods -n kube-system --as smoke
kubectl auth can-i get pods -n default --as smoke
```
## 2. HELM
##  Using helm install apache chart on your system
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install --generate-name  bitnami/apache
```
