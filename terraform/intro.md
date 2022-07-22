# what is (IaC) infrastructure as code?
- is a way to define your infrastructure as code.
- idempotence is the ability to repeat a command without changing the result.

## why terraform?
- work with all providers (aws, gcp, azure, etc)
- have a plan command to generate a plan for your infrastructure without running it.

# How Terraform works
Terraform core & terraform plugin

core downloads plugins and communicates with plugins using a protocol called RPC.

rpc = remote procedure call

## terraform core

- compiled binary written in go 
- cli tool to interact with terraform

## terraform plugin
plugin is a binary that is written in go.

- provider plugin is a plugin that deals with cloud providers.

- provisioner plugin is a plugin that deals with provisioning resources.

### local provisioner 
example
- take public ip and put it in ansible inventory
- run ansible playbook automatically


## state file
- terraform state file is a json file that contains the state of your infrastructure.

backend using s3 to store the state file and make it shareable.

lockstate is a mechanism to prevent multiple terraform runs from overwriting the state file.

use dynamodb to store the LockID 


terraform state command

u need to make resource out of managed state file.

```
terrafrom state rm <resource-name>
```



resource create manual on aws 
u need to manage  it using terraform

```
1- config resource in terraform file

terraform import resource.resource_name aws_resource resource_id
```


rename resource after creating it, but you don't want to delete it.

``` 
terraform state mv <resource_type>.<old-name> <resource_type>.<new-name>
```

unlock LockID

```
terraform force-unlock <unlock id>
```


### workspace
- terraform workspace is a directory that contains your terraform state file.

### create new workspace


```md
terraform workspace new <workspace-name>
```

### switch to workspace

```md
terraform workspace select <workspace-name>
```

### show current workspace

```md   
terraform workspace show
```

```
terraform workspace list

```

## outputs

```
terraform output outputs.name
```


## terraform taint

> if you want to replace a resource or rerun it agin , you can taint it. 
    
```
    terraform taint <resource-name>
```

untaint a resource

```
    terraform untaint <resource-name>
```
# modules

