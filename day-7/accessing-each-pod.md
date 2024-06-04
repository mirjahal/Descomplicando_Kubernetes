# Get IP address for each POD:

```
nslookup nginx-0.nginx.default.svc.cluster.local
```
```
nslookup nginx-1.nginx.default.svc.cluster.local
```
```
nslookup nginx-2.nginx.default.svc.cluster.local
```

# Accessing each POD
```
 wget -O- http://<POD-IP-ADDRESS>
 ```

# Services

### ClusterIP
```
kubectl expose deployment DEPLOYMENT_NAME
```

Example:
```
kubectl expose deployment nginx
```

### NodePort
```
kubectl expose deployment DEPLOYMENT_NAME --type NodePort
```

Example:
```
kubectl expose deployment nginx --type NodePort
```

### LoadBalancer
```
kubectl expose deployment DEPLOYMENT_NAME --type LoadBalancer
```

Example:
```
kubectl expose deployment nginx --type LoadBalancer
```

### ExternalName
```
kubectl create service externalname NAME --external-name external.name
```

Example:
```
kubectl create service externalname giropops-db --external-name db.giropops.com.br
```

# Creating service to expose another service
```
kubectl expose service SERVICE-NAME --name=NAME --type TYPE
```

Example:
```
kubectl expose service nginx --name=service-expose-service --type NodePort
```