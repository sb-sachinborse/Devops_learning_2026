# 02 — Install Minikube

Minikube starts a local Kubernetes cluster. Choose a supported driver for your OS (Docker is a common option). Follow the current official installation page because packages change over time: <https://minikube.sigs.k8s.io/docs/start/>.

## 1. Verify requirements

```bash
docker --version
docker info
kubectl version --client
```

Install `kubectl` and Minikube using the official instructions for your OS.

## 2. Start Minikube

With Docker installed and running:

```bash
minikube start --driver=docker
```

If your computer has sufficient resources and you want to specify them:

```bash
minikube start --driver=docker --cpus=4 --memory=6144
```

Adjust CPU and memory to your machine. Do not copy resource values blindly on a low-memory computer.

## 3. Verify the cluster

```bash
minikube status
kubectl config current-context
kubectl get nodes -o wide
kubectl get pods -A
```

The current context should usually be `minikube`.

## 4. Deploy an example

```bash
kubectl apply -f manifests/workloads/nginx-deployment.yaml
kubectl apply -f manifests/networking/nginx-service.yaml
kubectl get deploy,pods,svc
kubectl port-forward service/nginx 8080:80
```

Visit `http://localhost:8080`, then stop the port-forward process with `Ctrl+C`.

## 5. Open the dashboard (optional)

```bash
minikube dashboard
```

This command may open a browser and keep a local proxy running in the terminal.

## 6. Stop or delete

```bash
minikube stop
```

To remove the cluster and its local state:

```bash
minikube delete
```

## Kind vs Minikube

- **Kind:** Kubernetes nodes are Docker containers; great for disposable multi-node labs and CI.
- **Minikube:** beginner-friendly local cluster, dashboard, and add-ons.

Do not expect the commands, context names, networking, or ingress setup to be identical between them.
