# K8S ENV Variable

```yaml
spec:
    containers:
        - name: nginx
        image: nginx
        env:
            - name: App_color
        
```

- plain key value


```yaml
env:
    - name: APP-COLOR
      value: red
```


- config map

```
kubernetes create config map <name> --from-literal=<key>=<value>

kubernetes create config map <name> --from-file=<path>

# create one variable with list fo value in file
```

```
App_color: blue
APP-MODE: production

```

```yaml
kind: configMap
apiVersion: v1  
metadata:
  name: my-config-map
  namespace: dev  
data:
    APP-COLOR: blue
    APP-MODE: production


```




```
describe config map my-config-map

```


### call config map
```yaml
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
                name: conf-map # name of config map
        # call config map  u see all variables
```


### call one env only

```yaml
env:
    - name: APP-COLOR
      valueFrom:
        configMapKeyRef:
          name: my-config-map
          key: APP-COLOR
```



- secret
like config map but with encrypted data


you encrypt data and store it in secret

```yaml
envFrom:
    - secretKeyRef:
        name: my-secret
        key: my-key
```


### Types of secrets
### 1- generic
```
kubernetes create secret generic <secret-name> --from-literal=<key>=<value>
```

```
kubernetes create secret generic app-secret <secret-name> --from-literal=DB=mysql
```

```yaml
kind: Secret
apiVersion: v1
metadata:
  name: app-secret
    namespace: dev
data:
    DB: bxmfsw
    
```

### encrypt data in secret

```
echo -n 'mysql' | base64

```

```
kubectl get secrets
type: generic = opaque


```

```
kubectl get secret app-secret -o yaml
```


### Decode data
```
echo -n 'bxmfsw' | base64 --decode
```





#### call secret in pod

```yaml
envFrom:
    - secretRef:
        name: app-secret
```
### call one variable in pod

```yaml
env:
    - name: DB
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DB
```

## Volumes
call config or secret in pod with volume

```yaml
volumes:
    - name: my-volume
      secret:
        secretName: app-secret


```
> variables will be files not env variables
> key= file name  inside value

```
ls /opt/app-secret-volumes

Db_Host DB_Port DB_User DB_Password
```

# Example

create secret

```yaml
kubectl create secret generic app-secret --from-literal=NAME=ahmed \
--from-literal=TRACK=devops \
--dry-run -o yaml | kubectl apply -f -


kubectl create secret generic app-secret --from-literal=NAME=ahmed \
--from-literal=TRACK=devops \
--dry-run -o yaml > secrets.yaml

kubectl apply -f secrets.yaml

```





> if u not specify the replicas then it will be 1 by default

```
kubectl create deployment nginx-deployment --image=nginx
--dry-run=client -o yaml > nginx-deployment.yaml

```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
    namespace: dev
spec:
    replicas: 1
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
            
        envFrom:
            - secretRef:
                name: app-secret
```








