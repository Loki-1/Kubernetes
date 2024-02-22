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

#### check official documents on github for configuration


I took screenshot of that configurations through official document

![image](https://github.com/Loki-1/Kubernetes/assets/134843197/1fa187f6-9100-49eb-84cd-2ff31f0317f5)

I paste that configuration flag in components.yaml

```
vi components.yaml

```
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/de342bc8-a804-4c84-bee4-c21dfcdbfc8c)

#### Installation recheck

Now re-enter below commands

```
kubectl get all -A        # to check it installed properlly or not
                          # It will terminate previous installed metrics server pod and create new one - and its working fine
```
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/bc729d49-c989-40f2-9918-aa6eaf44435c)

#### Checking node metrics and pod metrics

```
Kubectl top nodes # for node metrics

kubectl top pods  # for pod metrics
```
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/355fd804-ae7a-4e17-b9e5-dde4736b404c)
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/2e7c498b-3cf4-4819-8177-c2da0dd4fe44)

