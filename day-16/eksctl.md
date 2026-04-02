# eksctl create cluster

```
eksctl create cluster --name=eks-cluster --version=1.35 --region=us-east-1 --nodegroup-name=eks-cluster-nodegroup --node-type=t3.medium --nodes=2 --nodes-min=1 --nodes-max=3 --managed
```

# vpc-cni addon

To check if the Amazon VPC CNI plugin is installed in your Kubernetes cluster, you can use several methods depending on whether it is a managed EKS add-on or a self-managed installation.

## Check for the aws-node DaemonSet:
The VPC CNI runs as a DaemonSet named aws-node in the kube-system namespace. Use the following command to verify its existence:
```
kubectl get ds aws-node -n kube-system
```

## Check for VPC CNI Pods:
Verify if the pods for the CNI are running across your nodes:
```
kubectl get pods -n kube-system -l k8s-app=aws-node
```

## Check if it is a Managed EKS Add-on:
If you are using Amazon EKS, you can check if the VPC CNI is registered as a managed add-on via the AWS CLI:
```
aws eks describe-addon --cluster-name <your-cluster-name> --addon-name vpc-cni
```

*An error message indicates it is not installed as a managed add-on, though it may still exist as a self-managed installation.*

## eksctl create addon
```
eksctl create addon --name vpc-cni --version v1.21.1-eksbuild.5 --cluster eks-cluster --force
```

Check Amazon VPC CNI versions:
*https://docs.aws.amazon.com/eks/latest/userguide/managing-vpc-cni.html*
