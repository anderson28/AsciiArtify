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
argocd login localhost:8080
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
[![asciicast](https://asciinema.org/a/654087.svg)](https://asciinema.org/a/654087)
