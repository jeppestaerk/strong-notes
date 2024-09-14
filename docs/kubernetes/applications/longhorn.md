```yaml title="longhorn-values.yaml" linenums="1"
persistence:
  defaultClassReplicaCount: 1
defaultSettings:
  defaultReplicaCount: 1
longhornUI:
  replicas: 1
```

```bash
helm repo add longhorn https://charts.longhorn.io --force-update
```

```bash
kubectl apply -f "https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/prerequisite/longhorn-iscsi-installation.yaml"
```

```bash
kubectl apply -f "https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/prerequisite/longhorn-nfs-installation.yaml"
```

```bash
helm upgrade \
  --install longhorn longhorn/longhorn \
  --namespace longhorn-system \
  --create-namespace \
  --values longhorn-values.yaml
```
