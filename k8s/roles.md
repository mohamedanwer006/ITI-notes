# api groups
### grouping endpoint 

#### /v1 => core 

#### /apps/v1 resource add


# Authorization

what can they do

- RBAC => rolled  based access c
- ABAC => attribute based access control
- Webhook =>  


- config file
--authorization-mode=Node,RBAC,Webhook

when user send request to k8s api, k8s will check the request is authorized or not

> moves step by step Node, RBAC, Webhook



# RBAC

create a role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: my-role-name
rules:
    - apiGroups: [""] # empty string means core api
      resources: ["pods"]
      verbs: ["get", "list"]
      resourceName: ["blue",'orange'] # blue and orange are names of pods if i want to specify a pod

    # - apiGroups: [""] # empty string means core api
    #     resources: ["pods"],["nodes"] # multiple resources can be specified in one rule if they have the same api group and same verbs
    #     verbs: ["get", "list"]

    - apiGroups: [""]
      resources: ["ConfigMaps"]
      verbs: ["get", "list"]
```
> not define namespace so the role will be applied to the default namespace

# RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: my-role-binding-name
subjects:
    - kind: User
      name: my-user-name
      apiGroup: rbac.authorization.k8s.io
    
roleRef:
    kind: Role
    name: my-role-name
    apiGroup: rbac.authorization.k8s.io

```

```

kubectl get rolebinding -A

kubectl get csr 
# return a list of certificate signing request users name 

kubectl certificate approve my-user


```


## check Access

```

kubectl auth can-i get pods --as my-user-name

if not use --as, it will check the current user

if not use --namespace, it will check the current namespace => default namespace

```

# Create a role
```

kubectl creat role my-role-name --resource=pods --verb=get,list
--dry-run -o yaml > my-role.yaml
```


# create a rolebinding
```
kubectl create rolebinding my-role-binding-name --role=my-role-name --user=my-user-name --dry-run -o yaml > my-role-binding-name.yaml
```

# ClusterRole

permission to access all resources in the cluster

> Same as role change kind to ClusterRole

> ClusterRoleBinding instead of RoleBinding


# image pull

image : docker.io/nginx/nginx

docker.io is a registry

nginx/nginx is a userAccount/image repository

## private registry

```
docker login registry.com

```

```
docker pull registry.com/nginx/nginx
```

on k8s

```
kubectl create secret docker-registry regcred --docker-server=registry.com --docker-username=my-user-name --docker-password=my-password --docker-email=my-email@org.com
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod-name
spec:
    containers:
        - name: my-container-name
        image: registry.com/nginx/nginx
    imagePullSecrets:
        - name: regcred
```

## Network policy

put restrictions on the coming or going traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy-name
spec:
    podSelector: # select pods to apply the policy to
        matchLabels:
            app: database
    
    policyTypes:
        - Ingress # or Egress
    ingress:
        - from:
            - podSelector:
                matchLabels:
                    app: api-pod # the pod that will call the database
              namespaceSelector: # the namespace for pod
                matchLabels:
                    app: api-namespace

            - ipBlock: # the ip block that will call the database from outside cluster
                cidr: 192.168.5.10/32
            ports:
                - protocol: TCP
                  port: 3306   
```

# Egress
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy-name
spec:
    podSelector: # select pods to apply the policy to
        matchLabels:
            app: database
    
    policyTypes:
        - Egress
    egress:
        - to:
            - podSelector:
                matchLabels:
                    app: api-pod # the pod that will call the database
              namespaceSelector: # the namespace for pod
                matchLabels:
                    app: api-namespace

            - ipBlock: # the ip block that will call the database from outside cluster
                cidr: 192.168.10.15/32
            ports:
                - protocol: TCP
                  port: 80
```



# NetworkPolicy Solutions

- kube-router
- calico
- romana
- weave-net

vi .minikube/config/config.json

"cni":"calico"

# DNS

Domain Name server (System)

1. /etc/hosts

```
192.168.1.10 google.com
192.168.1.11 web.com
```

## one server can serve multiple domains

# add dns server ip
2. ### /etc/resolv.conf
#### nameserver 192.168.1.100


chang order between hosts and resolv.conf

/etc/nsswitch.conf

hosts: files dns


### name resolution error => it cant get ip for that name


### Domain Names

.com.

## `` . `` root server 13 server



## ``.com`` top level domain

## google
dns =>  server for google

www => sub domain

mail => sub domain

drive => sub domain


root dns => .com Dns => google Dns => google.com Dns => www.google.com >>> browser will get the ip


search domain

/etc/resolv.conf

```
nameserver 192.168.1.100
search mycompany.com prod.mycompany.com
```

ping web

web.mycompany.com



## Record types

### A -> web-server - ipv4 address

### AAAA -> web-server -  ipv6 address

### CNAME food.web-server => eat.web-server,hungry.web-server


### CNAME subdomain with subdomain

### root domain with root domain 
#### use alias



```bash
$ nslookup www.google.com

server: 8.8.8.8
address: 172.217.0.132

#############

$ dig www.google.com
more information 
```



# kube DNS

on services

```
curl podName.namespace.svc.cluster.local
```














