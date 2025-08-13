ArgoCD Assignment

1. Environment Setup and install Argo CD:

Prerequisites:
I’m using macOS and already installed Homebrew.

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
Set context:
- kubectl config use-context kind-argocd-cluster
View nodes:
- kubectl get nodes -o wide --> we need to see nodes are ready

Install and expose ArgoCD UI on a local port (e.g., localhost:8080):
- kubectl create namespace argocd
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml ---> apply the official Argo CD install manifest to "argocd"
- kubectl get namespaces
- kubectl get pods -n argocd -o wide - we need to see that all pods are running
- kubectl -n argocd port-forward svc/argocd-server 8080:443.  ---> get to the link: https://localhost:8080
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo —> translate the secret to base64. copy the password
Login to ArgoCD UI with admin username and the copied password.
change the password.

2. Prepare Git Repository and Deploy Application with ArgoCD:

Link to the Git repository: https://github.com/MaayanAimelak28/argo-cd ---> added all the files including app.yaml
Directory structure shown via tree:
- brew install tree
- tree -a -I .git
- kubectl get application -n argocd
- mkdir tmp
- cd tmp
- kubectl apply -n argocd -f app.yaml
- kubectl get application -n argocd --> SYNC STATUS: OutOfSync
I synced through the Argo CD GUI:
- kubectl get application -n argocd --> SYNC STATUS: Synced

3. Demonstrate GitOps:

Changed the replicas to 2 and then check:
- kubectl get application -n argocd ---> SYNC STATUS: OutOfSync
I synced through the Argo CD GUI:
- kubectl get application -n argocd --> SYNC STATUS: Synced
- kubectl get pods -n web --> expect to see 2 pods

