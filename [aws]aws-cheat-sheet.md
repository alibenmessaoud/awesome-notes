#  AWS Certification Exam Cheat Sheet

### AWS Region, AZs, Edge locations

- A region is a separate geographic area isolated from the other regions & helps achieve the greatest possible fault tolerance and stability.
  - Communication **between regions is across the public Internet**
- A region has **multiple Availability Zones** (AZ)
- Each AZ is physically isolated, geographically separated from each other and designed as an independent failure zone
  - Communication between AZs are **low-latency private links (not public internet)**
- Edge locations are locations maintained by AWS through a worldwide network of data centers for the distribution of content to reduce latency.



### AWS Services

- Security & Identity Service 
- Networking Services
- Compute Services
- Storage & Content Delivery 
- Database Services
- Analytics Services
- Application Services
- Management Tools



### AWS Organizations

- Offers policy-based management for multiple AWS accounts.
  - Manage policies across multiple accounts.
  - Allows creation of groups of accounts and then apply policies to those groups.
- Simplify the billing for multiple accounts.
  - Single payment method for all the accounts in the organization through consolidated billing.



### Consolidate Billing

- Paying account with multiple linked accounts
- Paying account is independent and should be only used for billing purpose
- Paying account cannot access resources of other accounts unless given exclusively access through Cross Account roles
- All linked accounts are independent and soft limit of 20
- Provides Volume pricing discount for usage across the accounts
- Allows unused Reserved Instances to be applied across the group
- Free tier is not applicable across the accounts



### Tags & Resource Groups

- Tags help to label and manage/ organize resources.
  - Tags are meta data as key/value pairs with an AWS resources.
  - Can be inherited when created resources created from Auto Scaling, Cloud Formation, Elastic Beanstalk etc.
  - Can be used for
    - Cost allocation to categorize and track the AWS costs
    - Conditional Access Control policy to define permission to allow or deny access on resources based on tags
- Resource Group is a collection of resources that share one or more tags.



### IDS/IPS

- Intrusion detection and intrusion prevention systems
- Monitor events in your network for security threats and stop threats once detected.
- IDS/IPS strategies
  - **Host Based Firewall – Forward Deployed IDS** where the IDS itself is installed on the instances.
  - **Host Based Firewall – Traffic Replication** where IDS agents installed on instances which send/duplicate the data to a centralized IDS system.
  - **In-Line Firewall – Inbound IDS/IPS Tier** (like a WAF configuration) which identifies and drops suspect packets.



### DDOS Mitigation

- Minimize the Attack surface
  - use ELB/ CloudFront/ Route 53 to distribute load
  - maintain resources in private subnets and use Bastion servers
- Scale to absorb the attack
  - scaling helps buy time to analyze and respond to an attack
  - auto scaling with ELB to handle increase in load to help absorb attacks
  - CloudFront, Route 53 inherently scales as per the demand
- Learn normal behavior (IDS/ WAF)
  - analyze and benchmark to define rules on normal behavior
  - use CloudWatch
- Create a plan for attacks

### AWS Services Region, AZ, Subnet VPC limitations

- Services like IAM (user, role, group, SSL certificate), Route 53, STS are Global and available across regions.
- All other AWS services are limited to Region or within Region and do not exclusively copy data across regions unless configured.
- AMI are limited to region and need to be copied over to other region.
- EBS volumes are limited to the AZ, and can be migrated by creating snapshots and copying them to another region.
- Reserved instances cannot be migrated to another region (can be migrated to other AZ now).
- RDS instances are limited to the region and can be recreated in a different region by either using snapshots or promoting a Read Replica (master and copies).
- Placement groups (logical grouping of instances within a single AZ) are limited to the AZ
  - Cluster Placement groups are limited to single AZs
  - Spread Placement groups can span across multiple AZs
- S3 data is replicated within the region and can be move to another region using cross region replication
- DynamoDB maintains data within the region can be replicated to another region using DynamoDB cross region replication (using DynamoDB streams) or Data Pipeline using EMR (old method)
- Redshift Cluster span within an AZ only, and can be created in other AZ using snapshots

### Disaster Recovery

RTO & RPO

- Services
  - Region and AZ to launch services across multiple facilities
  - EC2 instances with the ability to scale and launch across AZs
  - EBS with Snapshot to recreate volumes in different AZ or region
  - AMI to quickly launch preconfigured EC2 instances
  - ELB and Auto Scaling to scale and launch instances across AZs
  - VPC to create private, isolated section
  - Elastic IP address as static IP address
  - ENI with pre allocated Mac Address
  - Route 53 is highly available and scalable DNS service to distribute traffic across EC2 instances and ELB in different AZs and regions
  - Direct Connect for speed data transfer (takes time to setup and expensive then VPN)
  - S3 and Glacier (with RTO of 3-5 hours) provides durable storage
  - RDS snapshots and Multi AZ support and Read Replicas across regions
  - DynamoDB with cross region replication
  - Redshift snapshots to recreate the cluster
  - Storage Gateway to backup the data in AWS
  - Import/Export to move large amount of data to AWS (if internet speed is the bottleneck)
  - CloudFormation, Elastic Beanstalk and Opsworks as orchestration tools for automation and recreate the infrastructure



# CCP

### Domain 1: Cloud Concepts

Define the AWS Cloud and its value proposition

> AWS is a secure cloud services platform, offering compute power, database storage, content delivery and other functionality to help businesses scale and grow. 

> Agility (speed, innovation); Elasticity (scale on demand, eliminate wasted capacity); Flexibility (broad set of products, low to no cost to entry); Security (shared responsibility model, compliance)

> cloud native technologies

Identify aspects of AWS Cloud economics

- Pay-as-you-go Pricing
- Tiered Pricing (Use More, Pay Less)
- Cost Optimization
  - Cost Explorer is a free tool that provides pre-configured reports for common AWS spend queries for current and historical periods, as well as forecasting.
- Trusted Advisor
  -  inspects your AWS environment to save you money & improve your system performance.

Advantages of Cloud Computing

- Trade capital expense for variable expense
- Benefit from massive economies of scale
- Stop guessing about capacity
- Increase speed and agility
- Stop spending money running and maintaining data centers
- Go global in minutes

- AWS Well-Architect-ed Framework
  - Features include agility, security, reliability, performance efficiency, cost optimization, and operational excellence.

List the different cloud architecture design principles

 Core Principles

- Scalability: Elasticity and Scalability

  - elasticity avoids the costs of these over-provisioned resources : pay-as-you-go
  - scalability is the ability to scale without changing the design (design to fail)
  - scaling types: (Horizontal increase in the number of resources & Vertical Scaling increase in the capabilities of the resources)
  - State: stateless, statefull: 
    - DynamoDB, Redis, to maintain state information 
    - ELB sticky sessions, to bind a transaction of a session to a specific resource.
  - Distributed
    - Push model, for e.g. through ELB
    - Pull model, for e.g. through SQS or Kinesis

