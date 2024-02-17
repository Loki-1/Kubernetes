# Kubernetes Cluster Setup (v1.28.1) - Kubeadm

## Prerequisite:
================
```
3 - Ubuntu Serves
1 - Manager  (4GB RAM , 2 Core) t2.medium
2 - Workers  (1 GB, 1 Core)     t2.micro
```
##### Note: Open Required Ports In AWS Security Groups. For now we will open All trafic.(for practice purpose only)

### Ports:
==========

#### Control Plane Node :-
=========================
```
Protocol| Direction     |Port Range  |	Purpose	                 | Used By
   TCP	| Inbound       | 6443	     | Kubernetes API server	 | All
   TCP	| Inbound	| 2379-2380  | etcd server client API	 | kube-apiserver, etcd
   TCP	| Inbound	| 10250	     | Kubelet API	         | Self, Control plane
   TCP	| Inbound	| 10259	     | kube-scheduler	         | Self
   TCP	| Inbound	| 10257	     | kube-controller-manager   | Self
```
#### Worker Nodes:-
===================
```
Protocol| Direction |	Port Range  |	Purpose	                  | Used By
   TCP	| Inbound   |	10250       | Kubelet API        	  | Self, Control plane
   TCP	| Inbound   | 30000-32767   | NodePort Servicest  	  | All
```
##   =========== Optional For better understanding:- ======================= 

#### On Master node & Woker nodes:-

```
sudo -i
hostnamectl set-hostname master01 --> [if master]
hostnamectl set-hostname worker01/02 --> [if workers]
bash
ip -br a
vi /etc/hosts
Note :- add below lines on both master and worker nodes :-
<masternode ip> master
<workernode1 ip> worker1
<workernode2 ip> worker2
```
##   =========== Optional Work end here:- ======================= 

##   =========== Commands for both master & worker nodes:- ============= 

#### update server
```
sudo -i 
sudo apt-get update && apt-get upgrade -y
```
########################################################################################################

#### Load the Kernel modules on all the nodes
```
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

########################################################################################################

#### Set the following Kernel params for K8s
```
sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
```
########################################################################################################

#### Reload the system changes
```
sudo sysctl --system
```
########################################################################################################

#### Install containerd run time
```
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update
sudo apt install -y containerd.io
```
#####################################################################################################

#### Configure containerd using systemd as cgroup
```
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1

sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl enable containerd
```
###################################################################################################

#### Add apt repository for k8s
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```
##   =========== Commands for both master & worker nodes end here:- ============= 
kubeadm init 

##################################################################################################
Install Pod Network addon:

curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calico.yaml -O
