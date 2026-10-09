# 07 — Requests, limits, probes and autoscaling

## Requests and limits

- **Requests** help the scheduler place a Pod and are used for resource accounting.
- **Limits** constrain resource use for supported resources. CPU is throttled; memory overuse can result in an OOM kill.

The NGINX Deployment sample defines modest requests and limits. Adjust them for your workload and node capacity.

## Probes

- **startupProbe:** allows slow-starting apps time to initialize.
- **readinessProbe:** determines whether a Pod should receive Service traffic.
- **livenessProbe:** detects a container that should be restarted.

Incorrect probes can cause restarts or remove healthy Pods from traffic. Test endpoints and timings before production use.

## Horizontal Pod Autoscaler (HPA)

HPA needs a metrics API, commonly metrics-server, and resource requests for CPU utilization calculations.

```bash
kubectl top nodes
kubectl top pods
kubectl get hpa -A
kubectl autoscale deployment nginx --cpu-percent=60 --min=2 --max=5
kubectl describe hpa nginx
```

If `kubectl top` says metrics are unavailable, install/enable a compatible metrics provider for your cluster. Do not assume it is present by default in every Kind/Minikube setup.

## Vertical Pod Autoscaler (VPA)

VPA is an additional component, not built into every cluster by default. Install it using its official project guidance and understand whether its update mode restarts Pods before using it on important workloads.

## Pod disruption and scheduling concepts

Explore resource requests, node selectors, affinity/anti-affinity, taints/tolerations, PodDisruptionBudgets, and topology spread constraints when you move beyond a single-node lab.
