The below notes were taken while going through the AWS Cloud Practitioner Certification Preparation Learning Path on
Cloud Academy

[TOC]

# What is Cloud Computing? 

Cloud computing is a remote virtual pool of on-demand shared resources offering Compute, Storage, Database and Network
services that can be rapidly deployed at scale.

**Virtualization** - The ability to run multiple VM's, possibly running different OS', on one physical server.
* Hardware is shared among the VM's - this sharing controlled by the Hypervisor, which sits between the hardware and the
  VM's. The Hypervisor is itself software.
	* "instance" == "VM"

* Benefits include reduced costs due to the ability for multiple VM's to run on one physical machine, which
  allows the business (or cloud infrastructure provider), to save electricty, storage (of the physical machines), etc.

The 4 main services offered by a cloud infrastructure provider are:
* Compute 
* Storage
* Database
* Network

**It makes sense to host your cloud as close to the users of the cloud as possible, to cut down on latency**

## Cloud Deployment Models

1. Public Cloud
* Shared responsibility between the user (business/individual) and the provider (AWS, Microsoft ( Azure ), Google (Cloud
  Platform), Snowflake, etc).
	* Provider handles the physical storage and maintainance of the machines while the user is responsible for
	  security within the cloud and maintaining whatever system they are running.
* Capital expenditure is low (don't need to hardware and people to maintain said hardware)
* Operational expenditure is variable - you only pay for the resources you use.

2. Private Cloud
* Hardware is typically kept on premise
	* Virtualization is still a part of the system, but the hardware is managed by the company/individual as well.
* Typically more secure
* Costs more (have to maintain the physical machines, electricty, *regardless of whether you use them or not*)

3. Hybrid Cloud
* Blend of both of the above (note that this does mean you get the *worst* of both in addition to the best of both)

## Key Cloud Concepts

* On-Demand Resourcing
	* When you want a resource (larger EC2 instance, another DB, etc.) it is *almost* always available.
* Scalability
	* 'Up' and 'down' scalability - alters the power and performance of an instance.
	* 'In' and 'out' scalability - adds/removes the numbers of instances your system is using.
* Economy of scale
	* Large cloud providers can pass the part of the savings associated with virtualization onto the customer.
* Flexibility and Elasticity
	* Choose the exact architecture you need (number of instances, OS', etc.)
* Growth
	* Global datacenters allow the user to reach their end users on a global scale, not having to worry about
	  latency since the cloud infrastructure provider usually has resources around the world.
* Utility Based Metering
	* Only pay for what you use (or leave on...)
* Shared Infrastructure
	* Virtualization allows multiple services to run on one machine, thus allowing multiple users to use the same
	  physical machine (allows for economies of scale for the cloud provider)
* Highly Available
	* Having data copied to different places allows a resilience that would otherwise need to be architected on its
	  own.
* Security
	* Cloud providers are usually more secure since they have to comply with worldwide regulations.

## Cloud Service Models

1. Software as a Service (SaaS)
	* (Most applications fit this model) - allows some customization, however that customization is within the
	  context of the application.
2. Platform as a Service (PaaS)
	* Allows customization from the OS level and up (network, host hardware etc. managed by provider).
3. Infrasture as a Service (IaaS)
	* Allows customization at the OS level and up (including virtual clouds). The hardware is typically managed by the provider.

## Common Use Cases of Cloud Computing 

* Migration of production services to the cloud (as opposed to on premises services)
* Peak season might but a strain on standard infrastructure; being based in the cloud would allow seasonal scaling
  ("Traffic Bursting")
* Backup & DR (Disaster Recovery)
* Web hosting of applications
* Test & Dev environments
* Low cost Proof of Concept
* Big Data & Data manipulation

## Data Center Architecture in the Cloud

* Location
	* public cloud providers will have regions worldwide, each region with multiple data centers
* Physical Security
	* Vendor is responsible for the physical security of their data center
* Mechanical & Electrical Infrastructure (CRAC (Computer Room Air Conditioning))
	* Generators, Fire Suppression, etc. are all on premise at the data center, and is therefore the responsibility
	  of the data center.
* Network Infrastructure (Switches/Routers/Firewalls)
	* The public cloud user is able to create configurations that simulate the logical effet of switches, routers
	  and firewalls.
	* In AWS, virtual clouds are called Virtual Private Networks (VPCs) and are configured by the end user. This
	  therefore means that the security & infrastructure *of the VPC* is the responsibility of the end user.
* Servers (Application/Directory/Database)
	* "instances" or "VMs" 
	* Providers offer services that are specific to hosting databases, or specific to high compute power
* Storage (NAS/SAN/Block Storage/Backup)
	* In the cloud, storage is effectively unlimited and highly scalable.

# Compute Fundamentals for AWS

* There are different types of compute resources on AWS - not only EC2

## EC2 - Elastic Cloud Compute

* EC2 is the "meat and potatoes" of AWS' compute infrastructure.

### AMIs - Amazon Machine Images

* An AMI is a template for a VM; this can include the OS, specific applications for your use case, custom configurations, etc.
* To create your own custom AMI:
	* Choose a basic AMI the first time you launch an EC2 instance (only specifying the OS you want for example),
	  and then once the instance is launched, you can install any applications related to your solution, configure
	  anything you like, install any applications that will need to be on all VM's of this type for your solution,
	  and save this entire setup as your own custom AMI.
	* This custom AMI can then be launched with all the configurations & installations already setup.
* AMI's are also available through the "Community AMIs" page and the "AWS Marketplace"

### Instance Types 

The key parameters of an instance type to pay attention to when launching are:
* vCPUs
* Memory (GB)
* Instance Storage
* Network Performance

* There are other parameters of lesser importance, all of which can be seen in the instance parameters table when
  launching your instance

* Instance types are grouped into families:
	* micro instances
		* very low cost - useful for low throughput purposes
	* general purpose
		* balanced mix of compute, memory and network capabilities
	* compute optimized
		* good for high performance front end servers, etc.
	* accelerated computing
		* GPU
		* FPGA - ( Field Programmable Gate Arrays )
			* used to create application specific hardware accelerations
			* High CPU performance, large memory and high network performance make these the go to for
			  solutions that require massive parallel processing; data science/genomics/financial computing
			  being some examples.
	* Memory optimized
		* lowest cost per GB of RAM than other instances
		* recommended for database applications/database components of a solution
	* Storage optimized
		* Use SSD backed instance storage, which offers high I/O thoughput.

### Instance Purchasing Options 

Different payment plans available:

* On-Demand Instances
	* launched at any time
	* flat rate per instance type
	* typically used for short term uses
* Reserved Instances
	* best applied to long term, predictable workloads (applications that you know need to be running for a set
	  period of time)
	* purchases for a set period of time for reduced cost (up to 75% reduced cost). Three options:
		1. All Upfront
			* complete payment for 1 or 3 year timeframe - highest discount.
		2. Partial Upfront
			* smaller upfront - smaller discount than All Upfront option.
		3. No Upfront
			* very small discount applied
* Scheduled Instances
	* pay for instances on a daily/weekly/monthly schedule 
	* **even if you don't use the instance, you will be charged**
	* less expensive than the On-Demand Instance, so good for recurring but predictably recurring use cases
* Spot Instances
	* Bid for unused EC2 resources.
	* Con: **Not guaranteed for a fixed period of time**
	* Pro: Can purchase large (expensive) EC2 instance types at a lower price
	* The "Spot Price" determines the price of an instance and bidding above that "gets" you the instance. If a
	  higher bid comes along while you are using the instance, **your instance will shut down**.
	  * Should only be used for tasks that can be interrupted/stopped - otherwise be prepared to continuously ensure
	    your bid price is above the spot price (either computationally or manually).
* On-Demand Capacity Reservations
	* allows you to reserve capacity (whether that is compute, memory, storage or network) within a particular
	  availability zone for any period of time.

### Tenancy 

"Tenancy" refers to the actual physical computer your VM is hosted on in the AWS data center.

* Shared Tenancy
	* your instance will be launched on any available host, most likely one that is shared with other AWS users.
	* AWS provides the security to ensure different users can't access each other's instances
	* this model is what allows "economies of scale" and makes cloud computing inexpensive.
* Dedicated Tenancy
	* Dedicated instances
		* hosted on hardware that no other AWS user can access
		* may be required by problem domain compliance requirements
		* more expensive
	* Dedicated Hosts
		* more visibility/control of the physical host than even the "Dedicated Instances" option.
		* again - may be required by problem domain compliance requirements

### User Data 

* commands that will run during the first boot cycle of the instance
* used to update software or OS'

### Storage Options

Two types:
1. Persistent Storage
	* attaching EPS (Elastic Block Storage) volumes
	* attached via AWS network - physically separated
	* can be detached from EC2 instances and maintain data 
	* data saved on EBS volumes are automatically copied to other volumes within the same availability zone for
	  backup purposes 
	* data can be encrypted and scheduled backup snapshots are possible
2. Ephemeral Storage
	* **deleted and gone forever when EC2 instance is stopped/terminated**
	* if instance is rebooted (think "continue instance use") - data is maintained
	* non-detachable

### Security

* Security group = "instance level firewall"
	* allows you to specify what traffic communicate with your instance (can be specified via IP range, protocol,
	  port range, inbound/outbound, etc.)
* Key-Pairs
	* Allows encrypted access to EC2 instances
	* Public Key = AWS keeps this to match to your private key
	* Private Key = your responsibility for safe keeping
	* **You can use the same key-pair for multiple instances** - this does mean that if your private key becomes
	  comprimised, whoever has the key would have access to all the instances associated with that key-pair.
* It is the user's responsibility to maintain and install latest OS updates & security patches released by the OS vendor
  - this is part of the shared responsibility model

## ECS - EC2 Container Service

ECS allows the user to run Docker-enabled applications packaged as containers across a cluster of EC2 instances,
**without requiring the user to manage a complex and administratively heavy cluster management system. **AWS Fargate**
manages this system for you.

Docker is software that allows everything an application needs to run to be put into a logical container, and that
container can then be run on any operating system.

Two different ECS Cluster deployement models:
1. Fargate Launch
	* the user is only required to specify CPU, memory and networking policies (in addition to having your
	  applications packaged in containers)
	* This is the option that trades lower customization for lower management overhead.
2. EC2 Launch
	* This is the option that trades higher management overhead for higher customization.
	* The user is responsible for patching and scaling instances, specifying instance types and how many
	  applications should be in a cluster.

* Monitoring of your ECS cluster and containers is provided by AWS CloudWatch
* An ECS Cluster is comprimised of multiple EC2 instances
* Security Groups, Elastic Load and Autoscaling can be applied.
* The cluster can be comprised of different EC2 instance types.
* **Clusters can only scale in a single region** (this is different from availability zones; clusters can span multiple
  zones).
* Containers can be scheduled to be deployed across the cluster.

## ECR - Elastic Container Registry

ECR provides a secure location to store and manage your docker images. This service allows developers to push, pull and
manage their library of docker images in a central and secure location

There are a few components used in ECR:

1. Registry
	* The component that hosts/stores docker images as well as create image repo's	
	* The default URL for the registry is:
	`https://<aws_account_id>.dkr.ecr.<region>.amazonaws.com`
2. Autorization Token 
	* **Before your docker client can access the registry (push & pull), it needs to be authenticated with an
	  Autorization token
	* To start the authorization process, run the following command using the AWS CLI (might need to be installed):
	`$ aws ecr get-login --region <region> --no-include-email`
	* The above command's output will be a docker login command:
	`docker login -u AWS -p <password>`
	* Authorization tokens last for 12 hours before the above process need to be performed again.
3. Repo 
	* Standard repo concept - allows grouping of docker images in whatever manner the user would like.
4. Repo Policy 
	* Repo policy's can be set up by the principal of the repo - who can then setup which actions different users
	  can perform on various repo's.
5. Image 
	* Once all the above has been completed, you/the user can push/pull docker images from your ECR.

## EKS - Elastic Container Service for Kubernetes

Kubernetes is an open source container orchestration tool designed to automate deploying, scaling and operating
containerized applications.

EKS allows the user to run Kubernetes across their infrastructure without having to interact with the Kubernetes
management system, known as the control plane. The AWS account owner only need to provision and maintain the worker
nodes if using EKS.

Kubernetes:
	* The control plane contains API's, the kubelet processes and the Kubernetes Master.
	* The control plane allocates containers onto nodes (according to CPU needs)
	* The control plane tracks the state of all Kubernetes objects, continually monitoring them.
EKS takes care of all the above processes for the AWS users

Worker nodes:
	* Kubernetes clusters are composed of nodes (worker machine - One-Demand EC2 instance on AWS)
	* Every node that is created uses a specific AMI (in order for Docker & Kubernetes to run on it)
	* Once the worker nodes are setup by the user, they can be connected to EKS with an endpoint

### Setting up EKS

1. Create an EKS Service Role
	* Create an IAM (Identity & Access Management) service role that allows EKS to provision and configure specific
	  resources. The EKS role needs the following permission policies:
		* AmazonEKSServicePolicy
		* AmazonEKSClusterPolicy
2. Create an EKS Cluster VPC
3. Install kubectl and AWS-IAM-Authenticator
	* kubectl = Kubernetes command line utility
4. Create your EKS Cluster (using above information)
5. Configure kubectl for EKS
	* run `$ update-kubeconfig` via AWS CLI to create a kubeconfig file for your EKS cluster
6. Provision and configure worker nodes
7. Configure worker nodes to join EKS Cluster

## Elastic Beanstalk Service 

**Elastic Beanstalk Service is free to use. However, any resources that Beanstalk sets up for your application (Compute,
Storage, Database or Network) are charged using the standard pricing of those resources.**

Elastick Beanstalk is a service that takes your uploaded code and automatically provisions and deployes the resources
needed to make the application operational.
	* This service is likely the most useful for engineers who may not have the familiarity, skills or desire to
	  manage the deployment, provisioning and monitoring of developed applications.

* Able to operate with a variety of platforms and programming languages, some examples being:
	* Single Container Docker
	* Multicontainer Docker
	* Preconfigured Docker
	* Python

Key Components:
* Application Version
	* reference to a specific version of the code/application that typically resides in S3
* Environment 
	* The Environment refers to the entire system of your deployed application (EC2 and S3 for example)
	* At this stage, the application has been deployed as a solution and is operational within the environment 
* Environment Configurations 
	* Parameters that dictate how the environment will have its resources provisioned by Beanstalk.
* Environment Tier
	* Applications that are communicating with other servers (usually via HTTP requests using port 80) are run in a
	  *web server environment*. AWS infrastructure usually used includes:
	  	* Route 53
		* Elastic Load Balancer
		* Auto Scaling
		* EC2
		* Security Groups
	* Applications that are doing backend jobs/processing of some kind are run in a *worker environment*. AWS
	  infrastructure usually used includes:
		* SQS Queue
		* IAM Service Role
		* Auto Scaling
		* EC2
* Configuration Template 
	* A template that provides the framework for creating a new, unique, environment.
* Platform
	* The set of components that can build your application when using Elastic Beanstalk (OS, server type,
	  programming language)

The typical Elastic Beanstalk workflow looks like the following:

![](images/elastic_beanstalk_workflow.png)

## Lambda 

"AWS Lambda is a serverless compute service that allows you to run your application code without having to manage EC2
instances."

[1 Hour video on AWS Lambda and serverless apps](https://www.youtube.com/watch?v=EBSdyoO3goc)

* "Serverless" doesn't actually mean "without servers" - it just means that the cloud infrastructure provider handles
  **all** the management of Compute resources for you (EC2 provisioning, scaling, etc). Because of this, the user doesn't
  have to worry about the Computer resources, and thus it is a "Serverless architecture" from the vantage point of the
  engineer.
* **Going serverless allows the user to spend  more time on writing code related to the business problem and less time
  on DevOps**
* The user only pays for Compute resources when Lambda is in use via Lambda Functions
* AWS Lambda charges Compute power per 100 milliseconds of use only when your code is running, in addition to the number
  of times your code runs.

All of the above makes **AWS Lambda highly scalable**; pay for what you use, and don't worry managing all the Compute
resources you need for you solution to run.

1. Upload code to Lambda (make sure Lambda supports the language your code is written in).
2. Configure Lambda Functions to execute upon specific triggers from supported event sources.
3. Once the trigger is initiated, Lambda will run your using only the required resources.

## AWS Batch

Used to manage and run batch computing workloads in AWS (mostly used for high specificity cases that require large
amounts of compute power). 

Components:

1. Jobs 
	* A "job" is a class of work to be done by AWS Batch.
	* Could be an executable program, a script, or an application within an ECS Cluster.
	* Jobs can have different states such as 'Submitted','Pending','Running','Failed',etc.
2.
3. Queues
	* jobs are placed into queues (multiple queues can have different priorities)
	* AWS Batch can bid on Spot instances on your behalf.
4. Scheduling
	* The scheduler takes care of ensuring the high priority items are run first (assuming dependencies are met).
5. Compute Environments 
	* Managed (*i.e. managed by Batch*)
		* Handles the provisioning, scaling and termination of compute resources based on need.
		* This environment is created as an ECS Cluster
	* Unmmanaged (*i.e. managed by the user*)
		* Greater customization = greater administration (by the user)
		* The user must create the ECS cluster.

As one would expect, this service lends itself to solutions that require/depend on parallel processing (in the Data
Science world, this could take the form of bagged models (GLM's, random forests, etc.))

## Lightsail

Similar to EC2, Lightsail is an VPS (Virtual Private Server), similar to EC2, however there are fewer configurable steps
during its creation.

* Designed to be simple compute resources that a small business or single user could use on an ongoing basis, or just as
  a "one-off" resource.

Lightsail can be accessed via the AWS Console under the Compute category and has a one-page setup.

Choose:
1. Region/availability zone
2. Instance image (OS) 
3. Blueprint (whether you want any apps pre-installed)
4. Launch Script (if you want)
5. Key-Pair (by default one is provided, however you can choose your own)
6. Instance plan (how much you pay)
	* Although the price is calculated as 31.25 days * 24 hours per day (aka monthly rate) - 
	* Instances are charged as On-Demand (only pay for it when using it).
7. Name for Lightsail instance
	* Also prompted to add 'tags' to help organize your Lightsail instances 

* Use the "Connect" tab to view IP to connect to via SSH.
* Use the "Snapshots" tab to backup the information on your instance.

* **Deleting your instance and shutting down are different.**

## ELB - Elastic Load Balancer

The main function of an ELB is to help manage and control the flow of inboud requests destined to a group of targets by
distributing these requests evenly across the targeted resource group
* Targets could be EC2 instances, Lambda functions, different Docker containers, etc.
* Targets can be in a single Availability Zone (AZ) or across multiple AZs

* The "Elastic" in the name means that an ELB will automatically scale up or down as incoming traffic
  increases/decreases *without any management on the part of the user*.
	* Dynamic scaling can be setup simply.

**ELB Types**

* See [this table](https://aws.amazon.com/elasticloadbalancing/features/#compare) for a comparison of the different ELB
  types

1. Application Load Balancer (ALB)
	* ALBs operate at level 7 of the [OSI model](https://en.wikipedia.org/wiki/OSI_model)
	* Flexible feature set for applications using HTTP/HTTPS protocols.
	* **Operates at the request level.**
	* Advanced routing, TLS (Transport Layer Security) termination and visibility features targeted at application
	  architectures.
	* Target groups can be setup so all requests of a specific protocol are routed to that group, through a specific
	  port.
2. Network Load Balancer (NLB)
	* NLBs operate at level 4 of the [OSI model](https://en.wikipedia.org/wiki/OSI_model)
	* Ultra-high performance while maintaining very low latency.
	* **Operates at the connection level.**
	* Can handle millions of requests per second.
3. Classic Load Balancer
	* Used for applications that were built in the existing EC2 Classic Environment.
	* **Operates at both the connection and request level.**
	* This should only be used for an existing application running in the EC2-Classic network (legacy AWS
	  infrastructure)

**ELB Components**

1. Listeners
	* Defines how inbound connections are routed to target groups based on ports and protocols set as **conditions**
	  (think `if else` statements).
2. Target Groups
	* A group of resources to which the ELB will route requests.
	* One ELB can have multiple different target groups, each associated with different listener configurations and
	  associated rules.
3. Rules
	* Rules (think `if else` statements) define how an incoming requests gets routed to which target group.
4. Health Checks
	* The ELB can (and does) contact each target within a target group using a specific protocol to receive a
	  response. If that response doesn't come back, the ELB marks that target as 'unhealthy' and stop sending
	  traffic to that target.
5. ELB Schemes
	* 5.1 Internet-Facing ELB
		* As the name suggests, this ELB scheme handles connections/requests coming from other
		  applications/servers through the internet. Due to this, this ELB scheme has a public DNS and
		  associated Public IP.
	* 5.2 Internal ELB
		* This scheme is only used for communication within an applciation/system/solution. Therefore, this ELB
		  only has a internal IP address and can therefore only communicate with requests that come from within
		  the users VPC.
6. ELB Nodes
	* Each AZ you intend to work in needs to have its own ELB node
7. Cross-Zone Load Balancing
	* Allows the ELB to send requests to targets that aren't in its AZ.

* An ELB can contain 1 or more listeners, each listener can contain 1 or more rules, each rule can contain 1 or more
  conditions. **All conditions result in a single action.**

### SSL Server Certificates 

SSL = Secure Sockets Layer
	* SSL is a cryptographic protocol, similar to TLS (Transport Layer Security)

* HTTPS requests will sometimes need to be used in lieu of HTTPS requests to ensure an encrypted connection between a
  client sending a request and your ALB.
	* In order set this up, you will an SSL certificate.
* The server certificate that the user will need to set up is an *X.509 certificate* (digital ID provisioned by a
  Certificate Authority and managed by AWS Certificate Manager ( ACM ))
	* This certificate is used to terminate the connection between the client and your application/solution, and
	  only then is the request decrypted and sent to the resources in the ELB target group.

* ACM allows you to provision and configure any SSL certifcates that will be used inside your AWS solution (most AZ's
  are supported). If the AZ you are in isn't supported, you will have to configure a certificate using IAM.

## EC2 Auto Scaling 

As the name suggests, EC2 Auto Scaling means that a solution can scale its compute resources up or down, based on
demand, so that the user doesn't have to worry about overloading any resources, and also doesn't have to worry about
spending money on resources they aren't using.

**Components**

1. Create a Launch Configuration or Launch Template
	* **This must be setup prior to setting up the Auto Scaling Group** - otherwise the group wouldn't know what
	  instance type and specifications to use when launching a new instance.
	* Launch Configuration and Launch Templates are very similar, the only real difference being that a Template
	  allows the user to specify a few advanced options, and has the entire setup process on one webpage (as opposed
	  to the Launch Configuration, which goes through multiple webpages.)
	* One of the two options is needed in order to specify new instance parameters such as:
		* the AMI to use.
		* the instance type to use.
		* whether the instance should have a public IP.
		* the storage volume the instance should use .
		* the security groups (if any) the new instance should be associated with.
2. Create an Auto Scaling Group
	* Select the Launch Template or Configuration that new instance will be launched from.
	* Set up the "Starting Instance" count.
	* Set up the minimum and maximum number of instances that your solution can scale between.
	* Set up the conditions that need to be met for "Scaling Up" (i.e. Average CPU usage >= 75% for 3 minutes).
	* Set up the conditions that need to be met for "Scaling Down" (i.e. Average CPU usage <= 30% for 3 minutes).
	* Set up AZs in which new instances will be created.

### Combining ELB & EC2 Auto Scaling 

Although ELBs and EC2 Auto Scaling *can* be used independently, they work best together.
	* If you have a fixed number of instances/compute resources in a target group of an ELB, if you need more, the
	  user will have to manually add more. If you need less, the user will have to manually remove some.
	* If you have an EC2 Auto Scaling group without an ELB, how are you going to distribute traffic/requests evenly
	  across your instances?

* ELBs allows incoming traffic to be *averaged* across all resources within a target group.
* EC2 Auto Scaling can be setup to *scale* the resources in that target group.

* To associate an ALB or NLB, you must associate the auto scaling group with the ALB or NLB target group. This is
  performed by editing the configuration of the Auto Scaling Group from the AWS Management Console.
	* Note that there are two sections that are related to ELBs; the "Classic Load Balancer" field and the "Target
	  Groups" field. The former is for the legacy ELB version (see above), and the latter ('Target Groups') should
	  be used for all newly created ALBs or NLBs.

# Storage Fundamentals for AWS

There are more storage options provided by AWS than those listed here, however the "big three" if you will are:

* EBS - Elastic Block Volume
	* Low latency
	* Should be used like a traditional hard drive
* S3 - Simple Storage Service
	* Best for large objects that don't require frequent reading and writing.
* EFS - Elastic File Storage
	* "Traditional" file system

## EBS - Elastic Block Storage

Provides storage to EC2 instances via 'EBS Volume'

* Persistent, block level storage connect to EC2 instances via AWS network.
* An EBS volume can be attached/accessed to/by **only one** EC2 instance, however multiple EBS volumes can be attached
  to a single instance.
* 'Snapshots' (aka backups) can be performed manually or setup to run on a scheduled basis.
	* Theses backups are stored in an S3 bucket.
	* EBS volumes can be recreated from snapshots
	* Snapshots can be copied from one availability region to another

**Reliability**
* Every write to an EBS volume is repeated multiple times to protect against a complete loss of data.
* **Volumes can only be attached to an EC2 instance in the same availability zone**

### Volume Types

* Having two volume options (below) allow the user to trade performance for cost in the best manner for their solution.
* Different volume types have different IOPS (Input/Output per Second) thresholds

1. SSD (Solid State Drive)
	* Best for smaller blocks of data.
	* There are two sub-types of SSD's:
		* 1.1 General Purpose SSD
		* 1.2 Provisioned IOPS SSD
			* Highest performance EBS volume (best for low-latency requirements)
2. HDD (Hard Disk Drive)
	* Best for larger blocks of data.
	* Designed for workloads that require a higher rate of throughput.
	* There are two sub-types of HDD's:
		* 2.1 Throughput optimized HDD
			* Best for large blocks of data that are still throughput intensive
			* **These volumes can NOT be used as boot volumes for instances**
		* 2.2 Cold HDD
			* Best for large blocks of data that don't need to be accessed frequently.
			* **These volumes can NOT be used as boot volumes for instances**

There are a few ways to create an EBS volume

1. During the launch of an EC2 instance
2. As a standalone EBS volume that can be atttached to an EC2 instance when required

During the creation of the volume, the user can choose:
* Whether to create the volume from a snapshot (of a previous volume) OR start a blank volume
* Size
* Volume type 
* **What happens to the volume when the EC2 instance terminates**
* To encrypt or not to encrypt

## S3 - Simple Storage Service 

* Most common storage service in AWS (applicable to many use cases).
* Theoretically unlimited storage.
* Supports individual files sizes up to 5 TB.
* S3 is object based - meaning **it does not store objects (files) in a traditional hierarchy**. The address space is
  flat and therefore each object has a unique URL by which it can be accessed.
	* You **can** create folders/directories **within** a bucket to help with organization; however, S3 itself is
	  not a hierarchical file system.
		* Each object saved in S3 has a "key", which can be thought of as the filepath (it includes and
		  directories within the bucket). The "full path" aka full URL for an object within S3 will be its
		  bucket name and the object key.
* S3 is a regional service; to ensure data persistence, AWS makes multiple copies of your data within different AZs
  within the region you selected. This provides "Eleven 9's" worth of data integrity (99.999999999% durability = very
  low likelihood of losing data).
* Availability is **not** the same as Durability; AWS provides 99.5% - 99.99% data *Availability*, which means you will
  be able to access your saved data 99.5% - 99.99% of the time. *Durability* refers to the the likelihood your data
  isn't lost or corrupted.
* **Data versioning is an option.**

To save an object in S3 (manually):
1. Create an S3 bucket with a **globally unique name**.
	* This can be accessed via the AWS Management Console, and selecting "S3" under the "Storage" header.
	* This means that your bucket name has to be unique **across all of AWS** (can't just be unique *to you*).
	* by default, your account has a soft limit of 100 buckets, however this can be increased by contacting AWS.

### Storage Classes

Different storage classes allows the user to choose the tradeoffs between cost and accessibility that best suits the
problem they are working on. There are 6 different storage classes:

1. S3 Standard
	* general purpose storage
		* High throughput and low latency
	* Lifecycle rules are an option 
		* Lifecycle rules allow the user to setup a configuration that automatically moves objects saved in S3
		  to a different storage class (i.e. if you haven't accessed some data in a while but still want to keep
		  it around for the "just in case" moments, you can have that data moved to a less expensive and less
		  easily accessible storage class).
	* SSL encryption is available for data both at rest in an S3 bucket and in transit to/from and S3 bucket.
2. S3 Intelligent Tiering (S3 INT)
	* Best for use cases where the data access rate is unknown in advance.
	* S3 INT has two subclasses; Frequent Access and Infrequent Access.
		* **These subclasses are not the same as the "meta" versions with the same name; these subclasses are
		  "within" the logical set of "S3 INT"**
	* By default, an object is placed in the Frequent Access tier; if it hasn't been accessed in 30 days, it is
	  moved to the Infrequent Access tier. As soon as it is accessed again, it is moved back to the Frequent Access
	  tier and the clock starts again.
	* Lifecycle rules are an option.
	* SSL encryption is available.
3. S3 Standard Infrequent Access (S3 S-IA)
	* Similar to the Infrequent Tier subclass of the S3 INT storage class (above).
	* Designed for object that aren't going to be accessed frequently (duh).
	* Lifecycle rules are an option.
	* SSL encryption is available.
4. S3 One Zone Infrequent Access (S3 Z-IA)
	* Designed for object that aren't going to be accessed frequently (duh).
	* Durability of Eleven 9's, however this is *within a single AZ, as opposed to S3 S-IA, which has the same
	  Durability but across multiple AZs*. This change offers the user a 20% decrease in cost.
	* Lifecycle rules are an option.
	* SSL encryption is available.

Glacier Classes:

* A fraction of the cost of the above storage classes, the tradeoff being you don't get instance access to your
  data.
* Best suited for "Cold Storage" - objects that will likely not need to be accessed but need to be kept around
  "just in case".
* Eleven 9's of Durability.
* **No GUI for moving objects into Glacier "Vaults" (as they are called);** the GUI can only be used to create
  the vaults. After that, data must be moved into the vaults via APIs, SDKs, or the AWS CLI (or, through
  Lifecycle rules set up in the more frequent access classes).

5. S3 Glacier
	* Data *can* be accessed via 3 different routes, each with a different cost (listed in descending order relative
	  to cost):
		1. Expedited
			* Available in 1-5 minutes
			* Data must be under 250 MB.
		2. Standard
			* Available in 3-5 hours.
			* Any size.
		3. Bulk
			* Available in 5-12 hours.
			* Used for retrieving PB's of data at a time.
6. S3 Glacier Deep Archive (S3 G-DA)
	* Minimal access.
	* Retrieval available within 12 hours (only one option).

* Check out [this link](https://aws.amazon.com/s3/pricing) for pricing information.
* Check out [this link](https://aws.amazon.com/s3/storage-classes/?nc=sn&loc=3) for a performance summary of all S3
  classes.
* Remember that the user gets 99.999999999% data durability by replicating the data across multiple AZ's within a single
  region.
	* If the user needs to access their data across region, they will need to configure *Cross Region Replication*,
	  which, as the name implies, replicates data across regions.

## EFS - Elastic File System

* **Can be concurrently accessed by multiple (up to thousands...) of EC2 instances**
* Uses a "traditional" hierarchical structure
* EFS automatically "auto scales"; the user doesn't need to provision "more space"
* Throughput and IOPS also dynamically scale
* [Is not supported for instances using a Windows OS](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AmazonEFS.html)
	* another reason to hate windows...
	* Checkout [AWS FSx](https://aws.amazon.com/fsx/) for Windows
* AWS Region agnostic.

Once the EFS is created, the user can create "mount points" within their VPC; once this is performed, any EC2 instance
can read and write data to the EFS.

## AWS Snowball & Snowmobile

* If the user needs to transfer a large amount of data to be the cloud (or from the cloud to on-premises in the case of
  a DR plan), AWS offers two options:
	* Snowball:
		* AWS ships a physical device (the "snowball") to the user that can contain 50TB - 80TB of data.
			* dust, water, and tamper resistant.
		* Multiple devices can be used to scale to Petabyte size.
		* This/These devices are then shipped back to AWS where the are uploaded to S3.
		* Configured for high speed data transfer. Each snowball has the following recievers installed,
		  supporting 10 Gigabit transfer speeds:
			* RJ45 (Cat6)
			* SFP+ Copper
			* SFP+ Optical
		* Encrypted by default
		* HIPAA Compliant
	* Snowmobile:
		* Exabyte-scale transfer service.
		* A "snowmobile" is a 45 foot long shipping container pulled by a semi-truck to the user to which data
		  can be uploaded and then sent back to AWS.
		* Can transfer up to 100PB per snowmobile.

## AWS Storage for On-Premises Backup and Disaster Recovery (DR)

**Cloud Storage and DR**

* Issues with traditional backup methods for DR:
	* backup drives may be stored in the same physical location as the production system storage. If this were to be
	  the case and a physical disaster occurred that effected the production system storage, the backups would
	  likelye be effected as well.
	* Scalability - As infrastucture expands, so will the needs of your backup storage.
	* Costs - An effective backup solution is a huge upfront cost for the user.
	* Availability - If your backup storage isn't cloud based, you (the user) might run into some delays retrieving
	  your data from an off site location.
* Benefits of Cloud Storage for DR:
	* Cost Efficient
	* Scalable 
	* Available and durable
	* Secure and reliable
	* **Zero maintainance of hardware**
	* Off-site Storage
	* Easy to test DR plans

**Considerations when planning a DR Storage Solution**

The values of the below two concepts will largely determine the path your DR plan takes:
* RTO - Recovery Time Objective
	* The maximum amount of time in which a service can remain unavailable before it is classed as damaging to the
	  business/objective.
* RPO - Recovery Point Objective
	* The maximum amount of time for which data could be lost for a service.

* How will the user the data in/out of AWS?
	* Direct Connection?
	* VPN Connection?
	* Internet Connection ?
* If you need to transfer large amounts of data as part of a DR plan, check out AWS Snowball & Snowmobile
* How quickly do you need your data back?
	* Depend on RTO requirement (and are therefore solution/problem dependent).
	* For example, if your RTO is 1 hour (meaning if a service is out for more than an hour there is catastrophic
	  damage to your business), this eliminates some services from your DR plan (S3 Glacier Storage classes, for
	  example).
* How much data do you need to import/export?
	* Calculate your target transfer rate:
		* check out [this link](http://www.thecloudcalculator.com/calculators/file-transfer.html) and you can
		  input the necessary inputs (or the available inputs e.g. AWS Glacier IOPS and how much data you might
		  need to transfer from your Glacier.)

### Using S3 as a Data Backup Solution

* Easily scalable and customizable (user can optimize the Durability, Availability & Cost for their needs)
* Remember that the user gets 99.999999999% data durability by replicating the data across multiple AZ's within a single
  region.
	* If the user needs to access their data across region, they will need to configure *Cross Region Replication*,
	  which, as the name implies, replicates data across regions.
	* From a DR perspective, this can reduce latency in the event that one region you are relying on is unavailable
	  for some reason.
* Multipart upload to S3:
	* AWS recommends that any objects larger than 100 MB utilize multipart uploading, which "chunks" the data and
	  uploads one chunk at a time, reassembling everything once all chunks are in S3.
		* There are multiple benefits of this serice, a couple being:
			1. Speed & Throughput - Since multiple parts can be uploaded in parallel, the user can reach the
			   end goal (having the entire object uploaded to S3) faster.
			2. Interruption Recovery - If there is a network issue (or any technological issue for that
			   matter), uploading in chunks ensures that only the chunk that was interrupted has to be
			   reuploaded and then the process can continue. The user won't run into a situation where the
			   entire object is 95% uploaded, there is a network error, and they they have to start all over
			   again from 0.
* Security:
	* Since S3 offers in-transit and static encryption, S3 is a good option for making sure your don't inadvertenly
	  leak sensitive data.
	* IAM Policies - Used to restrict access to S3 buckets depending on identities and permissions.
	* Bucket Policies - JSON policies are assigned at the bucket level and control who has access to the buckets
	  contents.
	* Access Control Lists - Allows a more granular way of assigning permission relative to IAM policies (read,
	  write, execute, etc.)
	* Lifecycle Policies - Used to automatically move data between S3 classes.
	* MFA Delete - Multifactor Authenticated Delete ensure that a user has to enter a 6 digit code to delete an
	  object, ensuring objects aren't accidentally deleted.
	* Versioning - "git for data" does what one would think; saves the object **each time a change is made**. This
	  obviously requires more space than if it were not configured.

### AWS Artifact

AWS Artifact allows the user of AWS services to see how those services align with compliance requirements of a specific
industry.

* Can be accessed from the AWS Management Console
* Specifies the scope of compliance for the combinations of AWS services and the regions/AZ's they reside in.

### AWS Storage Gateway 

* Sits between the users on-premises data storage and a backupt to AWS S3

There are a few options available:
1. Stored Volume Gateways:
	* Primary storage is the on-premises data center
	* Used to backup local storage volumes to S3 on an interval basis
2. Cached Volume Gateways:
	* Primary data storage is S3
	* Local data storage is used as a 'cache' for recently accessed data (cached volume).
3. Gateway-Virtual Tape Library
	* "Virtual Tape Library is a cloud based tape backup solution, replacing physical components with virtual ones,
	  while utilizing your existing tape backup application infrastructure."
	* Components:
		* Storage Gateway: Configured as a Tape-Gateway acting as a VTL with a capacity of 1500 virtual tapes.
		* Virtual Tapes: Virtual equivalent to a physical tape cartridge with capacity of 100 GB - 2.5 TiB. Data
		  stored on VT's are backed by S3 and visible in the virtual Tape library.
		* Virtual Tape Library (VTL): Virtual equivalent to a Tape Library containing Virtual Tapes.
		* Tape Drives: Each VTL comes with 10 Tape Drives, presented as iSCSI devices to your backup applications.
		* Media Changer: A virtual device presented as an iSCSI device to backup applications that manages tapes
		  between your Tape Drive and VTL.
		* Archive: Equivalent to an off-site storage facility, giving you the ability to archive tapes from your
		  VTL to AWS Glacier.

# Database Fundamentals for AWS

* Database: Any mechanism for storing, managing and retrieving information
* **"Databases are the foundation of modern application development. A database's implementation and how data is
  structured will determine how well an application will perform as it scales."**
* **Each database type is optimized to support a specific type of workload. Matching an application with the appropriate
  database type is essential for highly performant and cost-efficient operation.**

9 AWS Database Categories:

1. Relational Databases
2. Key-Value Databases
3. Document Databases
4. In-Memory Databases
5. Graph Databases
6. Columnar Databases
7. Time Series Databases
8. Quantum Ledger Databases
9. Search Databases

Choosing a database strategy:

* Choosing a database used to be a platform choice (as opposed to a choice based on the problem domain/technology); 3-4
  vendors would be considered, and once one was chosen (most likely based off of price point), *every application would
  be built using the chosen platform*. While one *can* make a database type work for most solutions, that doesn't make
  that database type the "right" choice.
* Choosing a database type should be based on the data itself; it is possible, sometimes logical, to have a single
  application use more than one database type.

2 Workload Types:
1. Operational Workloads
	* OLTP ( Online Transactional Processing ) applications are the most common built applications and are centered
	  around a set of common business processes that are: **Regular, Repeatable and Durable.**
	* Usually powered by relational databases.
2. Analytical Workloads
	* OLAP ( Online Analytics Processing ) applications are those used for data analysis and machine learning. **The
	  goal is to gain insight.** Workloads are often retrospective, streaming and predictive.

There are two "meta types" of databases:
1. Relational Databases
	* Used for structured data
	* Schemas are the logical blueprint of how data relates to other data. Schemas need to be full designed before
	  any data can be entered into a relational database.
		* Schema changes are costly in terms of time and computation. Additionally, schema changes run the risk
		  of corrupting data.
	* "Schema's are designed based on reporting requirements. This means that a database's expected output drives
	  the creation of the database and how data is stored inside it."
		* Personal (i.e. not in the course) note/opinion: I'm not sure how much I agree with this; this implies
		  that application developers and DBA's "work backward" from the use case to how the data should be
		  structured. What if a new requirement arises? It seems more logical to me to create a schema that maps
		  what the data represents (the "information" so to speak) together in a logical manner.
	* Modeling Data
		* Structured data is almost always stored in tables.
			* Tables have Primary Keys (PKs) that uniquely identify the information in that table.
			* Tables have Foreign Keys (FKs) that are PKs in another table.
	* Data Integrity
		* "ACID" is the acronym for the governing principals of databases that ensure data is reliable and
		  accurate:
			* Atomicity:
				* Refers to the elements that make up a single database transaction.
				* Transactions are treated as "all or nothing"; they either succeed completely or fail
				  completely.
			* Consistency
				* Refers to the database's state.
				* Transactions **must** take the database from one valid state to another valid state.
			* Isolation
				* Refers prevents one transaction from interfering with another.
			* Durability
				* Refers to data changes being permanent once the transaction is committed to the
				  database. 
		* Keys 
			* PKs and FKs are constrained to ensure database stability.
			* **Entity Integrity**: Every table must have a PK that is unique to that table and the PK can
			  not be blank for null
			* **Referential Integrity**: Every value in a FK column exists as a PK in its originating table.
	* Data Normalization
		* Data is stored in relational databased to be highly normalized.
		* Normalization is a process where information is organized efficiently and consistently before storing
		  it.
	* Scaling and Sharding
		* A "Shard" is a copy of an exact copy of databases schema, possibly filled with slightly different
		  information.
		* Example: A Shard might contain all data for a set of customers in a specific geographic zone. Another
		  shard would contain the same data for a different set of customers in a different geographic zone.
		* Scaling:
			* Horizontal: Adding a copy of the database server (aka "Sharding")
			* Vertical: Growing the server (more memory, CPU, disk volume).
				* Vertical scaling has limits (dictated by the physical components of the server). Once
				  this limit is reached, the database must be sharded to grow.
2. Non-relational Databases
	* Used for unstructured and semi-structured data
	* Shema-less (which allows unstructured and semi-structured data stored).
		* This aspect allows application developers to not have to wait for a database schema to be fully mapped
		  out before "getting to the real problem".
	* NoSQL
		* NoSQL = "Not Only SQL"
		* Broad term that encompasses different database models, some basic common characteristcs being:
			* Non-relational.
			* Open-source (typically this is the case - this isn't a necessity).
			* Schema-less.
			* Horizontally scalable.
				* "Shared Nothing" Architecture - each node has one shard of the NoSQL database, and can
				  thus work independently of all other processes on other nodes.
			* Do not adhere to ACID constraints.
				* By relaxing the "Consistency" principal of ACID, NoSQL systems can be highly durable
				  and available. Relaxing the Consistency principals isn't a problem with NoSQL because
				  NoSQL solutions were designed for inconsistent data.
		* Most NoSQL databases access their data using their own custom API, or possibly a combination of their
		  own custom API and "traditional" SQL. **There isn't a universal query language that is supported by
		  all NoSQL databases.**
	* Some examples of NoSQL databases models are:
		1. Key-value Databases
			* (think a database whose entire structure is similar to a map/Python dictionary)
		2. Document Store Database
			* Example: A database whose structure is similar to JSON; there can be nested keys, and the
			  values can only be accessed by going "down the hierarchy".
		3. Graph Store Database
	* Advantages of NoSQL over SQL:
		* Scaling is easier (horizontal instead of vertical scaling).
		* NoSQL = less consistency, higher scalability/performance.
		* SQL = more consistency, more difficult/less scalability.

## RDS - Relational Database Service

AWS's grouping of relational database engines. There are 6 options:

1. Amazon Aurora
	* AWS's cloud-native version of MySQL and PostGreSQL.
2. MySQL
	* Considered the #1 open source relational database management system.
3. PostGreSQL
	* Close #2 behind MySQL for open source DB's.
4. MariaDB
5. Oracle
6. Microsoft SQL Server

* Choose the instance type that you will run your DB on based on the problem domain; general purpose might be the best
  option for one problem, but memory-optimized might be needed for another.
* Multi AZ:
	* If the user wants to have a failover for the RDS DB in case something happens to the primary instance, select
	  *Multi AZ* when deploying the primary instance. This creates a second copy of the primary RDS instance in a
	  separate AZ within the same region as the primary one.
	* Replication of data from the primary instance to the secondary instance happens syncronously.
	* If the primary instance does fail (for whatever reason), the RDS failover process takes places
	  automatically without the need for user input. RDS will update the DNS record to point to the secondary
	  instance for you.
	* Fore more info, check [this link](https://cloudacademy.com/course/using-rds-multi-az-read-replicas/)
* Storage Scaling:
	* Storage Autoscaling is an option that can be selected for the following RDS engines:
		* MySQL, PostGreSQL, MariaDB, Oracle and Microsoft SQL Server.
			* All use EBS volumes for storage.
		* When setting storage autoscaling, the user must set the minimum (start size) of the DB and the maximum
		  size to which the DB can scale.
	* Amazon Aurora	uses shared cluster storage, and thus doesn't need to be manually set to autoscale; autoscaling
	  is automatically configured.
* Compute Scaling:
	* Vertical (enhancing performance of current instance(s)) or Horizontal scaling (increasing the count of
	  instances) can be scheduled, or happen immediately.
	* "Read Replicas" are copies of your database that can be created so that 'read only' traffic has a dedicated
	  instance. This allows the read and write functionality to each have a dedicated instance (the read replica
	  updates itself from the main DB on an asyncronous interval).
* Snapshots can be setup on a recurring interval.

### Creating a RDS DB

1. Click on 'RDS' from the AWS Management Console
2. Create a DB.
	* (there is an option to restore from S3 as well)
3. Choose the DB creation method: Standard or Easy:
	* Standard: allow the user to configure more specifications.
	* Easy: as it sounds, gets the user up and running faster with fewer configurable options (kind of like the
	  "Lightsail" of Storage).,
4. Choose a DB engine type (see above for options).
5. Choose the engine version
6. Choose a template:
	* "Production", "Dev/Test" and "Free Tier"
7. Create a DB instance identifier (note this is not the name of a table).
8. Choose DB instance size.
9. Choose storage type and the minimum and maximum storage (for storage).
10. Choose to enable/not enable Multi AZ.
11. Walk through the Standard configurations, selecting the appropriate methods for your use case. 

* Note that at the end of the RDS DB setup, there will be a section that has the estimate of the monthly cost of running
  your DB.

## Nonrelational Databases

### DynamoDB 

* AWS's Key-Value (NoSQL) Database 
* Associative array == dictionary == hash table/array (all very similar JSON)
	* **Key must be unique**
		* Most likely good to have a naming convention for keys to ensure the structure is organized.
* Data is stored and retrieved using `get`, `put` and `delete` commands
* Queries are based on the key
* Used for high performance applications with single digit latency.
* **Not optimized for search operations; it is very expensive to scan the entire key-value store**
* Use cases:
	* Commonly used for in-memory data caching. They can speed up applications by minimizing reads and writes to
	  slower disk-based systems.
* Advantages of DynamoDB:
	* Fully managed by AWS
	* Schema-less
	* Highly available
	* Fast (regardless of size (unlike relational DBs))
* Disadvantages of DynamoDB:
	* Eventual Consistency
		* This means that there is the possibility that stale data is returned from a query.
	* Queries are less flexible than SQL
		* Computation will have to be done in the application itself.
	* Workflow limitations:
		* Maximum record size of 400 KB
		* maximum indexes per table: 20 global, 5 secondary.
	* Provisioned throughput
		* IOPS must be set in advance; therefore if the user exceeds this threshold, the query will fail.

#### Creating a DynamoDB

Since DynamoDB is a NoSQL DB, there are fewer specifications required to get things up and running.

The bare minimum to get a DynamoDB started:

1. Choose a Table Name
2. Choose a PK for that table (used to partition across hosts for scalability and availability).
3. Accept remaining defaults
4. Create the DB

if you don't want to accept all the defaults, there are a few more options:

(continued from 2 above)
3. Add a secondary index
	* 1 query can only use 1 index, so if you want to search across multiple attributes, you will need to create
	  additionaly indices.
4. Select Read/Write Capacity
	* AWS bases the cost of DynamoDB on the amount of read/write capacity units.
	* by default, there are 5 read capacity units and 5 write capacity units (RCUs and WCUs).
	* There are two capacity modes the user can choose from:
		* Provisioned: The user sets the RCUs and WCUs 'up front'. This is useful if the workload is
		  known in advance.
		* On-Demand: As the name implies, RCUs and WCUs are **not** specified up front, and are scaled on
		  demand. **This is more expensive than the Provisioned mode,** and therefore should be used when you
		  are unsure what traffic you will have. Once you have an estimate of your RCUs and WCUs needed for
		  performance, you should switch to Provisioned mode to be more cost effective.
5. Set Encryption protocols
	* By default, data is encrypted at rest.

### DocumentDB 

* Designed to store, query and index JSON data.
* Scale horizontally
* Simiilar to key-value stores, however the value can be another key-value pair (nested however many times one would
  like). In the abstract, the values in Document Databases are called "Documents".
* Data inside a document can be queried, as opposed to a key-value database where the entire value is returned from a
  single query.
* Use cases:
	* commonly used for storing sensor data from IoT devices.

### Keyspaces (Apache Cassandra) 

* AWS's Column Store database
* Uses a "keyspace" to define the data it contains.
	* Keyspace = set of column families that are similar to table in a relational database.
		* A column family consists of multiple rows.
		* Each row can contain a different number of columns.
		* **Each column is limited to its row.** (in other words, columns do not span all rows in a column
		  family)
		* Each row has a the following components:
			* Row Key (unique identifier)
			* one or more columns. Each column contains a name-value pair, and a timestamp of the datetime
			  the data was inserted (in order to find the more recent version of that data).
* Efficient for data compression and partitioning, as well as applications ther rely heavily on parallel processing.

### ElastiCache

* AWS's In-Memory data store options (best for real-time data access applications)
	* **Not technically a database**; since it is a "data storage" option, it is included here.
* Primary use case is to provide an application with inexpensive access to data with sub-millisecond latency.
* cached data == stale data 
	* The price you pay for not having to query the database is the data isn't the most up-to-date.
	* Caching should provide a speed or cost advantage. It doesn't make sense to cache data that is dynamic or that
	  is seldom accessed.

There are two subtypes:
1. ElastiCache for Redis
2. ElastiCache for Memcached

### Neptune

* AWS's Graph Database
* Used for storing graphical data (e.g. networking applications)
* Components are:
	1. Nodes/Vertices - represent logical entities.
	2. Edges - represent the relationship between two (or more) vertices.
* Many Graph databases use their own proprietary query language
* Use cases:
	* Network based applications
	* Problems whose data can be modeled via a graph.

### Timestream

* AWS's Time Series database
* Timestamps are almost always the key in a time series databases

### Quantum Ledger 

* AWS's ledger database
* Useful for recording transactions of an application
* **All data is immutable**
	* the action of updating data creates a new version of the record.
	* changes to the database do not overwrite existing database records.
* Uses hashing to verify that data hasn't been changed.
	* Uses blockchain technology when creating hashes:
		* Uses the data **and the hash of the previous data** to create the new hash value.
		* Due to using this technology, data that is stored in a quantum ledger database can not be altered
		  without leaving a trace (even by a skilled programmer). Thus, this database is good for uses cases
		  where auditability might be a concern.
* Use cases:
	* Banking
	* Insurance applications (to track the history of claims)

### Elasticsearch Service

* AWS's search database
* Search databases often work with highly unstructured data that is far from consistent.
* Indexes are stored as JSON documents
* Uses an inverted index for fast full text searches.
	* Inverted Index: Lists every unique word in a document and identifies all documents where each word occurs.

# Network Fundamentals for AWS

The pillar of the networking on AWS is the VPC - Virtual Private Cloud. For a comprehensive tutorial, check [the
documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

## VPC

* VPCs are **isolated** segments of the AWS cloud; they can be thought of as distinct 'computational universes' that
  aren't connected to other VPCs (or the internet) by default.
* Default = 5 VPCs per region per AWS account.
* High level view of creating a VPC:
	1. Give your VPC a name.
	2. Define an IP address range the VPC can use - in the form of a CIDR block (Classless Inter-Domain Routing).

## Subnets

* Subnets are a subset of your VPC.
* In the same way that the VPC must have a CIDR block assigned to it, each subnet must have a CIDR block assigned to it.
* Subnets can communicate with other subnets within a VPC be default.
* A route table is a set of rules that determine how traffic flows into, out of and within your VPC.
* Subnets are private by default (i.e. not connected to the internet). To make a subnet public, an internet gateway
  (IGW) must be attached to your VPC, and then the address of the IGW must be added to the route table of the subnet you
  would like to make public.
* Subnets must be associated within an AZ within the region of your VPC.
* For high resiliency and availability, it is best to replicate services and their subnets across multiple AZs.
	* In the image below, services are replicated across AZs. If one of the 3 AZs fails, there won't be an
	  interruption in service.

![](images/multi_az_networking.png)

* Note that not all IP addresses in a subnet's CIDR block are available to be assigned to various hosts:
	* Certain IPs are reserved for AWS networking, DNS, and other routing services.

## NACLs - Network Access Control Lists 

* [AWS Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
	* "A network access control list (NACL) is an optional layer of security for your VPC that acts as a firewall
	  for controlling traffic in and out of **one or more** subnets."
* **Allow/Deny traffic at the network level**.
* NACLs are network firewalls. They determine what traffic is allowed to flow into/out of one or more subnets.
	* Note that this is not the same as route tables.
	* Route tables define **how** traffic moves **within** a subnet.
* NACLs define rules for both inbound and outbound traffic.
* Rules in NACLs are executed in a similar fashion to `if elif else` statements:
	* Once a condition is met, the action associated with that condition is executed and traffic is denied/allowed
	  accordingly. **No conditions are checked after one is met.**
* **Stateless**

## Security Groups

* **Control traffic at the instance level ( as opposed to the network level (NACLs) )**.
* Different setup from NACLs in that there isn't a "Allow/Deny" field in the setup table; with Security Groups, if there
  is a row specifying a Type, Protocol, Port, etc, then that type of traffic is allows. In short, if the traffic is
  specified in the security group, it's allows to communicate with the instance. If it isn't in the Security Group, it
  is denied.
* **Stateful**

## NAT Gateway 

* "A NAT Gateway allows intances within a private subnet to access the internet while blocking all traffic that
  originates from the internet."
	* This is necessary because the user is responsible for keeping the OS' of instances within a private subnet up
	  to date.
* NAT Gateways are logically located within a public subnet (and therefore access the internet the same way any other
  resource in the public subnet does - through the IGW).

## Bastion Hosts

* Since instances within a private subnet are, by designe, not able to be accessed via the internet, there needs to be a
  way to access these instance for developers/engineers to work; enter Bastion Hosts.
* Bastion hosts are EC2 instances used for developers/engineers to access EC2 instances within a private subnet.

In the image below:
* The engineer has two private SSH keys on his machine; one that accesses the bastion host and one that accesses the EC2
  instance in the private subnet.
* Working backwards (from EC2 instance to dev computer):
	* The security group of the EC2 instance in the private subnet must be configured to allow traffic from the
	  bastion host in the public subnet (via SSH).
	* The security group of the bastion host must be configured to allows traffic from the developer's/engineer's
	  computer (via SSH).
	* The NACL of the public subnet allows traffic from the IGW.
	* **SSH Agent Forwarding** allows the developer to use the bastion host and "jump" to the EC2 instance he wants
	  to work on.

![](images/bastion_host_networking.png)
