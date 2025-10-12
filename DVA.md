# AWS DVA
## Getting Started
### How do you choose a AWS region?
- Compliance with data government and legal requirement
- Proximity to reduce latency
- Service availability
- Pricing
### AWS Availability zone (AZ)
- Each region have many availability zones (min is 3, max is 6). Example:
    - ap-southeast-2a
    - ap-southeast-2b
    - ap-southeast-2c
- Each AZ have one or more discrete data centers with power, network and connectivity
- These AZs are seperated from each other, so that they are isolated from disaster
### Global services
- AWS have global services: IAM, Route 53, CloudFront, WAF
- Most AWS services are region-scoped
## IAM & AWS CLI
### IAM introduction
- IAM (Identity and Access Management) is a global service
- Root user is created by default, shouldn't be used or shared
### User & Group
- Group can only contains users, not other groups
- Users can belong to multiple groups
### IAM permissions
- Users and Groups can be assigned JSON documents call policies
- The policies define permissions of the user
- **The least privilege principle**: don't give user more permissions then needed (best practice)
- AWS now have multi session support allow multiple account login
### IAM policies
- A user can have policies from groups or inline polices attach directly
- IAM policies includes:
    - Version (always display 2012-10-17)
    - Id (optional)
    - Statement (required one or more)
- IAM policies statements include:
    - Sid (optional)
    - Effect (Allow, Deny)
    - Principle: Account/user/role this policy applied to
    - Action: List of this action in effect
    - Resource: List of resource in effect
    - Condition: Condition to apply policies (optional)
- IAM password policy:
    - Set minimum password length
    - Set specific character types
    - Allow IAM users to change password
    - Set up password rotation
    - Prevent password reuse
### MFA
- MFA = password you know + security device you own
- MFA devices options:
    - Virtual MFA device (Authenticator application)
    - Universal 2nd Factor Security Key (U2F)
    - Hardware key FOB MFA device
### AWS Access Key, CLI and SDK
- Three options to access AWS:
    - AWS management console (protected by password + MFA)
    - AWS command line interface (protected by access keys)
    - AWS software development kit (protected by access keys)
- Access keys are generated through the AWS console
- Users manage their own access keys (access key id ~ username, secret access key ~ password)
### AWS CLI
- A tool enables you to interract with AWS service using command line shell
- Direct access to AWS public services
- Used to develop script to manage resources
### AWS SDK
- Language specific APIs
- Enables you to access and manage AWS services programatically
- Embedded with application
### IAM Roles for Services
- IAM Roles are used to assign permission for AWS services
### IAM Security Tools
- IAM Credentials Report (account-level): List all account's user and status
- IAM Access Adviser (user-level): Show service permissions granted to a user and last access (used to revise permission)
- **Access Adviser** has been rename to **Last Access**
### IAM Guildlines & Best Practices
- Don't used root account for anything except for account setup
- One physical user = One AWS user
- Assign user and permission to groups
- Create strong password policy
- Enforce the use of MFA
- Create roles to give permission to AWS services
- Use access keys for SDK/CLI
- Use IAM Security Tools to audit permissions
- Don't share IAM users and access keys
## AWS EC2
- Elastic Compute Cloud (EC2) is a Infra as a Service
- Main capabilities:
    - Renting virtual machines (EC2)
    - Storing data on virtual drives (EBS)
    - Distributing load accross machines (ELB)
    - Scaling service using auto scaling group (ASG)

### AWS EC2
- Sizing and configuration options:
    - Operation system (OS): Linux, Windows and Mac OS
    - How much compute power & core (CPU)
    - How much random access memory (RAM)
    - How much storage space:
        - Network attached (EBS & EFS)
        - Hardward attached (EC2 Instance Store)
    - Network card: network speed and Public IP address
    - Firewall rules: Security group
    - Bootstrap script: EC2 User Data
### EC2 User Data
- A user input script that will be run ONCE on the FIRST time the instance start
- EC2 User Data is used to automate boot tasks such as:
    - Install update
    - Install software
    - Download common files from the internet
    - ...
- EC2 User Data will run with root user right
### EC2 Instance types
- AWS offering 7 types of EC2 Instance
- Naming convention: (m5.2xlarge)
    - m is the instance class
    - 5 is the generation of the instance
    - 2xlarge is the size of the instance
- **General purpose**
    - Great for variety of workloads such as web server and code repository
    - Balance compute, memory and networking
