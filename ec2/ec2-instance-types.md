# EC2 Instance Types

Different types of EC2 instances optimised for different use cases 
- https://aws.amazon.com/ec2/instance-types/

## Naming Convention

`m5.2xlarge`
- **m**: instance class
- **5**: generation
- **2xlarge**: size within the instance class

## General Purpose

- Balanced between compute, memory and networking.
- `t3.micro` used throughout the course is a general purpose EC2 instance

## Compute Optimized

Optimized for compute intensive tasks (CPU bound)
- Batch processing workloads
- High performance web servers
- Media transcoding
- High performance web servers
- High performance computing (HPC)
- Scientific modeling & machine learning
- Dedicated gaming servers

## Memory Optimized

Optimized for workloads that process large data sets in memory
- High performance, relational/non-relational databases
- Distributed web scale cache stores
- In-memory databases optimized for BI (business intelligence)
- Applications performing real-time processing of big unstructured data

## Storage Optimized

Optimized for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
- High frequency online transaction processing (OLTP) systems
- Relational & NoSQL databases
- Cache for in-memory databases (for example, Redis)
- Data warehousing applications
- Distributed file systems

## Accelerated Computing

Use hardware accelerators, or co-processors, to perform functions more efficiently. 
- Floating point number calculations,
- Graphics processing
- Data pattern matching

## HPC Optimized

Optimized to offer the best price performance for running HPC workloads at scale. 
- Large, complex simulations
- Deep learning workloads

## Example

| Instance      | vCPU | Mem (GiB) | Storage              | Network Performance    | EBS Bandwidth (Mbps) |
|---------------|------|-----------|----------------------|-----------------------|----------------------|
| t2.micro      | 1    | 1         | EBS-Only             | Low to Moderate       |                      |
| t2.xlarge     | 4    | 16        | EBS-Only             | Moderate              |                      |
| c5d.4xlarge   | 16   | 32        | 1 x 400 NVMe SSD     | Up to 10 Gbps         | 4,750                |
| r5.16xlarge   | 64   | 512       | EBS Only             | 20 Gbps               | 13,600               |
| m5.8xlarge    | 32   | 128       | EBS Only             | 10 Gbps               | 6,800                |