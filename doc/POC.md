# Proof of Concept
## ArgoCD Deployment Instructions 

*Create the Kubernetes cluster:*
```
k3d cluster create argo
```
*Create a namespace for ArgoCD:*
```
kubectl create namespace argocd
```
*Apply the ArgoCD installation manifest:*
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
*Check all ArgoCD resources:*
```
kubectl get all -n argocd
```
*Get the initial admin password:*
```
kubectl -n argocd get secrets argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
*Port forward ArgoCD to localhost:*
```
kubectl -n argocd port-forward svc/argocd-server 8080:443
```

Access the web UI by `http://localhost:8080` in your browser and logging in with the username 'admin' and the password.