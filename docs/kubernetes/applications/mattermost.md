## Requirements

!!! summary "Preperations"

    * [x] A running [PostgreSql](/kubernetes/applications/postgresql/) with a dedicated user, password and database for MatterMost

```yaml title="mattermost-values.yaml" linenums="1"
persistence:
  data:
    enabled: true
    size: 10Gi
  plugins:
    enabled: true
    size: 1Gi

externalDB:
  enabled: true
  externalDriverType: "postgres"
  externalConnectionString: "DB_USER:DB_PASSWORD@postgresql:5432/DB_DATABASE?sslmode=disable&connect_timeout=10"

mysql:
  enabled: false

securityContext:
  fsGroup: 2000
  runAsGroup: 2000
  runAsUser: 2000
```

```bash
helm repo add mattermost https://helm.mattermost.com --force-update
```

```bash
helm upgrade \
  --install mattermost mattermost/mattermost-team-edition \
  --namespace mattermost \
  --create-namespace \
  --values mattermost-values.yaml
```
