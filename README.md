# Kubernetes
## Kubernetes is a container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. A Kubernetes cluster consists of a set of nodes that work together to run containerized applications. 
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/69b5e016-0048-4fb4-8197-c079cfe1ed67)

### Nodes:

Master Node (Control Plane):

kube-apiserver: The API server is the entry point for all REST commands to the Kubernetes cluster. It processes REST operations, validates them, and updates the corresponding objects in etcd, which serves as the Kubernetes backing store for all cluster data.
etcd: A distributed key-value store that stores the configuration data of the Kubernetes cluster, representing the overall state of the cluster at any given point.
kube-scheduler: Watches for newly created Pods with no assigned node and selects a node for them to run on based on resource availability.
kube-controller-manager: Runs controller processes that regulate the state of the cluster, such as Node Controller, Replication Controller, Endpoints Controller, and Service Account & Token Controllers.
cloud-controller-manager (Optional): Integrates with cloud provider-specific APIs to manage resources such as Load Balancers and Volumes.