- **Compute optimized**
    - Great for compute intensive tasks require high performance processing: batch processing workloads, media transcoding, high performance webservers, high performance computing, scentific modeling/machine learning, dedicated gaming servers
- **Memory optimized**
    - Fast performance for workloads that process large datasets in memory
    - Great for high perfomance relational/non-relational datbases, distributed web scale cache stores, in-memory database optimized for BI, realtime processing of big unstructured data
- **Storage optimized**
    - Great for task that require high, sequential read and write access to large dataset on local storage
    - Use case: high frequency OLTP systems, relational &amp; no-sequel datbases, cache for in-memory databases, dataware housing application, distributed file systems
### Introduction to security groups
- Security groups is the fundamental for doing network security in AWS
- Control how trafic is allowed into (inbound network) or out (outbound network) of EC2 instances (regulate access to ports, authorize IP ranges)
- Security groups only include **ALLOW** rules
- Security groups can reference IP address or other security groups
- By default EC2 instances allow all outbound network traffic and block all inbound traffic
- Can be attached to multiple instances
- Locked down to a region/VPC combination
- Does live "outside" the EC2 instance, if a traffic is blocked the EC2 instance will not see the traffic
- It's good to maintain a seperate security group for SSH access
- If the application is not accessible (timeout), then it is a security group issue
- If the application gives a "connection refused" errro, then it is **NOT** a security group issue
### Classic port to know
- SSH (Secure Shell) - log into a linux instance: 22
- FTP (File transfer protocol) - upload files into a file share: 21
- SFTP (Secure file transfer protocol) - upload file using SSH: 22
- HTTP - access unsecured websites: 80
- HTTPS - access secured websites: 443
- RDP (Remote Destop protocol)  - log into a window instances: 3389
### SSH Overview
- Base on different OS there are different ways to connect to remote shell:
    - Mac, Linux and Window >= 10 can use SSH to connect
    - Window < 10 can use Putty to connect
    - In addition EC2 Instance Connect can be used with any browser to connect
### Purchasing options
- On-demand instances: short workload, predictable pricing, pay by second
- Reserved (1 &amp; 3 years):
    - Reserved instances: long workload
    - Convertible reserved instances: long workload with flexible instances
- Saving plans (1 &amp; 3 years): commitment to an amount of usage, long workload
- Spot instances: short workloads, cheap, can lose instance (less reliable)
- Dedicated host: book an entire physical server, control instance placement
- Dedicated instances: no other customer will share your hardware
- Capacity reservations: reserve capacity in a specific AZ for any duration
#### EC2 on-demand:
- Pay for what you use: Linux or Window (bill per second after 1st minutes), other OS (bill per hour)
- Highest cost but no upfront payment
- No long term commitment
- Suitable for short-term and interrupted workloads
#### Reserved instances:
- Up to 72% discount compared to on-demand
- Reserve a specific instance attributes (Instance type, Region, Tenancy, OS)
- Reservation period of 1 years (less discount) or 3 years (more discount)
- Payment option: No Upfront (less discount) - Partial Upfront - All Upfront (more discount)
- Reserved instance scope: Regional or Zonal
- Suitable for steady state usage applications
- Can be buy and sell at Reserved Instance Marketplace

**Convertible reserved instance**
- Can change the instance type, instance family, OS, scope and tenancy 
- Up to 66% discount
#### Saving plans
- Get a discount based on long term usage (upto 72%)
- Commited to a certain type of usage
- Usage beyond Saving plans is billed as On-demand price
- Locked to a specific instance family &amp; AWS region
- Flexible on instance size, OS, Tenancy
#### Spot instances
- Can get discount up to 90% compared to On-Demand
- Instance can be lost at any point if your max price is less then the current spot price
- The MOST cost effective instance in AWS
- Useful for workloads that are resilient to failure: batch jobs, data analysis, image processing, any kind of distributed workloads
- Not suited for critical jobs or databases
#### Dedicated hosts
- A physical server with EC2 instance capacity fully dedicated to your use
- Allow addressed of compliance requirements and use your existing server-bound software liences
- Purchase options:
    - On demand: pay per second for active Dedicated Host
    - Reserved: 1 &amp; 3 years (No Upfront, Partial Upfront, All Upfront)
