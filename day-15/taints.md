
Scale replicas

```
kubectl scale deployment nginx-deployment --replicas 20
```

Applying taint

NoExecute -> Remove all PODs from this node and relocate to a new one

```
kubectl taint node kind-worker maintenance=true:NoExecute
```

NoSchedule -> No new POD should be created on this node

```
kubectl taint node kind-worker special=true:NoSchedule
```

Tolerations in a POD spec session mean the POD or set of PODs could be scheduled on a node matches the taints with the specified tolerations

Ex.:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  ...
spec:
    ...
  template:
    ...
    spec:
      ...
      tolerations:
      - key: "special"
        operator: "Equal"
        value: "true"
        effect: "NoSchedule"
```

Removing taint
Put negative sign after last letter

```
kubectl taint node kind-worker maintenance=true:NoExecute-
```

Rollout PODs to rebalance among nodes

```
kubectl rollout restart deployment nginx-deployment
```