# Cert Manager

## Introduction

Cert Manager is an open-source Kubernetes add-on that automates the management and issuance of TLS certificates from various Certificate Authorities (CAs). It ensures that certificates are properly issued, renewed, and available to secure your Kubernetes services. In this guide, we will cover how to deploy Cert Manager using Helm and Helmfile.

## Configuration

The following is a sample configuration. You'll need to adjust this according to your environment:

### values

```yaml title="cert-manager/values.yaml" linenums="1"
--8<-- "kubernetes/cert-manager/values.yaml"
```

!!! Note "Configuration Breakdown"

    - `crds.enabled`: This option decides if the CRDs should be installed as part of the Helm installation.
    -  See all configurable values on [cert-managers artifacthub page](https://artifacthub.io/packages/helm/cert-manager/cert-manager?modal=values).

## Deployment

!!! Note "File structure"

    ```{ .console .no-copy }
    ├── cert-manager
    │   └── values.yaml
    └── helmfile.yaml
    ```

### with helm

```bash
helm repo add jetstack https://charts.jetstack.io --force-update
```

```bash
helm upgrade \
  --install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --values cert-manager/values.yaml
```

### with helmfile

```yaml title="helmfile.yaml" linenums="1"
repositories:
  - name: jetstack
    url: https://charts.jetstack.io

releases:
  - name: cert-manager
    namespace: cert-manager
    createNamespace: true
    chart: jetstack/cert-manager
    values:
      - cert-manager/values.yaml
```

```bash
helmfile apply
```

### Verification

```bash
kubectl get pods -n cert-manager
```

## Usage

### Create a Issuer

```yaml title="cert-manager/secret.yaml" linenums="1"
--8<-- "kubernetes/cert-manager/secret.yaml"
```

1.  Create a scpoed api token at cloudflare with `Zone:Read` and `DNS:Edit` permissions.

```bash
kubectl apply -f cert-manager/secret.yaml
```

```yaml title="cert-manager/issuer.yaml" linenums="1"
--8<-- "kubernetes/cert-manager/issuer.yaml"
```

1.  `ClusterIssuer` for full cluster isuer or `Issuer` for namespace scoped issuer.
2.  Use `https://acme-staging-v02.api.letsencrypt.org/directory` for staging enviroment.

```bash
kubectl apply -f cert-manager/issuer.yaml
```

### Create a Certificate

```yaml title="cert-manager/certificate.yaml" linenums="1"
--8<-- "kubernetes/cert-manager/certificate.yaml"
```

```bash
kubectl apply -f cert-manager/certificate.yaml
```

```bash
kubectl describe certificate default-cert -n default
```

## Troubleshooting

```bash
kubectl get certificates -A
```

```bash
kubectl describe certificate <certificate-name> -n <namespace>
```

## Infomation

!!! Note "Files"

    ```{ .console .no-copy }
    kubernetes
    ├── cert-manager
    │   ├── certificate.yaml
    │   ├── issuer.yaml
    │   ├── secret.yaml
    │   └── values.yaml
    └── helmfile.yaml
    ```

!!! Note "Links"

    - [Cert Manager Homepage](https://cert-manager.io/)
    - [Cert Manager Documentation](https://cert-manager.io/docs/)
    - [Cert Manager Source repository](https://github.com/cert-manager/cert-manager)
    - [Cert Manager Helm chart](https://artifacthub.io/packages/helm/cert-manager/cert-manager)
    - [Cert Manager Helm chart values](https://artifacthub.io/packages/helm/cert-manager/cert-manager?modal=values)
