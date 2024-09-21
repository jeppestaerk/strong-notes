```bash title="Set namespace for tips and tricks commands"
export NAMESPACE=
```

```bash title="List ALL resources in a namespace"
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --ignore-not-found --show-kind -n $NAMESPACE
```

```bash title="Delete ALL resources in a namespace"
kubectl delete "$(kubectl api-resources --namespaced --verbs=delete -o name | tr "\n" "," | sed -e 's/,$//')" --all -n $NAMESPACE
kubectl delete namespace $NAMESPACE
```
