# services

service run in namespace
its like a route table ,rules

> cluster ip is the default service if u not specify a service kind
## nodePort
have pod want to make it public

create service

1- port node 
2- service  send to pod using label  

nodePort 30000-32767
if you not assign port nodePort will be take randomly

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-app
spec:
  type: NodePort
  ports:
  - port: 80 # port service
    targetPort: 80 #port container
    nodePort: 30000 #port node
    selector:
        app: redis-app
```

```bash
kubectl get service 

kubectl get svc
```

>service forward request to pod 

-----


## ClusterIP

- used inside  cluster
- it is like a load balancer
forward randomly to pods

fronted call backend  

frontend pods call service >> service call backend pods


```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-app #name service dns translate this to ip so u can use instead of ip in code
spec:
    type: ClusterIP
    ports:
    - port: 80 # port service
      targetPort: 80 # if not add targetPort will be take service
    selector:
        app: redis-app

```

## for fast expose service
```
kubectl expose service redis-app --type=NodePort

kubectl expose deployment nginx  --name nginx-svc --type=NodePort --port 80
```

---

## LoadBalancer

U can use only loadBalancer or nodePort

> loadBalancer create nodePort 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-app #name service dns translate this to ip so u can use instead of ip in code
spec:
    type: LoadBalancer
    ports:
    - port: 80 # port service
      targetPort: 80 # if not add targetPort will be take service
    selector:
        app: redis-app
```

```bash
kubectl get endpoints
# get pods private ip for services

minikube ip
# return ip
```


## Access pod
```
kubectl get po

kubectl exec -it <pod-name> -- bash
```



