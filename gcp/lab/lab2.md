# Lab 2.1

### **From Cloud console, create a VPC named “auto-vpc” with auto-mode enabled,**
> 34 subnets each with a /20 CIDR block.
![alt](../assets/Screenshot%20from%202022-06-23%2013-48-26.png)

### **How many subnets created? And how many routes created for this VPC? Can you delete any of these routes?**
> 34 subnets each with a /20 CIDR block. 
> 35 routes each with a /20 CIDR block.  one of them is default for public access. 
> no can't delete any of these routes.

 ### **From Cloud console, create a VPC named “custom-vpc” with auto-mode disabled and create two subnets. How many routes created for this VPC?**
 > 3 routes tables

![alt](../assets/Screenshot%20from%202022-06-23%2014-06-05.png)

### **Using gcloud tool list all available VPCs and list subnets of each VPC.**

![us](../assets/Screenshot%20from%202022-06-23%2014-30-05.png)

![alt](../assets/Screenshot%20from%202022-06-23%2014-31-44.png)

### **In two different ways, How would you block internet access from you vpc?**

> By default, incoming traffic from outside your network is blocked
> create firewall rules to deny incoming traffic from outside  

### **Create a firewall rule to allow incoming SSH requests from internet to all instances in your vpc.**
![alt](../assets/Screenshot%20from%202022-06-23%2014-50-06.png)

### **Modify the previous firewall rule to allow only ssh requests coming through Google**
> 35.235.240.0/20. This range is the pool of IP addresses used by IAP

![alt](../assets/Screenshot%20from%202022-06-23%2014-58-02.png)


# lab 2.2

### **Create a VM with public ip then:**

![alt](../assets/Screenshot%20from%202022-06-23%2015-10-38.png)
- In two different ways, SSH into this VM.

  - using web browser. 
![alt](../assets/Screenshot%20from%202022-06-23%2015-36-02.png)


  - using gcloud
> gcloud will create a ssh key and add it to vm
> error happens because the firewall rule was set to allow only ssh from googl IAP range 

![central1](../assets/Screenshot%20from%202022-06-23%2015-26-57.png)
> fix it 
![alt](../assets/Screenshot%20from%202022-06-23%2015-31-58.png)
- Enforce SSH into this VM to be IAP protected.
> 35.235.240.0/20. This range is the pool of IP addresses used by IAP
> use in gcloud command to enforce access to IAP cloud instead of checking the external ip --tunnel-through-iap

![alt](../assets/Screenshot%20from%202022-06-23%2014-58-02.png)

### **Create a VM without public ip then:**
- SSH into this vm and update system packages.
```
sudo apt update && sudo apt upgrade -y
```
![alt](../assets/Screenshot%20from%202022-06-23%2016-08-23.png)
- Setup Nginx Web Server and test your setup.

```bash
sudo apt install nginx -y
sudo systemctl enable --now nginx
```

> changed the default page content and test it from browser.
![alt](../assets/Screenshot%20from%202022-06-23%2016-26-16.png)

- Create a custom image from this VM named “custom-img-nginx”.

![alt](../assets/Screenshot%20from%202022-06-23%2016-44-55.png)

![alt](../assets/Screenshot%20from%202022-06-24%2014-51-18.png)

![alt](../assets/Screenshot%20from%202022-06-24%2014-58-35.png)

![alt](../assets/Screenshot%20from%202022-06-24%2015-01-54.png)