- Disposable Resources Instead of Fixed Servers

  - Immutable infrastructure: launch and never update. Create new resources with new configuration => easier rollbacks.
  - Scripting; from snapshots; containers (Elastic Beanstalk and ECS); infra-as-code (CloudFormation and OpsWorks)

- Automation

  - AWS Elastic Beanstalk: push code and it automatically handles all the details, such as resource provisioning, load balancing, auto scaling, and monitoring.
  - EC2 Auto recovery: CloudWatch alarm to monitor EC2 instance and recover it if it becomes impaired (data that is in-memory is lost after reboot).
  - Auto Scaling: maintain availability and scale an EC2 capacity up or down automatically according to conditions you define.
  - CloudWatch Alarms: sends a message by Simple Notification Service when a particular metric goes beyond a specified threshold for a specified number of periods.
  - Amazon CloudWatch Events: delivers a near real-time stream of events describing changes in  resources. Using simple rules you can route each type of event to one or more targets.
  - AWS OpsWorks Lifecycle events: it supports continuous configuration through lifecycle events that automatically update your instances’ config to adapt to environment changes using Chef.
  - AWS Lambda Scheduled events: These events allow you to create a Lambda function and direct AWS Lambda to execute it on a regular schedule.

- Loose Coupling

  - Asynchronous Integration: through an intermediate layer (SQS, Kinesis); introduces additional resiliency
  - Service Discovery through ELB and Route 53
  - Well-Defined Interfaces to interact with each other, eg RESTful APIs with API Gateway 

- Services, Not Servers

  - Managed Services
  - Serverless Architectures

- Databases

  - managed database services.
  - RDS(SQL), NoSQL(DynamoDB), DWH (Redshift).
  - horizontal-scaling is often based on the partitioning of the data.

- Managing Increasing Volumes of Data

  - data lake architecture, store and you will have the tools to process it.

