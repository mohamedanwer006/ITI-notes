# Commands

```Dockerfile
FROM ubuntu
CMD sleep 5


FROM ubuntu
Entrypoint sleep

# docker run  ubuntu-sleeper 10
# concatenation with entrypoint

FROM ubuntu
Entrypoint sleep
CMD["5"]
# docker run  ubuntu-sleeper 10 =>override CMD


# override entrypoint
# docker run --entrypoint sleep2.0 ubuntu-sleeper 10

# command at start up : sleep2.0 10
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
    labels:
        app: nginx
spec:
    containers:
    - name: ubuntu-sleeper
    image: nginx
    command: ["sleep2.0"] # override entrypoint
    args: ["10"]      # override CMD

# or we can use command: ["sleep2.0", "10"] # override entrypoint and CMD
```

# Resources

any container created will have resources like CPU, Memory, Disk, Network, etc.

default resources it start with :

- 0.5 CPU
- 256Mib Memory

```yaml
spec:
  containers:
    - name: nginx
    image: nginx
    resources:
        cpu: "100m" # 100 milli-core CPU (0.1 core)
        1000 milli-core CPU (1 core)
        memory: "512Mi"
```

defaults limits:

- 1 CPU
- 512Mi Memory

```yaml
spec:
  containers:
    - name: nginx
    image: nginx
    resources:
        requests:
            cpu: 1 # 100 milli-core CPU (0.1 core)
            1000 milli-core CPU (1 core)
            memory: "1Gi"
        limits:
            cpu: 2 # 100 milli-core CPU (0.1 core)

            memory: "2Gi"
```

# if he get above limits

- cpu:
- still work but will be throttled

- memory:
  he will get memory
  but after it will terminate status (oom) out of memory

```
kubectl top pod <pod-name>
kubectl top node
kubectl add-ons enable metrics-server
```

```
restart policy:

by default it is always
```

```
if you  specify limits but don't specify requests

it will use requests as limits
```

# Namespace

cluster have multiple namespaces

```bash
kubectl get namespaces
kubectl get ns

# default namespace is default
# kube-system namespace is for system components like kube-dns, kube-proxy, etc.
# kube-public namespace is for public components


kubectl get pods -n <nam space>
kubectl get pods -n kube-system

# if don't specify namespace it will get pods from default namespace


# get pods from all namespaces

kubectl get pods --all-namespaces


```

# create namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
    labels:
        app: my-app

```

```bash

    kubectl create ns my-namespace
    kubectl get namespaces

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  nampespace: my-namespace
  name: nginx-pod
    labels:
        app: nginx
spec:
    containers:
    - name: ubuntu-sleeper
    image: nginx
    command: ["sleep2.0"] # override entrypoint
    args: ["10"]      # override CMD

```

# Change default namespace

```
kubectl config set-context $(kubectl config current-context) --namespace=<namespace-name>

```

# Resources quota

#### give resource quota to namespace

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: 4
    requests.memory: 5Gi
    limits.cpu: 10
    limits.memory: 10Gi
```

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-quota # name of quota (quota name) to use in command
spec:
  hard:
    pods: "10"
    requests.cpu: 4
    requests.memory: 5Gi
    limits.cpu: 10
    limits.memory: 10Gi
```

```
kubectl create ns dev
kubectl apply -f quota.yaml --namespace=dev
```

```
kubectl get resourcequota myquota --namespace=dev --output=yaml
```

# Limits range

how to create default limmit to pods created with out create it in pod itself


```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: my-limit-range
  namespace: dev
spec:
    limits:
        - default: # limits
            memory: "500Mi"
        defaultRequest: # request
            memory: "265Mi"
        type: Container

```

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: my-limit-range
  namespace: dev
spec:
    limits:
        - min: # minimum resources
            memory: "300Mi"
          max: # maximum resources
            memory: "800Mi"
        type: Container

```

# Taints and Toleration
```
choose which node you want to run pod on

schedular => create pod A choose node to run pod on it

taint is condition like label or annotation

toleration is condition same as taint but it is for pod

if taint - node == Toleration - pod A

put pod A on the node 

if there is no toleration then schedular will choose node randomly
```

# create taint for node

```md
kubectl taint nodes <node-name> <key>=<value>:<effect>

effect: 
- NoSchedule,
- NoExecute,
- PreferNoSchedule

> effect tells schedular how to handle this taint on the node

- noSchedule: pod should have toleration to be scheduled on this node 

- preferNoSchedule: pod should have toleration to be scheduled on this node but if the other node doesn't have space then schedular will choose this node


- noExecute: pod should have toleration to be scheduled on this node . after create taint if there is pods in the node but they don't have toleration then they will be removed from the node


```

```
kubectl taint nodes <node-name> <key>=<value>:<effect>
kubectl taint nodes node1 app=blue:NoSchedule

```

```yaml
apiVersion: v1
kind: pod
metadata:
  name: my-pod
  namespace: dev
spec:
  containers:
  - name: ubuntu-sleeper
    image: nginx     
    tolerations:
    - key: app
      operator: Equal # or Exists
      value: blue
      effect: NoSchedule
        
```

```
kubectl describe node minikube |grep Taints

Taints: <none>

so we can add pod  
because the muster and worker nodes on the same machine
```

# manual scheduling

apiserver when get data and find nodeName it will send it to node and doesn't ask schedular

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: dev
spec:
  containers:
  - name: ubuntu-sleeper
    image: nginx     
  nodeName: node02
```


# Node Selector

want to put pod on specific node

  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: ubuntu-sleeper
    image: nginx
  nodeSelector:
    size: Large # label

```

create label for node

```
kubectl label nodes <node-name> <key>=<value>
```

Select large or medium node
## Node Affinity
```yaml
# ...
spec:
    containers:
    #...
    affinity:
      nodeAffinity:
    # pod tell schedular to put pod on node with label size=large or medium
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In # Exists , NotExists , In , NotIn
              values:
              - large
              - medium

```

```
requiredDuringScheduling

pod don't run on node without label size=large or medium

IgnoredDuringExecution:

pod create on node size=large or medium 
if i remove label from node 
the pod still run on node


preferredDuringSchedulingRequiredDuringExecution:
Another option plain to add

same as above but will delete pod if the label removed
```