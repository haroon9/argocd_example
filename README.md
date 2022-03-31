# argocd_example

## ArgoCD installation
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Expose ArgoCD UI
Create a ``service.yaml`` file.

```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: argocd-server
  managedFields:
  name: argocd-server-nodeport
  namespace: argocd
spec:
  ports:
  - nodePort: 30443
    port: 8080
    protocol: TCP
  selector:
    app.kubernetes.io/name: argocd-server
  type: NodePort

```

Apply the ``service.yaml`` file.
```
kubectl apply -f service.yaml
```

## Find the default admin password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Login in to the UI
Using minikube
```
minikube service argocd-server -n argocd
```

## Install argocd-cli
To install argocd-cli run:
```
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```

Execute the following command to verify:
```
argocd version
```

## Login using argocd-cli
```
argocd login localhost:30443 --insecure
```