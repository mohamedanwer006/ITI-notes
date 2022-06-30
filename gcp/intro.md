# GCP
`34 region => 103 zones => 147 edge locations as CDN`
> CDN content delivery network like `cloudflare`

### open 
`console.cloud.com`

`cloudshell` 
#### It is a vm but have limits like cpu, memory, disk, network, etc.

#### Any resources on gcp put under project scope



# Resources hierarchy 

### To type of project:
1. organization => projects 
2. stand alone project 

> organization => folders => projects => resources

```
        company
        |           |           |
        Deptx       Depty       Deptz
        |           |           |
        Teamx    sharedinfra       Teamz
        |                  |
        Projectx           Projectz     projecty    
```

### From `select project`
1. create new project 

2. project name not unique can be change

3. projectID is unique across global GCP

4. projectNumber is unique across global GCP assigned by google

---
# API & Billing
## Billing

Configure billing account
to start create resources
=> `manage billing account`
> Enter credit card info and billing account info

=> `link a billing account`,`manage billing account` to project

### Budget & alerts

#### Create budget
---
## API
### Google Cloud API

#### How to call GCP

1. Console => web app
2. SDK
3. client library
4. command line

# Google cloud SDK

gcloud sdk

#### Authentication
```bash
$ gcloud auth login 
$ gcloud auth list

# list configuration
$ gcloud config configurations list

# configuration is a set of properties that are used to configure the behavior of the gcloud command-line tool.

# assign a user to project 

# create a configuration
gcloud config configurations create my-config


gcloud config configurations activate my-config

gcloud config set project my-project

gcloud config set account my-account

```



# IAM (Identity and access management)

it is  a system accounts 

want to manage access to resources 


`who` =>  `can do what` => `on what resource`

### who
- google account or cloud identity user
- service account
- google group
- cloud identity or G suite domain

### role
can do what on which resource

- primitive role
    1. owner
    2. editor
    3. viewer  read only
    4. billing admin
> owner can do everything
> editor can do everything except owner
> viewer can do everything except owner and editor

> Access to whole project

- predefined role

> roles offer more granularity than primitive roles

> roles are assigned to users or groups


- custom role
create a role


### Role launch stage
#### if u create a new role, it will be in `alpha` stage
#### then it will be in `beta` stage
#### then go to release stage `General availability` 

## Assign role

### Add principal to role

new principal take user or group or domain

select role

`save`



# service account

it is like a user but it is not a user but a service account 


- create a service account

- select project

- select service account

- service account name

- service account id

- service account email is unique globally across GCP

- accountid@projectid.gserviceaccount.com


```

gcloud auth activate-service-account 
--key-file= <json-key-path>

```

you can add it as  principal 

and assign role to it

like normal user



there is two types of service account

custom service account 

# default service account
## create by gcp 
## to manage some resources


### cant apply custom role to folders 


