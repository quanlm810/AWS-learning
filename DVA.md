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
