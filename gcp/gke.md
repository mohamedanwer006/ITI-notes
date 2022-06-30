# GKA
## types 
- auto pilot mode
 > you pay for workloads only
- standard mode

> cluster manage by cloud provider

> workloads means services run in cluster

## advantages of GKE
- Automatic scaling
- load balancing
- Node pools
- Automatic upgrades
- Node auto-repair
- logging and monitoring

Types of GKE Cluster
>there is no multi region cluster

- Zonal clusters (single zone or multiple zones)
  - A single zone cluster is a cluster that is located in a single zone.
  - A multi-zone cluster is a cluster that is located in multiple zones.
  > control plane running in single zone and nodes running in multiple zones 
- Regional clusters 
  - multiple replicas of clusters in the same region
  - but in multi zones
  - every zone has its own control plane and nodes

## Control plane version

- GKE version  from kubernetes version (community version)
- Community version 

### Control plane version
- Release channel upgrade automatic 

## node pools
like template for node

## Surge upgrade
update for nodes
control what number of vm to delete and number to be upgraded


## Authorized networks
who have access to the 
cluster control plane
