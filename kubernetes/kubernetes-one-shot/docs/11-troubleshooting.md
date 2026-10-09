# 11 — Troubleshooting checklist

## Start with these commands

```bash
kubectl config current-context
kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods -A
kubectl get events -A --sort-by=.metadata.creationTimestamp
```

## Pod is Pending

```bash
kubectl describe pod <pod-name> -n <namespace>
kubectl get pvc -n <namespace>
kubectl describe pvc <claim-name> -n <namespace>
```

Look for insufficient CPU/memory, unbound PVCs, node selectors, taints, or scheduling constraints.

## ImagePullBackOff / ErrImagePull

```bash
kubectl describe pod <pod-name> -n <namespace>
```

Check image name/tag, registry connectivity, private-registry credentials, and architecture compatibility.

## CrashLoopBackOff

```bash
kubectl logs <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --previous
kubectl describe pod <pod-name> -n <namespace>
```

Check startup command, environment, mounted files, dependencies, and probe configuration.

## Service is unreachable

```bash
kubectl get svc,endpointslices -n <namespace>
kubectl get pods --show-labels -n <namespace>
kubectl describe svc <service-name> -n <namespace>
```

Confirm Service selectors match Pod labels and target ports match the application port. Test locally with `kubectl port-forward`.

## Clean up a lab

```bash
kubectl delete -f manifests/workloads/nginx-deployment.yaml
kubectl delete -f manifests/networking/nginx-service.yaml
kind delete cluster --name dev-cluster
# or, for Minikube:
minikube delete
```
