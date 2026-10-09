# 03 — kubectl fundamentals

`kubectl` communicates with the Kubernetes API server using the active kubeconfig context.

## Inspect the cluster

```bash
kubectl version --client
kubectl config get-contexts
kubectl config current-context
kubectl cluster-info
kubectl get nodes -o wide
kubectl get namespaces
kubectl get pods -A
```

## Create, inspect, and delete resources

```bash
kubectl get pods
kubectl get pods -o wide
kubectl get pods -w
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs -f <pod-name>
kubectl exec -it <pod-name> -- sh
kubectl delete pod <pod-name>
```

Replace `<pod-name>` with a real name; angle-bracket placeholders are not literal commands.

## Apply YAML declaratively

```bash
kubectl apply -f manifests/workloads/nginx-deployment.yaml
kubectl diff -f manifests/workloads/nginx-deployment.yaml
kubectl get -f manifests/workloads/nginx-deployment.yaml
kubectl delete -f manifests/workloads/nginx-deployment.yaml
```

`kubectl diff` can return a non-zero exit status when differences exist. Review the proposed changes before applying them.

## Namespaces

```bash
kubectl create namespace dev
kubectl get pods -n dev
kubectl config set-context --current --namespace=dev
kubectl config set-context --current --namespace=default
kubectl delete namespace dev
```

Deleting a namespace deletes the namespaced resources inside it. Check first:

```bash
kubectl get all -n dev
```

## Output and selectors

```bash
kubectl get deploy,rs,pods,svc
kubectl get pods -o yaml
kubectl get pods -o json
kubectl get pods -l app=nginx
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl explain deployment.spec
```

## Rollouts and scaling

```bash
kubectl rollout status deployment/nginx
kubectl rollout history deployment/nginx
kubectl scale deployment/nginx --replicas=3
kubectl rollout undo deployment/nginx
```

## Useful aliases (optional)

Bash/Zsh:

```bash
alias k=kubectl
alias kgp='kubectl get pods'
alias kgs='kubectl get svc'
```

PowerShell:

```powershell
Set-Alias k kubectl
```
