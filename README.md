# Kubernetes:-
## Kubernetes is a container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. A Kubernetes cluster consists of a set of nodes that work together to run containerized applications.
## Cluster Architecture:-
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/69b5e016-0048-4fb4-8197-c079cfe1ed67)

## Nodes:-

## Master Node (Control Plane):-

### kube-apiserver:
#### The API server is the entry point for all REST commands to the Kubernetes cluster. It processes REST operations, validates them, and updates the corresponding objects in etcd, which serves as the Kubernetes backing store for all cluster data.
### etcd: 
#### A distributed key-value store that stores the configuration data of the Kubernetes cluster, representing the overall state of the cluster at any given point.
### kube-scheduler: 
#### Watches for newly created Pods with no assigned node and selects a node for them to run on based on resource availability.
### kube-controller-manager:
#### Runs controller processes that regulate the state of the cluster, such as Node Controller, Replication Controller, Endpoints Controller, and Service Account & Token Controllers.
### cloud-controller-manager (Optional): 
#### Integrates with cloud provider-specific APIs to manage resources such as Load Balancers and Volumes.

## Worker Nodes (formerly known as "minions"):-

### Kubelet:
#### An agent that runs on each node and ensures that containers are running in a Pod. Listens for instructions from the control plane and manages the containers on the node.
### Container Runtime:
#### The software responsible for running containers. Kubernetes is designed to be compatible with various container runtimes, including Docker, containerd, and others.
### Kube-proxy:
#### Maintains network rules on nodes.Enables communication across the cluster and performs simple load balancing for service endpoints.
### Pods:
#### The smallest deployable units in Kubernetes.Represents a single instance of a running process in a cluster and can contain one or more containers.

## Explanation of Terminology:-
#### The term "Control Plane Components" collectively refers to API Server, etcd, Controller Manager, and Scheduler. The term "Worker Nodes Components" collectively refers to Kubelet, Container Runtime, Kube-proxy, and Pods.
#### In more traditional terms, the control plane components can be considered as the brains of the operation, making decisions and managing the overall state, while the worker nodes do the actual execution and run the containerized workloads.
#### It's essential to use the updated terminology and concepts to align with the evolving Kubernetes ecosystem. Always refer to the latest documentation for the most accurate and current information.
