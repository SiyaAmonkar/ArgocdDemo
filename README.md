# ArgocdDemo

# ArgoCD Installation Steps
kubectl create ns argocd
curl -sSfL https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml | kubectl apply -n argocd -f -

# ArgoCD CLI Installation Steps
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.0.3/argocd-linux-amd64
chmod +x /usr/local/bin/argocd

# Access The Argo CD API 
*By default, the Argo CD API server is not exposed with an external IP. To access the API server, we change the argocd-server service type to LoadBalancer:*
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# Login Using The CLI
*To get ArgoCD server IP address*
kubectl get svc argocd-server -o yaml -n argocd | grep -i clusterIP
*To retrieve the password of argocd-server*
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
*Using the username admin and the password from above, login to Argo CD's IP*
argocd login <ARGOCD_SERVER IP> --username admin --password <password>
 

