# vm
## machine type
- n1-standard-1
- g1-small
- f1-micro 

## Boot disk
- size
- image (ubuntu, centos, windows)

- type
  - standard hdd
  - ssd
  - Extreme persistent disk fast one
  - balanced persistent disk  between ssd and hdd

> by default any disk is encrypted. you chose to generate a key. and you can use it to decrypt the disk. 
> select custom manage or google managed

- snapshot schedule
> regularly snapshot the disk.(backup)

- network interface
  - network (vpc)
  - subnet 
  - internal ip address
  - External ip address
  - network tags for firewall rules
  
- disks 
> additional disks

```
gcloud compute ssh --zone=us-east1-b "myitivm" --project="Anwer-gcp"
```

## access vm that doesn't have external ip address

### bastion host 
### identity aware proxy (IAP) zer0 trust networking

```
give role IAP secure tunnel to user
```

give CIDR of GCP IAP
to firewall rules
to make sure that only IAP can access the vm

 

