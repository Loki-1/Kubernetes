## Kubernetes Metrics Server

#### The Kubernetes Metrics Server is a component within the Kubernetes ecosystem responsible for collecting resource metrics from your cluster's nodes and pods. Here's a description along with its advantages:

### Description:

The Kubernetes Metrics Server is a scalable, efficient solution for gathering resource utilization data from your Kubernetes cluster. It operates as an aggregator, collecting metrics from various sources such as the Kubelet API on each node. These metrics include CPU and memory usage, which are crucial for understanding the health and performance of your applications running on Kubernetes.

### Advantages:

**Real-time Monitoring:** Metrics Server provides real-time insights into the resource usage of your cluster, allowing you to promptly detect any anomalies or performance issues.

**Efficiency:** It's designed to be lightweight and efficient, minimizing the impact on cluster resources while still providing valuable monitoring data.

**Horizontal Scalability:** As your cluster grows, Metrics Server scales seamlessly to accommodate the increasing volume of metrics, ensuring consistent performance without manual intervention.

**Compatibility:** It's a core component of Kubernetes, designed to work seamlessly with other Kubernetes tools and components, making integration straightforward.

**Autoscaling:** Metrics Server integrates with Kubernetes Horizontal Pod Autoscaler (HPA), enabling you to automatically scale the number of pods based on resource utilization metrics collected by Metrics Server. This dynamic scaling ensures optimal resource allocation and efficient utilization of cluster resources.

**Resource Optimization:** By analyzing resource metrics collected by Metrics Server, you can identify underutilized resources and optimize your cluster's resource allocation for better efficiency and cost savings.

**Custom Metrics:** Metrics Server can be extended to collect custom metrics specific to your applications, providing deeper insights into their performance and behavior within the Kubernetes environment.

#### Overall, the Kubernetes Metrics Server plays a vital role in maintaining the health, performance, and efficiency of your Kubernetes cluster by providing real-time resource utilization metrics and enabling proactive management and optimization.

## Kubernetes Metrics Server Installation on cluster(kubeadm)

#### Open Official website github
```
https://github.com/kubernetes-sigs/metrics-server
```
#### Open Readme file and then you will get installation link - it is in format of kubectl apply , copy http link and wget that link to download
```
wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

ls -ltr

vi components.yaml #read the file and try to understand it 
```
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/70de6bba-5684-4359-8e48-07d1f33b3ee5)

#### now try to install metric server on cluster
```
kubectl apply -f components.yaml

kubectl get all -A        # to check it installed properlly or not
                          # It installed but not load properly we need to do some changes/configurations  on components.yaml
 
```

![image](https://github.com/Loki-1/Kubernetes/assets/134843197/6e911f2f-affb-4af9-88bc-f7798eb2eea0)


![image](https://github.com/Loki-1/Kubernetes/assets/134843197/de342bc8-a804-4c84-bee4-c21dfcdbfc8c)

![image](https://github.com/Loki-1/Kubernetes/assets/134843197/bc729d49-c989-40f2-9918-aa6eaf44435c)
