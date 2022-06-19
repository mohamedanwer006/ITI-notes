# -------------------1---------------------


```
└─ mohamed@DevOps:$ kubectl run redis --image=redis
pod/redis created
```

```yaml
# my-redis.yaml
apiVersion: v1
kind: Pod
metadata: 
  name: redis
  labels:
      app: redis 
      type: db
spec: 
    containers:
        - name:
         redis
          image: redis:latest
```
# ------------------- 2 ---------------------

```yaml
# my-nginx.yaml
apiVersion: v1
kind: Pod
metadata: 
  name: nginx
  labels:
      app: app-nginx 
      type: server
spec: 
    containers:
        - name: nginx
          image: nginx123
          # image: nginx
```

NAME    READY   STATUS             RESTARTS   AGE
nginx   0/1     ImagePullBackOff   0          28s

## `kubectl apply -f my-nginx.yml` 

```
└─ mohamed@DevOps:$ kubectl get  po
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          113s
redis   1/1     Running   0          7m2s
```

# ------------------- 6 ---------------------

```
└─ mohamed@DevOps:$ kubectl get rs
No resources found in default namespace.
```
# ------------------- 7 ---------------------

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: replica-set-1
  labels:
      app: replica-myApp
spec:
    template:
        metadata:
            labels:
                app: my-busybox 
        spec:
            containers:
                - name: my-busybox
                  image: busybox
                  command: ['sh', '-c', 'echo The app is running! && sleep 55600']
    replicas: 3 
    selector:
        matchLabels:
            app: my-busybox 
```

## kubectl create -f my-replica.yml 
replicaset.apps/replica-set-1 created

```
└─ mohamed@DevOps:$ kubectl get  po
NAME                  READY   STATUS    RESTARTS   AGE
nginx                 1/1     Running   0          19m
redis                 1/1     Running   0          24m
replica-set-1-g8bss   1/1     Running   0          9s
replica-set-1-p9dk8   1/1     Running   0          9s
replica-set-1-rdb9t   1/1     Running   0          9s
```
# ------------------- 8 ---------------------
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: replica-set-1
  labels:
      app: replica-myApp
spec:
    template:
        metadata:
            labels:
                app: my-busybox 
        spec:
            containers:
                - name: my-busybox
                  image: busybox
                  command: ['sh', '-c', 'echo The app is running! && sleep 55600']
    replicas: 5 
    selector:
        matchLabels:
            app: my-busybox 
```
## `kubectl apply -f my-replica.yml`
```
└─ mohamed@DevOps:$ kubectl get  po
NAME                  READY   STATUS    RESTARTS   AGE
nginx                 1/1     Running   0          22m
redis                 1/1     Running   0          27m
replica-set-1-82hct   1/1     Running   0          8s
replica-set-1-g8bss   1/1     Running   0          2m52s
replica-set-1-p9dk8   1/1     Running   0          2m52s
replica-set-1-qgctd   1/1     Running   0          8s
replica-set-1-rdb9t   1/1     Running   0          2m52s
```

# ------------------- 9 ---------------------

# 5 pods are ready and running

# ------------------- 10 ---------------------

```
└─ mohamed@DevOps:$ kubectl get po
NAME                  READY   STATUS        RESTARTS   AGE
nginx                 1/1     Running       0          25m
redis                 1/1     Running       0          30m
replica-set-1-82hct   1/1     Terminating   0          2m51s
replica-set-1-fvg26   1/1     Running       0          20s
replica-set-1-g8bss   1/1     Running       0          5m35s
replica-set-1-p9dk8   1/1     Running       0          5m35s
replica-set-1-qgctd   1/1     Running       0          2m51s
replica-set-1-rdb9t   1/1     Running       0          5m35s

```
start a new one  because the replicaset is make sure that 5 pods are running


# ----------------------11----------------------

```
└─ mohamed@DevOps:$ kubectl get deploy --all-namespaces
NAMESPACE     NAME      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   coredns   1/1     1            1           25h

```

```
└─ mohamed@DevOps:$ kubectl get rs --all-namespaces
NAMESPACE     NAME                DESIRED   CURRENT   READY   AGE
kube-system   coredns-64897985d   1         1         1       25h
```

### 1 deploy 1 replica set

# ----------------------12----------------------

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
  labels:
    app: nginx
