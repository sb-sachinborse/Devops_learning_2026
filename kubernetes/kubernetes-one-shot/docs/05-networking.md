# 05 — Services, DNS, Ingress and networking

## Service types

- **ClusterIP:** internal virtual IP, default type.
- **NodePort:** exposes a port on nodes; useful for labs, not automatically a production design.
- **LoadBalancer:** requests an external load balancer from a supporting environment/cloud provider.
- **ExternalName:** maps a service name to a DNS name.

Apply the sample Service:

```bash
kubectl apply -f manifests/networking/nginx-service.yaml
kubectl get svc nginx
kubectl describe svc nginx
kubectl get endpointslices
kubectl port-forward service/nginx 8080:80
```

## Service discovery

Within a namespace, a service is typically resolved by its service name. Across namespaces, use a fully qualified DNS name such as `my-service.team-a.svc.cluster.local` (cluster domain can vary).

## Ingress

An Ingress resource needs an Ingress controller to do anything. Kind and Minikube require additional setup; the manifest alone does not install a controller. See the official docs: <https://kubernetes.io/docs/concepts/services-networking/ingress/>.

Inspect current resources:

```bash
kubectl get ingress -A
kubectl describe ingress <ingress-name> -n <namespace>
kubectl get ingressclass
```

## Network troubleshooting

```bash
kubectl get svc,endpointslices
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get networkpolicies -A
```

NetworkPolicies only take effect when the cluster's network plugin supports them. CNI examples mentioned in the transcript include Calico and Cilium; install and configure these according to their official documentation rather than applying random manifests to a working cluster.
