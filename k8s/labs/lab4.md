# ---------------------------------1--------------------------------

```

└─ mohamed@DevOps:$ kubectl get configmaps --all-namespaces
NAMESPACE         NAME                                 DATA   AGE
default           kube-root-ca.crt                     1      2d23h
kube-node-lease   kube-root-ca.crt                     1      2d23h
kube-public       cluster-info                         1      2d23h
kube-public       kube-root-ca.crt                     1      2d23h
kube-system       coredns                              1      2d23h
kube-system       extension-apiserver-authentication   6      2d23h
kube-system       kube-proxy                           2      2d23h
kube-system       kube-root-ca.crt                     1      2d23h
kube-system       kubeadm-config                       1      2d23h
kube-system       kubelet-config-1.23                  1      2d23h

```

# ---------------------------------2--------------------------------

```
└─ mohamed@DevOps:$ kubectl create  configmap conf-map --from-literal=APP-COLOR=darkblue --dry-run=client -o yaml

```

```yaml

apiVersion: v1
data:
  APP-COLOR: darkblue
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: conf-map


```

```
└─ mohamed@DevOps:$ kubectl create -f conf-map.yml 
configmap/conf-map created
```

# ---------------------------------3--------------------------------
```yml
apiVersion: v1
kind: Pod
metadata: 
  name: webapp-color
spec: 
    containers:
        - name: nginx
          image: nginx
          envFrom:
            - configMapRef:
                name: conf-map

```

```
└─ mohamed@DevOps:$ kubectl create -f webapp-color.yml 
pod/webapp-color created

root@webapp-color:/# printenv APP-COLOR
darkblue

```

# ---------------------------------4--------------------------------

#### default namespaces

```
└─ mohamed@DevOps:$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-4sqpv   kubernetes.io/service-account-token   3      2d23h
```

#### All namespaces

```
echo $(($(kubectl get secrets --all-namespaces | wc -l ) - 1)) 
39
```

# ---------------------------------5--------------------------------

```
3 varible  token , namespace , ca.crt

kubectl  describe secrets default-token-4sqpv
Name:         default-token-4sqpv
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: 274d8a9a-bc65-4ba8-b008-81577ce5ed19

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6ImtiR2pKUjZjM0prcjN5WUExTlNvU1Z5WmJFN1AxSmlTVGQwZjhKd1U0cmsifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImRlZmF1bHQtdG9rZW4tNHNxcHYiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGVmYXVsdCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjI3NGQ4YTlhLWJjNjUtNGJhOC1iMDA4LTgxNTc3Y2U1ZWQxOSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmRlZmF1bHQifQ.Havuv6ERmNeEgfFDQv0CTkA99xawapmuYRBYlbfb5A_TxMYgaXvQAeLIw8g7t4XKdonJ54Iu3AjOMdO9cqkaOlxe7QGepnvdAtxP0SYzzCr8SntT4HZlBhjeeb8uefuwbfWRvqVjIGT80FvDbJflD1qiTgV1pInaVgITxDh8BYwBVzNeldWSpfD-RpazFI_Rdp_CVkAE_O4mxlSauH6JpFJpjjJl_pkynokr2dJ27PlfSuFd0msnqBghZjGHakyijYn44Ezj8YtRF2mmaFclcEdJH6cdjR-gKqOD89TYu9udoLyYQ-XtraUJOY9gn3z5JfqCwRm_qjAWLoD7mQ01xw
ca.crt:     1111 bytes
namespace:  7 bytescrt:     1111 bytes
namespace:  
```

# ---------------------------------6--------------------------------
```yml
apiVersion: v1
kind: Pod
metadata: 
  name: db-pod
  labels:
spec: 
    containers:
        - name: mysql-container
          image: mysql:5.7
```
```
NAME                           READY   STATUS              RESTARTS   AGE
db-pod                         0/1     ContainerCreating   0          69s
db-pod                         0/1     CrashLoopBackOff   5 (29s ago)   5m20s
```
# ---------------------------------7--------------------------------
```
Startup script need Enviroment variables for the mysql container to run
```

# ---------------------------------8--------------------------------

```
└─ mohamed@DevOps:$ echo 'sql01' |base64
c3FsMDEK
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ ^C
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ echo 'user1' |base64
dXNlcjEK
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ echo 'password' |base64
cGFzc3dvcmQK
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ echo 'password123' |base64
cGFzc3dvcmQxMjMK
┌─/media/mohamed/D/ITI-notes/k8s

```

