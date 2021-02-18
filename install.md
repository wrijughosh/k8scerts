# Install Docker and Kubernetes to all nodes

Open 3 Azure Cloud shells and Log-in to Launcher VM in each case:
```bash
ssh pranabpaul@13.95.211.101                 (Use the public IP of the VM)
```
Log-in to controller and 2 workers from Launcher prompt using their private IPs:
```bash
ssh kuberoot@10.230.0.10
ssh kuberoot@10.230.0.20
ssh kuberoot@10.230.0.21
```
Run following commands in launcher, controller and both the workers VMs:
============================================================================
Run following commands to install docker, kubelet, kubeadm and kubectl:
## Get the Docker gpg key
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## Add the Docker repository
```bash
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
## Get the Kubernetes gpg key
```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
## Add the Kubernetes repository
```bash
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

## Update your packages
```bash
sudo apt-get update
```
## Install Docker, kubelet, kubeadm, and kubectl
```bash
sudo apt-get install -y docker-ce=5:19.03.9~3-0~ubuntu-bionic kubelet=1.18.6-00 kubeadm=1.18.6-00 kubectl=1.18.6-00
```

## Hold them at the current version
```bash
sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```
====================================================================

Run following commands in controller and both the workers VMs:
## Add the iptables rule to sysctl.conf
```bash
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
```
## Enable iptables immediately
```bash
sudo sysctl -p
```
===================================================================

Courtsey: https://gist.github.com/chadmcrowell/f2221cf33185c8c52d6e793320d69678
