# 04 — Kubernetes workloads

Kubernetes controllers reconcile the desired state declared in manifests.

## Pod

A Pod is the smallest deployable Kubernetes unit and can contain one or more tightly coupled containers. In most application cases, create Pods through a Deployment rather than creating bare Pods manually.

```bash
kubectl run hello --image=nginx:stable
kubectl get pod hello
kubectl describe pod hello
kubectl delete pod hello
```

## Deployment and ReplicaSet

```bash
kubectl apply -f manifests/workloads/nginx-deployment.yaml
kubectl get deployment,replicaset,pods
kubectl scale deployment/nginx --replicas=3
kubectl rollout status deployment/nginx
kubectl set image deployment/nginx nginx=nginx:stable
kubectl rollout undo deployment/nginx
```

A Deployment manages ReplicaSets; ReplicaSets maintain the requested number of matching Pods.

## Job and CronJob

A Job runs a task to completion. A CronJob creates Jobs on a schedule. The included example is [`../manifests/workloads/cronjob.yaml`](../manifests/workloads/cronjob.yaml).

```bash
kubectl apply -f manifests/workloads/cronjob.yaml
kubectl get cronjobs,jobs,pods
kubectl describe cronjob date-printer
kubectl delete -f manifests/workloads/cronjob.yaml
```

## DaemonSet

A DaemonSet runs a Pod on each eligible node, commonly for node-level agents such as log collectors. Inspect before applying cluster-wide agents:

```bash
kubectl get daemonsets -A
kubectl describe daemonset <daemonset-name> -n <namespace>
```

## StatefulSet

StatefulSets are designed for workloads that need stable identities and/or stable storage. They are not a drop-in replacement for a Deployment. Persistent volumes, storage classes, headless services, and application-level replication must be planned.

```bash
kubectl get statefulsets,pvc,pv
kubectl describe statefulset <statefulset-name>
```

## Common commands

```bash
kubectl get all
kubectl describe deployment nginx
kubectl logs deployment/nginx
kubectl delete deployment nginx
```