```yml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
    MYSQL_DATABASE: c3FsMDEK
    MYSQL_USER: dXNlcjEK
    MYSQL_PASSWORD: cGFzc3dvcmQK
    MYSQL_ROOT_PASSWORD: cGFzc3dvcmQxMjMK
```
# ---------------------------------9--------------------------------

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: db-pod
  labels:
spec: 
    containers:
        - name: mysql-container
          image: mysql:5.7
          envFrom:
            - secretRef:
                name: db-secret

```

```
└─ mohamed@DevOps:$ kubectl get po db-pod
NAME     READY   STATUS    RESTARTS   AGE
db-pod   1/1     Running   0          9s

```

# ---------------------------------10--------------------------------
```yml
apiVersion: v1
kind: Pod
metadata: 
  name: yellow
spec: 
    containers:
        - name: lemon
          image: busybox
          command: ['sh', '-c', 'echo The app is running! && sleep 55600']
        - name: gold
          image: redis
```

```
└─ mohamed@DevOps:$ kubectl get po
NAME     READY   STATUS    RESTARTS   AGE
yellow   2/2     Running   0          65s

```
# ---------------------------------11--------------------------------

```yml

apiVersion: v1
kind: Pod
metadata: 
  name: red
spec: 
    containers:
        - name: gold
          image: redis
    initContainers:
        - name: init-container
          image: busybox
          command: ['sh', '-c', "sleep 20"]
        
```

```

NAME     READY   STATUS     RESTARTS   AGE
red      0/1     Init:0/1   0          7s

└─ mohamed@DevOps:$ kubectl get po
NAME     READY   STATUS     RESTARTS   AGE
red      0/1     Init:0/1   0          25s
yellow   2/2     Running    0          9m12s
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ kubectl get po
NAME     READY   STATUS            RESTARTS   AGE
red      0/1     PodInitializing   0          26s

```

# ---------------------------------12--------------------------------

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: print-envars-greeting
spec: 
    containers:
        - name: print-container
          image: bash
          command: ["echo"]
          args: ["$(GREETING) $(COMPANY) $(GROUP)"]
          # command: ['echo ["$GREETING $COMPANY $GROUP"]']
          env:
            - name: GREETING 
              value: "Welcome to"
            - name: COMPANY
              value: "DevOps"
            - name: GROUP 
              value: "Industries"


```

```
└─ mohamed@DevOps:$ kubectl logs -f print-envars-greeting
Welcome to DevOps Industries
```


# ---------------------------------13--------------------------------


Where is the default kubeconfig file located in the current environment?

##### `$HOME/.kube/config`

# ---------------------------------14--------------------------------

How many clusters are defined in the default kubeconfig file?

##### `one cluster`

```yaml

clusters:
- cluster:
    certificate-authority: /home/mohamed/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Fri, 17 Jun 2022 14:28:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube

```
# ---------------------------------15--------------------------------
What is the user configured in the current context?
##### minikube user

```yml
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Fri, 17 Jun 2022 14:28:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube

```

# ---------------------------------16--------------------------------


```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
  labels:
spec:
    accessModes:
      - ReadWriteMany 
    capacity:
        storage: 100Mi
    hostPath:
        path: /pv/log # on node
        type: Directory

```

```
mohamed@DevOps:$ kubectl create -f pv-log.yml 
persistentvolume/pv-log created
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ kubectl get pv
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-log   100Mi      RWX            Retain           Available       
```

# ---------------------------------17--------------------------------


```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log1
spec:
    accessModes:
      - ReadWriteMany 
    resources:
        requests:
            storage: 500Mi

```

```
└─ mohamed@DevOps:$ kubectl create -f pv-claim.yml 
persistentvolumeclaim/claim-log1 created
┌─/media/mohamed/D/ITI-notes/k8s
└─ mohamed@DevOps:$ kubectl get pvc
NAME         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-log1   Bound    pvc-4c76277f-7f9c-499e-a58c-744ed119e4aa   500Mi      RWX            standard       2s
```

# ---------------------------------18--------------------------------


```yml
apiVersion: v1
kind: Pod
metadata: 
  name: nginx-pod-test
  labels:
      tier: backend
      type: test
spec: 
    containers:
        - name: nginx
          image: nginx:alpine
          volumeMounts:
            - mountPath: /var/log/nginx
              name: my-volume
    volumes:
      - name: my-volume
        persistentVolumeClaim: # point to pvc
            claimName: claim-log1
```

```
└─ mohamed@DevOps:$ kubectl get po 
NAME                    READY   STATUS             RESTARTS        AGE
nginx-pod-test          1/1     Running            0               6s
```