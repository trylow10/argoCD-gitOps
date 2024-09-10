### NGINX Deployment Workflow with Argo CD

This project demonstrates a **GitOps workflow** for deploying an NGINX web server using **Argo CD** on Kubernetes. Argo CD automates continuous delivery by syncing the cluster state with the desired state defined in a Git repository.

### Workflow Overview

1. **Git as Source of Truth**: Store Kubernetes manifests (like `nginx-deployment.yaml`) in Git. Any updates are pushed to this repository.
2. **Argo CD Syncing**: Argo CD monitors the Git repo and automatically syncs changes to the Kubernetes cluster.
3. **Automated Deployment**: Kubernetes resources (NGINX Deployment and Service) are automatically applied by Argo CD.

### Steps

1. **Create a Git Repo**: Store the `nginx-deployment.yaml` in your repo.
2. **Install Argo CD**: Install Argo CD on your Kubernetes cluster.
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
3. **Create Argo CD Application**: Link Argo CD to your Git repo.
   ```bash
   argocd app create nginx-app --repo https://github.com/<your-repo>/my-nginx-deployment --path / --dest-server https://kubernetes.default.svc --dest-namespace default
   ```
4. **Sync and Deploy**: Sync the application to deploy NGINX.
   ```bash
   argocd app sync nginx-app
   ```
5. **Monitor Changes**: Push any updates to the Git repo; Argo CD will automatically apply changes.
6. **Access NGINX**: Find the NodePort and access NGINX at `<NodeIP>:<NodePort>`.
   ```bash
   kubectl get svc nginx
   ```

### Rollback & Version Control

Easily rollback by pushing a previous commit to Git. Argo CD will automatically revert to the previous version.

### Cleanup

- Delete the Argo CD application:
  ```bash
  argocd app delete nginx-app
  ```
- Uninstall Argo CD:
  ```bash
  kubectl delete namespace argocd
  ```

### Benefits

- **Automated Deployments** using GitOps practices.
- **Version Control** of the Kubernetes state via Git.
- **Visibility** into app state through the Argo CD UI.
- **Rollback Support** by reverting Git commits.

This workflow simplifies Kubernetes application deployment, ensuring consistency, automation, and traceability.
