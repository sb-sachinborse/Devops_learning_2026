# 06 — ConfigMaps, Secrets and persistent storage

## ConfigMap

Use ConfigMaps for non-sensitive configuration. Example manifest: [`../manifests/storage/app-configmap.yaml`](../manifests/storage/app-configmap.yaml).

```bash
kubectl apply -f manifests/storage/app-configmap.yaml
kubectl get configmap app-config
kubectl describe configmap app-config
```

## Secret

Kubernetes Secret values are base64-encoded in the standard YAML representation; base64 is **not encryption**. Avoid committing real passwords, tokens, certificates, or production secret data to Git. The sample file contains only a clearly fake demo value.

```bash
kubectl apply -f manifests/security/demo-secret.yaml
kubectl get secret demo-secret
kubectl describe secret demo-secret
```

Do not print secret values into logs or screenshots. In real environments, use appropriate RBAC, encryption at rest, and a secret-management workflow.

## PersistentVolume and PersistentVolumeClaim

- **PV:** storage resource available to the cluster.
- **PVC:** a workload's request for storage.
- **StorageClass:** defines dynamic provisioning behavior, if supported by the environment.

```bash
kubectl get pv,pvc,storageclass
kubectl describe pvc <claim-name>
```

A sample PVC is included for learning, but dynamic provisioning depends on the local cluster's storage provisioner. Apply it only in a disposable lab and check status before relying on it:

```bash
kubectl apply -f manifests/storage/demo-pvc.yaml
kubectl get pvc
```

If it remains `Pending`, inspect the PVC events and available StorageClasses:

```bash
kubectl describe pvc demo-pvc
kubectl get storageclass
```
