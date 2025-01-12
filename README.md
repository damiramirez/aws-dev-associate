# aws-dev-associate
Repo for https://learn.cantrill.io/p/aws-certified-developer-associate

## AWS Accounts

### Account

An AWS Account is a container for identities (users) and Resources.

- You need an account name, unique email address and credit card
- There is a unique Root user - Full control over all of the AWS Account
- Resource bill to the AWS Account payment method as they are consumed
- By default all access to an AWS Account and resources is denied except for the Root User -> You can granted access to external identities

You can create additional identities inside the AWS Account that can be restricted

- IAM users, Groups and Roles
- Any IAM identity starts off with no permissions

A good practice is to use separate accounts for separate things (DEV, TEST, PROD) or teams or products or client. Another good practice is not to use the root account as it has full privileges. Instead, it is recommended to create other identities using IAM (Identity and Access Management) and grant them the necessary permissions to perform their tasks.

### MFA

AWS Multi-Factor Authentication (MFA) is a simple best practice that adds an extra layer of protection on top of your user name and password. With MFA enabled, when a user signs in to an AWS Management Console, they will be prompted for their user name and password (the first factor—what they know), as well as for an authentication code from their AWS MFA device (the second factor—what they have). Taken together, these multiple factors provide increased security for your AWS account settings and resources.

### IAM

*Manages identities - Authenticate - Authorize*

IAM is what allows additional identities to be created within an AWS account - identities which can be given restricted levels of access. IAM identities start with no permissions on an AWS Account, but can be granted permissions (almost) up to those held by the Account Root User.

With IAM you can create:

1. IAM User - Human or application that need access to your account
2. IAM Group - Collection of related users (development team, finance or HR)
3. IAM Role - Use by AWS Service or for granting external access to your account

IAM Policy Document can be used to allow or deny access to AWS Services only when they are attached to Users, Groups and Roles.

There is no cost associated with creating users, groups or roles but there is a limit. IAM is a global resilient service. Only controls what identities can do via policies. No direct control on external accounts or users. Identity federation(Facebook, Twitter, Google, etc) and MFA.

### IAM Access Keys

Access keys are how the AWS Command Line Tools (CLI Tools) interact with AWS accounts.

- Access Key ID
- Secret Access Key

## AWS Fundamentals
*Introduction to AWS*

### Public vs Private Services

A public service is something that is accesible using public endpoints (S3). A private service is something which runs within a VPC, so only things within that VPC can access the service. Anyways, there are permissions, so you can access VPCs only if configured and connect from VPC to internet via IGW (Internet GateWay).

### AWS Global Infrastructure

