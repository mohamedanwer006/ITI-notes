# network

## GCP Networking tiers
- premium
- standard
  
# VPC
### virtual private cloud
Isolate compute instances from each other

>In GCP VPC is global and does not have CIDR block.

### Create vpc

after create project there is a default vpc. 

## `create vpc` 
### subnet creation mode 
- automatic

will create subnet in the vpc in all regions.

- custom
    - select region
    - select ipv4 or ipv6
    - Add cidr block

after create subnet there is routes between subnet and other subnet.

## there there rout table 
#### one for public access
#### one for private access between subnet and other subnet.

bloc subnet one from subnet two.
## firewall rules
- name
- network (vpc)


priority is used to order the rules.

max number  means lowest priority.

high number means highest priority.

















