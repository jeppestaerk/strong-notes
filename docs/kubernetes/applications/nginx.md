```yaml title="nginx-values.yaml" linenums="1"
controller:
  extraArgs:
    default-ssl-certificate: default/default-tls
```

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx --force-update
```

```bash
helm upgrade \
  --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace nginx-system \
  --create-namespace \
  --values nginx-values.yaml
```
