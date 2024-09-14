# Authentik

## Introduction

Authentik is an open-source Identity Provider (IdP) offering advanced authentication, authorization, and auditing solutions. It provides full support for modern protocols like OAuth2, SAML, and OpenID Connect. This guide covers deploying Authentik into your Kubernetes cluster using Helm and Helmfile, with a focus on customization.

## Configuration

The following is a sample configuration. You'll need to adjust this according to your environment:

### values

```yaml title="authentik/values.yaml" linenums="1"
authentik:
  secret_key: "SECRET_KEY" # (1)!
  error_reporting:
    enabled: false
  postgresql:
    password: "DB_PASSWORD" # (2)!

server:
  ingress:
    ingressClassName: traefik # (3)!
    enabled: true
    hosts:
      - auth.domain.tld # (4)!

postgresql:
  enabled: true
  auth:
    password: "COPY_OF_DB_PASSWORD" # (5)!

redis:
  enabled: true
```

1.  Generate a password with `openssl rand 60 | base64 -w 0`
2.  Generate a password with `openssl rand 60 | base64 -w 0`
3.  `nginx` or others can be used aswell. If left out, the default ingress controller is used.
4.  External FQDN for ingress controller.
5.  Use the same password as provided in `authentik.postgresql.password`

!!! Note "Configuration Breakdown"

    - `authentik.secret_key`: This key is used for cookie singing and unique user IDs. Ensure it is unique and securely generated.
    - `authentik.postgresql.password`: This is the password for connecting Authentik to the PostgreSQL database.
    - `server.ingress`: This section configures how Authentik is accessed externally. In this case, it's using the Traefik ingress controller.
    - `postgresql.password`: This is the password used by Authentik for connecting to the PostgreSQL database.
    - `redis.enabled`: Enables Redis as a caching layer for Authentik.
    -  See all configurable values on [Authentiks artifacthub page](https://artifacthub.io/packages/helm/goauthentik/authentik?modal=values).

## Deployment

!!! Note "File structure"

    ```{ .console .no-copy }
    ├── authentik
    │   └── values.yaml
    └── helmfile.yaml
    ```

### with helm

```bash
helm repo add authentik https://charts.goauthentik.io --force-update
```

```bash
helm upgrade \
  --install authentik authentik/authentik \
  --namespace authentik \
  --create-namespace \
  --values authentik/values.yaml
```

### with helmfile

```yaml title="helmfile.yaml" linenums="1"
repositories:
  - name: authentik
    url: https://charts.goauthentik.io

releases:
  - name: authentik
    namespace: authentik
    createNamespace: true
    chart: authentik/authentik
    values:
      - authentik/values.yaml
```

```bash
helmfile apply
```

## Info

!!! Note "Links"

    - [Authentik Homepage](https://goauthentik.io)
    - [Authentik Documentation](https://docs.goauthentik.io/docs/)
    - [Authentik Source repository](https://github.com/goauthentik/authentik)
    - [Authentik Helm chart](https://artifacthub.io/packages/helm/goauthentik/authentik)
    - [Authentik Helm chart values](https://artifacthub.io/packages/helm/goauthentik/authentik?modal=values)
    - [Authentik Helm chart values file](https://github.com/goauthentik/helm/blob/main/charts/authentik/values.yaml)
