Checking swap:
```
cat /etc/fstab
```

Disable swap:
```
sudo swapoff -a
```

Creating modules-load config file
```
sudo vim /etc/modules-load.d/k8s.conf
```

Put in file /etc/modules-load.d/k8s.conf
```
overlay
br_netfilter
```

Load overlay module:
```
sudo modprobe overlay
```

Load br_netfilter module:
```
sudo modprobe br_netfilter
```

Create file to set system parameters
```
sudo vim /etc/sysctl.d/k8s.conf
```

Put in file /etc/sysctl.d/k8s.conf
```
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1 
```

Applying configs
```
sudo sysctl --system
```

Update lists
```
sudo apt-get update
```

Install apt-transport-https ca-certificates curl gpg 
#### https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg -y
```

Check if directory exists
```
ls /etc/apt/keyrings
```

Download the public signing key for the Kubernetes package repositories
```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the appropriate Kubernetes apt repository.
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update the apt package index, install kubelet, kubeadm and kubectl, and pin their version
```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

Enable the kubelet service before running kubeadm
```
sudo systemctl enable --now kubelet
```

Update and install requirements for containerd
```
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
```

Download docker key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Add docker repository
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update packages and install containerd
```
sudo apt-get update && sudo apt-get install -y containerd.io
```

Copy containerd default configs
```
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

Enable cgroup
```
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
```

Restart containerd
```
sudo systemctl restart containerd
```

Enable kubelet
```
sudo systemctl enable --now kubelet
```

Initialize control-plane
The init command looks like this
```
sudo kubeadm init --pod-network-cidr=10.10.0.0/16 --apiserver-advertise-address=123.12.34.56
```

Config cluster access
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

To add worker nodes in cluster
Generate token from the control-plane node
```
kubeadm token create --print-join-command
```

To add worker node in cluster
Get the join command printed by command above
The join command looks like this
```
kubeadm join 123.12.34.56:6443 --token 8l43b0.yypm1pkpk3qzoihh --discovery-token-ca-cert-hash sha256:y17a49097471e6834f75c767a350bfbe71a982dfe2d720c3aa72e0fa39987b03
```

Adding weave-net CNI plugin from the control-plane node
```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```