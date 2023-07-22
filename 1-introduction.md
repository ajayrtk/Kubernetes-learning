# What is Kubernetes
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. The open source project is hosted by the Cloud Native Computing Foundation (CNCF).

It schedules, runs and mananges isloated containers which are running on virtual | physical | cloud machnies.

## History
<img src="images/kubernetes_history.jpg" width=85% height=85%>

* The word Kubernetes is Greek for pilot or helmsman, the person who steers the ship.
* Google developed an internal system called 'Borg' (later named as 'Omega') to deply and manage thousands Google applications 
 and services on this cluster.
* In 2014, Google introduced Kubernetes an open source platform written in Golang and later denoated to CNCF.

## Online platforms for K8s
* Play with K8s
* Kunernetes playground
* Play with Kubernetes classroom

## Managed Kubernetes Cluster in the Cloud
The top managed Kubernetes offerings include the following:

* Google Kubernetes Engine (GKE)
* Azure Kubernetes Service (AKS)
* Amazon Elastic Kubernetes Service (EKS)
* IBM Cloud Kubernetes Service
* Red Hat OpenShift Online and Dedicated
* VMware Cloud PKS
* Alibaba Cloud Container Service for Kubernetes (ACK)

## Problems with Scaling up the Containers
* Containers cannot communicate with each other
* Autoscaling and laod balancing was not possible
* Contianers had to be managed carefully

## Feature of Kubernetes
Here are the essential Kubernetes features:

* Orchestration (cluster of any number of container running on different network)
* Automated Scheduling
* Self-Healing Capabilities
* Automated rollouts & rollback
* Horizontal Scaling 
* Load Balancing
* Fault tolerance (Node | Pod failure)
* Health monitoring of containers
* Offers enterprise-ready features
* Application-centric management
* Auto-scalable infrastructure
* Service Discovery
* Platform independent (cloud | virtual | physical)
* Secret and Configuration Management
* Offers environment consistency for development, testing, and production
* Infrastructure is loosely coupled to each component can act as a separate unit

## Kubernetes vs Docker Swarm
|Features | Kubernetes | Docker Swarm |
------------- | ------------- | ------------ | 
| Installation and Cluster configuration | Complicated and Time consuming | Fast and easy |
| Supports | Works with all container types - Rocket, Docker, ContainerD | Works with Docker only | 
| GUI | Available | No GUI |
| Data Volumes | Only shared with containers win same Pod | Can be shared with any other Container |
| Updates and Rollbacks | Process scheduling to maintain services while updating | Progressive updates of servies health monitoring throught the update|
| Autoscaling | Vertical and Horizontal | No support|
| Logging and Monitoring | Inbuitl tool present |  Use third party like Splunk


## Kubernetes Architecture
A set of master nodes that host the Control Plane components, which are the brains of the system, since they control the entire cluster. The control plane manages the worker nodes and the Pods in the cluster. 

<img src="images/kubernetes-architecture.jpg" width=80% height=80%>

### Control Plane Components
<img src="images/two_planes.jpg" width=80% height=80%>

The Control Plane manages clusters and resources such as worker nodes and pods. The control plane receives information such as cluster activity, internal and external requests. 

It ensures that every component in the cluster is kept in the desired state. It receives data about internal cluster events, external systems, and third-party applications, then processes the data and makes and executes decisions in response.

The control plane manages and maintains the worker nodes that hold the containerized applications. The control plane not only exposes the layer that deploys the containers, but also manages their lifecycle. 

There are several key parts to the control plane:
<img src="images/control_plane.jpg" width=80% height=80%>

* kube-apiserver - An API server that transmits data both within the cluster and with external services
* kube-scheduler - A scheduler that handles resource sharing among the nodes
* kube-controller-manager - A controller manager that watches the state of the nodes
* etcd - A persistent data store to keep configurations
* cloud-controller-manager - A controller manager and a cloud controller manager to manage control loops

1. **kube-apiserver**
* Central hub of the Kubernetes cluster that exposes the Kubernetes API
* Only way to interact with a running Kubernetes cluster
* Can issue commands to the API server using the Kubectl CLI or an HTTP client
* As front end of the Kubernetes API, it serves as the access point for client requests
* Designed to scale horizontally — scales by deploying more instances
* The communication between the API server and other components in the cluster happens over TLS to prevent unauthorized access to the cluster
* API management: Exposes the cluster API endpoint and handles all API requests.
* Authentication (Using client certificates, bearer tokens, and HTTP Basic Authentication) and Authorization (ABAC and RBAC and 
  evaluation)
