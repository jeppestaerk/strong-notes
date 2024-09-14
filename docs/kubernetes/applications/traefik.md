```yaml title="traefik-values.yaml" linenums="1"
ports:
  web:
    forwardedHeaders:
      insecure: true
  websecure:
    forwardedHeaders:
      insecure: true
ingressRoute:
  dashboard:
    enabled: true
  healthcheck:
    enabled: true
providers:
  kubernetesCRD:
    allowCrossNamespace: true
  kubernetesIngress:
    allowCrossNamespace: true
    publishedService:
      enabled: true
logs:
  access:
    enabled: true
priorityClassName: "system-cluster-critical"
```

```yaml title="traefik-config.yaml" linenums="1"
apiVersion: traefik.io/v1alpha1
kind: TLSStore
metadata:
  name: default
  namespace: default
spec:
  defaultCertificate:
    secretName: default-tls
```

```bash
helm repo add traefik https://traefik.github.io/charts --force-update
```

```bash
helm upgrade \
  --install traefik traefik/traefik \
  --namespace traefik-system \
  --create-namespace \
  --values traefik-values.yaml \
  --wait
```

```bash
kubectl apply -f traefik-config.yaml
```
