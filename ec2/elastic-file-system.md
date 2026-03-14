# Elastic File System

- Managed NFS (**network file system**) that can be mounted on many EC2
- EFS works with EC2 instances across AZs
- Highly available, scalable and expensive (3x gp2)
- Use cases: 
    - content management, 
    - web serving, 
    - data sharing, 
    - Wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access
- Compatible with Linux based AMI only
- Encryption at rest using KMS
- POSIX file system that has a standard file API
- Scales automatically, pay-per-use, no capacity planning

```
                +-----------------------------+
                |      Security Group         |
                |    +-------------------+    |
                |    |  EFS File System  |    |
                |    +-------------------+    |
                +-----------------------------+
                   ^           ^            ^
                   |           |            |
        +--------------+ +--------------+ +--------------+
        | us-east-1a   | | us-east-1b   | | us-east-1c   |
        |  +---------+ | |  +---------+ | |  +---------+ |
        |  |  EC2    | | |  |  EC2    | | |  |  EC2    | |
        |  |Instances| | |  |Instances| | |  |Instances| |
        |  +---------+ | |  +---------+ | |  +---------+ |
        +--------------+ +--------------+ +--------------+
```

## EFS Performance Configuration

- 1000s of concurrent NFS clients, 10 GB/s+ throughput
- Can grow to petabyte scale, drive high levels of throughput, and allow massively parallel access from compute instances

### Types

- **Regional** (Recommended): Store data redundantly acros AZs within the same region
    - production
- **One Zone**: Store data within a single AZ
    - development, backup
    - enabled by default

### Performance Modes

- Not editable after creation
- **General Purpose** (default) – latency-sensitive use cases (web server, CMS, etc…)
- **Max I/O** – higher latency, throughput, highly parallel (big data, media processing)

### Throughput Modes

- **Bursting** – throughput that scales with the amount of storage in the file system
    - Base throughput of 50 MiB/s / 1 TB storage, up to 100 MiBs / 1 TB
- **Provisioned** – specify a level of throughput that the file system can drive independent of the file system's size or burst credit balance, ex: 1 GiB/s for 1 TB storage
- **Elastic** – automatically scales throughput up or down based on your workloads
    - Up to 3GiB/s for reads and 1GiB/s for writes
    - Used for unpredictable workloads

### Storage Classes

Amazon EFS storage classes are designed for the most effective storage depending on use cases.

- **EFS Standard** – uses solid state drive (SSD) storage to deliver the lowest levels of latency for frequently accessed files. 
    - New file system data is first written to the EFS Standard storage class and then can be tiered to the EFS Infrequent Access and EFS Archive storage classes by using lifecycle management.
- **EFS Infrequent Access (IA)** – A cost-optimized storage class for data that is accessed only a few times each quarter.
- **EFS Archive** – A cost-optimized storage class for data that is accessed a few times each year or less.
    - supported on EFS file systems with Elastic throughput. 
    - Can't update file system’s throughput to Bursting or Provisioned once the file system has data in the Archive storage class.

## EBS vs EFS vs Instance Store

### Elastic Block Storage

- EBS volumes…
    - Attached to one instance (except multi-attach on io1/io2)
    - Locked to the Availability Zone (AZ)
    - gp2: IO increases if the disk size increases
    - gp3 & io1: can increase IO independently
- To migrate an EBS volume across AZ
    - Take a snapshot
    - Restore the snapshot on another AZ
- EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
- Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (you can disable that)

### Elastic File System

- Mounting 100s of instances across AZ, locked to the region
- Only for instances running POSIX systems
- More expensive than EBS
    - Can leverage Storage Tiers for cost savings

### Instance Store

- Physically attached to the host computer. 
- Can’t detach an instance store volume from one instance and attach it to a different instance.
- Exists only during the instance lifetime
