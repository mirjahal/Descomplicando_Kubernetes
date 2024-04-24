kubectl create deployment --image nginx --replicas 3 nginx-deployment

kubectl create deployment --image nginx --replicas 3 nginx-deployment --dry-run=client -o yaml > second-deployment.yaml