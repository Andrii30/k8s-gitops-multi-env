# k8s-gitops-multi-env

Promotes one Gitea Helm release through dev → staging → prod namespaces
using per-env values files and one ArgoCD ApplicationSet.

## Prerequisites
- A Kubernetes cluster with ArgoCD installed (shared setup, once per
  cluster).

## Deploy
```bash
kubectl apply -f argocd/gitea-appset.yaml
```

## Promote a change
Edit `values/values-staging.yaml` (e.g. bump a resource limit), commit,
push. ArgoCD's automated sync updates only the `gitea-staging`
Application — dev and prod are untouched until you edit their files too.

## Verify
```bash
argocd app list
for ns in gitea-dev gitea-staging gitea-prod; do kubectl get pods -n "$ns"; done
```
