### 1.From Cloud console, do the following:
#### Create custom role named "my-custom-role-1" with the following permissions only:
- Iam.roles.get
- Iam.roles.list
![alt](../assets/Screenshot%20from%202022-06-22%2022-53-46.png)


### 2.From Cloud console, Explore primitive and pre-defined roles and their permissions.

![alt](../assets/Screenshot%20from%202022-06-22%2023-03-19.png)


### 3.From Cloud console, Create a service account with id "my-first-serviceaccount".

![alt](../assets/Screenshot%20from%202022-06-22%2023-13-11.png)

### 4.From Cloud console, Assign the custom role "my-custom-role-1" to the service account "my-first-serviceaccount"

![alt](../assets/Screenshot%20from%202022-06-22%2023-11-46.png)

### 5.Using gcloud
- List all roles on your project.

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

![alt](../assets/Screenshot%20from%202022-06-22%2023-51-36.png)
- Using this service account, try to list all roles on your project.
> enable  => `iam.googleapis.com` 
![alt](../assets/Screenshot%20from%202022-06-23%2000-00-59.png)


![alt](../assets/Screenshot%20from%202022-06-23%2000-05-25.png)

- Try to delete custom role "my-custom-role-1"

![alt](../assets/Screenshot%20from%202022-06-23%2000-04-38.png)