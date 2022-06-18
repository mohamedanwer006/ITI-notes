# POD status

pending => container is created > running > terminated


# pod conditions
when ready = 1


- PodScheduled =true
- initialized = true
- ContainerReady = true
- Ready = true

What if i need to make another condition

i want to add condition to pod 

- http Test
- tcp test - port
- Exec test - command


## ReadinessProbe 
used to check if pod is ready

lifecycle of container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-app
  labels:
        app: redis-app
spec:
    containers:
    - name: redis-container
        image: redis
        ports:
        - containerPort: 80
        readinessProbe:
            httpGet:
                path: /api/home
                port: 80
            
            initialDelaySeconds: 5 #delay before start probe default 0
            periodSeconds: 5 #repeat probe every 5 seconds default 10
            timeoutSeconds: 3 #timeout after request 1 second default 1
            failureThreshold: 8 # if 8 times probe failed pod will be not ready default 3
            successThreshold: 1 # if 1 times probe success pod will be ready default 1
    
```

```yaml
readinessProbe:
    tcpSocket:
        port: 80
```

```yaml
readinessProbe:
    exec:
        command:
        - cat
        - /app/is-ready
```

## Startup Probes
useful for startup time

take same options as readiness probe

container not start until probe success

readinessProbe start after startup probs

```yaml
#...
startupProbe:
    tcpSocket:
        port: 80
    failureThreshold: 3
```

## LivenessProbe
used to check if pod is alive

take same options as readiness probe

#### readinessProbe on failure pod will be not ready. stop get traffic

#### livenessProbe on failure pod will restart pod 

```yaml
#...
livenessProbe:
    httpGet:
        path: /api/home
        port: 80
    failureThreshold: 3
    periodSeconds: 5
```

# Static pods

use when master is not available

you ssh to worker node

Add pod.yaml to kubelet. manifests folder


you find manifest folder in kubelet

/etc/kubernetes/manifests/

May be change

So check config file






# Daemon Sets 
same like replicaset but without replicas


make sure that pod is running
on all worker nodes

useful for monitoring and 
logs gathering from nodes

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: redis-app
spec:
    selector:
        matchLabels:
            app: redis-app
    template:
    ...    

```





# initContainer
run before app start or pod start

see same data for  container

useful if u need to change permissions of file for main container


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-app
  labels:
        app: redis-app
spec:
    containers:
    - name: redis-container
        image : busybox
        command: ["/bin/sh", "-c", "echo hello world && sleep 3600 "]
    initContainers:
        - name: redis-init
            image: busybox
            command: ["/bin/sh", "-c", "echo waiting && sleep 3600 "]  
        - name: redis-init
            image: busybox
            command: ["/bin/sh", "-c", "echo waiting for my db && sleep 3600 "]
```

```
kubectl logs redis-app -c redis-init
```

## multi container
containers on same pod

- All share same network
- Share same volumes

main container 

other helper containers
have names like 

- SIDECAR
  gather logs and send it to other place

- adapter
  edit data format and send it to other place - tool like grafana

- Ambassador
  work like proxy - sent data to multiple place