- Most expensive option
#### Dedicated instances
- Instance run on hardware that is dedicated to you
- May share hardware with other instances in same account
- No control over instance placement
#### Capacity reservations
- Reserve On-Demand instances capacity in a specifc AZ for any duration
- Always get access to the capacity whenever needed
- No time commitment, no billing discounts
- Combine with Regional Reserved instances and Saving Plans to benefit from billing discounts
- Charged with On-Demand rate whether you run the instances or not
- Suitable for short term uninterrupted workloads that needs to be in a specific AZ

## EC2 Instance Storage
### EBS Volume
- Elastic Block Store is a network attached drive that can be connect to EC2 instance:
    - Using network to communicate -> some latency
    - Can be easily detach/attach to EC2 instances
- Allow data to persist even after EC2 instance is terminated
- Most EBS can be attached to one instance at a time (some EBS have multi-attach feature)
- They are bound to AZ
    - To move EBS Volume accorss AZ a snapshot need to be created
- Have a provisioned capacity (size in GBs, and IOPS)
    - Get billed for the capacity
    - Can be increase over time
- Delete on termination: The EBS will be terminated along side the EC2 instance (automatically ticked for root volume)
### EBS Snapshot
- Make a backup for EBS volume at any point in time
- Not neccessary to detach EBS before taking snapshot, but it is recommended
- Can be copied accross AZ and Region
#### EBS Snapshot Features
- EBS Snapshot Archive: Move snapshot to an "archive tier" which is 75% cheaper (Take 24 - 72 hrs for restoring)
- Recycle bin for EBS Snapshot: Allow setup rules to retain deleted snapshot (1 day - 1 year) so it can be recorvered once deleted
- Fast Snapshot Restore (FSR): force full initialization of snapshot to have no latency on first use (cost money)
### AMI (Amazon Machine Image)
- An AMI is a customization of an EC2 instance
    - Inlcude software, configuration, OS, monitor, ...
    - Faster boot/configuration time because all softwares are pre-packaged
- AMI are built for specific region (can be copied accross regions)
- Types of AMI:
    - Public AMI (AWS provided)
    - Your own AMI (self-make and maintain)
    - AWS marketplace AMI (someone else make and sell it)
#### AMI process
- Start an EC2 instance and customize it
- Stop the instance (for data integrity)
- Build an AMI - This will also create an EBS snapshots
- Launch instances from other AMIs
### EC2 Instance Store
- EBS volumes are network drives with good but "limited" performance
- EC2 Instacne Store is a hardward disk attached directly to EC2
- Better I/O performance (can read upto 3M read/ 2M write IOPS)
- Lost if they are stopped (emphemeral)
- Good for buffer/cache/scratch data/temporary content
- Risk losing data if hardware fails
- Backup and Replication are your responsibility
### EBS Volume types
- Come in 6 different types:
    - gp2/gp3 (SSD): General purpose SSD volume that balance price and performance for a variety of task
    - io1/io2 Block Express (SSD): Highest perfromance SSD volumes for mission-critical low-latency or high-throughput workloads
    - st1 (HDD): Low cost HDD volume designed for frequent accessed, throughtput intensive workloads
    - sc1 (HDD): Lowest cost HDD volume designed for less frequent accessed workloads
- Volumes are characterized in Size|Throughput|IOPS
- Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes
#### General purpose SSD:
- Cost effective storage, low latency
- Use for system boot volumes, virtual desktops, development and test environment
- Size: 1Gb - 16Tbs
- gp3:
    - Baseline of 3000 IOPS and throughput of 125MiB/s
    - Can increase IOPS up to 16000 and throughput up to 1000MiB/s independently
- gp2:
    - Small gp2 volumes can burst IOPS to 3000
    - Size of volume and IOPS are linked, max IOPS is 16000 (3 IOPS per GiB)
#### Provisioned IOPS SSD:
- Use for critical business applications with substained IOPS performance
- Use for application that need more than 16000 IOPS
- Great for database workloads (sensitive to storage performance and consistency)
- io1 (4GiB - 16TiB):
    - Max PIOPS: 64k for Nitro EC2 instance &amp; 32k for other
    - Size of volume and IOPS are independent
- io2 Block Express (4GiB - 64TiB):
    - Sub milisecond latency
    - Max PIOPS: 256k (1k IOPS per GiB)
