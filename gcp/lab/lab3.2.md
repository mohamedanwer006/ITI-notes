# Lab 3.2
## **1. Using gsutil:**

## – Create 3 buckets.

![alt](../images/Screenshot%20from%202022-06-26%2001-09-44.png)

## – Enable Versioning for them.

![alt](../images/Screenshot%20from%202022-06-26%2001-18-10.png)

## – Upload a file into bucket-1 then copy it from bucket-1 into bucket-2 & bucket-3.
![alt](../images/Screenshot%20from%202022-06-26%2001-33-20.png)
![alt](../images/Screenshot%20from%202022-06-26%2001-35-49.png)

## – Delete the file from bucket-1

![alt](../images/Screenshot%20from%202022-06-26%2001-37-41.png)

### **2. Host a static website on a standard public GCS bucket [hint: link].**

![alt](../images/Screenshot%20from%202022-06-26%2002-23-36.png)

### **3. Deploy MySQL private instance and connect to it then create a new database.**
> ### Create  database instance
![alt](../images/Screenshot%20from%202022-06-26%2018-24-16.png)

> ### Create VM to connect to database from it
![alt](../images/Screenshot%20from%202022-06-26%2018-25-45.png)

> instal Cloud SQL Auth proxy on vm and connect to database
 
 ```bash
      sudo apt-get update
      sudo apt-get install mariadb-server-10.5
      sudo service mysqld stop
      sudo apt-get install wget
      wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
      chmod +x cloud_sql_proxy
    ./cloud_sql_proxy -instances=stellar-river-354117:us-east1:mysql-iti=tcp:3306
```
Open other terminal connect to it

```bash
mysql -u root -p --host 127.0.0.1 --port 3306
```

>> create database test


![alt](../images/Screenshot%20from%202022-06-26%2018-23-18.png)



