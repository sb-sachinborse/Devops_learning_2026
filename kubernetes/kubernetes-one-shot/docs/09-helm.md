# 09 — Helm

Helm is a package manager for Kubernetes. Charts template and package Kubernetes resources. Install Helm from <https://helm.sh/docs/intro/install/>.

Verify:

```bash
helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx
```

Before installing a chart, inspect its values and requirements:

```bash
helm show values bitnami/nginx
helm show chart bitnami/nginx
```

Example install (chart availability, values, and licensing may change):

```bash
helm install demo-nginx bitnami/nginx --namespace helm-demo --create-namespace
helm list -n helm-demo
kubectl get all -n helm-demo
helm status demo-nginx -n helm-demo
helm uninstall demo-nginx -n helm-demo
kubectl delete namespace helm-demo
```

Do not blindly copy remote installation scripts into production. Review chart source, permissions, image provenance, and values first.