- Removing Single Points of Failure

  - A system is HA if it can withstand the failure of 1 or more failures.

    > redundancy, detection and reaction to failure, automated resilience through AZ, fault isolation by sharding.

  - Standby Redundancy (1 instance of a resource, starts a new instance if there's a failure), Active Redundancy (+2 of same resources).

  - Synchronous replication (commit transaction after commit in primary and replicas instances), Asynchronous replication, Quorum-based replication (combination).

  - ELB or Route 53 to configure health checks and mask failure by routing traffic to healthy endpoints/ ElastiCache Redis engine supports replication with automatic failover.

- Optimize for Cost

  - selecting the right solutions

- Caching

  - Application Data Caching: ElastiCache, Memcached and Redis
  - Edge Caching: CloudFront

- Security

  - AWS works on shared security responsibility model
    - AWS is responsible for the security of the underlying cloud infrastructure
    - The customer is responsible for securing the workloads you deploy in AWS
  - AWS also provides ample security features
    - IAM (policies), Cognito (mobile via tokens), VPC (isolate), WAF, CloudWatch (log), CloudTrail (audit)

### Domain 2: Security

- Define the AWS Shared Responsibility model
  - includes having a clear understanding of what AWS and Customer responsibilities are
  - in/of Cloud
    - aws-in: protect the global infrastructure (hardware, software, network)/ provide compliance reports from third-party auditors/ security configuration of managed services (os, pacthes, firewall, recovery)
    - custmer-of: under your control of user (eg EC2, VPC, S3)/ mangagement (os, patches, AWS-provided firewall (called a security group))/ managed service (configure logical access controls  and protect the account credentials)
  - AWS Compliance Program: AWS respect all standards and best practice of security.
    - Stds: SOC 1/SSAE 16/ISAE 3402 (formerly SAS 70); SOC 2; SOC 3; FISMA, DIACAP, and FedRAMP; DOD CSM Levels 1-5; PCI DSS Level 1; ISO 9001 / ISO 27001; ITAR; FIPS; 140-2 MTCS Level 3;
  - Physical security
    - Storage Decommissioning using DoD 5220.22-M; and physically destroyed in accordance with industry-standard practices.
  - Network
    - Amazon Corporate network relies on user IDs, passwords, and Kerberos, while the AWS Production network require SSH public-key authentication through a bastion host.
    - automated monitoring systems against attacks: DDOS (AWS uses proprietary DDoS mitigation techniques); Man in the Middle (APIs are available via SS); IP spoofing (AWS-controlled, host-based firewall infrastructure will not permit an instance to send traffic with a source IP or MAC address other than its own.); Port Scanning (Detect and block by AWS); Packet Sniffing by other tenants (the hypervisor will not deliver any traffic to them that is not addressed to them, Even two virtual instances that are owned by the same customer located on the same physical host cannot listen to each other’s traffic.)
  - Secure Design Principles
    - design reviews by the AWS Security Team, threat modeling, and completion of a risk assessment
    - Static code analysis
    - Recurring penetration testing 
  - AWS Account Security Features
    - AWS Credentials: Password (root or IAM user account); MFA (root or IAM user account); Key pairs (SSH for EC2 or signed urls for CloudFront); X.509 Certficates (SOAP, SSL for HTTPS)
    - Individual User Accounts: create IAM user account with least privilege. 
    - Secure HTTPS Access Points
    - Security Logs: CloudTrail which provides logs of all requests
    - Trusted Advisor Security Checks: 
  
- Define AWS Cloud security and compliance concepts
  - Infrastructure Security: Network firewalls, In transit encryption, Private or dedicated connections
  - Infrastructure Resilience: Autoscaling, CloudFront, Route 53 can be used to prevent DDoS.
  - Data Encryption: EBS, S3, Glacier, RDS & Redshift; AWS KMS; Dedicated hardware-based cryptographic; 
  - Standards and Best Practices: assessment service(AWS Inspector assesses applications for vulnerabilities in network, OS, & storage); tools to manage the creation and decommissioning according to standards; AWS Config identifies AWS resources then track & manage changes to those resources over time; AWS CloudFormation to create standard, preconfigured environments;
  - Monitoring and Logging: CloudTrail deep observes (who, what, when, from)  into API calls; Log aggregation; CloudWatch sends alert notifications for a specific event;
  - Identity: Identity and Access Management (IAM) to define user/permission; Multi-Factor Authentication for hardware-based authenticators; Directory Service allows you to federate;
  - Security Support: Real-time insight through Trusted Advisor; Support with Technical Account Manager;
  
- Identify AWS access management capabilities: IAM

  - It is all about Permissions and Policies (define who has access and what actions can they perform)

  - IAM Policy is a document (JSON) that formally states one or more permissions.

    - *Principal A is allowed/denied (effect) to perform Action B on Resource C given Conditions D*

    ```json
    {
        "Version": "2019-12-17",
        "Statement": {
           "Principal": {"AWS": ["arn:aws:iam::ACCOUNT-ID-WITHOUT-HYPHENS:root"]},
           "Action": "s3:ListBucket",
           "Effect": "Allow",
           "Resource": "arn:aws:s3:::example_bucket",
           "Condition": {"StringLike": {
               "s3:prefix": [ "home/${aws:username}/" ]
                  }
              }
        }
    }
    ```

    - An Entity is associated with Multiple Policies/ Policy = {statement}/ policy refers to a permission.
    -  Identity-based, or IAM permissions: attached to user/group/role and specify what user/group/role can do; User/group/role acts as principal; A permission can be applied to almost all the aws services; IAM Policies can either be inline or managed;
    - Resource-based permissions: attached to a resource; specifies both the Principal and the Actions; inline only, not managed; supported only by some AWS services; 
    - Managed Policies and Inline Policies
      - managed: standalone; can be attached to multiple entities; has an arn; created by aws and customer;
      - inline: embedded directly into a single entity (user/group/role); created by customer; deletion of entity or resource includes the deletion the policy.
    - IAM Policy Variables
      - Policy has placeholders of variables; a request data take part in each placeholders and the policy get evaluated; policy variables are case sensitive; (aws:username, aws:userid, aws:SourceIp, aws:CurrentTime, etc.)

- Identify resources for security support
  
  - AWS Support: provide tools to make sure the AWS environment is built and operated to be secure, highly available, efficient, and cost effective.
  - AWS Support provides: Real-time insight through AWS Trusted Advisor + Proactive support and advocacy through Technical Account Manager (TAM).
  - Icludes services like CloudTrail
    - AWS CloudTrail helps to get a history of AWS API calls and related events for the AWS account.

### Domain 3: Technology

- Define methods of deploying and operating in the AWS Cloud
  - AWS provide multiple options for provisioning your IT infrastructure and the deployment:
    - Elastic Beanstalk: is a high-level deployment tool; It handles the details of the environment (provisioning, load balancing, scaling, & monitoring)
    - CloudFormation: helps you model and set up your Amazon Web Services resources; create a template that describes all the AWS resources that you want;
    - OpsWorks: configuration management service to configure and operate applications by using Chef.
    - CodeCommit: is a host highly scalable private git repositories; It integrates with CodePipeline & CodeDeploy to streamline your development and release process.
    - CodePipeline: is a continuous integration and continuous delivery service;
    -  CodeDeploy: is a service that automates code deployments and software deployments; 
    - Non Aws
      - Infrastructure as Code(Terraform, Salt stack); Config managment (Chef, Puppet); Continuous Integration (Jenkins, Team city); VCS(Github, Gitlab)
    - Good Practice
      - Provision infrastructure from code; Deploy from VCS;
      - Logging for every action (CloudWatch Logs and CloudTrail)
      - Automatic Configuration; Use variables; Monitor (CloudWatch); 
      - "Blue/Green" method for old production/new version

- Define the AWS global infrastructure

  - 69 AZs within 22 Regions; ++ Edge Locations Global CDN (as CloudFront and Lambda@Edge to cache data and reduce latency)
  - High Availability Through Multiple Availability Zones
  - Further Improving Availability by Deploying in Multiple Regions
  - IAM: Users, Groups, Roles, Accounts – Global ; Key Pairs – Global (RSA key pair) or Regional (EC2 created key pairs)
  - Virtual Private Cloud: 
    - VPC are created within a region
    - Subnet can span only a single Availability Zone
    - A security group is tied to a region and can be assigned only to instances in the same region.
    - You cannot create an endpoint between a VPC and an AWS service in a different region.
    - Elastic IP address created within the region can be assigned to instances within the region only.
  - EC2
    - Resource Identifiers (AMI, EBS, Volume) – Regional
    - Instances – AZ + Resource Identifiers - Regional
    - EBS Volumes (attatched to one instance) – AZ
    - AMIs – Regional + Can copy
    - Auto Scaling – Regional (across multiple AZs)
    - Elastic Load Balancer (distributes traffic across instances in multiple AZs) – Regional
    - Cluster Placement Groups – AZ
  - S3 – Global but Data is Regional
    - S3 buckets are created within the selected region + Objects replicated across AZs 
  - Route53 – Global (at edge locations)
  - DynamoDb – Regional
    - Data stored within the same region and replicated across multiple AZs
  - WAF – Global
  - CloudFront – Global
  - Storage Gateway – Regional
  - - 

- Identify the core AWS services
  - Compute Services
    - EC2: Elastic Compute Cloud; provides secure, resizable compute capacity in the cloud; Instance Types: {General Purpose, Compute Optimised, Memory Optimised, Accelerated Computing, Storage Optimised}; Pricing: {On-Demand, Spot Instances, Reserved Instances, Dedicated}; Security Groups (virtual firewall associated to an instance that controls the traffic); Access: Console, CLI (aws, linux, windows), aws Query API, SDK; 
    - ECS: EC2 Container Service; container management service for Docker based on EC2; it eliminates provisioning EC2 cluster; ECS is a regional; 
      - ECS with EC2 launch type aka ECS container instance: EC2 instance that runs the ECS Container Agent.
      - ECS with Fargate launch: No EC2 instances to manage anymore. You pay for running tasks.
      - Task definition is a description of an application that contains one or more docker containers.
      - ECS vs Elastic Beanstalk: Elastic Beanstalk is more of an application management platform.
    - Elastic Beanstalk: quickly deploy and manage apps; reduces management complexity; enables automated infrastructure management and code deployment with (Mgt, Provision, Balancing, Scaling, Deploy, Monitor); 
      - Elastic Beanstalk Components: Env, versions, config;
      - Elastic Beanstalk environment requires an environment tier, platform, & environment type;
        - Web Environment Tier -> web app: ELB + ASG + EC2 + R53 + host manager
        - Worker Environment Tier -> background jobs + ASG + EC2 + IAM role + SQS
      - Works inside a VPC; Supports IAM; Link to CloudFront to distribute S3 content; Cloud Trail; RDS inside Elastic Beanstalk containers; S3; 
      - Deploy: all at once, rolling, rolling with batch, immutable, blue-green
    - Lambda: Serverless computing; run code without provisioning or managing servers; priced on a pay per use; zero administration; Scalability and availability; Security: code stored and encrypted in S3; 
      - Lambda functions and event sources;
      - Lambda@Edge: run code closer to users;
    - Auto Scaling: ensure a correct number of EC2 instances are always running to handle the load of the application.
      - better fault tolerance + HA!
    
  - Storage Services 
    - S3: value object store (not a block level storage); private storage by default; unlimited storage space; pay as you use; volume increases = low rates; 
      - Buckets: container of objects; ownership is not transferable; names are globally unique but bucket is regional; same performance (flat structure, no folder physically, cannot nest buckets); limit to100 buckets by account; deletable; pagination; retrieve max 1000 objects;
      - Objects: Identified by key name and version ID; {k, v, md, versionid, subresources, access}, md = {system, user (prefixed by "x-amz-meta")}; lives in a single region; versioning;
      - Operations: list (get keys of max 1000, pagination, filter by key as prefix and delimiter); retrieve (all, multipart using http headers, using pre-signed urls, md goes in header); upload (max 5 Gb in PUT, multipart max 5TB, Return Etag of md5); copy (max 5 Gb in PUT, multipart max 5TB, user-md will be copied, can update md values); delete (single or multiple, non-versioned -> delete with key, versioned -> play with key+version); restore from Glacier (restore before access archive, cost archive and copy);
      - Pre-Signed URLs: create for download and upload; expires;
      - Multipart Upload: max 10k parts of 5Mb-5Gb + last part 5Mb; No order; file size max 5TB; (+) : recover fom network issues; Process: 1/ init parts upload ids, md; 2/ upload, overwrite for same id, Etag returned for md5, 3/ on complete, S3 concat parts and return Etag, on abort, delete parts and charge. Need to receive complete or abort or it will charge storage; 
    - Virtual Hosted Style vs Path-Style Request: http://s3-eu-west-1.amazonaws.com/mybucket/ob.txt vs http://mybucket.s3.amazonaws.com/ob.txt
      - Pricing: cost vary by region, charges for storage Gb/mo, requests, data transfer outside region; 
    - EBS: block-level storage volumes (as raw, unformatted, external drive), can be attached to a running instance, recommended for frequent and granular updates (db, fs), take snapshot & store on S3, support encryption, available on same AZ of the instance (can create instance from snapshot on other AZ), size (Magnetic: 1 Gb to 1 Tb; General Purpose & Provisioned IOPS (SSD): 1 Gb to 16 Tb), IO (General Purpose: max throughput 10,000 IOPS and 160 MB/s, Provisioned IOPS: max throughput 20,000 IOPS and 320 MB/s).
      - (+): availability via replication to any AZ; persistence independently of os (even on reboot/stop); encryption ((AES-256 + key over AWS KMS, happens on server that hosts the EC2 -> EBS storage), Snapshots (copy to S3, compressed, version)
      - EBS volumes creation: new, restore from snapshots, 
      - EBS volumes detachment: detach explicitly or after stopping instance, unmount, forced detach, charges still!
      - EBS Volume Deletion: wipe data; can't be attached
      - EBS Volume Types: SSD (general purpose gp2, privisioned IOPS io1) + HDD (throughput optimized st1, cold sc1) [sdd use +10k IOPS/volume, hdd use 500 IOPS/volume]
    - EFS: managed FS service, to setup & scale file storage; Support NFS4 protocol, multiple EC2 can access EFS instance at the same time (provide ds for workload); Store {data + md} on ++AZ/Region, high level of throughput + parallel access; file access semantics (consistency + lock); control access through POSIX permissions; Use AWS DataSync to move data between EFS & on-premise; backup schedule using EFS-to-EFS;
      - Perf mode(gp or max); Throughput mode (bursting/ provisioned); Mount target: IPv4 + NFSv4 endpoint to access data from AZs; Components: id, token, fs size, nb and mount targets, state; Mgmt: files, dirs, slinks, delete; Monitor (Cloud Watch alarms logs events + Cloud trail monitor + logs); Security; Pricing: pay storage + previsioned throughput;
    - Glacier: optimized for archival, rarely retrieved called cold data, extremely low-cost, durability of 99.999999999% for an archive by syncing across multiple devices, any kind of data, retrieval time varies from hours to 1-5 minutes, AES-256 encryption via KMS, Management (API, CLI, Console), Use cases include (media archives, regulatory compliance, financial and healthcare, long-term backups).
      - Data Model concepts: Vault (container for storing archives, unique address, unlimited archives, can create max 1K vaults/region), Archive (data, identified, single or multipart upload requests), Jobs (to retrieve, inventory, retrieval requests are asynchronous operations, multiple jobs in progress by vault), Notification (asynchronous jobs, use SNS).
      - Retrievals Options: Std within 3-5 hours; Bulk of Pb within 5-12 hours; Expedited within 1-5 minutes (On-Demand and Provisioned); 
      - Operations: Vault ops (crud, inventory clean empty vaults, infos); Archive ops (upload: using AWS Import/export service, md for upload; download: async, 2 step by init job & download after notification; range of retrievals; simple delete, update need delete & re-upload).
    
  - Security, Identity, and Compliance
    -  IAM: Permissions and Policies; Permission allows you to define who has access and what actions can they perform; An Entity can be associated with Multiple Policies; 
    -  Organizations, 
    - WAF: web application firewall that helps protect web applications from attacks; 
      - integrates with Amazon CloudFront and the ALB; 
        - with CloudFront: WAF rules run in all Edge Locations, located around the world close to the end users. Block requests before reaching the web servers.
        - with ALB: WAF rules run in region and can be used to protect internet-facing as well as internal load balancers.
        - with API Gateway: Can help secure and protect the REST APIs.
      - How WAF Works
        - Conditions (define basic characteristics to watch): content for XSS, IP@, query parts, SQL, strings
        - Rules (Rules are basically Combination of Conditions)
        - Web ACLs: provides Combination of Rules + Action (default, block, allow, count)
    
  - Databases
    - RDS: set up, operate, and scale DB; manages common admin tasks; managed service; shell (root ssh) access to DB instances is not provided;
      - features & benefits: CPU, RAM, storage, and IOPS scale independently; manages manual/ automated backups, patching, detection failure/ recovery; ha by primary instance and a synchronous secondary instance; Read Replicas to increase read scaling; Secure via IAM; VPC, SSL, encryption;
      - RDS Components
        - DB Instance: isolated database environment; runs a DB engine; access from CLI; console, RDS API; its capacity is baed on its class; storage (5 GB to 6 TB, : Magnetic, General Purpose (SSD), and Provisioned IOPS (SSD)); can host multiple databases/ single Oracle database with multiple schemas; 
        - Regions and AZs: Multi-AZ deployment => synchronous standby replica; 
        - Security Groups: controls the access of IP address ranges or EC2 instances;
        - DB Parameter Groups: engine configuration of each instance
        - DB Option Groups: engines offer tools to manage; eg APEX;
      - RDS Interfaces: cli, api & sdk, console
      - Pricing: by class of instance; bill by the instance-hour, storage billing per GB per month and pro-rated if scaled; I/O requests; Backup; Data transfer; RI for DB can be purchased;
    - Aurora: fully managed RDS engine compatible with MySQL & PostgreSQL; 5x & 3x fatser than MySQL & PostgreSQL; HA substorage system; Storage auto-scale up 10 Gb - 64 Tb; fast cloning operation; fault tolerant & self healing; Aurora DB cluster of +1 instances & a cluster of volumes; cluster of volumes spans over +AZs. 
      - Cluster types : Primary instance + Replica; Reboot of Primary tells Aurora to reboor the others;
      - Endpoints: cluster endpoint for the primary instance, reader endpoint for the replica, custom endpoint for specific set of instances, instance endpoint for a specific instance; 
      - Storage & HA: 6 copies over 3 AZs; Classes: Memory optimized and Burstable perf;  
      - A non-serverless Aurora DB cluster is called a provisioned DB cluster.
      - Serverless: support multi AZs failover; always encrypted; Serverless config: scale up/down or pause capacity by configuration to save off;
      - Deletion protection is enabled;
      - Monitoring: RDS events for all data (instance, cluster, snapshot, paramter & security group); RDS enhanced monitoring (real time); RDS Perf Insights (check load & troubleshoot); CloudWatch metrics, Alarms & Logs 
      - Security: IAM, Use VPC security group to control acces from an EC2, TLLS/ SSL; Firewall; Encryption;
      - Pricing: charge for instance, io requests, backup storage & data transfer; On-demand (pay by hour for instance hour); RI (1y deal/ 3y dela;)
    - DynamoDB: fully managed NoSQL DB; synchronously replicates data across three Regions so HA+DD on SDD; secure auth & access;
      - primary keys: Partition Key (simple 1 column pk, determine location of data in a partition); Partition Key and Sort Key (composite key 2 columns pk, 1st is partition & 2nd sort key)
      - Can use Elasticache in front of DynamoDB; 
      - Perf: auto scale horizontally; allows provisioned table reads and writes (Scale up throughput when needed); automatically partitions, reallocates and re-partitions & provisions additional capacity; 
      - Consistency: read types (eventually consistent: maximizes the read throughput but ...; strongly consistent: read returns a result that reflects all successful writes!)
      - Security: Fine Grained Access Control (FGAC) to control over data in the table; control who can perform on items of a table; integrated with IAM.
      - Encryption: SSL/TLS; data, indexes, copies; once enabled for a table, cannot be disabled; Streams do not support encryption; 256-bit AES; backup encryption uses S3's server side encryption; 
      - Pricing: Provisioned throughput (Pay flat, hourly rate); Reserved capacity (Pay a one-time upfront fee);
    - ElastiCache: in-memory caching; managed service to deploy and run Memcached or Redis protocol-compliant cache clusters; improve web app perf to acces data fast via cache; simplify and offload the management, monitoring, and operation; automate common administrative tasks; detect and replace failed cache nodes; integration with CloudWatch to enhance cache node perfs; 
      - Redis: advanced key-value cache & store (like a DB); 
        -  ElastiCache enables the management, monitoring and operation of a Redis nodes
        -  can be used as kv datastore, HA, & scale to 16 nodes + 5 replicas of 3.55 Tb of in-memory data; 
        -  Master slave mode and replication + mAZs
        -  Scale up vertically to the largest node; 
        -  Backup and Restore allows users to create snapshots of the Redis clusters
      - Memcached: in-memory key-value store for small chunks
        -  ElastiCache for Memcached is used to cache a variety of objects (from content in RDS or its DB under EC2; web page; transient session data)
        -  Scale up vertically as the node size & horizontally as number of nodes;
        -  Autodiscover nodes added by client when removed from the EC cluster.
    
  - Migration – Database Migration Service
    - homogeneous & heterogeneous db migration; To move to RDS DBs; 
    - Schema conversion tool: for heterogeneous;
    - Basic schema copy: quickly migrate dbs
    
  - Networking and Content Delivery
  
    - VPC: virtual network, logically isolated, enable use to network as it is possible in physical network. 
  
      - Sizing: Allowed IP adresses (CIDR block) size is between 2^4 – 16 available IP address & 2^16 – 65536 IP address; t’s possible to specify a range of publicly routable IP addresses; 
  
      - IP Addresses: instance can get Private, Public and Elastic IP address assigned to it;
  
        - Private: internal use inside the subnet; within the IP address range of the subnet;
        - Public: communication between instances with other public services; .
        - Elastic: static & persistent public addresses which can be associated and disassociated with the instance, as required; owned by the account unless released; 
  
      - Elastic Network Interface (ENI): Each Instance is attached with default elastic network interface (Primary Network Interface eth0)
  
        An attached instance will possibly have primary private IP address, multiple secondary private IP addresses; an Elastic IP address per private IP address; One; public IP address; One or more security groups; A MAC address;
  
      - Route Tables: Route table defines rules to route traffic inside the subnet; Each VPC has a implicit router; Each VPC has a Main Route table with custom routes; Route tables needs to be updated for Internet gateways, Virtual Private gateways, VPC Peering, VPC Endpoints, NAT Device etc.
  
      - Internet Gateways – IGW:  allows communication between instances in the VPC and the Internet; scale horizontally, redundant & HA; 
  
        - provide a target in the VPC route tables for Internet-routable traffic,
        - perform NAT for instances that have been NOT been assigned public IP addresses.
  
      - NAT: enables instances in a private subnet to connect to the Internet, but prevents the Internet from initiating connections with the instances; No IPv6, use egress-only Internet gateway;
  
      - Egress-only Internet gateway: NAT gateway, but for IPv6 only traffic; allows only outbound communication over IPv6 from instances in the VPC to the Internet; 
  
      - VPC Security: Security groups (Act as a firewall for EC2 instances, inbound & outbound traffic at the instance level); ACLs (Act as a firewall for associated subnets, inbound and outbound traffic at the subnet level); Flow logs (Capture information about the IP traffic going to and from network interfaces)
  
      - VPC Flow logs: enables you to capture information about the IP traffic in VPC; Flow log data is stored using Amazon CloudWatch Logs;  No capture real-time log streams for network interfaces; Flow logs can be created & plugged on other service's network interfaces;
  
      - Subnets: Subnet spans a single Availability Zone; configured with an IGW; Public or Private; can be configured to Enable assignment of the Public IP address; Sizing; Security: Subnet security can be configured using Security groups and NACLs;
  
      - Shared VPCs: multiple account to work and share a centrally-managed VPC; owner shares one or more subnets with other accounts of the same organization.
  
      - *Endpoints, Peering, VPN Connections & CloudHub & AWS Security Group vs NACLs*;
  
    - CloudFront: speed up distribution of static, dynamic web or streaming content to end users; use data centers called edge locations; distribute content with low latency and high data transfer speeds; rout each user request to the nearest edge location; reduces the number of network request hops; 
  
      - (+): eliminates the complexity of operating a network of cache servers; copies of objects are held in multiple edge locations around the world; keeps persistent connections with the origin servers; use of collapsing simultaneous viewer requests for the same file into a single request on the origin server; integrates with WAF; objects can be uploaded to the Origin servers with public read or OAI permissions (Origin Access Identity);
  
      - Configuration: 1/ configure origin to distribute files hosted on EC/S3/On-premises; 2/ access; 3/Create a CloudFront distribution; 4/ CloudFront sends the distribution configuration to all the edge locations; 
  
      - Content Delivery: 5/ When user access the website, file or the object the DNS routes the request to the CloudFront edge location; 6/ If the requested object does not exist in the cache at the edge location, CloudFront requests the object from the Origin server and returns it; 7/ at expiration time, for any new request CloudFront checks with the Origin server for any latest versions;
  
      - Delivery Methods: 
  
        - Web distributions: static and dynamic content; multimedia content on demand
        -  RMTP distributions: S3 bucket as the origin; stream media files; 2 distributions are required, Web distribution for media Player and RMTP distribution for media files; 
        - Origin: S3 bucket or an HTTP server as origin, domain mapped, files must be publicly readable or secured using OAI; OAI for S3 only; Distribution can have multiple origins for each bucket; 
  
      - Cache Behavior:  define a Path patterns to apply for the request; Path pattern in a cache behavior determines which requests are routed to the origin;
  
      - Viewer Protocol Policy: configured to define the access protocol HTTP and/or HTTPS;
  
        HTTPS: allow HTTP or HTTPS; use HTTPS only, or redirect all to HTTPS; 
  
        Configure distribution to use the protocol of request or HTTPS only;
  
        Bordel HTTPS;
  
        Improving CloudFront Edge Caches: cache max-age must be longer for less origin access but ...; 
  
        - cache based on the query parameters for web dist; for Web, configure to forward only the query strings for which your origin will return unique objects; use the same case for the parameters; use the same parameter order; for RMTP, CloudFront removes any query string parameters.
  
        - cache based on Cookies: by default is not considering cookies; Configure CloudFront to forward only specified cookies cache all possible combinations and values; case sensitive; Create separate cache behaviors for static and dynamic content; No cookies for RTMP.
        - cache based on headers: CloudFront doesn’t consider headers when caching; same as cookies and query params; No headers for RTMP.
  
        Object Caching & Expiration:
  
        - life is 24 hours; 
  
        - Low expiration time helps serve content that changes frequently; high expiration time helps improve performance and reduce load on the origin; 
        - the origin returns a 304 if content does not change after expiration else 200;
        - non-accessed object will be removed before expiration; 
        - use Cache-Control max-age directive;
        - When the origin returns an HTTP 4xx or 5xx status code, it caches the response for five minutes and then submits the next request for the object to the origin to see if the problem has been resolved; 
  
      - Restrict viewer access
  
        - Use special CloudFront signed URLs or signed cookies (start & end date, ip address); create & use CF urls and not S3 urls is recommended; 
        - Add an account as trusted signer to the distribution to create signed URLs
  
      - Signed URLs vs Signed Cookies
  
        - Use signed URLs in the following cases: Web and no RTMP;
        - Use signed cookies in the following cases: don’t want to change the current URLs.
  
      - Serving Compressed Files: compress files of certain types and serve the compressed result; 
  
      - Distribution Details: 
  
        - Price Class: price varies from location to another; and it depends on the serving;
        - Alternate Domain Names (CNAMEs): a distribution get a domain name
        - Geo Restriction (Geoblocking): restrict access based on location or a list;
  
      - CloudFront with Amazon S3: 
  
        - For an RTMP distribution, S3 bucket is the only supported origin and custom origins cannot be used;
        - CloudFront can take up to an hour to update if S3 location is moved and OAI is applied; 
  
      - OAI
  
        - public read permissions objects in S3 and hence the objects are accessible from both S3 as well as CloudFront; S3 URL is not exposed until it is shared directly; Keep ti secret if you want to use signed urls;
        - OAI can be used to prevent users from accessing S3 directly; so create it and associate it within a distribution; 
  
      - Working with Objects: CF can play with objects to modify or include headers (validate id, origin of the request, CORS, etc.)
  
      - Adding & Updating Objects: objects can be updated either by overwriting the Original object or create a different object; 
  
      - Removing/Invalidating Objects: upon expiry (TTL); change the name (versioning) or invalidate it;
  
      - Access Logs: Log each request; CloudFront delivers access logs for a distribution periodically, up to several times an hour; 
  
      - CloudFront Cost: charges are based on actual usage: 
  
        - Data Transfer Out to Internet
        - HTTP/HTTPS Requests
        - Invalidation Requests
        - Dedicated IP Custom SSL certificates associated with a CloudFront distribution
  
    - Route 53: HAS Domain Name System (DNS) web service
  
      - Responsible of Domain registration, Domain Name System (DNS) service, Health checking
      - Supported DNS Resource Record Types: A, AAAA, CNAME, etc.
      - Alias resource record sets: Alias record is similar to a CNAME, it enables routing of queries to a CloudFront distribution, Elastic Beanstalk, ELB, an S3 bucket configured as a static website, or another Route 53 resource. 
      - Route 53 Hosted Zone: It is an A container for records, which include information about how to route traffic for a domain/ sub domain; has the same name as the corresponding domain; either a public hosted zone or a private hosted zone; Create records in the hosted zone; 
      - Route 53 Split-view (Split-horizon) DNS: it enables to access an internal version of a website using the same domain name that is used publicly; 
      - Route 53 Routing Policy: Simple Routing, Weighted Routing, Latency-based Routing, Failover Routing, Geolocation Routing, 
  
    - Direct Connect: establish a dedicated network connection from your premises to AWS. Direct Connect helps to create virtual interfaces directly to the AWS bypassing Internet service providers in the network path.
  
      - Connection goes over a standard 1 Gb or 10 Gb Ethernet fiber-optic cable between the customer router and Direct Connect router. 
      - Supported speeds of 50Mbps, 100Mbps, 200Mbps, 300Mbps, 400Mbps, and 500Mbps.
      - Provides access to Services in the region it is associated with.
      - (+): Reduced Bandwidth Costs, Consistent Network Performance, Compatible, Private Connectivity to VPC, Elastic;
      - Direct Connect vs IPSec VPN Connections: a VPC VPN Connection utilizes IPSec to establish encrypted connection. VPN Connections are easy to setup & have low to modest bandwidth requirements, and can tolerate connectivity. AWS Direct Connect does not involve the Internet.
      - Direct Connect Anatomy: Amazon maintains AWS Direct Connect PoP (point of presence) across different locations (Colocation Facilities). Connection between the PoP and the Customer GW is called Cross Connect. Once the Cross Connect and the connectivity between the CGW and Customer DataCenter is established, Virtual Interfaces can be created.
      - Direct Connect Redundancy: Direct Connect connections do not provide redundancy; Redundancy can be provided by Establishing a second Direct Connect connection away;
      - Direct Connect LAG (link aggregation group): aggregate multiple connections at a single AWS Direct Connect endpoint; 
  
    - ELB: distribute traffic automatically across multiple healthy EC2 instances; single point of contact to the client; increases the app availability by adding or removing of EC2 instances; a distributed system that is fault tolerant and actively monitored; first line of defense against attacks on network; can offload the work of encryption and decryption; by default, routes each request independently to the registered instance with the smallest load; If an EC2 instance fails, ELB automatically reroutes the traffic to the remaining running healthy EC2 instances; Load Balancers only work across AZs within a region;
  
      - ALB: operates at the layer 7 (application layer); support path-based routing; support port-based in a single instance; support containerized apps; protocols: http/https/ websocket /secure websocket/ IPv6/ SSL TLS; support request tracing, sticky session; L7 features like X-Forwarded-For headers; Provide Access Logs; Provide deletion protection; supports Connection Idle Timeout (Round trip client to instance); integrates CloudWatch to provide metrics; integrates WAF; integrates with CloudTrail to receive a history;
      - NLB: operates at the level 4 (network Layer); load balancing of TCP traffic; HA; High Throughput; Low Latency; Preserve source IP address; Static IP support; Elastic IP support; Health Checks; DNS Fail-over (R53); Flow log data is stored using CloudWatch Logs; 
  
  - Management Tools
  
    - CloudWatch: monitors AWS resources and applications in real-time; collect and track metrics or custom metrics; can set alarms to send notifications or do some ruled changes; 
      - stores the log data indefinitely; alarm history is stored for only 14 days;
      - provides system-wide visibility into resource utilization, application performance, and operational health.
      - Concepts: 
        - metrics (time-ordered set of data points, defined by a name, a namespace, and one or more dimensions, exist only in the region in which they are created, cannot be deleted, but they automatically expire in 14 days); 
        - Namespaces (containers for metrics, eg AWS/EC2), 
        - Dimensions (a name/value pair that uniquely identifies a metric, design a structure for the statistics plan, used to filter result); 
        - Time Stamps, Units, Custom Metrics
        - Statistics (metric data aggregations over specified periods of time), 
        - Periods (length of time associated with a specific statistic), 
        - Aggregation (Multiple data points can be published with the same or similar time stamps on a single stats), 
        - Alarms (watches a single metric, initiate actions on behalf of the user [SNS, autoscale, EC2 op], has state [OK, ALARM, INSUFFICIENT_DATA], Alarms exist only in the region in which they are created), 
      - CloudWatch can be accessed using AWS console, CW CLI, AWS CLI, API, SDK
      - Logs: from  EC2 instances, CloudTrail, Route 53, and other sources; require an agent to be installed on the EC2 instances for eg. 
        - An VPC endpoint can be configured to keep traffic between VPC and CW Logs from leaving the Amazon network. It doesn’t require an IGW, NAT, VPN, or Direct Connect. Log data is encrypted while in transit and while it is at rest.
        - Concepts: log event (a record of some activity), log stream (sequence of log events), log groups (group of log streams that share the same retention), Metric Filters (extract metric observations from ingested events and transform them to data points in a CloudWatch metric), Retention Settings (used to specify how long log events are kept in CloudWatch Logs)
        - Use cases: track number of errors for e.g. 404, 500, for even specific literal; monitor particular API activity as captured by CloudTrail by creating alarms in CloudWatch and receive notifications; store the log data in highly durable storage; log retention can be modified so any logs older than this setting are automatically deleted; log information about the DNS queries that Route 53 receives
        - Real-time Processing of Log Data with Subscriptions: subscribe to get access to log events and deliver them to Kinesis or Lambda for custom processing; 
        - Searching and Filtering
    - CloudTrail: get a history of AWS API calls & related events for the AWS account (console, sdk, cli, api) => identify the caller with context; 
      - per AWS account // enabled per region for all the supporting services
      - enables security analysis, resource change tracking, and compliance auditing
      - captures AWS API calls and store in S3; for multi region service encryption based on S3 server side encryption; publish new log files each 5 mn;
      - Option: deliver events to a log group to be monitored by CloudWatch Logs.
      - Option: SNS can be configured to notify the delivery of a new log file.
      - How to activate ? create a trail (a config) to enable logging of the AWS API activity and related events in your account (over console, cli, CT apis)
      - A trail can be applied to all regions (CT creates like sub trails on the other regions to store on a single S3) or a single region. CT support 5 trails per region.
      - Use cases: [track creation, modification or deletion of AWS resources, demonstrate compliance with internal policy and regulatory standards, identify the recent changes or actions to troubleshoot any issues]
      - CloudTrail Processing Library (CPL): helps to read messages delivered to SNS or SQS, downloads and reads the log files, allows custom logic processing;
      - AWS Config vs CloudTrail: AWS Config reports on **WHAT** has changed, whereas CloudTrail reports on **WHO** made the change, **WHEN**, and from **WHICH** location. AWS Config focuses on the configuration of the AWS resources and reports with detailed snapshots on **HOW** the resources have changed, whereas CloudTrail focuses on the events, or API calls, that drive those changes. It focuses on the user, application, and activity performed on the system.
    - CloudFormation: helps to create and manage a collection of related AWS resources, provision and update them; consists of templates and stacks; applying version control to the infrastructure as done for software code; 
      - Supports Chef & Puppet Integration => deploy and configure right down the application layer
      - By default, automatic rollback on error feature is enabled so it can rollback to the  point before the error; 
      - WaitCondition resource feature blocks the creation of other resources until a completion signal is received from an external source;
      - Concepts: templates (blueprints to build, json/ yaml, reusable), stacks (manage resources as single unit, defined based on a template), Change Sets (proposed changes to apply on updates)
    - AWS Config: resource inventory, configuration history, and change notifications to enable governance; detailed view of each configuration; point-in-time and historical state changes; it does not cover all the services 
      - Use cases: Security Analysis & Resource Administration (continuous monitoring and governance, evaluate misconfig for risks); Auditing & Compliance (maintain a complete inventory, retrieve historical configurations); Change Management (understand relationships between resources), Troubleshooting, Discovery; 
      - Concepts: Config Rules (desired configuration or account), Resource Relationship (map of relationships via discovery), Configuration Items (point-in-time view, + include metadata, attributes, relationships, current configuration, and related events), Configuration Snapshot (collection of the configuration), History (configuration items for a given resource over any time period); Stream (an automatically updated list of all configuration items); Recorder (recorder stores the configurations); 
      - How it works: 1/ discovers the supported AWS resources and and generates a configuration item for each resource; 2/ generates configuration items when the configuration of a resource changes and  maintains historical records of the configuration items; 3/ keeps track of all changes to the resources by invoking the Describe or the List API call for each resource;  
      - OpsWorks: configuration management service; helps to configure and operate applications in a cloud enterprise by using Chef & Puppet;
      - Trusted Advisor: inspects the AWS environment to make recommendations for system performance, saving money, availability and closing security gaps.
        - He checks Cost Optimization, Security, Fault Tolerance, Performance, Service Limits;
      - Service Health Dashboard: displays the general status of AWS services.
      - Personal Health Dashboard: provides alerts and remediation guidance when AWS is experiencing events.
  
  - Messaging
  
    - SQS : Simple Queue Service; ha distributed queue system; provides fault tolerant, loosely coupled, flexibility of distributed components of applications to send & receive without requiring each component to be concurrently available
      - Keys: Redundant infrastructure; At-Least-Once delivery; Message Attributes (can contain up to 10 metadata attributes), Message Sample (behavior of retrieving messages as short or long polling), Batching, Order (preserve order); Multiple writers and readers; Variable message size; 
    - SNS: Simple Notification Service
      - service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients
      - Supported Transport Protocols: HTTP/S, Email, Email-JSON, SQS, SMS
      - SNS Supported Endpoints: Email Notifications, Mobile Push Notifications, SQS Queues, SMS Notifications, HTTP/HTTPS Endpoints, Lambda
- Identify resources for technology support
  
  - includes AWS Support Models and the key features and benefits the model provides to the customers: 
    - Basic: 24×7 customer service, documentation, whitepapers, and support forums; 6 core Trusted Advisor checks; Personal Health Dashboard;
    - Developer: Cloud Support Associates via email during biz hours;  Unlimited cases by the primary contact; < 24h for general cases; <12 for system impaired;
    - Biz: 24×7 Support Engineers via email, chat & phone; Personal Health Dashboard & Health API; ull set of Trusted Advisor checks; Unlimited contacts/Unlimited cases; < 4h Production system impaired; 1 < Production system down;
    - Entreprise: 24×7 Sr. Support Engineers via email, chat & phone; Architecture support;  Access to a Well-Architected Review; Operational reviews, recommendations, and reporting; Guidance by Designated Technical Account Manager; Business-critical system down < 15 minutes;

### Domain 4: Billing and Pricing

- Compare and contrast the various pricing models for AWS
  - includes AWS Pricing: Pay as you go; Pay less when you reserve (eg EC2); Pay even less by using more (storage); Pay even less as AWS grows; Free services (VPC, Elastic Beanstalk, CloudFormation, IAM, Auto Scaling, OpsWorks, Consolidated Billing); Free tier;
  - AWS Pricing Resources:
    - Simple Monthly Calculator tool to effectively estimate the costs
    - Economic Center provides access to tools, & resources to compare the costs of services
    - Account Activity to view current charges & activity by service and by usage type.
    - Usage Reports provides usage reports, specifying usage types, timeframe, service operations, and more can customize reports.
  - AWS Pricing Fundamental Characteristics
    - AWS basically charges for Compute, Storage & Data Transfer Out – aggregated across EC2, S3, RDS, SimpleDB, SQS, SNS, and VPC and then charged at the outbound data transfer rate
    - AWS does not charge Inbound data transfer across all AWS Services in all regions
      and Outbound data transfer charges between AWS Services within the same region.
  - EC2, cost depends on
    - Clock Hours of Server Time (for the time they are running), Config (physical capacity)
    - Machine Purchase Type (On demand, RI Dedicated, Spot), Auto Scaling & Number of Instances
    - Load Balancing (Number of hours the ELB runs and the amount of data it processes)
    - CloudWatch Detailed Monitoring (Basic monitoring is free, Detailed monitoring)
    - Elastic IP Addresses, Operating Systems and Software Packages
  - S3, cost depends on
    - Storage Class (class has different rates), Storage (Number and size of objects , type of storage), Requests (Number and type of requests)
  - EBS
    - Volumes (GP, Provsioned IOPS, Magnetic), Data Transfer Out, Snapshot
    - IOPS (in GP IO is included, in Magnetic IO is charged by request, in Provisioned is charged by amount)
  - RDS
    - Clock Hours of Server Time, Database Characteristics
    - Database Purchase Type (RI, oD), Number of Database Instances
    - Additional Storage, Requests (# IO), Deployment Type (1/+AZ)
  - CloudFront
    - Traffic Distribution, Requests (http/https), Data Transfer Out
- Recognize the various account structures in relation to AWS billing and pricing
  - includes AWS Organizations: account management service, centrally manage associated accounts, policies across multiple accounts, consolidate billing, hierarchical grouping of accounts
    - Control over AWS services and API actions that each account can access: overrule account permissions! An Organizations block is more powerful than an admin action!
    - Support for AWS IAM: User can access only what is allowed by both the AWS Organizations policies and IAM policies.
    - Concepts: Org [An entity created to consolidate AWS accounts, has one master account], Root [Parent container for all the accounts for the organization.], OU [A container for accounts within a root.], Account [A standard AWS account that contains AWS resources.], Invitation [Process of asking another account to join an organization.], Handshake [A multi-step process of exchanging information between two parties], 
    - Service control policy (SCP)
      - Service control policy specifies the services and actions that users and roles can use in the accounts that the SCP affects.
      - SCPs are similar to IAM permission policies except that they don’t grant any permissions.
    - Whitelisting vs. blacklisting
      - Explicitly specify the access that is allowed. All other access is implicitly blocked.
      - Default behavior of AWS Organizations. Explicitly specify the access that is not allowed.
    
  - Consolidated Billing: an accounting and billing feature, combined view of charges, 
  
    - payments from multiple AWS Linked Accounts within the organization to a single Payer Account. Payer account cannot access data belonging to the linked accounts. Linked accounts can access Payer account through Cross Account Access roles.
  
    - Process: Payer account add an account to be part of the consolidated bill, The account need to accept.
  
    - Scenarios:
  
      - have multiple accounts and projects to track
      - have multiple cost centers to track.
      - have acquired a project or company
  
    - (+): 1 bill, easy tracking, combined Usage & Volume Discounts, 1 free tier;
  
    - Volume Pricing Discounts: all the accounts on the bill are like 1 single account, so AWS combines the usage as it is a single user to give a lower pricing.
  
      Bob used 8 Tb, Susan used 4 Tb, $0.17/Gb for the first 10 Tb, $0.13/Gb for the next 40 Tb;
      Actual Individual Bill – AWS would have charged Bob and Susan each $174.08 per TB for their usage, for a total of $2088.96;
      Volume Discount Bill – Combined 12 Tb total that Bob and Susan used, would cost the paying account ($174.08 * 10 Tb) + ($133.12 * 2 Tb) = $1740.80 + $266.24 = $2007.04;
  
    - EC2 Reserved Instances
  
      - Linked accounts can receive the hourly cost benefit of EC2 RI purchased by any other account;
      - Linked accounts receive the cost benefit from other’s RI only if instances are launched in the same AZ where the RI were purchased
      - Capacity reservation only applies to the product platform, instance type, and AZ specified in the purchase
      - For e.g., Bob and Susan each have an account on Bob’s consolidated bill. Susan has 5 RI of the same type, and Bob has none. During one particular hour, Susan uses 3 instances and Bob uses 6, for a total of 9 instances used on Bob’s consolidated bill. AWS will bill 5 as RI, and the remaining 4 as normal instances.
  
  - Billing and Cost Management: service to pay AWS bill, monitor usage, and budget costs
  
    - Analyzing costs with Cost Explorer: allows filter graphs by API operations, AZ, service, custom cost allocation tags, EC2 instance type, purchase options, region, usage type, usage type groups, or, if Consolidated Billing used, by linked account.
    - Budget: track costs; to see usage-to-date; estimation; Use Cost explorer to provide forecasts of your estimated costs; create CloudWatch alarms that notify (over your budgeted amounts, estimated costs exceed budgets) (SNS, or email)
    - Cost Allocation Tags: 
      - Tags used to organize AWS resources (to activate for usage in cost explorer)
      - Cost allocation tags used to track the AWS costs on a detailed level.
      - two types of cost allocation tags (AWS-generated tag, user-defined tags)
    - Alerts on Cost Limits based on CW to send email
- Identify resources available for billing support
  - includes tools like TCO Calculator which helps compare cost of running applications in an on-premises or colocation environment to AWS; 
    - estimate the cost savings when using AWS instead of other Providers
  - includes Cost Explorer allows you to view and analyze costs
    - Cost Explorer is a tool that enables you to view and analyze your costs and usage.











