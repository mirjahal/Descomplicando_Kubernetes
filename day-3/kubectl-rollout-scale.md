# kubectl rollout

Manage the rollout of one or many resources.
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_rollout/

Valid resource types include:

* deployments
* daemonsets
* statefulsets

```
kubectl rollout SUBCOMMAND
```

Examples:
```
# Rollback to the previous deployment
kubectl rollout undo deployment/abc

# Check the rollout status of a daemonset
kubectl rollout status daemonset/foo

# Restart a deployment
kubectl rollout restart deployment/abc

# Restart deployments with the 'app=nginx' label
kubectl rollout restart deployment --selector=app=nginx

# Roll back to a previous rollout of nginx-deployment in giropops namespace
kubectl rollout undo deployment -n giropops nginx-deployment

# View previous rollout revisions and configurations
kubectl rollout history deployment -n giropops nginx-deployment

# Roll back to revision 9
kubectl rollout undo deployment -n giropops nginx-deployment --to-revision 9

# Mark the provided resource as paused
# Paused resources will not be reconciled by a controller 
# Use "kubectl rollout resume" to resume a paused resource
# Currently only deployments support being paused
kubectl rollout pause deployment -n giropops nginx-deployment

# Resume a paused resource
# Paused resources will not be reconciled by a controller
# By resuming a resource, we allow it to be reconciled again
# Currently only deployments support being resumed
kubectl rollout resume deployment -n giropops nginx-deployment

# Restart a resource
kubectl rollout restart deployment -n giropops nginx-deployment
```

# kubectl scale

Set a new size for a deployment, replica set, replication controller, or stateful set
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_scale/

Examples:
```
# Scale a replica set named 'foo' to 3
kubectl scale --replicas=3 rs/foo

# Scale a resource identified by type and name specified in "foo.yaml" to 3
kubectl scale --replicas=3 -f foo.yaml

# If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

# Scale multiple replication controllers
kubectl scale --replicas=5 rc/example1 rc/example2 rc/example3

# Scale stateful set named 'web' to 3
kubectl scale --replicas=3 statefulset/web
Options

# Scale replicas to 10 for nginx-deployment deployment in giropops namespace
kubectl scale deployment -n giropops nginx-deployment --replicas 10
```