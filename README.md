# ArgoCD Installation Steps
kubectl create ns argocd \
curl -sSfL https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml | kubectl apply -n argocd -f -

# ArgoCD CLI Installation Steps
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.0.3/argocd-linux-amd64 \
chmod +x /usr/local/bin/argocd 

# Access The Argo CD API 
*By default, the Argo CD API server is not exposed with an external IP. To access the API server, we change the argocd-server service type to LoadBalancer:* \
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}' 

# Login Using The CLI
*To get ArgoCD server IP address* \
kubectl get svc argocd-server -o yaml -n argocd | grep -i clusterIP 

*To retrieve the password of argocd-server* \
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d 

*Using the username admin and the password from above, login to Argo CD's IP* \
argocd login <ARGOCD_SERVER IP> --username admin --password  `<password>` 

  # Create An Application From A Git Repository
 argocd app create demo --repo <github_repo_url>  --path <path_to_pod.yaml_file> --dest-server <cluster_server_url> --dest-namespace <argo_namespace> --revision <branch_name> --sync-policy automated
 
# To sync repository changes with ArgoCD app
 argocd app sync demo

# To retrieve the status of application
 argocd app get demo
 
 # To switch the branch of the repo
  argocd app patch <application_name> --patch '{"spec": { "source": { "targetRevision": "<name_of_the branch>" } }}' --type merge
  
 # ResourceHooks added which creates workflows and templates in 3 stages
 Presync: Pod deletes the old copies of workflows template and cron workflows
 Sync: Creates workflow templates
 PostSync: Creates cron workflows
 
