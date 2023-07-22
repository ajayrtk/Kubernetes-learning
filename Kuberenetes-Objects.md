# Kubernetes Objects

They provide detailed instructions to Kubernetes on how the applications must be set up and managed.

For example, a **Deployment** object might specify that you want three replicas of some application running at all times, while a **Service** object might define how you want to expose your application to the internet. 

Kubernetes then takes these instructions and automatically configures and manages the application accordingly, ensuring that it always matches the desired state.

Kubernetes uses objects to represent the state of the cluster. All objects are identified by a uniques name and a UID.

## Types of Objects in Kubernetes
There are many different types of Kubernetes objects.

### 1. Pods
* They are used to deploy, scale, and manage containerized applications in a cluster.
* A Pod can host a single container or a group of containers that need to "sit closer together". By grouping two or more containers in a single Pod, they will be able to communicate much faster and share data more easily. 
* When a Pod contains multiple containers, all of the containers always run on a single worker node. They never span across multiple worker nodes.
* Pods is that they are ephemeral in nature. This means that they are not guaranteed to have a long-term lifespan. They can be created, destroyed, and recreated at any time if required.





```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod1                
spec:                                    
  containers:                      
    - name: container01                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Welcome to testpod1 container01; sleep 5 ; done"]
  restartPolicy: Never         # Defaults to Always
```

### Create a pod with the help of yaml file

```
kubectl apply -f pod1.yaml
```

### Get the details of the pod with more columns

```
kubectl get pods -o wide 
```

### Get the detail description of the pod

``` 
 kubectl describe pod testpod1
```

### Get the logs of the container

```
 kubectl logs -f testpod1
```

### Get the logs of the specific container if multiple container is running inside the pod

``` 
 kubectl logs -f testpod1 -c container01
```
### Delete a container
```
kubectl delete pods testpod1
kubectl delete -f pod1.yaml

```

### Creating a container with annotation
```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod02
  annotations:
    description: My first annotation container                
spec:                                    
  containers:                      
    - name: container02                    
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Welcome to testpod02 container02; sleep 5 ; done"]
  restartPolicy: Never         # Defaults to Always
```

``` 
 kubectl describe pod testpod02
 kubectl delete pod testpod02
 kubectl delete pod2.yaml
```

### Create multiple containers

```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod3                 
spec:                                    
  containers:   
  - name: container03                    
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Welcome to testpod3 container03; sleep 5 ; done"]                   
    - name: container04                   
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Welcome to testpod03 container04; sleep 5 ; done"]
```

```
kubectl apply -f pod3.yaml
kubectl describe pod testpod3
kubectl exec testpod3 -c container03 -- hostname -i
kubectl exec testpod3 -c container04 -- hostname -i
kubectl exec testpod3 -it -c container03 -- /bin/bash
#ps
#ps -ef
```
### Creating a pod with environment variable

```
kind: Pod
apiVersion: v1
metadata:
  name: testpod4
spec:
  containers:
    - name: container05
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Welcome to testpod04 container05; sleep 5 ; done"]
      env:                        # List of environment variables to be used inside the pod
      - name: MYNAME
        value: Ajay Rawat
```

```
kubectl get pods
kubectl describe pod testpod4
kubectl exec testpod4 -it -- /bin/bash
#env
# $MYNAME
```

```
kind: Pod
apiVersion: v1
metadata:
  name: testpod5
spec:
  containers:
    - name: container06
      image: httpd
      ports:
       - containerPort: 80  
```

```
kubectl get pods
kubectl get pods -o wide
curl 10.244.0.32:80
```
## History

* The name Kubernetes originates from Greek, meaning helmsman or pilot.
* Google developed an internal system called 'Borg' (later named as 'Omega') to deply and manage thousands Google applications 
 and services on this cluster.
* In 2014, Google introduced Kubernetes an open source platform written in Golang and later denoated to CNCF.

## Online platforms for K8s

* Kunernetes playground
* Play with K8s
* Play with Kubernetes classroom

## Cloud based K8s services

* GKE - Google Kubernetes services
* AKS - Azure Kubernetes services
* Amazon EKS - Elastic Kubernetes services

## Kubernetes Installation Tools

* Minicube
* Kubeadm
* Kind

## Problems with Scaling up the Containers

* Containers cannot communicate with each other
* Autoscaling and laod balancing was not possible
* Contianers had to be managed carefully

## Feature of Kubernetes

_Here are the essential Kubernetes features:_

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

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. 

![My Image](images/kubernetes-architecture.jpg)

### Control Plane Components

The Kubernetes control plane manages clusters and resources such as worker nodes and pods. The control plane receives information such as cluster activity, internal and external requests. 

It ensures that every component in the cluster is kept in the desired state. It receives data about internal cluster events, external systems, and third-party applications, then processes the data and makes and executes decisions in response.

The control plane manages and maintains the worker nodes that hold the containerized applications. The control plane not only exposes the layer that deploys the containers, but also manages their lifecycle. 

There are several key parts to the control plane:

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
* 
Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.







Control plane component that runs controller processes.

Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

There are many different types of controllers. Some examples of them are:

Node controller: Responsible for noticing and responding when nodes go down.
Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
EndpointSlice controller: Populates EndpointSlice objects (to provide a link between Services and Pods).
ServiceAccount controller: Create default ServiceAccounts for new namespaces.

#### Master Node




* Should point to documentation on first mentio
  [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/),
  [pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/),
  [services](https://kubernetes.io/docs/concepts/services-networking/service/),
  [deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/),
  [replication controllers](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/),
  [jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/),
  [labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/),
  [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/),
  etc.
* Most examples should be cloudprovider-independent (e.g., using PVCs, not PDs).
  * Other examples with cloudprovider-specific bits could be somewhere else.
* Actually show the app working -- console output, and or screenshots.
  * Ascii animations and screencasts are recommended.
* Follows [config best practices](https://kubernetes.io/docs/concepts/configuration/overview/).
* Shouldn't duplicate the [user guide](https://kubernetes.io/docs/home/).
* Docker images are pre-built, and source is contained in a subfolder.
  * Source is the Dockerfile and any custom files needed beyond the
    upstream app being packaged.
  * Images are pushed to `gcr.io/google-samples`. Contact @jeffmendoza
    to have an image pushed
  * Images are tagged with a version (not latest) that is referenced
    in the example config.
* Only use the code highlighting types
  [supported by Rouge](https://github.com/jneen/rouge/wiki/list-of-supported-languages-and-lexers),
  as this is what GitHub Pages uses.
* Commands to be copied using the `shell` syntax highlighting type, and
  do not include any kind of prompt.
* Example output is in a separate block quote to distinguish it from
  the command (which doesn't have a prompt).
* When providing an example command or config for which the user is
  expected to substitute text with something specific to them, use
  angle brackets: `<IDENTIFIER>` for the text to be substituted.

### At the end

* Should have a section suggesting what to look at next, both in terms
  of "additional resources" and "what example to look at next".


<!-- BEGIN MUNGE: GENERATED_ANALYTICS -->
[![Analytics](https://kubernetes-site.appspot.com/UA-36037335-10/GitHub/examples/guidelines.md?pixel)]()
<!-- END MUNGE: GENERATED_ANALYTICS -->
