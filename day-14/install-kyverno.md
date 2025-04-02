### Add repo

```
helm repo add kyverno https://kyverno.github.io/kyverno/ helm repo update
```

### Install Kyverno

```
helm install kyverno kyverno/kyverno --namespace kyverno --create-namespace
```