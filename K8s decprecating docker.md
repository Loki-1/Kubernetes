# Kubernetes is Deprecating Docker?!
#### When people think of containers, they think of Docker and Kubernetes. Docker has been the big name when it comes to building and running containers, and Kubernetes has been the big name when it comes to managing and orchestrating them. It might seem a bit shocking to hear that Kubernetes is deprecating support for Docker as a container runtime starting with Kubernetes version 1.20.
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/54c5416f-62ab-4cd6-b7da-5dad310a4f1d)

# What’s Changing with Docker?
### • Kubernetes deprecating Docker is actually not as big of a deal as it sounds.
#### Kubernetes is removing support for Docker as a container runtime. Kubernetes does not actually handle the process of running containers on a machine. Instead, it relies on another piece of software called a container runtime.
  ![image](https://github.com/Loki-1/Kubernetes/assets/134843197/84620e98-27bc-4a72-bb29-455350612b41)
  
##### • The container runtime runs containers on a host, and Kubernetes tells the container runtime on each host what to do. • You can actually choose from a variety of options when it comes to what software you want to use as your container runtime when running Kubernetes. • Up to now, popular option was to use Docker as the container runtime.
#### • However, this(Docker) will no longer be an option as container run time for Kubernetes in the future.

### Why is Kubernetes Deprecating Docker?
#### Docker does not run containers directly. It simply creates a more human-accessible and feature-rich interface on top of a separate, underlying container runtime. 
### When it is used as a container runtime for Kubernetes, Docker is just a middle- man between Kubernetes and containerd.
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/614592cc-c44d-4da9-9a33-95b0ca95aba1)
### However, Kubernetes can use containerd directly as a container runtime, meaning Docker is no longer needed in this middle-man role.
![image](https://github.com/Loki-1/Kubernetes/assets/134843197/84620e98-27bc-4a72-bb29-455350612b41)
### What’s the Role of Docker Going Forward?
#### • Although Docker is not needed as a container runtime in Kubernetes, it still has a role to play in the Kubernetes ecosystem, and in your workflow.
#### • Docker is still going strong as a tool for developing and building container images, as well as running them locally.
#### • Kubernetes can still run containers built using Docker’s Open ContainerInitiative (OCI) image format, meaning you can still use Dockerfiles and build your container images using Docker.
#### • Kubernetes will also continue to be able to pull from Docker registries (such as Docker hub). This means that Docker will remain a powerful contender when it comes to managing the images once they are built.
#### • Docker will continue to be a useful tool on your development workflows and continuous integration (CI) systems, even if you don’t need it to run your containers underneath in Kubernetes.
