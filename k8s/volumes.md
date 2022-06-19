# Volumes
#### Save data in volume and share it with pods
#### All pods see the same data

no worries about pods being restarted or deleted


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: my-namespace
spec:
    containers:
    - name: my-container
      image: alpine
      volumeMounts:
        - name: my-volume
          mountPath: /opt
    volumes: # volume out side container
    - name: my-volume
      hostPath:
        path: /data # on node
        type: Directory
```
if create replicas on 3 node 

it create 3 volumes on 3 different nodes !!!!!



> The 3 pods not see each other ??


types any thing you can save data on it
cloud EBS  disk, nfs

```yaml

volumes:
    - name: my-volume
        awsElasticBlockStore:
            volumeID: vol-12345
            fsType: ext4

# it needs credentials for access
```

> Another problem is if pod deleted then volume will be deleted


#### So we need persistent volume
## persistent volume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-volume
  labels:
spec:
    accessModes:
      - ReadWriteOnce # read and write  one pod will mount  first one make mount , ReadOnlyMany many pods read only  ReadWriteMany many pods can read and write
    capacity:
        storage: 1Gi
    awsElasticBlockStore:
        volumeID: vol-12345
        fsType: ext4
```


## Persistent volume claim

da el birbt bein pod wl pv 
    
```yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
  namespace: my-namespace
spec:
    accessModes:
      - ReadWriteOnce 
    resources:
        requests:
            storage: 1Gi
```

```
kubernetes create -f my-claim 
```

### we can use pvc with different pods and deployment

## delete pvc 

#### ReclaimPolicy: 

- Retain # keep the volume after pod deleted  status not available but data still exist (default)

- Recycle # delete the volume data after pod deleted and make status available for new pod


- delete # delete the volume after pod deleted

pv => pvc => pod

mount with pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
    namespace: my-namespace
spec:
    containers:
    - name: my-container
      image: alpine
      volumeMounts:
        - name: data-volume
          mountPath: /opt
volumes:
    - name: my-volume
        persistentVolumeClaim: # point to pvc
            claimName: my-claim
```

# StorageClass

call cloud provider to create volume when binding to pvc 

u can attach  many pvc with different name to the storage class it will create disk for each pvc

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage
provisioner: kubernetes.io/gcp-pd
```
## create my clam

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
  namespace: my-namespace
spec:
    accessModes:
      - ReadWriteOnce
    storageClassName: google-storage
    resources:
        requests:
            storage: 1Gi
```





