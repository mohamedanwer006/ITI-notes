# Lab 3.3
## 1.Using gcloud & Docker:
- ### **Configure Docker & gcloud to work with GCR of your project.**
> enable Container Registry api
![alt](../images/Screenshot%20from%202022-06-26%2019-07-59.png)

- ### **Push Nginx docker image to GCR (make the image private).**
> configure docker to use gcr.io

```
└─ mohamed@DevOps:$ gcloud auth configure-docker gcr.io
Adding credentials for: gcr.io
After update, the following will be written to your Docker config file located at [/home/mohamed/.docker/config.json]:
 {
  "credHelpers": {
    "gcr.io": "gcloud"
  }
}

Do you want to continue (Y/n)?  y

Docker configuration file updated.
```

>> tag with  gcr.io/stellar-river-354117/mynginx:1.1

```bash
┌─~
└─ mohamed@DevOps:$ docker tag nginx:alpine  gcr.io/stellar-river-354117/mynginx:2.0
┌─~
└─ mohamed@DevOps:$ docker push gcr.io/stellar-river-354117/mynginx:2.0
The push refers to repository [gcr.io/stellar-river-354117/mynginx]
889ab273892d: Layer already exists 
9ec529c881ef: Layer already exists 
5da18141d115: Layer already exists 
97a932510759: Layer already exists 
c1dc3f68a3a7: Layer already exists 
24302eb7d908: Layer already exists 
2.0: digest: sha256:20a1077e25510e824d6f9ce7af07aa02d86536848ddab3e4ef7d1804608d8125 size: 1568
```
![push](../images/Screenshot%20from%202022-06-26%2019-33-46.png)
- ### **Pull this image into a k8s setup or on a VM (hint: attach a SA on ur vm or gke with correct iam role).**

> create a autopilot gke cluster
![alt](../images/Screenshot%20from%202022-06-26%2019-53-43.png)

> create deployment with mynginx:2.0 image

![alt](../images/Screenshot%20from%202022-06-26%2019-55-06.png)
![alt](../images/Screenshot%20from%202022-06-26%2019-56-12.png)
> Expose

![alt](../images/Screenshot%20from%202022-06-26%2020-03-02.png)
![alt](../images/Screenshot%20from%202022-06-26%2020-02-43.png)

## Using Cloud Functions:
### **Create a Function that runs whenever a file is uploaded to a cloud storage bucket.**

![alt](../images/Screenshot%20from%202022-06-26%2020-20-53.png)
![alt](../images/Screenshot%20from%202022-06-26%2020-23-51.png)

> test upload draw.png
![alt](../images/Screenshot%20from%202022-06-26%2020-27-09.png)

![alt](../images/Screenshot%20from%202022-06-26%2020-27-56.png)

## 3 -Using Cloud Run
- ### **Run a pre-built docker image (pulled from GCR)**
> create a cloud run service and  use the image you just pushed mynginx:2.0

![alt](../images/Screenshot%20from%202022-06-26%2020-48-45.png)
![alt](../images/Screenshot%20from%202022-06-26%2020-48-56.png)

- ### **Build and Run any sample app**
> create simple nodejs app
![alt](../images/Screenshot%20from%202022-06-26%2020-55-09.png)
>deploy from local folder
![alt](../images/Screenshot%20from%202022-06-26%2021-00-26.png)
![alt](../images/Screenshot%20from%202022-06-26%2021-03-21.png)
> test app
![alt](../images/Screenshot%20from%202022-06-26%2021-03-06.png)

## Using App Engine:
### **Run the sample hello-world python app**
> Enable cloud build api
> create app engine service
![alt](../images/Screenshot%20from%202022-06-26%2021-10-37.png)

> clone simple flask app
>> deploy app
![alt](../images/Screenshot%20from%202022-06-26%2021-34-21.png)

> test app gcloud app browse
![alt](../images/Screenshot%20from%202022-06-26%2021-35-58.png)
