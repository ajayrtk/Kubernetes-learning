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



## Example of Labels
```
kind: Pod
apiVersion: v1
metadata:
  name: labelpod
  labels:                                                   
    env: development
    class: pods
spec:
    containers:
      - name: container01 
        image: ubuntu
        command: ["/bin/bash", "-c", "while true; do echo Welcome to Label Pod in container01; sleep 5 ; done"]
```

```
vi lablepod01.yaml
kubectl apply -f lablepod01.yaml
kubectl get pods -o wide
kubectl get pods --show-labels
--imperative format
kubectl label pods labelpod myname=John
kubectl get pods --show-labels

kubectl get pod -l env=development
kubectl get pod -l env!=development

kubectl delete pod -l env!=development
kubectl get pod -l 'env in (development,testing)'
kubectl get pod -l 'env notin (development,testing)'
```

## Example of Node Selector
```
kind: Pod
apiVersion: v1
metadata:
  name: nodelabels
  labels:
    env: development
spec:
  containers:
  - name: container02
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo Welcome to nodelabels Pod in container 02; sleep 5 ; done"]
    nodeSelector:                                         
      hardware: t2-medium
```
