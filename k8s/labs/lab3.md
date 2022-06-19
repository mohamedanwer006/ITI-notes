# ----------------------------------1------------------------------------

#### 1 DaemonSets 

```
└─ mohamed@DevOps:$ kubectl get DaemonSets --all-namespaces
NAMESPACE     NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   47h
```

# ----------------------------------2------------------------------------

```
└─ mohamed@DevOps:$ kubectl get DaemonSets -n kube-system
NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   47h

```

# ----------------------------------3------------------------------------

`$ kubectl get po -n kube-system`

`$ kubectl describe pod  kube-proxy-lfzz2 -n kube-system`

or 

```
 $ kubectl describe DaemonSets -n kube-system

   kube-proxy:
    Image:      k8s.gcr.io/kube-proxy:v1.23.3

```
# ----------------------------------4------------------------------------

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata: 
  name: elasticsearch
  namespace: kube-system
spec:
    template:
        metadata:
            labels:
                app: fluent-myApp 
        spec:
            containers:
                - name: elasticsearch
                  image: k8s.gcr.io/fluentd-elasticsearch:1.20
    selector:
        matchLabels:
            app: fluent-myApp 
```

```
└─ mohamed@DevOps:$ kubectl get po -n kube-system
NAME                               READY   STATUS    RESTARTS      AGE
coredns-64897985d-2jd2n            1/1     Running   1 (46h ago)   47h
elasticsearch-gv56z                1/1     Running   0             82s
```

# ----------------------------------5------------------------------------

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: nginx-pod
  labels:
      tier: backend
spec: 
    containers:
        - name: nginx
          image: nginx:alpine

```
# ----------------------------------6------------------------------------

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: nginx-pod-test
  labels:
      tier: backend
spec: 
    containers:
        - name: nginx
          image: nginx:alpine

```
# ----------------------------------7------------------------------------


```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  ports:
  - port: 80 # port service
  selector:
        tier: backend
```

# ----------------------------------8------------------------------------

### `kubectl exec -it nginx-pod-test -- sh`

### `apk add curl` 

```
└─ mohamed@DevOps:$ kubectl exec -it nginx-pod-test -- sh
/ # curl nginx-pod  
curl: (6) Could not resolve host: nginx-pod
/ # curl 172.17.0.7
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

# ----------------------------------9------------------------------------

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
    replicas: 2
    selector:
        matchLabels:
          app: nginx-app         
    template:
        metadata:
            labels:
                app: nginx-app
        spec:
            containers:
            - name: nginx-container
              image: nginx
```

```
└─ mohamed@DevOps:$ kubectl create -f nginx-dep.yaml 
deployment.apps/web-app created
```

# ----------------------------------10------------------------------------


```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-service 
spec:
  type: NodePort
  ports:
  - port: 80 # port service
    targetPort: 80 #port container
    nodePort: 30082 #port node
  selector:
        app: nginx-app

```


# ----------------------------------11------------------------------------

```
└─ mohamed@DevOps:$ curl 192.168.49.2:30082
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

# ----------------------------------12------------------------------------

```
$ kubectl get pods --all-namespaces -o json | jq -r '.items | map(select(.metadata.ownerReferences[]?.kind == "Node" ) | .metadata.name) | .[]'

etcd-minikube
kube-apiserver-minikube
kube-controller-manager-minikube
kube-scheduler-minikube
```

or 
```
└─ mohamed@DevOps:$ minikube ssh
docker@minikube:~$ ls /etc/
Display all 131 possibilities? (y or n)
docker@minikube:~$ ls /etc/k
kernel/     kubernetes/ 
docker@minikube:~$ ls /etc/kubernetes/
addons/                  controller-manager.conf  manifests/               
admin.conf               kubelet.conf             scheduler.conf           
docker@minikube:~$ ls /etc/kubernetes/manifests/
etcd.yaml  kube-apiserver.yaml  kube-controller-manager.yaml  kube-scheduler.yaml
docker@minikube:~$ 
```

# ----------------------------------13------------------------------------

on minikube master 
