# Kubertes GitOps Instructions

ArgoCD Setup
```bash
# create namespace to argo
kubectl create namespace argocd

# add argo repo to charts
helm repo add argo https://argoproj.github.io/argo-helm

# install argo
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# to expose the argocd project
kubectl port-forward service/argocd-server -n argocd 8080:443

# get secret
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

ArgoCD CLI
```bash
# Download the binary
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

# permission
chmod +x /usr/local/bin/argocd

# check version
argocd version

# login 
argocd localhost:8080 --username admin --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo) --port-forward-namespace argocd
```

ArgoCD App Example
```bash
argocd app create guestbook \
--repo https://gitlab.com/l2548/gitops-app-example.git \
--path guestbook \
--dest-server https://kubernetes.default.svc \
--dest-namespace guestbook
```