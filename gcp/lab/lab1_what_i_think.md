### 1.From Cloud console, do the following:
#### Create custom role named "my-custom-role-1" with the following permissions only:

- Iam.roles.get
- Iam.roles.list

what happen in background?
```
- browser send request to google api and return list of roles
- select roles from the list    
- browser again will send post request with payload of permissions to iam api  and  save role  
- I think they saved in google databases some where and linked with  the project ID and account ID

```
why do we need to create custom role?

```
because we need to assign custom role to service account or assign custom role to user account
so when they try to make request to google api , they will 
get error if they don't have permission to do that
```
![alt](../assets/Screenshot%20from%202022-06-22%2022-53-46.png)


### 2.From Cloud console, Explore primitive and pre-defined roles and their permissions.

![alt](../assets/Screenshot%20from%202022-06-22%2023-03-19.png)

```
- same as above browser will make request to iam.role.api and  it will return list of roles

-using filter we can filter the roles we want
```


### 3.From Cloud console, Create a service account with id "my-first-serviceaccount".


```
why we need to create service account?
    because sometimes we need to call google api from apps like jenkins or from vms ...


the service account linked with project id and saved in database with its json-key file
```




![alt](../assets/Screenshot%20from%202022-06-22%2023-13-11.png)

### 4.From Cloud console, Assign the custom role "my-custom-role-1" to the service account "my-first-serviceaccount"

```
why
we need to assign some permission to service account?
so it can access google api, specifically iam.roles 

thats why we attach to it `custom role`
```

![alt](../assets/Screenshot%20from%202022-06-22%2023-11-46.png)

### 5.Using gcloud
- List all roles on your project.

```
gcloud send get request to iam.roles.list api and it will response with list of roles



```




![alt](../assets/Screenshot%20from%202022-06-22%2023-15-46.png)

- Describe the predefined role "roles/compute.viewer" and view its details & permissions

![alt](../assets/Screenshot%20from%202022-06-22%2023-19-03.png)

- Describe the custom role "my-custom-role-1" and view its details & permissions.
![alt](../assets/Screenshot%20from%202022-06-22%2023-36-10.png)

- List all authenticated accounts.
![alt](../assets/Screenshot%20from%202022-06-22%2023-42-11.png)

- Activate the service account "my-first-serviceaccount".

![alt](../assets/Screenshot%20from%202022-06-22%2023-50-32.png)
- List all authenticated accounts again.

```
i think here gcloud does not send request to api, it get the authenticated accounts from local machine configuration file
```
![alt](../assets/Screenshot%20from%202022-06-22%2023-51-36.png)
- Using this service account, try to list all roles on your project.

> enable  => `iam.googleapis.com` 
![alt](../assets/Screenshot%20from%202022-06-23%2000-00-59.png)


```
gcloud send request to iam.roles.list api with service account key-info 

api will check if it have permission to do that
if so it will return list of roles
```

![alt](../assets/Screenshot%20from%202022-06-23%2000-05-25.png)

- Try to delete custom role "my-custom-role-1"

```
same as above
except when the api backend check if it have permission to do that it will return error

that this service account does not have permission to do that

```

![alt](../assets/Screenshot%20from%202022-06-23%2000-04-38.png)