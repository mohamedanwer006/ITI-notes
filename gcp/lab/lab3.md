# LAB 2.2
### **Create a VM without public ip then:**
- SSH into this vm and update system packages.

```
sudo apt update && sudo apt upgrade -y
```
<!-- ![alt](../assets/Screenshot%20from%202022-06-23%2016-08-23.png) -->
- Setup Nginx Web Server and test your setup.

```bash
sudo apt install nginx -y
sudo systemctl enable --now nginx
```

![alt](../assets/Screenshot%20from%202022-06-24%2014-51-18.png)

![alt](../assets/Screenshot%20from%202022-06-24%2014-58-35.png)

![alt](../assets/Screenshot%20from%202022-06-24%2015-01-54.png)

- Create a custom image from this VM named “custom-img-nginx”.

![alt](../assets/Screenshot%20from%202022-06-24%2015-47-42.png)
---
---

### **3.Create MIG (min 3 and max 5) of a template using the custom image “custom-img-nginx”.**
![alt](../img/Screenshot%20from%202022-06-24%2020-48-23.png)
![alt](../img/Screenshot%20from%202022-06-24%2020-48-05.png)

### **4.Create a Global (or Regional) HTTP Load balancer to access your MIGs Nginx setup.**
>load balancer
![alt](../img/Screenshot%20from%202022-06-24%2020-39-16.png)

>Firewall roles
![alt](../img/Screenshot%20from%202022-06-24%2020-49-06.png)

![alt](../img/Screenshot%20from%202022-06-24%2021-12-59.png)

### **5.Try to configure IAP at the load balancer level to protect your ingress access. Is it possible to have IAP enabled for HTTP resources?**
![alt](../img/Screenshot%20from%202022-06-24%2021-37-53.png)
![alt](../img/Screenshot%20from%202022-06-24%2021-42-28.png)

---
---

# Lab 3.1
### **1.Create a private GKE cluster.**

![alt](../img/Screenshot%20from%202022-06-24%2022-44-02.png)



### **2.Deploy Nginx as a deployment using latest Nginx docker image on Docker Hub.**

![alt](../img/Screenshot%20from%202022-06-24%2022-52-26.png)


### **3.Expose your Nginx deployment using Kubernetes LoadBalancer Service.**
![alt](../img/Screenshot%20from%202022-06-24%2022-58-49.png)
![alt](../img/Screenshot%20from%202022-06-24%2022-58-32.png)

### **4.What is the type of GCP Load Balancer that is created for your LB service?**

> ##  Network (target pool-based)



### **5.Use kubectl to view container logs.**

![alt](../img/Screenshot%20from%202022-06-25%2000-25-01.png)



### **6.Use cloud logging service to view container logs. [hint: search about cloud logging service for gke]**

![alt](../img/Screenshot%20from%202022-06-25%2000-58-56.png)

### **8.Create an autopilot GKE cluster with public control plane.**

![alt](../img/Screenshot%20from%202022-06-25%2001-17-31.png)


### **9.Enforce the cluster’s control plane to accept only connections from your local machine.**
![alt](../img/Screenshot%20from%202022-06-25%2001-18-46.png)


### **10.Install kubectl on local machine and use it to connect to the cluster.**
![alt](../img/Screenshot%20from%202022-06-25%2001-22-16.png)