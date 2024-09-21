```dockerfile title="Dockerfile" linenums="1"
FROM traefik/whoami:latest
ENTRYPOINT ["/whoami"]
EXPOSE 80
```

```bash
docker build . -t hello-world/whoami:latest
```

```bash
docker tag hello-world/whoami:latest registry.valhal.cloud/hello-world/whoami:latest
```

```bash
docker push registry.valhal.cloud/hello-world/whoami:latest
```

```bash
# test image local on: http://localhost:8080
docker run --name whoami -p 8080:80 registry.valhal.cloud/hello-world/whoami:latest
```

```bash
kubectl create namespace hello-world
```

```bash
kubectl --namespace hello-world create secret docker-registry regcred --docker-server=registry.valhal.cloud --docker-username=<username> --docker-password=<password>
```

```yaml title="hello-world.yaml" linenums="1"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: whoami-deployment
  namespace: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: whoami-pod
  template:
    metadata:
      labels:
        app: whoami-pod
    spec:
      imagePullSecrets:
        - name: regcred
      containers:
        - name: whoami
          image: registry.valhal.cloud/hello-world/whoami:latest
          ports:
            - name: http
              containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: whoami-serivce
  namespace: hello-world
spec:
  selector:
    app: whoami
  ports:
    - name: web
      port: 80
      targetPort: http
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: whoami-ingress
  namespace: hello-world
spec:
  rules:
    - host: whoami.valhal.dev
      http:
        paths:
          - pathType: ImplementationSpecific
            path: "/"
            backend:
              service:
                name: whoami-serivce
                port:
                  name: web
```

```bash
kubectl apply -f hello-world.yaml
```

> [https://whoami.valhal.dev](https://whoami.valhal.dev)