- Support EBS Multi-attach
#### Hard disk drives (HHD):
- Cannot be used as boot volume
- Size: 125 GiB - 16TiB
- Throughput Optimized HDD (st1):
    - Use for big data, datawarehouses, log processing
    - Max throughput of 500 MiB/s - max IOPS 500
- Cold HDD (sc1):
    - For data with infrequently accessed (archived data)
    - Use for scenerios require the lowest cost possible
    - Max throughput of 250 MiB/s - max IOPS 250
### EBS multi-attach - io1/io2 family
- Allow EBS to attach to multiple EC2 instances in the same AZ
- Each instance will have full read/write permission
- Use case:
    - Archieve higher application availability
    - Application must manage concurrent write operations
- Upto 16 EC2 instances at a time
- Must use a file system that is cluster-aware
### EFS - Elastic file system
- Managed NFS (network file system) that can be mounted to many EC2 instances
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive, pay per use
- Use cases: content management, web serving, data sharing, Wordpress
- Uses security group to control access to EFS
- Only compatible with Linux base AMI (not Windows)
- Encryption at rest using KMS
- File system scale automatically, pay per use, no capacity planing
#### Performance &amp; storage classes
- EFS scale:
    - 1000s of concurrent NFS clients, 10 GB+/s throughput
    - Grow to PB-scale automatically
- Performance mode (set at creation time):
    - General purpose (default): for latency sensitive use-cases (web server, CMS, etc...)
    - Max I/O: higher latency, throughput, highly parallel (big data, media processing)
    - Throughput Mode:
        - Bursting: 1Tb = 50MB/s + burst up to 100MB/s
        - Provision: Set throughput regardless of storage size
        - Elastic: Automatically scales throughput up or down based on your workloads
- Storage classes:
    - Storage tier (lifecycle management feature - move file after N days):
        - Standard: for frequently accessed files
        - Infrequent access (EFS-IA): cost to retrieve files, lower storage price
        - Archive: rarely accessed data (few times each year), 50% cheaper
    - Implement live cycle policy to move file between storage tiers
    - Availability and durability:
        - Standard: Multi AZ, great for production
        - One zone: One AZ, great for dev, back up enabled by default, compatible with IA
### EBS vs EFS
- EBS volumes:
    - Attach to one instance at a time (except multi-attach io1/io2)
    - Are lock at AZ level
    - Migrate accross AZ using snapshot
    - Root EBS volume will terminate by default if the EC2 instance is destroyed
- EFS:
    - Mount up to 100s of instances accross AZ
    - Only for linux instance
    - EFS have higher price point than EBS
    - Can leverage storage tier for cost saving

## ELB &amp; ASG
### Scalability
- Scalability means that your system can handle a greater load by adapting
- There are two types of scalability:
    - Vertical scalability
    - Horizontal scalability (elasticity)
- Scalability is linked to but different to High Availability
#### Vertical scalability
- Vertical scalability means increasing the size of the instance
- Vertical scalability is very common for non-distributed systems, such as a database
- RDS, ElasticCache are services that can scale vertically
- There a limit to how much you can scale (hardware limit)
#### Horizontal scalability
- Horizontal scalability means increasing the number of instances/systems for your application
- Horizontal scalability implies distributed systems
- Very common for web application/modern applications
### High availability
- High availability means running your application on at least 2 data centers
- The goal of high availability is to survive a data center loss
- High availability can be passive (for RDS mutli AZ) or active (for horizontal scaling)
### Load balancing
- Load balancers are servers that forward traffic to multiple servers downstream
- Allow spreading load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failure of downstream instances
- Do regular health checks to your instances
- Provide SSL termination for your websites
- Enforce stickiness with cookies
- High availability across zones
- Seperate public traffic from private traffic
#### Elastic load balancer
- Elastic load balancer is a managed load balancer
- It cost less to setup your own load balancer but require more effort on your end
- Integrated with AWS services/offerings
#### Health checks
- Health checks are crucial for load balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and route
#### Types of load balancers
- There are 4 types of load balancers:
    - Classic load balancer (v1 - old generation) - 2009 - CLB
    - Application load balancer (v2 - new generation) - 2016 - ALB (for HTTP, HTTPS, WebSocket)
    - Network load balancer (v2 - new generation) - 2017 - NLB (for TCP, TLS, UDP)
    - Gateway load balancer (v2 - new generation) - 2020 - GWLB (operate at network level, IP protocol)
- Load balancer can be setup as internal (private) or external (public)
