# 01 — Install Kind and create a Kubernetes cluster

Kind (Kubernetes IN Docker) runs Kubernetes nodes as Docker containers. It is useful for local labs and multi-node practice.

## 1. Check prerequisites

```bash
docker --version
docker info
kubectl version --client
```

If Docker is not running, start Docker Desktop (Windows/macOS) or the Docker service (Linux) first.

## 2. Install `kubectl`

Use the official installation instructions for your operating system: <https://kubernetes.io/docs/tasks/tools/>. Verify:

```bash
kubectl version --client
```

## 3. Install Kind

Use the official instructions: <https://kind.sigs.k8s.io/docs/user/quick-start/#installation>.

**macOS with Homebrew:**

```bash
brew install kind
```

**Windows with winget (if available):**

```powershell
winget install Kubernetes.kind
```

If the package identifier is not available in your package source, use the official Kind installation page instead.

**Linux x86-64 example:**

```bash
[ "$(uname -m)" = "x86_64" ] && ARCH=amd64 || ARCH=arm64
curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-${ARCH}"
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

Check the current official release before pinning a version in a real project.

```bash
kind version
```

## 4. Create a one-node cluster

```bash
kind create cluster --name dev-cluster
kubectl cluster-info --context kind-dev-cluster
kubectl get nodes -o wide
```

Expected result: a node named similar to `dev-cluster-control-plane` with `STATUS` `Ready`. Image tags and version strings vary by release.

## 5. Create a multi-node cluster

Use the config file at [`../manifests/kind-multi-node.yaml`](../manifests/kind-multi-node.yaml):

```bash
kind delete cluster --name dev-cluster
kind create cluster --name dev-cluster --config manifests/kind-multi-node.yaml
kubectl get nodes -o wide
```

Run these commands from the repository root. The config has one control-plane node and two workers. For an ingress controller or NodePort labs, additional port mappings may be needed.

## 6. Check context and cluster

```bash
kubectl config get-contexts
kubectl config current-context
kubectl get namespaces
kubectl get pods -A
```

The context should be `kind-dev-cluster`. If you have several clusters, always check the current context before applying YAML.

## 7. Deploy a sample app

```bash
kubectl apply -f manifests/workloads/nginx-deployment.yaml
kubectl apply -f manifests/networking/nginx-service.yaml
kubectl rollout status deployment/nginx
kubectl get pods,deploy,svc
kubectl port-forward service/nginx 8080:80
```

Open `http://localhost:8080`. Stop port forwarding with `Ctrl+C`.

## 8. Delete the cluster

```bash
kind delete cluster --name dev-cluster
```

## Troubleshooting

- `Cannot connect to the Docker daemon`: start Docker and retry `docker info`.
- Node remains `NotReady`: run `kubectl describe node <node-name>` and `kubectl get pods -A`.
- Image pull failure: inspect `kubectl describe pod <pod-name>`; check image name and network access.
- Wrong cluster: check `kubectl config current-context` before applying anything.

Official docs: <https://kind.sigs.k8s.io/>.
