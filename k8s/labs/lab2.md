# -------------------------1---------------------

```
└─ mohamed@DevOps:$ kubectl get ns
NAME              STATUS   AGE
default           Active   26h
kube-node-lease   Active   26h
kube-public       Active   26h
kube-system       Active   26h
```

# ----------------------2----------------------

```
$ echo $(( $(kubectl get po --namespace=kube-system | wc -l ) -1 ))

7
```
#### 7 pods in kube-system namespace


```
─ mohamed@DevOps:$ kubectl get po --namespace=kube-system
NAME                               READY   STATUS    RESTARTS      AGE
coredns-64897985d-2jd2n            1/1     Running   1 (24h ago)   26h
etcd-minikube                      1/1     Running   1 (42m ago)   26h
kube-apiserver-minikube            1/1     Running   1 (24h ago)   26h
kube-controller-manager-minikube   1/1     Running   1 (24h ago)   26h
kube-proxy-lfzz2                   1/1     Running   1 (24h ago)   26h
kube-scheduler-minikube            1/1     Running   1 (42m ago)   26h
storage-provisioner                1/1     Running   3 (41m ago)   26h

```

# ----------------------3----------------------

```
└─ mohamed@DevOps:$ kubectl create ns finance
namespace/finance created
```

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: beta
  namespace: finance
spec:
    replicas: 2
    selector:
        matchLabels:
          app: redis-app      
    template:
        metadata:
            labels:
                app:  redis-app   
        spec:
            containers:
            - name:  redis-container
              image: redis
              resources:
                limits:
                  cpu: 1
                  memory: 2Gi
                requests:
                  cpu: 500m
                  memory: 1Gi

```

```
└─ mohamed@DevOps:$ kubectl create -f redis-dep.yml 
deployment.apps/beta created
```

```
└─ mohamed@DevOps:$ kubectl get deploy --namespace=finance
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
beta   2/2     2            2           37s
```

# ----------------------4----------------------

```
└─ mohamed@DevOps:$ kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   26h   v1.23.3
```
# ----------------------5----------------------

#### 5- Do you see any taints on master? NO

```
└─ mohamed@DevOps:$ kubectl describe node minikube |grep Taints
Taints:             <none>
```

# ----------------------6----------------------
```
$ kubectl label nodes minikube color=blue
```
# ----------------------7----------------------

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-deploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: blue
  template:
    metadata:
      labels:
        app: blue
    spec:
      containers:
        - name: blue-container
          image: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```

```
─ mohamed@DevOps:$ kubectl get deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
blue-deploy   2/2     2            2           152m
```