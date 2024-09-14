```yaml title="redis-values.yaml" linenums="1"
auth:
  password: "TOP SECRET PASSWORD"
```

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami --force-update
```

```bash
helm upgrade \
  --install redis bitnami/redis \
  --namespace redis \
  --create-namespace \
  --values redis-values.yaml
```