* Processing API requests and validating data for the API objects like pods, services, etc. (Validation and Mutation Admission controllers)
* It is the only component that communicates with etcd
* Coordinates all the processes between the control plane and worker node components

2. **kube-scheduler**
* Watches newly created Pods with no assigned node, and selects a node for them to run on.
* A special type of controller, whose only task is to schedule application instances onto worker nodes. 
* It selects the best worker node for each new application instance object and assigns it to the instance.
* Its task is to schedule pods onto nodes and evaluate the requirement of each pod.
* After the evaluation is complete it has to select the most suitable node.
* It also keeps a track of the state of all pods.

3. **kube-controller-manager**
* It continuously monitors the state of the cluster via the kube API server.
* When the current state does not match the desired state, it makes changes to achieve the desired state.
* The controller also communicates with important information if a node goes offline.
* There are many different types of controllers. Some examples of them are:
  * Node controller: Responsible for noticing and responding when nodes go down.
  * Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
  * EndpointSlice controller: Populates EndpointSlice objects (to provide a link between Services and Pods).
  * ServiceAccount controller: Create default ServiceAccounts for new namespaces.

4. **etcd**
* It is distributed key-value store database.
* It reliably stores the state of the cluster, including all information with regard to cluster configuration and more dynamic   
  information like what nodes need to be running, etc.
* Kube API server interacts directly with etcd.
* It uses raft consensus algorithm for strong consistency and availability. 

5. **cloud-controller-manager**
* It runs controllers that interact with the underlying cloud providers.
* It allows the cloud vendor’s code and the Kubernetes code to evolve independently of each other. 
* It’s responsible for features like load balancing, storage volumes as and when required.

## Workload Plane
<img src="images/data_plane.jpg" width=80% height=80%>

Application run on the worker nodes. Workload Plane is sometimes referred to as the Data Plane. A set of worker nodes that form the Workload Plane, which is where your workloads (or applications) run. The worker node(s) host the Pods that are the components of the application workload. 

In addition to applications, several Kubernetes components also run on these nodes. They perform the task of running, monitoring and providing connectivity between your applications.

1. **kublets**

* Kubelet is an agent component that runs on every node in the cluster. 
* It does not run as a container instead runs as a daemon, managed by systemd.
* It is responsible for registering worker nodes with the API server and working with the podSpec from the API server. 
* It brings the podSpec to the desired state by creating containers.
* kubelet is responsible for the following:
  * Creating, modifying, and deleting containers for the pod.
  * Responsible for handling liveliness, readiness, and startup probes.
  * Responsible for Mounting volumes by reading pod configuration and creating respective directories on the host for the volume 
    mount.
  * Collecting and reporting Node and pod status via calls to the API server.
* Kubelet is also a controller where it watches for pod changes and utilizes the node’s container runtime to pull images, run containers.
* Following are some of the key things about kubelet.
  * Kubelet uses the CRI (container runtime interface) gRPC interface to talk to the container runtime.
  * It also exposes an HTTP endpoint to stream logs and provides exec sessions for clients.
  * Uses the CSI (container storage interface) gRPC to configure block volumes.
  * It uses the CNI plugin configured in the cluster to allocate the pod IP address and set up any necessary network routes and
    firewall rules for the pod.

<img src="images/kubelet.jpg" width=65% height=50%>

2. **Container Runtime**

3. **Kube Proxy**

## How Kubernetes runs an application
<img src="images/deploying_application.jpg" width=85% height=85%>

These actions take place when you deploy the application:
1. You submit the application manifest to the Kubernetes API. The API Server writes the objects defined in the manifest to etcd.
2. A controller notices the newly created objects and creates several new objects - one for each application instance.
3. The Scheduler assigns a node to each instance.
4. The Kubelet notices that an instance is assigned to the Kubelet’s node. It runs the application instance via the Container   
  Runtime.
5. The Kube Proxy notices that the application instances are ready to accept connections from clients and configures a load balancer for them.
6. The Kubelets and the Controllers monitor the system and keep the applications running.

## Deploying a Kubernetes cluster

### Kubernetes running in Docker Desktop
<img src="images/docker_desktop.jpg" width=80% height=80%>

### Running a single-node Kubernetes cluster using Minikube
<img src="images/minikube.jpg" width=80% height=80%>

### Running a multi-node Kubernetes cluster using kind
<img src="images/kind.jpg" width=80% height=80%>



#### Master Node