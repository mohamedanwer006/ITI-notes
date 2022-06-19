# K8s Authentication

who can access the k8s API ?

- files - username and password
- certificates 
- External Authentication providers - LDAP, OAuth, etc.
- Service Accounts - kubectl get serviceaccounts


user = > file - certificate 

service - app => serviceaccount


```
kubectl create serviceaccounts my-service-account
kubectl get serviceaccounts

kubectl describe serviceaccounts my-service-account


```

>by default there a default service account and attach to pod by default

Attach service account to a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: my-namespace
    labels:
        app: my-app
spec:
    containers:
        - name: my-container
          image: nginx
    serviceAccountName: my-service-account # attach service account to pod override default service account

```
###  To remove auto mount for default service account

```yaml
autoMountServiceAccountToken: false
```

token saved  inside the pod in
>/var/run/secrets/kubernetes.io/serviceaccount/token


kubectl describe secret <secret-name> 

> get token

## Set permission for service account

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account

```


# Security KUBECONFIG

place where config stored

### $HOME/.kube/config

- cluster 
 cluster ip
 cluster cert

- Contexts
  context name
  cluster name
  user name


- Users
    user name
    user cert


```yaml
current-context: my-context 
```

```
kubectl config view

kubectl config view --kubeconfig=/path/to/kubeconfig

```

```yaml
apiVersion: v1
kind: Config

current-context: my-context # edit for use another cluster and user

clusters:
  - name: my-cluster
    cluster:
      server: https://kubernetes.example.com
      certificate-authority: /path/to/ca.pem

contexts:
  - name: my-context
    context:
      cluster: my-cluster
      user: my-user
      namespace: my-namespace # namespace to use for this context (defaults to default namespace)

users:
  - name: my-user
    user:
      client-certificate: /path/to/crt
      client-key: /path/to/key.pem

```








## Create mycert.crt

```
openssl req -new -x509 -nodes -newkey rsa:2048 -keyout mycert.key -out mycert.crt -days 365


Generate a self signed certificate

mycert.key
mycert.crt

give mycert.crt to kubernetes admin

```


