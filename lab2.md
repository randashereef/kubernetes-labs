# Kubectl config:
# Create a k3s cluster with 1 server (controlplane) and 1 agent (worker)
```
#in server:
curl https://get.k3s.io | sh
cat /var/lib/rancher/k3s/server/node-token
#in worker:
curl  https://get.k3s.io | K3S_URL=https://192.168.124.174:6443 K3S_TOKEN=K107ff32a825416f9cc248fa3e0fb39111c6494e50343f3c175247e515acbe5e8b5::server:d74f7fea7f13f2cdb08754a7b8535f54 sh -
```
# In your cluster create a new namespace called iti-43.
```
kubectl create namespace --dry-run=client -o yaml iti-43 >> ~/namespace.yaml
kubectl apply -f namespace.yaml
```
#  Edit the kubectl config file to add a new context that uses the default user with the new namespace you just created
```
vim kubectl.yaml
```
```

piVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJkekNDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdGMyVnkKZG1WeUxXTmhRREUyT0RJek16azNOalV3SGhjTk1qTXdOREkwTVRJek5qQTFXaGNOTXpNd05ESXhNVEl6TmpBMQpXakFqTVNFd0h3WURWUVFEREJock0zTXRjMlZ5ZG1WeUxXTmhRREUyT0RJek16azNOalV3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFUd0RvVkVIUWNjSHlWeGQvcThpYVhwTzY3ZHB4M2QybjNFUzdIU3pqQisKeXJaZ0tKckFsemZZclF0cW9VWGRqOW9GWThsUENJbnhJTlkyNTlRUXJka0RvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVStUNmlUb3NiSEhMa1VQQjl0ZjdPCldRRmNTbUl3Q2dZSUtvWkl6ajBFQXdJRFNBQXdSUUloQVAxQnJ6a0R0NG83S0hOVCt5WWFjaTBOVHp5U2RmWnEKMEt0NE9aQUtRa095QWlBb2NOMU1LZ2ZURDVjSURWUDNYRVpzU0NQZ2wyWEpIbWlzaDBGMi9mTzdRZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K

    server: https://127.0.0.1:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
- context:
    cluster: default
    user: default
    namespace: iti-43
  name: lab
current-context: lab
kind: Config
preferences: {}
users:
- name: default
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJrRENDQVRlZ0F3SUJBZ0lJYk1CY3FpTjNRZzh3Q2dZSUtvWkl6ajBFQXdJd0l6RWhNQjhHQTFVRUF3d1kKYXpOekxXTnNhV1Z1ZEMxallVQXhOamd5TXpNNU56WTFNQjRYRFRJek1EUXlOREV5TXpZd05Wb1hEVEkwTURReQpNekV5TXpZd05Wb3dNREVYTUJVR0ExVUVDaE1PYzNsemRHVnRPbTFoYzNSbGNuTXhGVEFUQmdOVkJBTVRESE41CmMzUmxiVHBoWkcxcGJqQlpNQk1HQnlxR1NNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJIS1lWY3VSMnBNUnU4YVAKZ1dENGFJbUdPRHNicUNYWlRBVVpCYnk0N3YxeUpIVXBkVUZCVTNpL3k3cGJBL3pqMDZ2MjFYZkFEcTlQODNCVApUSGZNWFRpalNEQkdNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUtCZ2dyQmdFRkJRY0RBakFmCkJnTlZIU01FR0RBV2dCUXg0cVBNTmozd25JUTRIV3JTc3hvMHdDU1dxakFLQmdncWhrak9QUVFEQWdOSEFEQkUKQWlBbkRQUFEwU0dyb1ZJV2pRQVg1TnV5R1pwVVB1SUw1YXFBZmgxUXBBSUQrd0lnT3hoQ05GRUZmU20wVWZtSgpmbXE2MEZWa2M5OFhvYktkL3QvQm43V0k2Njg9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KLS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJlRENDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdFkyeHAKWlc1MExXTmhRREUyT0RJek16azNOalV3SGhjTk1qTXdOREkwTVRJek5qQTFXaGNOTXpNd05ESXhNVEl6TmpBMQpXakFqTVNFd0h3WURWUVFEREJock0zTXRZMnhwWlc1MExXTmhRREUyT0RJek16azNOalV3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFSbXkxaWt6TVZYdmYrTjUwd1RBMzFYcG1mUFBIbFFqVHU2c3V3TTdMMlcKQlBvQVdEZ1BTeG41dE4zVTRlM1dSdXRtNmdHS3dTNlRjL3hmVTlla3FkcnFvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVU1lS2p6RFk5OEp5RU9CMXEwck1hCk5NQWtscW93Q2dZSUtvWkl6ajBFQXdJRFNRQXdSZ0loQUxsSi9wT25yRExtR2wySTc0ZGZINnVhbzVXUmxOc2wKcmdCb2V2dFNhdnpSQWlFQTFCV2l6QnJ0M0tCcDBBSFY0dDM1WW91OFF1WlJmRzdTL1hteUpOclI1RHM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    client-key-data: LS0tLS1CRUdJTiBFQyBQUklWQVRFIEtFWS0tLS0tCk1IY0NBUUVFSU5saWYwbmxPZmJJbzRuTFBZRmpqbnRZbUtPWWxTQVk4b3ArSmZTYlBFeWpvQW9HQ0NxR1NNNDkKQXdFSG9VUURRZ0FFY3BoVnk1SGFreEc3eG8rQllQaG9pWVk0T3h1b0pkbE1CUmtGdkxqdS9YSWtkU2wxUVVGVAplTC9MdWxzRC9PUFRxL2JWZDhBT3IwL3pjRk5NZDh4ZE9BPT0KLS0tLS1FTkQgRUMgUFJJVkFURSBLRVktLS0tLQo=
```
```
kubectl get pods --kubeconfig=/root/kubectl.yaml
```

# Kubectl plugin: 
# Create a new kubectl plugin called “kubectl hostnames” that should display the hostnames of all your nodes on the cluster
```
vim  kubectl-hostnames
```
```
#!/bin/bash
kubectl get nodes | cut -d' ' -f 1 | tail -2
```
```
chmod +x  kubectl-hostnames
cp  kubectl-hostnames /usr/local/bin/
kubectl hostnames
```
# Creating deployments:
# Create a deployment YAML file that has 3 replicas of image "nginx:alpine"
```
kubectl create deployment --dry-run=client --image=nginx:alpine --replicas=3 nginx-deployment -o yaml >> ~/nginx.yaml
kubectl apply -f nginx.yaml
```


