# Create a certificate

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout chave-privada.key -out certificado.crt
```

Creating a TLS secret
```
kubectl create secret tls meu-service-web-tls-secret --cert=certificado.crt --key=chave-privada.key
```

```
kubectl create configmap nginx-config --from-file=nginx.conf
```

```
kubectl apply -f giropops-pod-secret-tls.yaml
```

```
kubectl expose pod giropops-pod-secret-tls
```

```
kubectl port-forward service/giropops-pod-secret-tls 4443:443
```


# Challenge

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx.key -out nginx.crt
```

```
kubectl create secret tls nginx-secret --cert=nginx.crt --key=nginx.key
```


### nginx.conf file

```
events { }

http {
  server {
    listen 80;
    listen 443 ssl;
    
    ssl_certificate /etc/nginx/tls/nginx.crt;
    ssl_certificate_key /etc/nginx/tls/nginx.key;

    location / {
      return 200 'Bem-vindo ao Nginx!\n';
      add_header Content-Type text/plain;
    } 
  }
}
```

```
kubectl create configmap nginx-config --from-file=nginx.conf
```

### nginx-config-map.yaml file

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    events { }

    http {
      server {
        listen 80;
        listen 443 ssl;

        ssl_certificate /etc/nginx/tls/nginx.crt;
        ssl_certificate_key /etc/nginx/tls/nginx.key;

        location / {
          return 200 'Bem-vindo ao Nginx!\n';
          add_header Content-Type text/plain;
        }
      }
    }
```

### nginx-https-pod.yaml file

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-https
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-https
    image: nginx
    ports:
      - containerPort: 80
      - containerPort: 443
    volumeMounts:
      - name: nginx-config-volume
        mountPath: /etc/nginx/nginx.conf
        subPath: nginx.conf
      - name: nginx-tls
        mountPath: /etc/nginx/tls
  volumes:
  - name: nginx-config-volume
    configMap:
      name: nginx-config
  - name: nginx-tls
    secret:
      secretName: nginx-secret
      items:
        - key: tls.crt
          path: nginx.crt
        - key: tls.key
          path: nginx.key
```

### nginx-service.yaml file

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  selector:
    app: nginx
  ports:
  - port: 443
    name: http
    targetPort: 443
    nodePort: 32400
  type: NodePort
```