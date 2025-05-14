## Adding label to node

```
kubectl label nodes kind-worker region=sa-east-1 az=sa-east-1a
kubectl label nodes kind-worker2 region=sa-east-1 az=sa-east-1b
kubectl label nodes kind-worker3 region=us-east-1 az=us-east-1a
```

## Show labels from node

```
kubectl get nodes kind-worker --show-labels
```

```
kubectl get nodes -L region
```