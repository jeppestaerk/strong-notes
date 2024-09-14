```yaml title="metallb-config.yaml" linenums="1"
---
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: address-pool
  namespace: metallb-system
spec:
  addresses:
    - 10.10.10.0/24
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: advertisement
  namespace: metallb-system
---
```

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami --force-update
```

```bash
helm upgrade \
  --install metallb bitnami/metallb \
  --namespace metallb-system \
  --create-namespace \
  --wait
```

```bash
kubectl apply -f metallb-config.yaml
```