spec:
    replicas: 3
    selector:
        matchLabels:
          app: busy      
    
    template:
        metadata:
            labels:
                app: busy
        spec:
            containers:
            - name: busybox
              image: busybox
              command: ["sleep", "5600"]
```
```
└─ mohamed@DevOps:$ kubectl get po
NAME                           READY   STATUS    RESTARTS   AGE
deployment-1-7856fc98b-4dzhb   1/1     Running   0          18s
deployment-1-7856fc98b-9pnc6   1/1     Running   0          18s
deployment-1-7856fc98b-zb7bt   1/1     Running   0          18s

```
# ----------------------13----------------------

### 2 deploy 2 replica set

```
└─ mohamed@DevOps:$ kubectl get deploy --all-namespaces
NAMESPACE     NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
default       busybox-deployment   3/3     3            3           3m2s
kube-system   coredns              1/1     1            1           25h

└─ mohamed@DevOps:$ kubectl get rs --all-namespaces
NAMESPACE     NAME                           DESIRED   CURRENT   READY   AGE
default       busybox-deployment-7856fc98b   3         3         3       3m22s
kube-system   coredns-64897985d              1         1         1       25h
```
# ----------------------14----------------------

### 3 pods

```
└─ mohamed@DevOps:$ kubectl get po
NAME                           READY   STATUS    RESTARTS   AGE
deployment-1-7856fc98b-4dzhb   1/1     Running   0          18s
deployment-1-7856fc98b-9pnc6   1/1     Running   0          18s
deployment-1-7856fc98b-zb7bt   1/1     Running   0          18s
```
# ----------------------15----------------------

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
    replicas: 3
    selector:
        matchLabels:
          app: busy      
    template:
        metadata:
            labels:
                app: busy
        spec:
            containers:
            - name: busybox
              image: nginx
              command: ["sleep", "5600"]
```

```
$ kubectl get po

NAME                           READY   STATUS        RESTARTS   AGE
deployment-1-7856fc98b-4dzhb   1/1     Terminating   0          2m2s
deployment-1-7856fc98b-9pnc6   1/1     Terminating   0          2m2s
deployment-1-7856fc98b-zb7bt   1/1     Terminating   0          2m2s
deployment-1-9fb654744-5p9xn   1/1     Running       0          26s
deployment-1-9fb654744-d2v9c   1/1     Running       0          17s
deployment-1-9fb654744-hw5m7   1/1     Running       0          22s
```

# ----------------------16----------------------

### `$ kubectl describe deploy deployment-1`

```
StrategyType:           RollingUpdate

```

```
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  4m32s  deployment-controller  Scaled up replica set deployment-1-7856fc98b to 3
  Normal  ScalingReplicaSet  2m56s  deployment-controller  Scaled up replica set deployment-1-9fb654744 to 1
  Normal  ScalingReplicaSet  2m52s  deployment-controller  Scaled down replica set deployment-1-7856fc98b to 2
  Normal  ScalingReplicaSet  2m52s  deployment-controller  Scaled up replica set deployment-1-9fb654744 to 2
  Normal  ScalingReplicaSet  2m47s  deployment-controller  Scaled down replica set deployment-1-7856fc98b to 1
  Normal  ScalingReplicaSet  2m47s  deployment-controller  Scaled up replica set deployment-1-9fb654744 to 3
  Normal  ScalingReplicaSet  2m43s  deployment-controller  Scaled down replica set deployment-1-7856fc98b to 0
```

# ----------------------17----------------------

```tex
$ kubectl rollout undo deployment deployment-1

deployment.apps/deployment-1 rolled back

```
### What is the used image with the deployment-1?
> return to the previous image busybox


# ----------------------18----------------------

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
    replicas: 3
    selector:
        matchLabels:
          app: nginx-app         
    template:
        metadata:
            labels:
                app: nginx-app
                type: front-end
        spec:
            containers:
            - name: nginx-container
              image: nginx
```

## ` kubectl create -f nginx-deployment.yaml`

```
$ kubectl get po

NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-769fdbb55c-78zbm   1/1     Running   0          15s
nginx-deployment-769fdbb55c-82vpt   1/1     Running   0          15s
nginx-deployment-769fdbb55c-n42xp   1/1     Running   0          15s

```
