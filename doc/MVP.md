# Minimum Viable Product
## ArgoCD APP Deployment Instructions

*Get the latest version of ArgoCD CLI:*
```
VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')
```
*Download ArgoCD CLI:*
```
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
```
*Make the ArgoCD CLI executable:*
```
chmod +x /usr/local/bin/argocd
```
*Login to the ArgoCD* 
```
argocd login localhost:8080 --username admin --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d) --insecure
```
*Create a new namespace for application*
```
kubectl create namespace asciiartify-mvp
```
*Create an application*
```
argocd app create asciiartify-mvp --repo https://github.com/den-vasyliev/go-demo-app.git --path helm --dest-namespace asciiartify-mvp --dest-server https://kubernetes.default.svc
```
*Check the status of the created application:*
```
argocd app get asciiartify-mvp
```
*Synchronize changes from the repository*
```
argocd app sync asciiartify-mvp
```
*Check the status in the argocd application and the hash of the last comit in the repository*
```
argocd app get asciiartify-mvp | tail -n +10 | head -n 2
git ls-remote https://github.com/anderson28/go-demo-app.git HEAD | awk '{print substr($1, 1, 7)}'
```  
*Make changes to the repository and make a new commit. Check the hash*
```
git ls-remote https://github.com/anderson28/go-demo-app.git HEAD | awk '{print substr($1, 1, 7)}'
```
*Change the application synchronization policy to automated*
```
argocd app set asciiartify-mvp --sync-policy automated
```
*Check that the changes are applied automatically*
```
argocd app get asciiartify-mvp | tail -n +10 | head -n 2
```
*Сreate a tunnel between local port 8088 and port 80*
```
kubectl port-forward -n asciiartify-mvp svc/ambassador 8088:80
```
*Test the application, convert img.png*

![Image](img.png)

```
curl -F 'image=@doc/img.png' localhost:8088/img/
```
## Democast
[![asciicast](https://asciinema.org/a/654910.svg)](https://asciinema.org/a/654910)
