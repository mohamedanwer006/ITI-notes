# GCP Storage 

## Cloud Storage

## Storage classes

### Standard storage
used for frequently accessed data 
(hot data)

### Nearline  storage
best for infrequently accessed data like backups and or data archives
(once per month)

### Coldline storge 

a low cost storage for data that is infrequently accessed and long-lived 
(once per 90 days)

### Archive storage
the lowest cost storage for data that is infrequently accessed and long-lived best choice for data that is access less than (one a year)

----

All storages have 
- unlimited storage no min object size
- worldwide accessibility and locations
- low latency and high durability
- a uniform experience - same API and tools
- geo-redundant storage
- pay only for what you use
- use https/tls 

> usse sdk gsutil  tool to manage storage

### cloud storage integrate with other google cloud services

- export tables from both bigquery and cloud sql to cloud storage

- App enginlogs 
- store instance startup scripts
- compute engine images and object 

---
## Google cloud second core storage options is
## Cloud sql
 
Fully managed relational database service for mysql, postgresql and sql server

- fully managed doesn't require any installation or maintenance
- can scale 
- supports manged backups 

- accessible  with other google cloud services or external services
- can be used with app engine standard driver like connector/j for java and mysqldb for python

---
## The third option is
## Cloud spanner
cloud spanner is a fully managed relational database service that scales horizontally and supports strong consistency and speaks sql

suited for applications that require :
- SQL relational database management system with joins and secondary indexes
- Built-in high availability and strong consistency
- Database size exceed 2TB
- High number of input/output operations per second (IOPS)

---
## The fourth option is
## Firestore
cloud firestore is a fully managed NoSQL document database that scales automatically and supports real-time data  synchronization
for mobile and web applications

- Flexible 
- Horizontally scalable
- NoSQL cloud database
- offline caching 

- From a  pricing perspective you charge for each document you store and each read and write operation you perform

- querying is also charged at the rate of one document read per query whether it returns data or not

- ingress is free and in many cases egress is free

- firestore has a free quota of 1GB of storage and 50,000 reads, 20,000 writes, and 20,000 deletes per day

----

## The fifth option and the last one is
## Cloud Bigtable

Is google's NoSQL big data database service

Bigtable is designed to handle massive workloads at consistent low latency and high throughput

it's great choice for both operational and analytical applications , icluding IoT, financial services, and user analytics

Customers often choose Bigtable for:
- they work with more than 1TB of semi-structured or structured data
- Data is fast with high throughput, or it's rapidly changing
- they work with non-relational data
- data is time-series or has natural semantic ordering
- they work with big data, running asynchronous batch or sync real-time processing on the data
- they run machine learning algorithms on the data

Cloud Bigtable can interact with other google cloud services  and third party-client 


- Using API data can be read from and written to Bigtable through a data service layer. like manage VMs , HBase Rest server, or Java server using HBase client, Typically this used to serve data to applications,dashboards, and data services

- Data can also be streamed in through a variety of popular streaming processing frameworks like Dataflow streaming, Spark streaming, and Storm

- Data can also be read from and written to Cloud Bigtable through batch processes like hadoop map reduce, Dataflow or spark


> BigQuery on the edge between data storage and data processing


