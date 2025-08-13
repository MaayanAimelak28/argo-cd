ArgoCD Assignment

1. Environment Setup and install ArgoCD

Prerequisites:
- Im using macOS and already installed Homebrew.

- Install a local Kubernetes cluster using Kind:
brew install kind
kind --version

- Install and configure kubectl:
brew install kubectl
kubectl version --client --output=yaml

- Install Docker Desktop on Mac: https://docs.docker.com/desktop/setup/install/mac-install/
I have Apple M1 Max so i need to install Docker Desktop for Mac (Apple silicon / arm64)
docker --version

Create kind cluster:
- kind create cluster --name argocd-cluster
- kind get clusters
Enter to the cluster:
- kubectl config use-context kind-argocd-cluster
See the nodes:
- kubectl get nodes -o wide --> we need to see is on ready

Install and expose ArgoCD UI on a local port (e.g., localhost:8080):
- kubectl create namespace argocd
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml ---> thats install:
- kubectl get namespaces
- kubectl get pods -n argocd -o wide - we need to see that all pods are running
- kubectl -n argocd port-forward svc/argocd-server 8080:443.  ---> get to the link: https://localhost:8080
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo —> we need to translate the secret to base64. copy the password
Login to ArgoCD UI with admin username and the copyied password.
change the password.

3. Prepare Git Repository:

link to github repo: https://github.com/MaayanAimelak28/argo-cd


