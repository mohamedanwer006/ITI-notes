# Helm
it is a tool for managing Kubernetes resources.

it is like yum and apt-get for Kubernetes.


# Helm Chart
it is a package that contains a set of Kubernetes resources.

when create upgrade it create a new revision of the chart.


# 3 ways strategy 

when role back it rollback to the previous revision. but it create it with new version number


if there a manual change in cluster if update it will save it 

if there is a change in chart if 


## helm components

secrets # secrets are used to store metadata about chart

### helm chart

folder=>
        |- templates/
        |- values.yaml 
        |- Charts.yaml # metadata about chart
        |- charts/ 


## charts type chart application vs chart library
- chart application is a chart that is used to deploy a new application.

- chart library is a utility that is used to create a chart.


> chart library does not have a template folder.


# override chart values

```sh
helm install my-app nginx --set "image.tag=1.0.0"

helm install my-app nginx --values custom.yaml

# custom.yaml should be have the same structure as values.yaml
```

```sh
helm pull --untar bitnami/nginx

code . 

# edit values.yaml

```

### list all charts
```sh
helm list
```

### history of chart

```sh
helm history my-app
```

### rollback to previous version

```sh
helm rollback my-app 1
```

### create new helm chart
    
```sh
    helm create my-app nginx
```

### get template of chart
used to render the chart template

> final manifest after replace all the values

```sh
helm template my-app ./nginx
```

### Objects

- Release 
- Chart
- Capability
- Value

#### To use `.Object.<>`

### check configuration

```sh 
helm install or upgrade --dry-run --debug <chart-name>
```

### functions

upper('string') # return string in upper case
```yml

{{ upper .Values.name.app}}

{{ randAlpha 5 | lower }}

generate a random string
and return it in lower case

```

if u in scope but you want to refer to root use $

### named template
_helpers.tpl

```
{{- define "helpers" -}}
{{- range $key, $value := .Values.metadata.label }}
{{ $key }}: {{ $value }}
{{- end }}
{{- end }}
```

to use it

```
{{- template "helpers" . }}


```
```
{{- include "helpers" . | indent 4}}
```

helm ignore _helpers.tpl

### Chart hooks


helm upgrade => varify=> render=> deploy

what if i need to take packup before deploy


create job to packup

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:

```



