# 08 — RBAC and service accounts

RBAC controls which identities can perform which API actions.

- **Role / ClusterRole:** permissions.
- **RoleBinding / ClusterRoleBinding:** attach permissions to a user, group, or service account.
- Prefer namespace-scoped Roles where possible. Avoid giving application identities `cluster-admin`.

Inspect current access:

```bash
kubectl auth can-i get pods
kubectl auth can-i create deployments -n default
kubectl get serviceaccounts
kubectl get roles,rolebindings
kubectl get clusterroles,clusterrolebindings
```

A minimal read-only Role and RoleBinding example is provided in `../manifests/security/`. Apply in a disposable cluster:

```bash
kubectl create namespace rbac-demo
kubectl apply -n rbac-demo -f manifests/security/read-only-role.yaml
kubectl auth can-i get pods --as=system:serviceaccount:rbac-demo:pod-reader -n rbac-demo
kubectl delete namespace rbac-demo
```

The sample grants read access to Pods only; test the exact resource and namespace. Do not assume a `kubectl auth can-i` result reflects a binding that has not been applied.
