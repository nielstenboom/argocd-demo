# ArgoCD demo

To try this yourself:

Setup kind (https://kind.sigs.k8s.io/)
```bash
brew install kind
# make sure you gave docker enough resources in preferences before doing this step
kind create cluster
```

Then install argoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# wait for a bit until all pods run
# get password & port forward ui
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Now go to http://localhost:8080 and login with: admin | {output of kubectl get secret command above}. If all is well, you should be greeted by the ArgoCD UI.

## Deploy single custom chart
```
kubectl apply -f apps/guestbook-helm.yaml
```

## Deploy single opensource chart
```
kubectl apply -f apps/prometheus.yaml
```
or
```
kubectl apply -f apps/ingress-nginx.yaml
```

## Deploy app-of-apps (group of applications)
```
kubectl apply -f app-of-apps/platform-apps.yaml
```


# URLs
- https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/
- https://argoproj.github.io/argo-cd/user-guide/best_practices/
