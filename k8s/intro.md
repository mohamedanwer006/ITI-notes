# K8S

## What is K8S?
K8S is a container orchestration system.

## K8S Architecture
- Master Node 
    - contain controls plane components
    - kube-apiserver
    - kube-controller-manager
    - ETCD 
    - kubelet - install using apt
    - kube-scheduler

> we can install them as container or using apt

- Worker Node
    - kubelet - install using apt
    - kube-proxy
    - container runtime

One master many worker node in cluster

There can be another master node as backup


----

### ETCD or ETCD Cluster
ETCD is a distributed key-value store database.
in master node

----

## kube-scheduler
it is choose only the node to run the pod

- first filter nodes cpu with available cpu 

- rank nodes put the node with most cpu 

----

## kubelet in worker node

big boss in worker node
- register node with master node

- create pods 

- monitor pods and resources

----

## kube-apiserver

big boss in master node

create pods => post Request to kube-apiserver => authenticate user => validate user => create objects in etcd and put details about pod in etcd => Response pod created   

kube-scheduler => listen to api server and it picks the node to run the pod => it also checks the resources of the node and if it is available then it tells the api server to run the pod on that node => apiserver calls kubelet to run the pod on that node

-----

### kubectl tool to manage k8s

mange many cluster in one terminal. change the config file of kubectl

```
apt-get install kubectl
```

```
kubectl get nodes
```
#### request to get all the nodes in the cluster
#### apiserver retrieves the list of nodes from etcd and returns it to kubectl


----

## controller-manager
one container contains multi controllers

- node-controller
- Deployment-controller
- ...
-----

### Node-controller

call api server to get the list of nodes

- node monitor period = 5 seconds
- node monitor grace period = 40 seconds
- POD Eviction grace period = 5 minutes


----

## kube-proxy
- mange network for nodes

- assign ip address to pods
- all server in cluster in on network

list pods
```
kubectl get pods

kubectl get po
```

```
minikube start --driver=docker
```

```
kubectl get po
```

----
## POD

- Contain the containers

- it can have multiple containers but with different name

- only one container called main container other called helper containers

----

## Example

Create Pod Nginx 
> imperative way using cli

```bash

kubectl run nginx --image=nginx 

kubectl get po

# name    ready   status
# nginx  0/1     ContainerCreating

# name    ready   status
# nginx  1/1     Running

kubectl describe po nginx

```

```
kubectl api-resources
```

> declarative way using yaml

```yaml
apiVersion: v1
kind: Pod

metadata: # define the metadata = dictionary
  name: nginx
  labels:
      app: myApp  # define the label its like a tag in ec2
      type: frontend
spec: # define the specification = list
    containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
            # helper containers 
        # - name: nginx2
        #   image: nginx
        #   ports:
        #     - containerPort: 80
```
### create pod file
#### first time create pod
```
kubectl create -f my-nginx.yaml
```
#### if change file and want to update the pod

```
kubectl apply -f my-nginx.yaml
```
----
## High Availability
load balancer & Scaling
### Replication Controller 
what number of replicas of a pod should be created

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: nginx-replicaset
  labels:
      app: replica-myApp
      type: replica-frontend
spec:
    template:
        metadata:
            labels:
                app: myApp 
                type: frontend
        spec:
            containers:
                - name: nginx
                  image: nginx
    replicas: 3 # define the number of replicas
    selector:
        matchLabels: # match pod labels to create
            app: myApp 

```


```
kubectl create -f my-nginx-replicaset.yaml
```

```
kubectl get replicaset
```

```
kubectl get po
```

### Update replicaset
```yaml
#...

spec:
    replicas: 5
#...

```

```bash
kubectl replace -f my-nginx-replicaset.yaml
kubectl apply -f my-nginx-replicaset.yaml
```

```bash
kubectl scale --replicas=5 -f nginx-replicaset.yml
```

## Delete replicaset
``` 
kubectl delete -f my-nginx-replicaset.yaml
```






