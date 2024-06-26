# AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

# AWS credentials
aws configure

# eks-ctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

# Creating a cluster
eksctl create cluster --name=eks-cluster --version=1.24 --region=us-east-1 --nodegroup-name=eks-cluster-nodegroup --node-type=t3.medium --nodes=2 --nodes-min=1 --nodes-max=3 --managed

# Update kube config
aws eks --region us-east-1 update-kubeconfig --name eks-cluster

# Installing kube-prometheus
git clone https://github.com/prometheus-operator/kube-prometheus

cd kube-prometheus

kubectl create -f manifests/setup

kubectl apply -f manifests/

### Accessing granafa
kubectl port-forward -n monitoring svc/grafana 33000:3000

# Scaling nodegroup
eksctl scale nodegroup --cluster=eks-cluster --nodes=0 --nodes-min=0 --nodes-max=1 --name=eks-cluster-nodegroup -r us-east-1

# List Contexts
kubectl config get-contexts

### Switch Between Contexts
kubectl config use-context <NAME>

# Deleting a cluster
eksctl delete cluster --name=eks-cluster -r us-east-1