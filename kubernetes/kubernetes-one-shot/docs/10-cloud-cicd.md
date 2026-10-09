# 10 — Cloud, CI/CD, GitOps and monitoring overview

The transcript describes more advanced work including AWS EKS, a three-tier application, Jenkins CI/CD, Argo CD GitOps, MongoDB, Prometheus, and Grafana. Treat this as a sequence of separate labs, not one universal command block.

## Suggested order

1. Run the NGINX and voting-demo local labs.
2. Build and tag your own container image; publish it to a registry.
3. Deploy frontend/backend services and a database with appropriate configuration and persistent storage.
4. Add a CI pipeline (for example Jenkins) to test, build, scan, and push images.
5. Store deployment manifests or Helm values in Git and use Argo CD for reconciliation.
6. Add metrics collection and dashboards with Prometheus and Grafana.
7. Only then recreate the architecture on a managed cluster such as EKS.

## EKS cost warning

EKS and related cloud infrastructure can create charges. Instance sizes, region, node count, load balancers, storage, NAT gateways, and data transfer all affect cost. Free-tier eligibility changes and should never be assumed. Set a budget alert and delete all lab resources when done.

Use AWS's current official EKS guide: <https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html>. Do not paste AWS access keys into GitHub or commit kubeconfig files.

## CI/CD vs GitOps

- **CI:** test code, build images, scan, and publish artifacts.
- **CD:** deploy a tested version to an environment.
- **GitOps:** Git declares desired state; a controller such as Argo CD continuously reconciles the cluster to that state.

Keep secrets outside the repository and use least-privilege credentials.

## Monitoring

Prometheus collects time-series metrics; Grafana visualizes them. In Kubernetes, common metrics sources include node-exporter for host metrics, kube-state-metrics for Kubernetes object state, and metrics-server for basic resource metrics used by `kubectl top`/HPA. These tools serve different purposes and are not interchangeable.

For a local chart-based lab, review the current official chart documentation and values before installing. Then verify:

```bash
kubectl get pods -A
kubectl get svc -A
kubectl logs <pod-name> -n <namespace>
```

## Example workflow commands

```bash
# Build your own app image (run from the app directory)
docker build -t <registry>/<username>/<app>:<tag> .
docker push <registry>/<username>/<app>:<tag>

# Update manifests to use the published image, then:
kubectl apply -f <manifest-directory>
kubectl rollout status deployment/<deployment-name>
```

Replace all placeholders. Do not use these literal angle-bracket values in commands.
