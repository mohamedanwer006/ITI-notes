# deployment

it is above replicaset

it create replicaset and deployment

# deployment strategy   
```md
kubectl describe deployment <deployment-name>
```

## DeploymentStrategyType: 
- RollingUpdate
- Recreate


```md
kubectl rollout undo deployment <deployment-name>
```
# create deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
    replicas: 3
    selector:
        matchLabels:
        app: nginx      
    
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx
              image: nginx
```

# Apply deployment
```md
kubectl apply -f <deployment-file>
```

```md

kubectl create deploy app-deployment --image=nginx
kubectl get deploy
kubectl get rs

deployment name = app-deployment
replicaset name = app-deployment+random-string
pod name = replicaset-name+random-string
<!-- ready => number of replicas -->
```


```bash
kubectl create app2 --image=nginx --replicas=3 --dry-run=client -o yaml > app.yaml

# dry-run: client means that it will not create any resources in the cluster.
```

## update deployment


edit yaml file for deployment 

```md
kubectl edit deployment <deployment-name>

```
```
kubectl set image deployment nginx-deployment nginx=nginx
```

get info about deployment

```
kubectl get deployment -owide
```

# Rollout
change between the last two deployments
```md
kubectl rollout undo deployment <deployment-name>
```
# Rollout history
```md
kubectl rollout history deployment <deployment-name>
```
`-to--reversion <number>` used to rollback to a previous version of the deployment.



