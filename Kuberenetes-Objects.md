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

### 2. Deployment
* It is used to manage the lifecycle of one or more identical Pods. 
* It allows to declaratively manage the desired state of your application, such as the number of replicas, the image to use for the Pods, and the resources required. 
* Deployment creates Pods that match template, using a ReplicaSet. A ReplicaSet is responsible for creating and scaling Pods, and for ensuring that Pods that fail are replaced.
* Deployment objects are used to manage stateless applications. 

### 3. ReplicaSets
* Deployments don’t manage Pods directly. That’s the job of the ReplicaSet object. 
* When we create a Deployment a ReplicaSet is created automatically. 
* It ensures that the desired number of replicas are running at all times by creating or deleting Pods as needed.  
* To accomplish this, Kubernetes uses a concept called reconciliation loops. A reconciliation loop is a process that compares    the desired state of a system with its current state and takes actions to bring the current state in line with the desired state.

### 4. StatefulSet
* StatefulSet object is used to manage stateful applications. 
* The word "state" means any stored data that the application or component needs to do its work. 
* This state is typically stored in a persistent storage backend, such as a disk, or a database. And the state is maintained even if the underlying Pods/containers are recreated. 
* StatefulSet ensures that each Pod is uniquely identified by a number, starting at zero. This allows for a consistent naming scheme for each Pod and its associated resources, such as persistent storage (for example, when Volumes are used).
* When a Pod in a StatefulSet must be replaced, due to node failure, the new Pod is assigned the same numeric identifier as the previous Pod. This ensures that the new Pod has the same unique identity and is attached to the same persistent storage (Volume) that the previous Pod was using.

### 5. DaemonSets
A DaemonSet ensures that a copy of a Pod is running across all, or a subset of nodes in a Kubernetes cluster.

DaemonSets are useful for running system-level services, such as logging or monitoring agents, that need to run on every node in a cluster. Logging agents are used to collect log data from all the nodes in a cluster and send it to a centralized logging system for storage and analysis. Monitoring agents are used to collect metrics and performance data from all the nodes in a cluster, and send it to a monitoring system for analysis and alerting.

Like ReplicaSets, DaemonSets are managed by a reconciliation loop. A reconciliation loop is a mechanism that continuously checks and compares the desired state of a resource with the current state. The loop runs periodically and ensures that the DaemonSet is always in the desired state, automatically creating or deleting Pods as necessary.

### 6. PersistentVolume
PersistentVolume represents a piece of storage that you can attach to your Pod(s).

The reason it's called "persistent" is because it's not tied to the life cycle of your Pod. In other words, even if your Pod gets deleted, the PersistentVolume will survive.

And there are a lot of different types of storage that you can attach using a PersistentVolume, like local disks, network storage, and cloud storage.

There are a few different use cases for PersistentVolumes in Kubernetes. One common use case is for databases. If you're running a database inside a Pod, you'll likely want to store the database files on a separate piece of storage that can persist even if the Pod gets deleted. And PersistentVolume can do that.

### 7. Service
A Kubernetes Service is a way to access a group of Pods that provide the same functionality. It creates a single, consistent point of entry for clients to access the service, regardless of the location of the Pods.

For example, imagine you have a Kubernetes cluster with multiple Pods running a web application. Each Pod has its own IP address, but this can change at any time if the Pod is moved to another node, or recreated. So the IP address becomes a "moving target". The destination(s) that clients should reach is unstable, and hard to track.

To make it easier for clients to access the web application, you can create a Kubernetes Service that has a stable IP address. Clients can then connect to that IP, and their requests will be routed to one of the Pods running the web application.

One of the key benefits of using a Service is that it provides a stable endpoint that doesn't change even if the underlying Pods are recreated or replaced. This makes it much easier to update and maintain the application, as clients don't need to be updated with new IP addresses.

Furthermore, the Service also provides some simple load balancing. If clients would connect to a certain IP address of a specific Pod, that Pod would be overused, while the other ones would be sitting idle, doing nothing. But the Service can spread out requests to multiple Pods (load balance). By spreading these out, all Pods are used equally. However, each one has less work to do, as it only receives a small part of the total number of incoming requests.

### 8. Namespaces
A Kubernetes namespace is a way to divide a single Kubernetes cluster into multiple virtual clusters. This allows resources to be isolated from one another. Once a namespace is created, you can launch Kubernetes objects, like Pods, which will only exist in that namespace.

For example, imagine you have a Kubernetes cluster running two applications, "AppA" and "AppB". To keep things organized, you create two namespaces, "AppA-Namespace" and "AppB-Namespace".

Now, when you deploy Pods for "AppA", you can do so within the "AppA-Namespace". Similarly, when you deploy Pods for "AppB", you can do so within the "AppB-Namespace". What's a possible use case?

Well, imagine AppA, and AppB are almost identical. One is version 1.16 of an app, the other is version 1.17. They use almost the same objects, the same Pod structures, the same Services, and so on. Since they're so similar, there's a risk of them interfering with each other. For example, AppA might accidentally send requests to a similar Service or Pod used by AppB. But you want to test the new 1.17 version in a realistic scenario in the same cluster, using the same objects and definitions.

By using namespaces, you can perform as many operations as you need while eliminating the risk of impacting resources that are in another namespace. It's almost as if you have a second Kubernetes cluster. AppA runs in its own (virtual) cluster. AppB runs in a separate (virtual) cluster. But you don't actually have to go through the trouble of setting up an additional cluster. AppA and AppB are logically isolated from each other when they exist in separate namespaces. Even if they run identical Pods that want to access Services with identical names, there's no risk of them interfering with each other.

### 9-10. ConfigMaps & Secrets
ConfigMaps and Secrets are two very important objects that allow you to configure the apps that run in your Pods. Configuring apps refers to setting various parameters or options that control the behaviors of the apps. This can include things like database connection strings or API keys.

ConfigMaps are used to store non-sensitive configuration values. For example, environment variables used to provide runtime configuration information such as the URL of an external API, rather than confidential information such as passwords, are considered non-sensitive data.

Secrets, on the other hand, are meant to hold sensitive configuration values, such as database passwords, API keys, and other information that only authorized apps should be able to access.

ConfigMaps and Secrets can be injected into Pods with the help of environment variables, command-line arguments, or configuration files included in the volumes attached to those Pods.

By using ConfigMaps and Secrets, you decouple the applications running in your Pods from their configuration values. This means you can easily update the configuration of your applications without having to rebuild or redeploy them.

### 11. Job
A job object is used to run specific tasks that have the following properties:

They are short-lived.
They need to be executed once.
But most importantly, Kubernetes has to make sure that the task has been executed correctly and finished its job.
An example of where a Kubernetes job can be useful is a database backup. This does not run continuously, so it's a short-lived process. It just needs to start, complete, then exit. It has to be executed once. Even if the backup needs to happen weekly, there will be one separate job per week (CronJobs can be used for jobs that repeat periodically).

Each job has to finish its own weekly task. But it needs to ensure that the Pod executing the backup fully completes this job. For example, a Pod might start a backup. But the database is huge, so this can take hours. For some reason, the backup process fails at 68% progress. Kubernetes sees that the Pod has failed the task, so it can create another one to retry. It will keep on retrying until, finally, one Pod manages to back up the entire database.

In this case, the job required one Pod, and one successful run for one Pod (to ensure the task was completed successfully). But other jobs might require multiple Pods (which can run in parallel) and/or multiple successful runs. For example, a job will be considered complete, only when at least 3 Pods had successful runs and achieved 100% progress on their tasks.

## How to create Pods

### Examples of Pod creation

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