AWS have created a [global public cloud platform](https://aws.amazon.com/es/about-aws/global-infrastructure/regions_az/) which consists of isolated 'regions' connected together by high speed global networking.

- AWS Regions: Area of the world that they have selected and inside this region is a full deployment of AWS Infrastructure. Regions are geographically spread, this means that we can use theses regions to design system which can withstand global level disasters.
- AWS Edge Locations: Smaller than regions, generally only have content distribution services, as well as some types of edge computing. Really useful to allow for fast efficient data transfer (cache).
- AWS Availability Zone: AWS Regions are divided into multiple AZs (One or more data centers), which are isolated data centers with their own power, networking, and cooling infrastructure. Deploying resources across multiple AZs within a region ensures redundancy and fault tolerance, as failures in one AZ do not affect others.

**Resilience:** Ability of a system or application to withstand and recover from failures, disruptions, or unexpected events while maintaining its functionality and providing reliable services.

- Globally Resilient: Service operates globally with a single database and its data is replicated across multiple regions inside AWS. Region can fail and the service continues running (IAM, Route53).
- Region Resilient: Service which operate in a single region with one set of data per region. Replicate data in different AZs.
- AZ Resilient: Service that run form a single AZ, if the AZ fails then the service will fail.

### Virtual Private Cloud - VPC

VPC is the service that you will use to create private networks inside AWS. Other private services will run from. You create within a specific region. VPCs are region resilient, operate from multiple AZ in a specific AWS Region. VPCs are private and isolated unless you decide otherwise.

A default VPC is created once per region when an AWS account is first created. There can be only one per region and can have many custom VPC per region.

**Default VPC:** If anything needs to communicate with a VPC and assuming that is allowed, it needs to communicate to that VPC CIDR, and outgoing connections will initially originate from somewhere in that VPC CIDR. `CIDR: 172.31.0.0/16`.

Each subnet inside of VPC is located in one AZ. Each subnet use part of the VPCs range of IP address, its CIDR range. /20 Subnet in each AZ in the region. It came with a Internet Gateway, a default VPC security group and network ACL or NACL. Subnets assign a public IPv4 address.

### Elastic Compute Cloud - EC2

It allows you to provision virtual machines known as instances with resources you select and an operating system of your choosing. 

Is a IaaS service, provides access to VMs known as EC2 instance. Private service, run in the Private AWS Zone. The instance is configured to lunch into a single VPC Subnet. You can configure the public access, VPC must to be public too. 

- AZ Resilient - Instance fails if AZ fails
- Different instance sizes and capabilities
- AWS handle the virtualization, physical hardware, networking, storage and facilities.
- On-Demand Billing - Per Second
- Local on-host storage or Elastic Block Store (EBS)

There are a few states that an instance can be in:
- Running - Pay for the 4 things
- Stopped - Only pay for storage
- Terminated - Not pay

**AMI:** is an image of an EC2 instance, can be used to create an EC2 instance or an AMI can be created from a EC2 instance. It contains attached permissions, boot volume of the instance and block device mapping (links the volume).

You can connect to the EC2 with SSH Key Pair or Remote Desktop. If you use Key Pair, you need to download the Private Key and use it to connect via ssh `ssh -i "PrivateKey.pem user@dnsinstance`

### Simple Storage Service - S3

It provides a near infinitely scalable object storage platform, storing each file as an entity called an object. It charges only for what is used.

Accessible from anywhere (Global) with a public internet connection. It tends to be the default storage location for data ingestion and output for many AWS services.

**Objects** (or files) are stored in folders called buckets. These buckets can be public or private. Files sizes go from Zero bytes to 5TB. Objects has Version ID, Metadata, Access Control and Subresources.

**Buckets** are created in a specific AWS Region, data that is inside the bucket has a primary home region and never leaves that region, unless you configure it. Bucket is identified by his name and need to be globally unique. Flat structure, all objects stored within the bucket at the same level, there is no file system.

S3 Patterns and Anti-Patterns
- S3 is an object store (Audio, Image, etc) - not file or block
- You can't mount a S3 Bucket - You can use EBS
- Great for large scale data storage, distribution or upload
- Great for offload
- Input and/or Outputs to many AWS products

### CloudFormation - CFN

CloudFormation is an Infrastructure as Code (IaC) product in AWS which allows automation infrastructure creation, update and deletion.

Templates created in YAML or JSON can be used to automate infrastructure operations. Templates are used to create stacks, which are used to interact with resources in an AWS account.

Components of a template:

- List of resources - mandatory
- Template Format Version
- Description - must go after template format version
- Metadata
- Parameters - configuration of resources
- Mappings - key/value pairs which can be used for lookups
- Conditions
- Outputs

When you upload a template, it will be save on a S3 bucket.

### CloudWatch - CW

CloudWatch is a core supporting service within AWS which provides metric, log and event management services. It's used through other AWS services for health and performance monitoring, log management and nerveless architectures.

- Collects and manages operational data
  - Metrics - AWS Products, Apps, on-premises
  - CloudWatch Logs - Aws Products, Apps, on-premises
  - CloudWatch Events - AWS Services and Schedules

For external metrics or logs, you need to install the CloudWatch Agent.

**Namespace:** Is a container for metrics. It provides a way to organize and categorize related metrics within CloudWatch.
**Metric:** Collection of related data points in a time ordered structure.
**Datapoint:** A specific measurement captured by a monitored service or resource. Consist in a Timestamp and a value.
**Dimension:** Name values pairs to separate things. It separate datapoints for different things or perspectives within the same metric.

CloudWatch also allows us to take action based on metrics. **Alarms** are created and linked to a specific metric. You can set a criteria, so when the status is not in a good state, you can define an action.

### Shared Responsibility Model

The Shared Responsibility Model - is how AWS provide clarity around which areas of systems security are theirs, and which are owned by the customer.

AWS is responsible for the security **of** the cloud
- Protecting the infrastructure, physical data centers, and the global network that AWS operates. Regions, AZ and Edge Location.
- AWS manages the security and compliance of the underlying infrastructure, such as the servers, storage, and networking components.
- AWS provides a secure foundation with built-in security features and services, including identity and access management, network security, encryption, and compliance certifications.
- AWS regularly updates and patches the infrastructure and services to protect against known vulnerabilities and threats.

Customers are responsible for security **in** the cloud, 
- Securing their applications, data, and operating systems running on AWS services.
- Customers are responsible for implementing appropriate access controls, managing user identities, and defining permissions and roles for their AWS resources.
- Customers must configure and manage their own operating systems, applications, and network settings to ensure security.
- Customers are responsible for protecting their data, implementing encryption, and managing backups and disaster recovery.

![Shared Responsibility Model](./assets/SharedResponsabilityModel.png)

### High Availability vs Fault Tolerance vs Disaster Recovery

**High Availability:** Ensure an agreed level of operational performance, usually uptime, for a higher than normal period. Minimizes downtime and ensures the continuous availability of services. HA architectures typically involve redundant hardware, load balancing, and automatic failover mechanisms to ensure service continuity.

**Fault Tolerance:** Property that enables a system to continue operating properly in the event of the failure of some of its components. FT systems are designed to detect and recover from failures without disrupting ongoing operations. This is typically achieved through redundant components, where backup systems or resources are available to seamlessly take over in case of failure. The goal is to eliminate single points of failure and maintain service availability without interruption.

**Disaster Recovery:** Set of policies, tools and procedures to enable the recovery or continuation of vital technology infrastructure and systems following a natural or human-induced disaster. Designed to keep the crucial and non replaceable parts of your system safe. DR plans typically involve creating backups, replicating data to off-site locations, and establishing procedures to restore operations and minimize downtime. The focus is on recovering systems and data to a functional state after a significant disruption.

- HA - Minimize any outage
- FT - Operate through faults
- DR - Used when these don't work

### Route53 - R53

It allows users to manage the domain names and the routing of internet traffic to various resources, such as AWS services, EC2 instances, S3 buckets, load balancers, and other endpoints. Is a global service with a single database, you don't need to pick up a Region. Globally Resilient.

DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.

In AWS, the most common records are (will be on exam):

- A: URL to IPv4
- AAAA: URL to IPv6
- CNAME: URL to URL
- ALIAS: URL to AWS resource

Route 53 can use:

- Public domain names you own
- Private domain names that can be resolved by your instances in your VPCs

Route53 has advanced features such as:

- Load balancing (through DNS - also called client load balancing)
- Health checks (although limited…)
- Routing policy: simple, failover, geolocation, geoproximity, latency, weighted

Prefer Alias over CNAME for AWS resources (for performance reasons)

## IAM, Accounts and AWS Organizations

