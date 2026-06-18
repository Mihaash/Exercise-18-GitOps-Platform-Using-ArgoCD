# Exercise 18: GitOps Platform Using ArgoCD

## Objective
Create a GitOps deployment platform for EKS using ArgoCD.

## Repository Structure
The project follows the required structure:
```
gitops/
├── base/        # Common Kubernetes manifests (Deployment, Service)
├── dev/         # Development environment overlay (1 replica)
├── qa/          # QA environment overlay (2 replicas)
└── prod/        # Production environment overlay (3 replicas)
```

## ArgoCD Configuration
The ArgoCD `Application` manifests are located in `argocd-apps/`. Each application is configured with:
- **Auto Sync**: Automatically syncs changes from the repository.
- **Self Heal**: Automatically corrects drift between Git and the cluster.
- **Pruning**: Automatically removes resources that are no longer in Git.

### Example Sync Policy:
```yaml
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

## How it works
1. **Git Commit**: Developer pushes changes to the `gitops/` directory.
2. **ArgoCD**: ArgoCD detects the change in the repository.
3. **EKS**: ArgoCD applies the Kustomized manifests to the EKS cluster in the respective namespace (`dev`, `qa`, or `prod`).

## Usage
To deploy these applications, apply the manifests in `argocd-apps/` to your ArgoCD cluster:
```bash
kubectl apply -f argocd-apps/
```
*Note: Ensure you update the `repoURL` in the application manifests to point to your actual Git repository.*
