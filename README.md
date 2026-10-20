# kubesentinel-gitops

Kubernetes manifests for the [KubeSentinel](https://github.com/glichy-gem/kubesentinel) demo app. In Phase 2 Argo CD watches this repository, so every deploy is a Git commit that the agent can later query.

## Layout

```
namespace.yaml          the demo namespace
kustomization.yaml      root: namespace + all apps
apps/orders-api/        Deployment (2 replicas, probes, limits) and NodePort Service
apps/redis/             Deployment (in-memory only) and ClusterIP Service
argocd/                 Argo CD Application manifests, one per service
```

The image tag for `orders-api` is the `images` block in `apps/orders-api/kustomization.yaml`.
CI in the app repo rewrites it after each successful build; those commits are the deploy history.

## Apply manually (Phase 1)

```bash
kubectl apply -k .
kubectl -n demo get pods
```

Note: the image lives in GHCR and is tagged with a Git SHA written by CI. Until the first CI run the tag is `bootstrap`, which does not exist.

## Deploy with Argo CD (Phase 2)

```bash
kubectl apply -f argocd/
```

Argo CD then keeps the `demo` namespace equal to this repository (auto-sync, prune and self-heal).
