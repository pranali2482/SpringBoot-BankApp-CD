```bash
🚀 EKS-Terraform

Setup VM
Install AWS CLI and configure it.
Insatall eksctl
Install Kubectl
Install Terraform
Terraform init
Terraform apply
aws eks update-kubeconfig --region ap-south-1 --name eks-cluster


🚀 Argo CD Installation on Kubernetes

Step 1: Create the Argo CD Namespace
kubectl create namespace argocd 
Step 2: Install Argo CD using the Official Manifest
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Step 3: Wait for All Argo CD Pods to Be Ready
kubectl get pods -n argocd
Step 4: Expose Argo CD Server(Change Argo CD Server Service Type to LoadBalancer)
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
Step 5: Get Initial Admin Password
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d


📦 Ways to create application in ArgoCd for your project

 1. Using the Argo CD Web UI

 2. Using Argo CD CLI (argocd app create)
  First install argocd CLI
  curl -sSL -o argocd https://github.com/argoproj/argo-cd/releases/download/v2.6.5/argocd-linux-amd64
  chmod +x argocd
  sudo mv argocd /usr/local/bin/
  argocd version

  Login to Argo CD:
  argocd login argocd-server-address --username admin --password <your-password> --insecure

  argocd app create my-app \
  --repo https://github.com/your-org/your-repo.git \
  --path k8s/app \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --directory-recurse \
  --sync-policy automated
   
3. Declarative Approach via YAML-You can define the application as a YAML file and apply it with kubectl
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/your-org/your-repo.git
    path: k8s/app
    targetRevision: HEAD
    directory:
      recurse: true
  destination:
    server: https://kubernetes.default.svc
    namespace: my-namespace
  syncPolicy:
    automated: {}
    syncOptions:
      - CreateNamespace=true



