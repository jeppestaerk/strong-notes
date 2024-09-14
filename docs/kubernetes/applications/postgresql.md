```yaml title="postgresql-values.yaml" linenums="1"
global:
  postgresql:
    auth:
      username: "DB_USER" # (1)!
      password: "DB_PASSWORD" # (2)!
      database: "DB_DATABASE"
```

1.  Use applications name
2.  Use a random generate password 32 caraters

```bash
repo add bitnami https://charts.bitnami.com/bitnami --force-update
```

```bash
helm upgrade \
  --install postgresql bitnami/postgresql \
  --namespace postgresql \
  --create-namespace \
  --values postgresql-values.yaml
```
