# kubesentinel-gitops

Kubernetes manifests for the [KubeSentinel](https://github.com/glichy-gem/kubesentinel) demo app. In Phase 2 Argo CD watches this repository, so every deploy is a Git commit that the agent can later query.

## Layout

```
namespace.yaml          the demo namespace
kustomization.yaml      root: namespace + all apps
apps/orders-api/        Deployment (2 replicas, probes, limits) and NodePort Service
apps/redis/             Deployment (in-memory only) and ClusterIP Service
```

## Apply manually (Phase 1)

```bash
kubectl apply -k .
kubectl -n demo get pods
```

The `orders-api` image tag is `orders-api:dev`, a locally built image loaded into kind. Phase 2 replaces it with a Git SHA tag written here by CI.
