# Agenda: Kubernetes Setup Using Kubeadm In AWS EC2 Ubuntu Servers
=======================================================

### Prerequisite:
==========
```
3 - Ubuntu Serves
1 - Manager  (4GB RAM , 2 Core) t2.medium
2 - Workers  (1 GB, 1 Core)     t2.micro
Note: Open Required Ports In AWS Security Groups. For now we will open All trafic.(for practice purpose only)
```
### Ports:
======

### Control Plane Node :-
==================
```
Protocol| Direction     |Port Range  |	Purpose	                 | Used By
   TCP	| Inbound       | 6443	     | Kubernetes API server	 | All
   TCP	| Inbound	| 2379-2380  | etcd server client API	 | kube-apiserver, etcd
   TCP	| Inbound	| 10250	     | Kubelet API	         | Self, Control plane
   TCP	| Inbound	| 10259	     | kube-scheduler	         | Self
   TCP	| Inbound	| 10257	     | kube-controller-manager   | Self
```
### Worker Nodes:-
============
```
Protocol| Direction |	Port Range  |	Purpose	                  | Used By
   TCP	| Inbound   |	10250       | Kubelet API        	  | Self, Control plane
   TCP	| Inbound   | 30000-32767   | NodePort Servicest  	  | All
```

## ========== COMMON FOR MASTER & SLAVES START ====

### First, login as ‘root’ user because the following set of commands need to be executed with ‘sudo’ permissions.
```
sudo su -
```
### Turn Off Swap Space
```
swapoff -a

sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```
### Install Containerd
```
wget https://github.com/containerd/containerd/releases/download/v1.6.16/containerd-1.6.16-linux-amd64.tar.gz

tar Cxzvf /usr/local containerd-1.6.16-linux-amd64.tar.gz

wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service

mkdir -p /usr/local/lib/systemd/system

mv containerd.service /usr/local/lib/systemd/system/containerd.service

systemctl daemon-reload

systemctl enable --now containerd
```
### Install Runc
```
wget https://github.com/opencontainers/runc/releases/download/v1.1.4/runc.amd64

install -m 755 runc.amd64 /usr/local/sbin/runc
```
### Install CNI
```
wget https://github.com/containernetworking/plugins/releases/download/v1.2.0/cni-plugins-linux-amd64-v1.2.0.tgz
mkdir -p /opt/cni/bin
tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v1.2.0.tgz
```
### Install CRICTL

#### VERSION="v1.26.0" # check latest version in /releases page(link :- https://github.com/kubernetes-sigs/cri-tools/releases)
```
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.29.0/crictl-v1.29.0-linux-amd64.tar.gz
sudo tar zxvf crictl-v1.29.0-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-v1.29.0-linux-amd64.tar.gz
```
```
cat <<EOF | sudo tee /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: false
pull-image-on-create: false
EOF
```
### Install Required packages and apt keys.
```
apt-get update -y
apt-get update && sudo apt-get install -y apt-transport-https curl
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update -y
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward
modprobe br_netfilter
sysctl -p /etc/sysctl.conf
```
### Install kubeadm, Kubelet Containerd And Kubectl 
```
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
sudo apt-mark hold kubelet kubeadm kubectl
```

### Enable and start kubelet service
```
systemctl daemon-reload
systemctl start kubelet
systemctl enable kubelet.service
systemctl start containerd
systemctl enable containerd

mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
systemctl restart containerd
```
## ========== COMMON FOR MASTER & SLAVES END =====

## =========== In Master Node Start ====================

### Steps Only For Kubernetes Master
### Switch to the root user.
```
sudo su -
```
### Initialize Kubernates master by executing below commond.
```
kubeadm init --apiserver-advertise-address=<ControllerVM-PrivateIP> --pod-network-cidr=10.244.0.0/16 
```
### exit root user & exeucte as normal user
```
exit
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
#### Note : above 4 line Important ----you did mistake multiple times here

### To verify, if kubectl is working or not, run the following command.
```
kubectl get pods -o wide --all-namespaces
```
#### You will notice from the previous command, that all the pods are running except one: ‘kube-dns’. For resolving this we will install a # pod network.
#### To install the weave pod network, run the following command:

### First enable these ports on security groups : TCP 6783 and UDP 6783/6784
```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
kubectl get nodes
kubectl get pods --all-namespaces
```

### Get token
```
kubeadm token create --print-join-command
```
## ========= In Master Node End ====================

### Add Worker Machines to Kubernates Master
=========================================

Copy kubeadm join token from and execute in Worker Nodes to join to cluster



kubectl commonds has to be executed in master machine.

### Check Nodes
=============
```
kubectl get nodes
```

### Deploy Sample Application
==========================
```
kubectl run nginx-demo --image=nginx --port=80

kubectl expose deployment nginx-demo --port=80 --type=NodePort
```

https://www.mankier.com/1/kubeadm-init


### Get Node Port details
=====================
```
kubectl get services
```

### References: 
===========

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
https://www.mirantis.com/blog/how-install-kubernetes-kubeadm/
https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker
https://github.com/containerd/containerd/blob/main/docs/getting-started.md
https://kubernetes.io/docs/reference/networking/ports-and-protocols/
https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#install
https://github.com/skooner-k8s/skooner
https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#eks
https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md
