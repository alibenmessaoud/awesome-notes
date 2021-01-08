QA



Shared Security Responsibility Model

1. In the shared security model, AWS is responsible for which of the following security best practices (check all that apply) :
   1. **Penetration testing**
   2. Operating system account security management (User responsibility)
   3. **Threat modeling**
   4. User group access management (User responsibility)
   5. **Static code analysis** (AWS development cycle responsibility)
2. You are running a web-application on AWS consisting of the following components an Elastic Load Balancer (ELB) an Auto-Scaling Group of EC2 instances running Linux/PHP/Apache, and Relational DataBase Service (RDS) MySQL. Which security measures fall into AWS’s responsibility?
   1. Protect the EC2 instances against unsolicited access by enforcing the principle of least-privilege access (User responsibility)
   2. **Protect against IP spoofing or packet sniffing**
   3. Assure all communication between EC2 instances and ELB is encrypted (User responsibility)
   4. Install latest security patches on ELB. RDS and EC2 instances (User responsibility)
3. In AWS, which security aspects are the customer’s responsibility? Choose 4 answers
   1. Controlling physical access to compute resources (AWS responsibility)
   2. **Patch management on the EC2 instances operating system**
   3. **Encryption of EBS (Elastic Block Storage) volumes**
   4. **Life-cycle management of IAM credentials**
   5. Decommissioning storage devices (AWS responsibility)
   6. **Security Group and ACL (Access Control List) settings**
4. Per the AWS Acceptable Use Policy, penetration testing of EC2 instances:
   1. May be performed by AWS, and will be performed by AWS upon customer request.
   2. May be performed by AWS, and is periodically performed by AWS.
   3. Are expressly prohibited under all circumstances.
   4. **May be performed by the customer on their own instances with prior authorization from AWS.**
   5. May be performed by the customer on their own instances, only if performed from EC2 instances
5. Which is an operational process performed by AWS for data security?
   1. AES-256 encryption of data stored on any shared storage device (User responsibility)
   2. **Decommissioning of storage devices using industry-standard practices**
   3. Background virus scans of EBS volumes and EBS snapshots (No virus scan is performed by AWS on User instances)
   4. Replication of data across multiple AWS Regions (AWS does not replicate data across regions unless done by User)
   5. Secure wiping of EBS data when an EBS volume is unmounted (data is not wiped off on EBS volume when unmounted and it can be remounted on other EC2 instance)



----



IAM Access Management



1. IAM’s Policy Evaluation Logic always starts with a default ____________ for every request, except for those that use the AWS account’s root security credentials b

   1. Permit
   2. **Deny**
   3. Cancel

2. An organization has created 10 IAM users. The organization wants each of the IAM users to have access to a separate DynamoDB table. All the users are added to the same group and the organization wants to setup a group level policy for this. How can the organization achieve this?

   1. Define the group policy and add a condition which allows the access based on the IAM name
   2. **Create a DynamoDB table with the same name as the IAM user name and define the policy rule which grants access based on the DynamoDB ARN using a variable**
   3. Create a separate DynamoDB database for each user and configure a policy in the group based on the DB variable
   4. It is not possible to have a group level policy which allows different IAM users to different DynamoDB Tables

3. An organization has setup multiple IAM users. The organization wants that each IAM user accesses the IAM console only within the organization and not from outside. How can it achieve this?

   1. Create an IAM policy with the security group and use that security group for AWS console login
   2. **Create an IAM policy with a condition which denies access when the IP address range is not from the organization**
   3. Configure the EC2 instance security group which allows traffic only from the organization’s IP range
   4. Create an IAM policy with VPC and allow a secure gateway between the organization and AWS Console

4. Can I attach more than one policy to a particular entity?

   1. **Yes always**
   2. Only if within GovCloud
   3. No
   4. Only if within VPC

5. A __________ is a document that provides a formal statement of one or more permissions.

   1. **policy**
   2. permission
   3. Role
   4. resource

6. A __________ is the concept of allowing (or disallowing) an entity such as a user, group, or role some type of access to one or more resources.

   1. user
   2. AWS Account
   3. resource
   4. **permission**

7. True or False: When using IAM to control access to your RDS resources, the key names that can be used are case sensitive. For example, aws:CurrentTime is NOT equivalent to AWS:currenttime.

   1. TRUE
   2. **FALSE** (Refer [link](http://docs.aws.amazon.com/directconnect/latest/UserGuide/using_iam.html#keys))

8. A user has set an IAM policy where it allows all requests if a request from IP 10.10.10.1/32. Another policy allows all the requests between 5 PM to 7 PM. What will happen when a user is requesting access from IP 10.10.10.1/32 at 6 PM?

   1. IAM will throw an error for policy conflict
   2. It is not possible to set a policy based on the time or IP
   3. It will deny access
   4. **It will allow access**

9. Which of the following are correct statements with policy evaluation logic in AWS Identity and Access Management? Choose 2 answers.

   1. **By default, all requests are denied**
   2. An explicit allow overrides an explicit deny
   3. **An explicit allow overrides default deny**
   4. An explicit deny does not override an explicit allow
   5. By default, all request are allowed

10. A web design company currently runs several FTP servers that their 250 customers use to upload and download large graphic files. They wish to move this system to AWS to make it more scalable, but they wish to maintain customer privacy and keep costs to a minimum. What AWS architecture would you recommend?

     

    **[PROFESSIONAL]**

    1. **Ask their customers to use an S3 client instead of an FTP client. Create a single S3 bucket. Create an IAM user for each customer. Put the IAM Users in a Group that has an IAM policy that permits access to subdirectories within the bucket via use of the ‘username’ Policy variable.**
    2. Create a single S3 bucket with Reduced Redundancy Storage turned on and ask their customers to use an S3 client instead of an FTP client. Create a bucket for each customer with a Bucket Policy that permits access only to that one customer. (Creating bucket for each user is not a scalable model, also 100 buckets are a limit earlier without extending which has since changed [link](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/))
    3. Create an auto-scaling group of FTP servers with a scaling policy to automatically scale-in when minimum network traffic on the auto-scaling group is below a given threshold. Load a central list of ftp users from S3 as part of the user Data startup script on each Instance (Expensive)
    4. Create a single S3 bucket with Requester Pays turned on and ask their customers to use an S3 client instead of an FTP client. Create a bucket tor each customer with a Bucket Policy that permits access only to that one customer. (Creating bucket for each user is not a scalable model, also 100 buckets are a limit earlier without extending which has since changed [link](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/))



---



CloudTrail

1. You currently operate a web application in the AWS US-East region. The application runs on an auto-scaled layer of EC2 instances and an RDS Multi-AZ database. Your IT security compliance officer has tasked you to develop a reliable and durable logging solution to track changes made to your EC2, IAM and RDS resources. The solution must ensure the integrity and confidentiality of your log data. Which of these solutions would you recommend?
   1. **Create a new CloudTrail trail with one new S3 bucket to store the logs and with the global services option selected. Use IAM roles, S3 bucket policies and Multi Factor Authentication (MFA) Delete on the S3 bucket that stores your logs. (**Single New bucket with global services option for IAM and MFA delete for confidentiality**)
      **
   2. Create a new CloudTrail with one new S3 bucket to store the logs. Configure SNS to send log file delivery notifications to your management system. Use IAM roles and S3 bucket policies on the S3 bucket that stores your logs. (Missing Global Services for IAM)
   3. Create a new CloudTrail trail with an existing S3 bucket to store the logs and with the global services option selected Use S3 ACLs and Multi Factor Authentication (MFA) Delete on the S3 bucket that stores your logs. (Existing bucket prevents confidentiality)
   4. Create three new CloudTrail trails with three new S3 buckets to store the logs one for the AWS Management console, one for AWS SDKs and one for command line tools. Use IAM roles and S3 bucket policies on the S3 buckets that store your logs (3 buckets not needed, Missing Global services options)
2. Which of the following are true regarding AWS CloudTrail? Choose 3 answers
   1. **CloudTrail is enabled globally** (it can be enabled for all regions and also per region basis)
   2. CloudTrail is enabled by default (**was not enabled by default, however, it is enabled by default as per the latest [AWS enhancements](https://aws.amazon.com/blogs/aws/new-amazon-web-services-extends-cloudtrail-to-all-aws-customers/)**)
   3. **CloudTrail is enabled on a per-region basis** (it can be enabled for all regions and also per region basis)
   4. CloudTrail is enabled on a per-service basis (once enabled it is applicable for all the supported services, service can’t be selected)
   5. **Logs can be delivered to a single Amazon S3 bucket for aggregation**
   6. CloudTrail is enabled for all available services within a region. (is enabled only for CloudTrail supported services)
   7. Logs can only be processed and delivered to the region in which they are generated. (can be logged to bucket in any region)
3. An organization has configured the custom metric upload with CloudWatch. The organization has given permission to its employees to upload data using CLI as well SDK. How can the user track the calls made to CloudWatch?
   1. The user can enable logging with CloudWatch which logs all the activities
   2. **Use CloudTrail to monitor the API calls**
   3. Create an IAM user and allow each user to log the data using the S3 bucket
   4. Enable detailed monitoring with CloudWatch
4. A user is trying to understand the CloudWatch metrics for the AWS services. It is required that the user should first understand the namespace for the AWS services. Which of the below mentioned is not a valid namespace for the AWS services?
   1. AWS/StorageGateway
   2. **AWS/CloudTrail (**[CloudWatch supported namespaces](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/aws-namespaces.html)**)
      **
   3. AWS/ElastiCache
   4. AWS/SWF
5. Your CTO thinks your AWS account was hacked. What is the only way to know for certain if there was unauthorized access and what they did, assuming your hackers are very sophisticated AWS engineers and doing everything they can to cover their tracks?
   1. **Use CloudTrail Log File Integrity Validation**. (Refer [link](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-validation-intro.html))
   2. Use AWS Config SNS Subscriptions and process events in real time.
   3. Use CloudTrail backed up to AWS S3 and Glacier.
   4. Use AWS Config Timeline forensics.
6. Your CTO has asked you to make sure that you know what all users of your AWS account are doing to change resources at all times. She wants a report of who is doing what over time, reported to her once per week, for as broad a resource type group as possible. How should you do this?
   1. **Create a global AWS CloudTrail Trail. Configure a script to aggregate the log data delivered to S3 once per week and deliver this to the CTO.**
   2. Use CloudWatch Events Rules with an SNS topic subscribed to all AWS API calls. Subscribe the CTO to an email type delivery on this SNS Topic.
   3. Use AWS IAM credential reports to deliver a CSV of all uses of IAM User Tokens over time to the CTO.
   4. Use AWS Config with an SNS subscription on a Lambda, and insert these changes over time into a DynamoDB table. Generate reports based on the contents of this table.



---



AWS Global Infra



1. You would like to create a mirror image of your production environment in another region for disaster recovery purposes. Which of the following AWS resources do not need to be recreated in the second region? (Choose 2 answers)
   1. **Route 53 Record Sets**
   2. **IAM Roles**
   3. Elastic IP Addresses (EIP) (are specific to a region)
   4. EC2 Key Pairs (are specific to a region)
   5. Launch configurations
   6. Security Groups (are specific to a region)
2. When using the following AWS services, which should be implemented in multiple Availability Zones for high availability solutions? Choose 2 answers
   1. Amazon DynamoDB (already replicates across AZs)
   2. **Amazon Elastic Compute Cloud (EC2)**
   3. **Amazon Elastic Load Balancing**
   4. Amazon Simple Notification Service (SNS) (Global Managed Service)
   5. Amazon Simple Storage Service (S3) (Global Managed Service)
3. What is the scope of an EBS volume?
   1. VPC
   2. Region
   3. Placement Group
   4. **Availability Zone**
4. What is the scope of AWS IAM?
   1. **Global** (IAM resources are all global; there is not regional constraint)
   2. Availability Zone
   3. Region
   4. Placement Group
5. What is the scope of an EC2 EIP?
   1. Placement Group
   2. Availability Zone
   3. **Region** (An Elastic IP address is tied to a region and can be associated only with an instance in the same region. Refer [link](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/resources.html))
   4. VPC
6. What is the scope of an EC2 security group?
   1. Availability Zone
   2. Placement Group
   3. **Region** (A security group is tied to a region and can be assigned only to instances in the same region)
   4. VPC



---



Overview 



1. Which AWS services belong to the Compute services? Choose 2 answers
   1. **Lambda**
   2. **EC2**
   3. S3
   4. EMR
   5. CloudFront
2. Which AWS service provides low cost storage option for archival and long-term backup?
   1. **Glacier**
   2. S3
   3. EBS
   4. CloudFront
3. Which AWS services belong to the Storage services? Choose 2 answers
   1. **EFS**
   2. IAM
   3. EMR
   4. **S3**
   5. CloudFront
4. A Company allows users to upload videos on its platform. They want to convert the videos to multiple formats supported on multiple devices and platforms. Which AWS service can they leverage for the requirement?
   1. AWS SWF
   2. AWS Video Converter
   3. **AWS Elastic Transcoder**
   4. AWS Data Pipeline
5. Which analytic service helps analyze data in S3 using standard SQL?
   1. **Athena**
   2. EMR
   3. Elasticsearch
   4. Kinesis
6. What features does AWS’s Route 53 service provide? Choose the 2 correct answers:
   1. Content Caching
   2. **Domain Name System (DNS) service**
   3. Database Management
   4. **Domain Registration**
7. You are trying to organize and import (to AWS) gigabytes of data that are currently structured in JSON-like, name-value documents. What AWS service would best fit your needs?
   1. Lambda
   2. **DynamoDB**
   3. RDS
   4. Aurora
8. What AWS database is primarily used to analyze data using standard SQL formatting with compatibility for your existing business intelligence tools? Choose the correct answer:
   1. **Redshift**
   2. RDS
   3. DynamoDB
   4. ElastiCache
9. A company wants their application to use pre-configured machine image with software installed and configured. which AWS feature can help for the same?
   1. **Amazon Machine Image**
   2. AWS CloudFormation
   3. AWS Lambda
   4. AWS Lightsail
10. What AWS service can be used for track API event calls for security analysis, resource change tracking?
    1. AWS CloudWatch
    2. AWS CloudFormation
    3. **AWS CloudTrail**
    4. AWS OpsWorks
11. Which AWS service can help Offload the read traffic from your database in order to reduce latency caused by read-heavy workload?
    1. **ElastiCache**
    2. DynamoDB
    3. S3
    4. EFS
12. What service allows system administrators to run “Infrastructure as code”?
    1. **CloudFormation**
    2. CloudWatch
    3. CloudTrail
    4. CodeDeploy





---



EC2



1. What are the Amazon EC2 API tools?
   1. They don’t exist. The Amazon EC2 AMI tools, instead, are used to manage permissions.
   2. **Command-line tools to the Amazon EC2 web service**
   3. They are a set of graphical tools to manage EC2 instances.
   4. They don’t exist. The Amazon API tools are a client interface to Amazon Web Services.
2. When a user is launching an instance with EC2, which of the below mentioned options is not available during the instance launch console for a key pair?
   1. Proceed without the key pair
   2. **Upload a new key pair**
   3. Select an existing key pair
   4. Create a new key pair





---



ECS



1. You need a solution to distribute traffic evenly across all of the containers for a task running on Amazon ECS. Your task definitions define dynamic host port mapping for your containers. What AWS feature provides this functionally?
   1. **Application Load Balancers support dynamic host port mapping.**
   2. CloudFront custom origins support dynamic host port mapping.
   3. All Elastic Load Balancing instances support dynamic host port mapping.
   4. Classic Load Balancers support dynamic host port mapping.
2. Your security team requires each Amazon ECS task to have an IAM policy that limits the task’s privileges to only those required for its use of AWS services. How can you achieve this?
   1. **Use IAM roles for Amazon ECS tasks to associate a specific IAM role with each ECS task definition**
   2. Use IAM roles on the Amazon ECS container instances to associate IAM role with each ECS task on that instance
   3. Connect to each running amazon ECS container instance and add discrete credentials
   4. Reboot each Amazon ECS task programmatically to generate new instance metadata for each task



---



Elastic Beanstalk



1. An organization is planning to use AWS for their production roll out. The organization wants to implement automation for deployment such that it will automatically create a LAMP stack, download the latest PHP installable from S3 and setup the ELB. Which of the below mentioned AWS services meets the requirement for making an orderly deployment of the software?

   1. **AWS Elastic Beanstalk**
   2. AWS CloudFront
   3. AWS CloudFormation
   4. AWS DevOps

2. What does Amazon Elastic Beanstalk provide?

   1. A scalable storage appliance on top of Amazon Web Services.
   2. **An application container on top of Amazon Web Services**
   3. A service by this name doesn’t exist.
   4. A scalable cluster of EC2 instances

3. You want to have multiple versions of your application running at the same time, with all versions launched via AWS Elastic Beanstalk. Is this possible?

   1. However if you have 2 AWS accounts this can be done
   2. AWS Elastic Beanstalk is not designed to support multiple running environments
   3. **AWS Elastic Beanstalk is designed to support a number of multiple running environments**
   4. However AWS Elastic Beanstalk is designed to support only 2 multiple running environments

4. A .NET application that you manage is running in Elastic Beanstalk. Your developers tell you they will need access to application log files to debug issues that arise. The infrastructure will scale up and down. How can you ensure the developers will be able to access only the log files?

   1. Access the log files directly from Elastic Beanstalk
   2. **Enable log file rotation to S3 within the Elastic Beanstalk configuration**
   3. Ask your developers to enable log file rotation in the applications web.config file
   4. Connect to each Instance launched by Elastic Beanstalk and create a Windows Scheduled task to rotate the log files to S3

5. Your team has a tomcat-based Java application you need to deploy into development, test and production environments. After some research, you opt to use Elastic Beanstalk due to its tight integration with your developer tools and RDS due to its ease of management. Your QA team lead points out that you need to roll a sanitized set of production data into your environment on a nightly basis. Similarly, other software teams in your org want access to that same restored data via their EC2 instances in your VPC .The optimal setup for persistence and security that meets the above requirements would be the following. 

   [PROFESSIONAL]

   1. Create your RDS instance as part of your Elastic Beanstalk definition and alter its security group to allow access to it from hosts in your application subnets. (Not optimal for persistence as the RDS is associated with the Elastic Beanstalk lifecycle and would not live independently)
   2. Create your RDS instance separately and add its IP address to your application’s DB connection strings in your code. Alter its security group to allow access to it from hosts within your VPC’s IP address block. (RDS is connected using DNS endpoint only)
   3. **Create your RDS instance separately and pass its DNS name to your app’s DB connection string as an environment variable. Create a security group for client machines and add it as a valid source for DB traffic to the security group of the RDS instance itself.** (Security group allows instances to access the RDS with new instances launched without any changes)
   4. Create your RDS instance separately and pass its DNS name to your DB connection string as an environment variable. Alter its security group to allow access to it from hosts in your application subnets. (Not optimal for security adding individual hosts)

6. Your must architect the migration of a web application to AWS. The application consists of Linux web servers running a custom web server. You are required to save the logs generated from the application to a durable location. What options could you select to migrate the application to AWS? (Choose 2) 

   [PROFESSIONAL]

   1. Create an AWS Elastic Beanstalk application using the custom web server platform. Specify the web server executable and the application project and source files. Enable log file rotation to Amazon Simple Storage Service (S3). (EB does not work with Custom server executable)
   2. Create Dockerfile for the application. Create an AWS OpsWorks stack consisting of a custom layer. Create custom recipes to install Docker and to deploy your Docker container using the Dockerfile. Create custom recipes to install and configure the application to publish the logs to Amazon CloudWatch Logs (although this is one of the option, the last sentence mentions configure the application to push the logs to S3, which would need changes to application as it needs to use SDK or CLI)
   3. Create Dockerfile for the application. Create an AWS OpsWorks stack consisting of a Docker layer that uses the Dockerfile. Create custom recipes to install and configure Amazon Kinesis to publish the logs into Amazon CloudWatch. (Kinesis not needed)
   4. **Create a Dockerfile for the application. Create an AWS Elastic Beanstalk application using the Docker platform and the Dockerfile. Enable logging the Docker configuration to automatically publish the application logs. Enable log file rotation to Amazon S3.** (Use Docker configuration with awslogs and EB with Docker)
   5. **Use VM import/Export to import a virtual machine image of the server into AWS as an AMI. Create an Amazon Elastic Compute Cloud (EC2) instance from AMI, and install and configure the Amazon CloudWatch Logs agent. Create a new AMI from the instance. Create an AWS Elastic Beanstalk application using the AMI platform and the new AMI.** (Use VM Import/Export to create AMI and CloudWatch logs agent to log)

7. Which of the following groups is AWS Elastic Beanstalk best suited for?

   1. **Those who want to deploy and manage their applications within minutes in the AWS cloud.**
   2. Those who want to privately store and manage Git repositories in the AWS cloud.
   3. Those who want to automate the deployment of applications to instances and to update the applications as required.
   4. Those who want to model, visualize, and automate the steps required to release software.

8. When thinking of AWS Elastic Beanstalk’s model, which is true?

   1. Applications have many deployments, deployments have many environments.
   2. Environments have many applications, applications have many deployments.
   3. **Applications have many environments, environments have many deployments.** (Applications group logical services. Environments belong to Applications, and typically represent different deployment levels (dev, stage, prod, forth). Deployments belong to environments, and are pushes of bundles of code for the environments to run.)
   4. Deployments have many environments, environments have many applications.

9. If you’re trying to configure an AWS Elastic Beanstalk worker tier for easy debugging if there are problems finishing queue jobs, what should you configure?

   1. Configure Rolling Deployments.
   2. Configure Enhanced Health Reporting
   3. Configure Blue-Green Deployments.
   4. **Configure a Dead Letter Queue** (Elastic Beanstalk worker environments support SQS dead letter queues, where worker can send messages that for some reason could not be successfully processed. Dead letter queue provides the ability to sideline, isolate and analyze the unsuccessfully processed messages. Refer [link](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features-managing-env-tiers.html#worker-deadletter))

10. When thinking of AWS Elastic Beanstalk, which statement is true?

    1. Worker tiers pull jobs from SNS.
    2. Worker tiers pull jobs from HTTP.
    3. Worker tiers pull jobs from JSON.
    4. **Worker tiers pull jobs from SQS.** (Elastic Beanstalk installs a daemon on each EC2 instance in the Auto Scaling group to process SQS messages in the worker environment. Refer [link](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features-managing-env-tiers.html))

11. You are building a Ruby on Rails application for internal, non-production use, which uses MySQL as a database. You want developers without very much AWS experience to be able to deploy new code with a single command line push. You also want to set this up as simply as possible. Which tool is ideal for this setup?

    1. AWS CloudFormation
    2. AWS OpsWorks
    3. AWS ELB + EC2 with CLI Push
    4. **AWS Elastic Beanstalk**

12. What AWS products and features can be deployed by Elastic Beanstalk? Choose 3 answers.

    1. **Auto scaling groups**
    2. Route 53 hosted zones
    3. **Elastic Load Balancers**
    4. **RDS Instances**
    5. Elastic IP addresses
    6. SQS Queues

13. AWS Elastic Beanstalk stores your application files and optionally server log files in ____.

    1. Amazon Storage Gateway
    2. Amazon Glacier
    3. Amazon EC2
    4. **Amazon S3**

14. When you use the AWS Elastic Beanstalk console to deploy a new application ____.

    1. Need to upload each file separately
    2. Need to create each file and path
    3. **Need to upload a source bundle**
    4. Need to create each file



----



Lambda



Your serverless architecture using AWS API Gateway, AWS Lambda, and AWS DynamoDB experienced a large increase in traffic to a sustained 400 requests per second, and dramatically increased in failure rates. Your requests, during normal operation, last 500 milliseconds on average. Your DynamoDB table did not exceed 50% of provisioned throughput, and Table primary keys are designed correctly. What is the most likely issue?

1. Your API Gateway deployment is throttling your requests.
2. Your AWS API Gateway Deployment is bottlenecking on request (de)serialization.
3. **You did not request a limit increase on concurrent Lambda function executions.** (Refer [link](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_lambda) – AWS API Gateway by default throttles at 500 requests per second steady-state, and 1000 requests per second at spike. Lambda, by default, throttles at 100 concurrent requests for safety. At 500 milliseconds (half of a second) per request, you can expect to support 200 requests per second at 100 concurrency. This is less than the 400 requests per second your system now requires. Make a limit increase request via the AWS Support Console.)
4. You used Consistent Read requests on DynamoDB and are experiencing semaphore lock.



----



Auto scaling 



1. A user is trying to setup a scheduled scaling activity using Auto Scaling. The user wants to setup the recurring schedule. Which of the below mentioned parameters is not required in this case?

   1. **Maximum size**
   2. Auto Scaling group name
   3. End time
   4. Recurrence value

2. A user has configured Auto Scaling with 3 instances. The user had created a new AMI after updating one of the instances. If the user wants to terminate two specific instances to ensure that Auto Scaling launches an instances with the new launch configuration, which command should he run?

   1. as-delete-instance-in-auto-scaling-group <Instance ID> –no-decrement-desired-capacity
   2. as-terminate-instance-in-auto-scaling-group <Instance ID> –update-desired-capacity
   3. as-terminate-instance-in-auto-scaling-group <Instance ID> –decrement-desired-capacity
   4. **as-terminate-instance-in-auto-scaling-group  –no-decrement-desired-capacity**

3. A user is planning to scale up an application by 8 AM and scale down by 7 PM daily using Auto Scaling. What should the user do in this case?

   1. Setup the scaling policy to scale up and down based on the CloudWatch alarms
   2. User should increase the desired capacity at 8 AM and decrease it by 7 PM manually
   3. User should setup a batch process which launches the EC2 instance at a specific time
   4. **Setup scheduled actions to scale up or down at a specific time**

4. An organization has setup Auto Scaling with ELB. Due to some manual error, one of the instances got rebooted. Thus, it failed the Auto Scaling health check. Auto Scaling has marked it for replacement. How can the system admin ensure that the instance does not get terminated?

   1. Update the Auto Scaling group to ignore the instance reboot event
   2. It is not possible to change the status once it is marked for replacement
   3. Manually add that instance to the Auto Scaling group after reboot to avoid replacement
   4. **Change the health of the instance to healthy using the Auto Scaling commands**

5. A user has configured Auto Scaling with the minimum capacity as 2 and the desired capacity as 2. The user is trying to terminate one of the existing instance with the command: as-terminate-instance-in-auto-scaling-group<Instance ID> –decrement-desired-capacity. What will Auto Scaling do in this scenario?

   1. Terminates the instance and does not launch a new instance
   2. Terminates the instance and updates the desired capacity to 1
   3. Terminates the instance and updates the desired capacity & minimum size to 1
   4. **Throws an error**

6. An organization has configured Auto Scaling for hosting their application. The system admin wants to understand the Auto Scaling health check process. If the instance is unhealthy, Auto Scaling launches an instance and terminates the unhealthy instance. What is the order execution?

   1. Auto Scaling launches a new instance first and then terminates the unhealthy instance
   2. Auto Scaling performs the launch and terminate processes in a random order
   3. Auto Scaling launches and terminates the instances simultaneously
   4. **Auto Scaling terminates the instance first and then launches a new instance**

7. A user has configured ELB with Auto Scaling. The user suspended the Auto Scaling terminate process only for a while. What will happen to the availability zone rebalancing process (AZRebalance) during this period?

   1. Auto Scaling will not launch or terminate any instances
   2. **Auto Scaling will allow the instances to grow more than the maximum size**
   3. Auto Scaling will keep launching instances till the maximum instance size
   4. It is not possible to suspend the terminate process while keeping the launch active

8. An organization has configured Auto Scaling with ELB. There is a memory issue in the application which is causing CPU utilization to go above 90%. The higher CPU usage triggers an event for Auto Scaling as per the scaling policy. If the user wants to find the root cause inside the application without triggering a scaling activity, how can he achieve this?

   1. Stop the scaling process until research is completed
   2. It is not possible to find the root cause from that instance without triggering scaling
   3. Delete Auto Scaling until research is completed
   4. **Suspend the scaling process until research is completed**

9. A user has configured ELB with Auto Scaling. The user suspended the Auto Scaling Alarm Notification (which notifies Auto Scaling for CloudWatch alarms) process for a while. What will Auto Scaling do during this period?

   1. AWS will not receive the alarms from CloudWatch
   2. **AWS will receive the alarms but will not execute the Auto Scaling policy**
   3. Auto Scaling will execute the policy but it will not launch the instances until the process is resumed
   4. It is not possible to suspend the AlarmNotification process

10. An organization has configured two single availability zones. The Auto Scaling groups are configured in separate zones. The user wants to merge the groups such that one group spans across multiple zones. How can the user configure this?

    1. Run the command as-join-auto-scaling-group to join the two groups
    2. **Run the command as-update-auto-scaling-group to configure one group to span across zones and delete the other group**
    3. Run the command as-copy-auto-scaling-group to join the two groups
    4. Run the command as-merge-auto-scaling-group to merge the groups

11. An organization has configured Auto Scaling with ELB. One of the instance health check returns the status as Impaired to Auto Scaling. What will Auto Scaling do in this scenario?

    1. Perform a health check until cool down before declaring that the instance has failed
    2. **Terminate the instance and launch a new instance**
    3. Notify the user using SNS for the failed state
    4. Notify ELB to stop sending traffic to the impaired instance

12. A user has setup an Auto Scaling group. The group has failed to launch a single instance for more than 24 hours. What will happen to Auto Scaling in this condition

    1. Auto Scaling will keep trying to launch the instance for 72 hours
    2. **Auto Scaling will suspend the scaling process**
    3. Auto Scaling will start an instance in a separate region
    4. The Auto Scaling group will be terminated automatically

13. A user is planning to setup infrastructure on AWS for the Christmas sales. The user is planning to use Auto Scaling based on the schedule for proactive scaling. What advise would you give to the user?

    1. It is good to schedule now because if the user forgets later on it will not scale up
    2. The scaling should be setup only one week before Christmas
    3. **Wait till end of November before scheduling the activity**
    4. It is not advisable to use scheduled based scaling

14. A user is trying to setup a recurring Auto Scaling process. The user has setup one process to scale up every day at 8 am and scale down at 7 PM. The user is trying to setup another recurring process which scales up on the 1st of every month at 8 AM and scales down the same day at 7 PM. What will Auto Scaling do in this scenario

    1. Auto Scaling will execute both processes but will add just one instance on the 1st
    2. Auto Scaling will add two instances on the 1st of the month
    3. Auto Scaling will schedule both the processes but execute only one process randomly
    4. **Auto Scaling will throw an error since there is a conflict in the schedule of two separate Auto Scaling Processes**

15. A sys admin is trying to understand the Auto Scaling activities. Which of the below mentioned processes is not performed by Auto Scaling?

    1. **Reboot Instance**
    2. Schedule Actions
    3. Replace Unhealthy
    4. Availability Zone Re-Balancing

16. You have started a new job and are reviewing your company’s infrastructure on AWS. You notice one web application where they have an Elastic Load Balancer in front of web instances in an Auto Scaling Group. When you check the metrics for the ELB in CloudWatch you see four healthy instances in Availability Zone (AZ) A and zero in AZ B. There are zero unhealthy instances. What do you need to fix to balance the instances across AZs?

    1. Set the ELB to only be attached to another AZ
    2. **Make sure Auto Scaling is configured to launch in both AZs**
    3. Make sure your AMI is available in both AZs
    4. Make sure the maximum size of the Auto Scaling Group is greater than 4

17. You have been asked to leverage Amazon VPC EC2 and SQS to implement an application that submits and receives millions of messages per second to a message queue. You want to ensure your application has sufficient bandwidth between your EC2 instances and SQS. Which option will provide the most scalable solution for communicating between the application and SQS?

    1. Ensure the application instances are properly configured with an Elastic Load Balancer
    2. Ensure the application instances are launched in private subnets with the EBS-optimized option enabled
    3. Ensure the application instances are launched in public subnets with the associate-public-IP-address=trueoption enabled
    4. **Launch application instances in private subnets with an Auto Scaling group and Auto Scaling triggers configured to watch the SQS queue size**

18. You have decided to change the Instance type for instances running in your application tier that are using Auto Scaling. In which area below would you change the instance type definition?

    1. **Auto Scaling launch configuration**
    2. Auto Scaling group
    3. Auto Scaling policy
    4. Auto Scaling tags

19. A user is trying to delete an Auto Scaling group from CLI. Which of the below mentioned steps are to be performed by the user?

    1. Terminate the instances with the ec2-terminate-instance command
    2. Terminate the Auto Scaling instances with the as-terminate-instance command
    3. **Set the minimum size and desired capacity to 0**
    4. There is no need to change the capacity. Run the as-delete-group command and it will reset all values to 0

20. A user has created a web application with Auto Scaling. The user is regularly monitoring the application and he observed that the traffic is highest on Thursday and Friday between 8 AM to 6 PM. What is the best solution to handle scaling in this case?

    1. Add a new instance manually by 8 AM Thursday and terminate the same by 6 PM Friday
    2. **Schedule Auto Scaling to scale up by 8 AM Thursday and scale down after 6 PM on Friday**
    3. Schedule a policy which may scale up every day at 8 AM and scales down by 6 PM
    4. Configure a batch process to add a instance by 8 AM and remove it by Friday 6 PM

21. A user has configured the Auto Scaling group with the minimum capacity as 3 and the maximum capacity as 5. When the user configures the AS group, how many instances will Auto Scaling launch?

    1. **3**
    2. 0
    3. 5
    4. 2

22. A sys admin is maintaining an application on AWS. The application is installed on EC2 and user has configured ELB and Auto Scaling. Considering future load increase, the user is planning to launch new servers proactively so that they get registered with ELB. How can the user add these instances with Auto Scaling?

    1. **Increase the desired capacity of the Auto Scaling group**
    2. Increase the maximum limit of the Auto Scaling group
    3. Launch an instance manually and register it with ELB on the fly
    4. Decrease the minimum limit of the Auto Scaling group

23. In reviewing the auto scaling events for your application you notice that your application is scaling up and down multiple times in the same hour. What design choice could you make to optimize for the cost while preserving elasticity? Choose 2 answers.

    1. **Modify the Amazon CloudWatch alarm period that triggers your auto scaling scale down policy.**
    2. Modify the Auto scaling group termination policy to terminate the oldest instance first.
    3. Modify the Auto scaling policy to use scheduled scaling actions.
    4. **Modify the Auto scaling group cool down timers.**
    5. Modify the Auto scaling group termination policy to terminate newest instance first.

24. You have a business critical two tier web app currently deployed in two availability zones in a single region, using Elastic Load Balancing and Auto Scaling. The app depends on synchronous replication (very low latency connectivity) at the database layer. The application needs to remain fully available even if one application Availability Zone goes off-line, and Auto scaling cannot launch new instances in the remaining Availability Zones. How can the current architecture be enhanced to ensure this? 

    **[PROFESSIONAL]**

    1. Deploy in two regions using Weighted Round Robin (WRR), with Auto Scaling minimums set for 100% peak load per region.
    2. **Deploy in three AZs, with Auto Scaling minimum set to handle 50% peak load per zone.**
    3. Deploy in three AZs, with Auto Scaling minimum set to handle 33% peak load per zone. (Loss of one AZ will handle only 66% if the autoscaling also fails)
    4. Deploy in two regions using Weighted Round Robin (WRR), with Auto Scaling minimums set for 50% peak load per region.

25. A user has created a launch configuration for Auto Scaling where CloudWatch detailed monitoring is disabled. The user wants to now enable detailed monitoring. How can the user achieve this?

    1. Update the Launch config with CLI to set InstanceMonitoringDisabled = false
    2. The user should change the Auto Scaling group from the AWS console to enable detailed monitoring
    3. Update the Launch config with CLI to set InstanceMonitoring.Enabled = true
    4. **Create a new Launch Config with detail monitoring enabled and update the Auto Scaling group**

26. A user has created an Auto Scaling group with default configurations from CLI. The user wants to setup the CloudWatch alarm on the EC2 instances, which are launched by the Auto Scaling group. The user has setup an alarm to monitor the CPU utilization every minute. Which of the below mentioned statements is true?

    1. It will fetch the data at every minute but the four data points [corresponding to 4 minutes] will not have value since the EC2 basic monitoring metrics are collected every five minutes
    2. **It will fetch the data at every minute as detailed monitoring on EC2 will be enabled by the default launch configuration of Auto Scaling**
    3. The alarm creation will fail since the user has not enabled detailed monitoring on the EC2 instances
    4. The user has to first enable detailed monitoring on the EC2 instances to support alarm monitoring at every minute

27. A customer has a website which shows all the deals available across the market. The site experiences a load of 5 large EC2 instances generally. However, a week before Thanksgiving vacation they encounter a load of almost 20 large instances. The load during that period varies over the day based on the office timings. Which of the below mentioned solutions is cost effective as well as help the website achieve better performance?

    1. Keep only 10 instances running and manually launch 10 instances every day during office hours.
    2. **Setup to run 10 instances during the pre-vacation period and only scale up during the office time by launching 10 more instances using the AutoScaling schedule.**
    3. During the pre-vacation period setup a scenario where the organization has 15 instances running and 5 instances to scale up and down using Auto Scaling based on the network I/O policy.
    4. During the pre-vacation period setup 20 instances to run continuously.

28. When Auto Scaling is launching a new instance based on condition, which of the below mentioned policies will it follow?

    1. Based on the criteria defined with cross zone Load balancing
    2. Launch an instance which has the highest load distribution
    3. **Launch an instance in the AZ with the fewest instances**
    4. Launch an instance in the AZ which has the highest instances

29. The user has created multiple AutoScaling groups. The user is trying to create a new AS group but it fails. How can the user know that he has reached the AS group limit specified by AutoScaling in that region?

    1. **Run the command: as-describe-account-limits**
    2. Run the command: as-describe-group-limits
    3. Run the command: as-max-account-limits
    4. Run the command: as-list-account-limits

30. A user is trying to save some cost on the AWS services. Which of the below mentioned options will not help him save cost?

    1. Delete the unutilized EBS volumes once the instance is terminated
    2. Delete the Auto Scaling launch configuration after the instances are terminated (Auto Scaling Launch config does not cost anything)
    3. Release the elastic IP if not required once the instance is terminated
    4. Delete the AWS ELB after the instances are terminated

31. To scale up the AWS resources using manual Auto Scaling, which of the below mentioned parameters should the user change?

    1. Maximum capacity
    2. **Desired capacity**
    3. Preferred capacity
    4. Current capacity

32. For AWS Auto Scaling, what is the first transition state an existing instance enters after leaving steady state in Standby mode?

    1. Detaching
    2. Terminating:Wait
    3. **Pending** (You can put any instance that is in an InService state into a Standby state. This enables you to remove the instance from service, troubleshoot or make changes to it, and then put it back into service. Instances in a Standby state continue to be managed by the Auto Scaling group. However, they are not an active part of your application until you put them back into service. Refer [link](http://docs.aws.amazon.com/AutoScaling/latest/DeveloperGuide/AutoScalingGroupLifecycle.html))
    4. EnteringStandby

33. For AWS Auto Scaling, what is the first transition state an instance enters after leaving steady state when scaling in due to health check failure or decreased load?

    1. **Terminating** (When Auto Scaling responds to a scale in event, it terminates one or more instances. These instances are detached from the Auto Scaling group and enter the Terminating state. Refer [link](http://docs.aws.amazon.com/AutoScaling/latest/DeveloperGuide/AutoScalingGroupLifecycle.html))
    2. Detaching
    3. Terminating:Wait
    4. EnteringStandby

34. A user has setup Auto Scaling with ELB on the EC2 instances. The user wants to configure that whenever the CPU utilization is below 10%, Auto Scaling should remove one instance. How can the user configure this?

    1. The user can get an email using SNS when the CPU utilization is less than 10%. The user can use the desired capacity of Auto Scaling to remove the instance
    2. Use CloudWatch to monitor the data and Auto Scaling to remove the instances using scheduled actions
    3. Configure CloudWatch to send a notification to Auto Scaling Launch configuration when the CPU utilization is less than 10% and configure the Auto Scaling policy to remove the instance
    4. **Configure CloudWatch to send a notification to the Auto Scaling group when the CPU Utilization is less than 10% and configure the Auto Scaling policy to remove the instance**

35. A user has enabled detailed CloudWatch metric monitoring on an Auto Scaling group. Which of the below mentioned metrics will help the user identify the total number of instances in an Auto Scaling group including pending, terminating and running instances?

    1. **GroupTotalInstances** (Refer [link](http://docs.aws.amazon.com/autoscaling/latest/userguide/as-instance-monitoring.html#as-group-metrics))
    2. GroupSumInstances
    3. It is not possible to get a count of all the three metrics together. The user has to find the individual number of running, terminating and pending instances and sum it
    4. GroupInstancesCount

36. Your startup wants to implement an order fulfillment process for selling a personalized gadget that needs an average of 3-4 days to produce with some orders taking up to 6 months you expect 10 orders per day on your first day. 1000 orders per day after 6 months and 10,000 orders after 12 months. Orders coming in are checked for consistency then dispatched to your manufacturing plant for production quality control packaging shipment and payment processing. If the product does not meet the quality standards at any stage of the process employees may force the process to repeat a step. Customers are notified via email about order status and any critical issues with their orders such as payment failure. Your case architecture includes AWS Elastic Beanstalk for your website with an RDS MySQL instance for customer data and orders. How can you implement the order fulfillment process while making sure that the emails are delivered reliably?

     

    **[PROFESSIONAL]**

    1. Add a business process management application to your Elastic Beanstalk app servers and re-use the ROS database for tracking order status use one of the Elastic Beanstalk instances to send emails to customers.
    2. Use SWF with an Auto Scaling group of activity workers and a decider instance in another Auto Scaling group with min/max=1 Use the decider instance to send emails to customers.
    3. **Use SWF with an Auto Scaling group of activity workers and a decider instance in another Auto Scaling group with min/max=1 use SES to send emails to customers.**
    4. Use an SQS queue to manage all process tasks Use an Auto Scaling group of EC2 Instances that poll the tasks and execute them. Use SES to send emails to customers.



---



S3



1. What does Amazon S3 stand for?
   1. Simple Storage Solution.
   2. Storage Storage Storage (triple redundancy Storage).
   3. Storage Server Solution.
   4. **Simple Storage Service**
2. What are characteristics of Amazon S3? Choose 2 answers
   1. **Objects are directly accessible via a URL**
   2. S3 should be used to host a relational database
   3. S3 allows you to store objects or virtually unlimited size
   4. **S3 allows you to store virtually unlimited amounts of data**
   5. S3 offers Provisioned IOPS
3. You are building an automated transcription service in which Amazon EC2 worker instances process an uploaded audio file and generate a text file. You must store both of these files in the same durable storage until the text file is retrieved. You do not know what the storage capacity requirements are. Which storage option is both cost-efficient and scalable?
   1. Multiple Amazon EBS volume with snapshots
   2. A single Amazon Glacier vault
   3. **A single Amazon S3 bucket**
   4. Multiple instance stores
4. A user wants to upload a complete folder to AWS S3 using the S3 Management console. How can the user perform this activity?
   1. Just drag and drop the folder using the flash tool provided by S3
   2. Use the Enable Enhanced Folder option from the S3 console while uploading objects
   3. The user cannot upload the whole folder in one go with the S3 management console
   4. **Use the Enable Enhanced Uploader option from the S3 console while uploading objects** (NOTE – Its no longer supported by AWS)
5. A media company produces new video files on-premises every day with a total size of around 100GB after compression. All files have a size of 1-2 GB and need to be uploaded to Amazon S3 every night in a fixed time window between 3am and 5am. Current upload takes almost 3 hours, although less than half of the available bandwidth is used. What step(s) would ensure that the file uploads are able to complete in the allotted time window?
   1. Increase your network bandwidth to provide faster throughput to S3
   2. **Upload the files in parallel to S3 using mulipart upload**
   3. Pack all files into a single archive, upload it to S3, then extract the files in AWS
   4. Use AWS Import/Export to transfer the video files
6. A company is deploying a two-tier, highly available web application to AWS. Which service provides durable storage for static content while utilizing lower Overall CPU resources for the web tier?
   1. Amazon EBS volume
   2. **Amazon S3**
   3. Amazon EC2 instance store
   4. Amazon RDS instance
7. You have an application running on an Amazon Elastic Compute Cloud instance, that uploads 5 GB video objects to Amazon Simple Storage Service (S3). Video uploads are taking longer than expected, resulting in poor application performance. Which method will help improve performance of your application?
   1. Enable enhanced networking
   2. **Use Amazon S3 multipart upload**
   3. Leveraging Amazon CloudFront, use the HTTP POST method to reduce latency.
   4. Use Amazon Elastic Block Store Provisioned IOPs and use an Amazon EBS-optimized instance
8. When you put objects in Amazon S3, what is the indication that an object was successfully stored?
   1. Each S3 account has a special bucket named_s3_logs. Success codes are written to this bucket with a timestamp and checksum.
   2. A success code is inserted into the S3 object metadata.
   3. **A HTTP 200 result code and MD5 checksum, taken together, indicate that the operation was successful.**
   4. Amazon S3 is engineered for 99.999999999% durability. Therefore there is no need to confirm that data was inserted.
9. You have private video content in S3 that you want to serve to subscribed users on the Internet. User IDs, credentials, and subscriptions are stored in an Amazon RDS database. Which configuration will allow you to securely serve private content to your users?
   1. **Generate pre-signed URLs for each user as they request access to protected S3 content**
   2. Create an IAM user for each subscribed user and assign the GetObject permission to each IAM user
   3. Create an S3 bucket policy that limits access to your private content to only your subscribed users’ credentials
   4. Create a CloudFront Origin Identity user for your subscribed users and assign the GetObject permission to this user
10. You run an ad-supported photo sharing website using S3 to serve photos to visitors of your site. At some point you find out that other sites have been linking to the photos on your site, causing loss to your business. What is an effective method to mitigate this?
    1. **Remove public read access and use signed URLs with expiry dates.**
    2. Use CloudFront distributions for static content.
    3. Block the IPs of the offending websites in Security Groups.
    4. Store photos on an EBS volume of the web server.
11. You are designing a web application that stores static assets in an Amazon Simple Storage Service (S3) bucket. You expect this bucket to immediately receive over 150 PUT requests per second. What should you do to ensure optimal performance?
    1. Use multi-part upload.
    2. **Add a random prefix to the key names.**
    3. Amazon S3 will automatically manage performance at this scale.
    4. Use a predictable naming scheme, such as sequential numbers or date time sequences, in the key names
12. What is the maximum number of S3 buckets available per AWS Account?
    1. 100 Per region
    2. There is no Limit
    3. **100 Per Account** (Refer [documentation](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html))**
       **
    4. 500 Per Account
    5. 100 Per IAM User
13. Your customer needs to create an application to allow contractors to upload videos to Amazon Simple Storage Service (S3) so they can be transcoded into a different format. She creates AWS Identity and Access Management (IAM) users for her application developers, and in just one week, they have the application hosted on a fleet of Amazon Elastic Compute Cloud (EC2) instances. The attached IAM role is assigned to the instances. As expected, a contractor who authenticates to the application is given a pre-signed URL that points to the location for video upload. However, contractors are reporting that they cannot upload their videos. Which of the following are valid reasons for this behavior? Choose 2 answers { “Version”: “2012-10-17”, “Statement”: [ { “Effect”: “Allow”, “Action”: “s3:*”, “Resource”: “*” } ] }
    1. The IAM role does not explicitly grant permission to upload the object. (The role has all permissions for all activities on S3)
    2. The contractorsˈ accounts have not been granted “write” access to the S3 bucket. (using pre-signed urls the contractors account don’t need to have access but only the creator of the pre-signed urls)
    3. **The application is not using valid security credentials to generate the pre-signed URL.**
    4. The developers do not have access to upload objects to the S3 bucket. (developers are not uploading the objects but its using pre-signed urls)
    5. The S3 bucket still has the associated default permissions. (does not matter as long as the user has permission to upload)
    6. **The pre-signed URL has expired.**



---



EBS



1. _____ is a durable, block-level storage volume that you can attach to a single, running Amazon EC2 instance.
   1. Amazon S3
   2. **Amazon EBS**
   3. None of these
   4. All of these
2. Which Amazon storage do you think is the best for my database-style applications that frequently encounter many random reads and writes across the dataset?
   1. None of these.
   2. Amazon Instance Storage
   3. Any of these
   4. **Amazon EBS**
3. What does Amazon EBS stand for?
   1. Elastic Block Storage
   2. Elastic Business Server
   3. Elastic Blade Server
   4. **Elastic Block Store**
4. Which Amazon Storage behaves like raw, unformatted, external block devices that you can attach to your instances?
   1. None of these.
   2. Amazon Instance Storage
   3. **Amazon EBS**
   4. All of these
5. A user has created numerous EBS volumes. What is the general limit for each AWS account for the maximum number of EBS volumes that can be created?
   1. 10000
   2. **5000**
   3. 100
   4. 1000
6. Select the correct set of steps for exposing the snapshot only to specific AWS accounts
   1. Select Public for all the accounts and check mark those accounts with whom you want to expose the snapshots and click save.
   2. **Select Private and enter the IDs of those AWS accounts, and click Save.**
   3. Select Public, enter the IDs of those AWS accounts, and click Save.
   4. Select Public, mark the IDs of those AWS accounts as private, and click Save.
7. If an Amazon EBS volume is the root device of an instance, can I detach it without stopping the instance?
   1. Yes but only if Windows instance
   2. **No**
   3. Yes
   4. Yes but only if a Linux instance
8. Can we attach an EBS volume to more than one EC2 instance at the same time?
   1. Yes
   2. **No**
   3. Only EC2-optimized EBS volumes.
   4. Only in read mode.
9. Do the Amazon EBS volumes persist independently from the running life of an Amazon EC2 instance?
   1. **Only if instructed to when created**
   2. Yes
   3. No
10. Can I delete a snapshot of the root device of an EBS volume used by a registered AMI?
    1. Only via API
    2. Only via Console
    3. Yes
    4. **No**
11. By default, EBS volumes that are created and attached to an instance at launch are deleted when that instance is terminated. You can modify this behavior by changing the value of the flag_____ to false when you launch the instance
    1. **DeleteOnTermination**
    2. RemoveOnDeletion
    3. RemoveOnTermination
    4. TerminateOnDeletion
12. Your company policies require encryption of sensitive data at rest. You are considering the possible options for protecting data while storing it at rest on an EBS data volume, attached to an EC2 instance. Which of these options would allow you to encrypt your data at rest? (Choose 3 answers)
    1. **Implement third party volume encryption tools**
    2. Do nothing as EBS volumes are encrypted by default
    3. **Encrypt data inside your applications before storing it on EBS**
    4. **Encrypt data using native data encryption drivers at the file system level**
    5. Implement SSL/TLS for all services running on the server
13. Which of the following are true regarding encrypted Amazon Elastic Block Store (EBS) volumes? Choose 2 answers
    1. **Supported on all Amazon EBS volume types**
    2. **Snapshots are automatically encrypted**
    3. Available to all instance types
    4. Existing volumes can be encrypted
    5. Shared volumes can be encrypted**
       **
14. How can you secure data at rest on an EBS volume?
    1. Encrypt the volume using the S3 server-side encryption service
    2. Attach the volume to an instance using EC2’s SSL interface.
    3. Create an IAM policy that restricts read and write access to the volume.
    4. Write the data randomly instead of sequentially.
    5. **Use an encrypted file system on top of the EBS volume**
15. A user has deployed an application on an EBS backed EC2 instance. For a better performance of application, it requires dedicated EC2 to EBS traffic. How can the user achieve this?
    1. Launch the EC2 instance as EBS dedicated with PIOPS EBS
    2. Launch the EC2 instance as EBS enhanced with PIOPS EBS
    3. Launch the EC2 instance as EBS dedicated with PIOPS EBS
    4. **Launch the EC2 instance as EBS optimized with PIOPS EBS**
16. A user is trying to launch an EBS backed EC2 instance under free usage. The user wants to achieve encryption of the EBS volume. How can the user encrypt the data at rest?
    1. **Use AWS EBS encryption to encrypt the data at rest (**Encryption is allowed on micro instances**)**
    2. User cannot use EBS encryption and has to encrypt the data manually or using a third party tool (Encryption was not allowed on micro instances before)
    3. The user has to select the encryption enabled flag while launching the EC2 instance
    4. Encryption of volume is not available as a part of the free usage tier
17. A user is planning to schedule a backup for an EBS volume. The user wants security of the snapshot data. How can the user achieve data encryption with a snapshot?
    1. **Use encrypted EBS volumes so that the snapshot will be encrypted by AWS**
    2. While creating a snapshot select the snapshot with encryption
    3. By default the snapshot is encrypted by AWS
    4. Enable server side encryption for the snapshot using S3
18. A user has launched an EBS backed EC2 instance. The user has rebooted the instance. Which of the below mentioned statements is not true with respect to the reboot action?
    1. The private and public address remains the same
    2. The Elastic IP remains associated with the instance
    3. The volume is preserved
    4. **The instance runs on a new host computer**
19. A user has launched an EBS backed EC2 instance. What will be the difference while performing the restart or stop/start options on that instance?
    1. **For restart it does not charge for an extra hour, while every stop/start it will be charged as a separate hour**
    2. Every restart is charged by AWS as a separate hour, while multiple start/stop actions during a single hour will be counted as a single hour
    3. For every restart or start/stop it will be charged as a separate hour
    4. For restart it charges extra only once, while for every stop/start it will be charged as a separate hour
20. A user has launched an EBS backed instance. The user started the instance at 9 AM in the morning. Between 9 AM to 10 AM, the user is testing some script. Thus, he stopped the instance twice and restarted it. In the same hour the user rebooted the instance once. For how many instance hours will AWS charge the user?
    1. **3 hours**
    2. 4 hours
    3. 2 hours
    4. 1 hour
21. You are running a database on an EC2 instance, with the data stored on Elastic Block Store (EBS) for persistence At times throughout the day, you are seeing large variance in the response times of the database queries Looking into the instance with the isolate command you see a lot of wait time on the disk volume that the database’s data is stored on. What two ways can you improve the performance of the database’s storage while maintaining the current persistence of the data? Choose 2 answers
    1. Move to an SSD backed instance
    2. **Move the database to an EBS-Optimized Instance**
    3. **Use Provisioned IOPs EBS**
    4. Use the ephemeral storage on an m2.4xLarge Instance Instead
22. An organization wants to move to Cloud. They are looking for a secure encrypted database storage option. Which of the below mentioned AWS functionalities helps them to achieve this?
    1. AWS MFA with EBS
    2. **AWS EBS encryption**
    3. Multi-tier encryption with Redshift
    4. AWS S3 server-side storage
23. A user has stored data on an encrypted EBS volume. The user wants to share the data with his friend’s AWS account. How can user achieve this?
    1. Create an AMI from the volume and share the AMI
    2. **Copy the data to an unencrypted volume and then share**
    3. Take a snapshot and share the snapshot with a friend
    4. If both the accounts are using the same encryption key then the user can share the volume directly
24. A user is using an EBS backed instance. Which of the below mentioned statements is true?
    1. The user will be charged for volume and instance only when the instance is running
    2. **The user will be charged for the volume even if the instance is stopped**
    3. The user will be charged only for the instance running cost
    4. The user will not be charged for the volume if the instance is stopped
25. A user is planning to use EBS for his DB requirement. The user already has an EC2 instance running in the VPC private subnet. How can the user attach the EBS volume to a running instance?
    1. The user must create EBS within the same VPC and then attach it to a running instance.
    2. **The user can create EBS in the same zone as the subnet of instance and attach that EBS to instance.** (Should be in the same AZ)
    3. It is not possible to attach an EBS to an instance running in VPC until the instance is stopped.
    4. The user can specify the same subnet while creating EBS and then attach it to a running instance.
26. A user is creating an EBS volume. He asks for your advice. Which advice mentioned below should you not give to the user for creating an EBS volume?
    1. Take the snapshot of the volume when the instance is stopped
    2. Stripe multiple volumes attached to the same instance
    3. **Create an AMI from the attached volume** (AMI is created from the snapshot)
    4. Attach multiple volumes to the same instance
27. An EC2 instance has one additional EBS volume attached to it. How can a user attach the same volume to another running instance in the same AZ?
    1. Terminate the first instance and only then attach to the new instance
    2. Attach the volume as read only to the second instance
    3. **Detach the volume first and attach to new instance**
    4. No need to detach. Just select the volume and attach it to the new instance, it will take care of mapping internally
28. What is the scope of an EBS volume?
    1. VPC
    2. Region
    3. Placement Group
    4. **Availability Zone**



---



Glacier



1. What is Amazon Glacier?
   1. You mean Amazon “Iceberg”: it’s a low-cost storage service.
   2. A security tool that allows to “freeze” an EBS volume and perform computer forensics on it.
   3. **A low-cost storage service that provides secure and durable storage for data archiving and backup**
   4. It’s a security tool that allows to “freeze” an EC2 instance and perform computer forensics on it.
2. Amazon Glacier is designed for: (Choose 2 answers)
   1. Active database storage
   2. **Infrequently accessed data**
   3. **Data archives**
   4. Frequently accessed data
   5. Cached session data
3. An organization is generating digital policy files which are required by the admins for verification. Once the files are verified they may not be required in the future unless there is some compliance issue. If the organization wants to save them in a cost effective way, which is the best possible solution?
   1. AWS RRS
   2. AWS S3
   3. AWS RDS
   4. **AWS Glacier**
4. A user has moved an object to Glacier using the life cycle rules. The user requests to restore the archive after 6 months. When the restore request is completed the user accesses that archive. Which of the below mentioned statements is not true in this condition?
   1. The archive will be available as an object for the duration specified by the user during the restoration request
   2. **The restored object’s storage class will be RRS** (After the object is restored the storage class still remains GLACIER. [Read more](http://docs.aws.amazon.com/AmazonS3/latest/dev/restoring-objects.html))
   3. The user can modify the restoration period only by issuing a new restore request with the updated period
   4. The user needs to pay storage for both RRS (restored) and Glacier (Archive) Rates
5. To meet regulatory requirements, a pharmaceuticals company needs to archive data after a drug trial test is concluded. Each drug trial test may generate up to several thousands of files, with compressed file sizes ranging from 1 byte to 100MB. Once archived, data rarely needs to be restored, and on the rare occasion when restoration is needed, the company has 24 hours to restore specific files that match certain metadata. Searches must be possible by numeric file ID, drug name, participant names, date ranges, and other metadata. Which is the most cost-effective architectural approach that can meet the requirements?
   1. Store individual files in Amazon Glacier, using the file ID as the archive name. When restoring data, query the Amazon Glacier vault for files matching the search criteria. (Individual files are expensive and does not allow searching by participant names etc)
   2. Store individual files in Amazon S3, and store search metadata in an Amazon Relational Database Service (RDS) multi-AZ database. Create a lifecycle rule to move the data to Amazon Glacier after a certain number of days. When restoring data, query the Amazon RDS database for files matching the search criteria, and move the files matching the search criteria back to S3 Standard class. (As the data is not needed can be stored to Glacier directly and the data need not be moved back to S3 standard)
   3. Store individual files in Amazon Glacier, and store the search metadata in an Amazon RDS multi-AZ database. When restoring data, query the Amazon RDS database for files matching the search criteria, and retrieve the archive name that matches the file ID returned from the database query. (Individual files and Multi-AZ is expensive)
   4. **First, compress and then concatenate all files for a completed drug trial test into a single Amazon Glacier archive. Store the associated byte ranges for the compressed files along with other search metadata in an Amazon RDS database with regular snapshotting. When restoring data, query the database for files that match the search criteria, and create restored files from the retrieved byte ranges.**
   5. Store individual compressed files and search metadata in Amazon Simple Storage Service (S3). Create a lifecycle rule to move the data to Amazon Glacier, after a certain number of days. When restoring data, query the Amazon S3 bucket for files matching the search criteria, and retrieve the file to S3 reduced redundancy in order to move it back to S3 Standard class. (Once the data is moved from S3 to Glacier the metadata is lost, as Glacier does not have metadata and must be maintained externally)
6. A user is uploading archives to Glacier. The user is trying to understand key Glacier resources. Which of the below mentioned options is not a Glacier resource?
   1. Notification configuration
   2. **Archive ID**
   3. Job
   4. Archive



---



IAM



1. IAM’s Policy Evaluation Logic always starts with a default ____________ for every request, except for those that use the AWS account’s root security credentials b

   1. Permit
   2. **Deny**
   3. Cancel

2. An organization has created 10 IAM users. The organization wants each of the IAM users to have access to a separate DynamoDB table. All the users are added to the same group and the organization wants to setup a group level policy for this. How can the organization achieve this?

   1. Define the group policy and add a condition which allows the access based on the IAM name
   2. **Create a DynamoDB table with the same name as the IAM user name and define the policy rule which grants access based on the DynamoDB ARN using a variable**
   3. Create a separate DynamoDB database for each user and configure a policy in the group based on the DB variable
   4. It is not possible to have a group level policy which allows different IAM users to different DynamoDB Tables

3. An organization has setup multiple IAM users. The organization wants that each IAM user accesses the IAM console only within the organization and not from outside. How can it achieve this?

   1. Create an IAM policy with the security group and use that security group for AWS console login
   2. **Create an IAM policy with a condition which denies access when the IP address range is not from the organization**
   3. Configure the EC2 instance security group which allows traffic only from the organization’s IP range
   4. Create an IAM policy with VPC and allow a secure gateway between the organization and AWS Console

4. Can I attach more than one policy to a particular entity?

   1. **Yes always**
   2. Only if within GovCloud
   3. No
   4. Only if within VPC

5. A __________ is a document that provides a formal statement of one or more permissions.

   1. **policy**
   2. permission
   3. Role
   4. resource

6. A __________ is the concept of allowing (or disallowing) an entity such as a user, group, or role some type of access to one or more resources.

   1. user
   2. AWS Account
   3. resource
   4. **permission**

7. True or False: When using IAM to control access to your RDS resources, the key names that can be used are case sensitive. For example, aws:CurrentTime is NOT equivalent to AWS:currenttime.

   1. TRUE
   2. **FALSE** (Refer [link](http://docs.aws.amazon.com/directconnect/latest/UserGuide/using_iam.html#keys))

8. A user has set an IAM policy where it allows all requests if a request from IP 10.10.10.1/32. Another policy allows all the requests between 5 PM to 7 PM. What will happen when a user is requesting access from IP 10.10.10.1/32 at 6 PM?

   1. IAM will throw an error for policy conflict
   2. It is not possible to set a policy based on the time or IP
   3. It will deny access
   4. **It will allow access**

9. Which of the following are correct statements with policy evaluation logic in AWS Identity and Access Management? Choose 2 answers.

   1. **By default, all requests are denied**
   2. An explicit allow overrides an explicit deny
   3. **An explicit allow overrides default deny**
   4. An explicit deny does not override an explicit allow
   5. By default, all request are allowed

10. A web design company currently runs several FTP servers that their 250 customers use to upload and download large graphic files. They wish to move this system to AWS to make it more scalable, but they wish to maintain customer privacy and keep costs to a minimum. What AWS architecture would you recommend?

     

    **[PROFESSIONAL]**

    1. **Ask their customers to use an S3 client instead of an FTP client. Create a single S3 bucket. Create an IAM user for each customer. Put the IAM Users in a Group that has an IAM policy that permits access to subdirectories within the bucket via use of the ‘username’ Policy variable.**
    2. Create a single S3 bucket with Reduced Redundancy Storage turned on and ask their customers to use an S3 client instead of an FTP client. Create a bucket for each customer with a Bucket Policy that permits access only to that one customer. (Creating bucket for each user is not a scalable model, also 100 buckets are a limit earlier without extending which has since changed [link](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/))
    3. Create an auto-scaling group of FTP servers with a scaling policy to automatically scale-in when minimum network traffic on the auto-scaling group is below a given threshold. Load a central list of ftp users from S3 as part of the user Data startup script on each Instance (Expensive)
    4. Create a single S3 bucket with Requester Pays turned on and ask their customers to use an S3 client instead of an FTP client. Create a bucket tor each customer with a Bucket Policy that permits access only to that one customer. (Creating bucket for each user is not a scalable model, also 100 buckets are a limit earlier without extending which has since changed [link](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/))



----



WAF



1. The Web Application Development team is worried about malicious activity from 200 random IP addresses. Which action will ensure security and scalability from this type of threat?
   1. Use inbound security group rules to block the IP addresses.
   2. Use inbound network ACL rules to block the IP addresses.
   3. **Use AWS WAF to block the IP addresses.**
   4. Write iptables rules on the instance to block the IP addresses.
2. You’ve been hired to enhance the overall security posture for a very large e-commerce site. They have a well architected multi-tier application running in a VPC that uses ELBs in front of both the web and the app tier with static assets served directly from S3. They are using a combination of RDS and DynamoDB for their dynamic data and then archiving nightly into S3 for further processing with EMR. They are concerned because they found questionable log entries and suspect someone is attempting to gain unauthorized access. Which approach provides a cost effective scalable mitigation to this kind of attack? [Old Exam Question]
   1. Recommend mat they lease space at a DirectConnect partner location and establish a 1G DirectConnect connection to their VPC they would then establish Internet connectivity into their space, filter the traffic in hardware Web Application Firewall (WAF). And then pass the traffic through the DirectConnect connection into their application running in their VPC. (Not cost effective)
   2. Add previously identified hostile source IPs as an explicit INBOUND DENY NACL to the web tier subnet. (does not protect against new source)
   3. **Add a WAF tier by creating a new ELB and an AutoScaling group of EC2 Instances running a host-based WAF. They would redirect Route 53 to resolve to the new WAF tier ELB. The WAF tier would then pass the traffic to the current web tier. Web tier Security Groups would be updated to only allow traffic from the WAF tier Security Group**
   4. Remove all but TLS 1.2 from the web tier ELB and enable Advanced Protocol Filtering This will enable the ELB itself to perform WAF functionality. (No advanced protocol filtering in ELB)



---



RDS



1. What does Amazon RDS stand for?
   1. Regional Data Server.
   2. **Relational Database Service**
   3. Regional Database Service.
2. How many relational database engines does RDS currently support?
   1. **MySQL, Postgres, MariaDB, Oracle and Microsoft SQL Server**
   2. Just two: MySQL and Oracle.
   3. Five: MySQL, PostgreSQL, MongoDB, Cassandra and SQLite.
   4. Just one: MySQL.
3. If I modify a DB Instance or the DB parameter group associated with the instance, should I reboot the instance for the changes to take effect?
   1. No
   2. **Yes**
4. What is the name of licensing model in which I can use your existing Oracle Database licenses to run Oracle deployments on Amazon RDS?
   1. **Bring Your Own License**
   2. Role Bases License
   3. Enterprise License
   4. License Included
5. Will I be charged if the DB instance is idle?
   1. No
   2. **Yes**
   3. Only is running in GovCloud
   4. Only if running in VPC
6. What is the minimum charge for the data transferred between Amazon RDS and Amazon EC2 Instances in the same Availability Zone?
   1. USD 0.10 per GB
   2. **No charge. It is free.**
   3. USD 0.02 per GB
   4. USD 0.01 per GB
7. Does Amazon RDS allow direct host access via Telnet, Secure Shell (SSH), or Windows Remote Desktop Connection?
   1. Yes
   2. **No**
   3. Depends on if it is in VPC or not
8. What are the two types of licensing options available for using Amazon RDS for Oracle?
   1. BYOL and Enterprise License
   2. **BYOL and License Included**
   3. Enterprise License and License Included
   4. Role based License and License Included
9. A user plans to use RDS as a managed DB platform. Which of the below mentioned features is not supported by RDS?
   1. Automated backup
   2. **Automated scaling to manage a higher load**
   3. Automated failure detection and recovery
   4. Automated software patching
10. A user is launching an AWS RDS with MySQL. Which of the below mentioned options allows the user to configure the InnoDB engine parameters?
    1. Options group
    2. Engine parameters
    3. **Parameter groups**
    4. DB parameters
11. A user is planning to use the AWS RDS with MySQL. Which of the below mentioned services the user is not going to pay?
    1. Data transfer
    2. **RDS CloudWatch metrics**
    3. Data storage
    4. I/O requests per month



---



DynamoDB



1. Which of the following are use cases for Amazon DynamoDB? Choose 3 answers

   1. Storing BLOB data.
   2. **Managing web sessions**
   3. **Storing JSON documents**
   4. **Storing metadata for Amazon S3 objects**
   5. Running relational joins and complex updates.
   6. Storing large amounts of infrequently accessed data.

2. You are configuring your company’s application to use Auto Scaling and need to move user state information. Which of the following AWS services provides a shared data store with durability and low latency?

   1. AWS ElastiCache Memcached (does not allow writes)
   2. Amazon Simple Storage Service (does not provide low latency)
   3. Amazon EC2 instance storage (not durable)
   4. **Amazon DynamoDB**

3. Does Dynamo DB support in-place atomic updates?

   1. It is not defined
   2. No
   3. **Yes**
   4. It does support in-place non-atomic updates

4. What is the maximum write throughput I can provision for a single Dynamic DB table?

   1. 1,000 write capacity units
   2. 100,000 write capacity units
   3. **Dynamic DB is designed to scale without limits, but if you go beyond 10,000 you have to contact AWS first**
   4. 10,000 write capacity units

5. For a DynamoDB table, what happens if the application performs more reads or writes than your provisioned capacity?

   1. Nothing
   2. requests above the provisioned capacity will be performed but you will receive 400 error codes.
   3. requests above the provisioned capacity will be performed but you will receive 200 error codes.
   4. **requests above the provisioned capacity will be throttled and you will receive 400 error codes.**

6. In which of the following situations might you benefit from using DynamoDB? (Choose 2 answers)

   1. You need fully managed database to handle highly complex queries
   2. **You need to deal with massive amount of “hot” data and require very low latency**
   3. **You need a rapid ingestion of clickstream in order to collect data about user behavior**
   4. Your on-premises data center runs Oracle database, and you need to host a backup in AWS cloud

7. You are designing a file-sharing service. This service will have millions of files in it. Revenue for the service will come from fees based on how much storage a user is using. You also want to store metadata on each file, such as title, description and whether the object is public or private. How do you achieve all of these goals in a way that is economical and can scale to millions of users? 

   **[PROFESSIONAL]**

   1. Store all files in Amazon Simple Storage Service (S3). Create a bucket for each user. Store metadata in the filename of each object, and access it with LIST commands against the S3 API. (expensive and slow as it returns only 1000 items at a time)
   2. **Store all files in Amazon S3. Create Amazon DynamoDB tables for the corresponding key-value pairs on the associated metadata, when objects are uploaded.**
   3. Create a striped set of 4000 IOPS Elastic Load Balancing volumes to store the data. Use a database running in Amazon Relational Database Service (RDS) to store the metadata.(not economical with volumes)
   4. Create a striped set of 4000 IOPS Elastic Load Balancing volumes to store the data. Create Amazon DynamoDB tables for the corresponding key-value pairs on the associated metadata, when objects are uploaded. (not economical with volumes)

8. A utility company is building an application that stores data coming from more than 10,000 sensors. Each sensor has a unique ID and will send a datapoint (approximately 1KB) every 10 minutes throughout the day. Each datapoint contains the information coming from the sensor as well as a timestamp. This company would like to query information coming from a particular sensor for the past week very rapidly and want to delete all the data that is older than 4 weeks. Using Amazon DynamoDB for its scalability and rapidity, how do you implement this in the most cost effective way? 

   **[PROFESSIONAL]**

   1. One table, with a primary key that is the sensor ID and a hash key that is the timestamp (Single table impacts performance)
   2. One table, with a primary key that is the concatenation of the sensor ID and timestamp (Single table and concatenation impacts performance)
   3. One table for each week, with a primary key that is the concatenation of the sensor ID and timestamp (Concatenation will cause queries would be slower, if at all)
   4. **One table for each week, with a primary key that is the sensor ID and a hash key that is the timestamp** (Composite key with Sensor ID and timestamp would help for faster queries)

9. You have recently joined a startup company building sensors to measure street noise and air quality in urban areas. The company has been running a pilot deployment of around 100 sensors for 3 months. Each sensor uploads 1KB of sensor data every minute to a backend hosted on AWS. During the pilot, you measured a peak of 10 IOPS on the database, and you stored an average of 3GB of sensor data per month in the database. The current deployment consists of a load-balanced auto scaled Ingestion layer using EC2 instances and a PostgreSQL RDS database with 500GB standard storage. The pilot is considered a success and your CEO has managed to get the attention or some potential investors. The business plan requires a deployment of at least 100K sensors, which needs to be supported by the backend. You also need to store sensor data for at least two years to be able to compare year over year Improvements. To secure funding, you have to make sure that the platform meets these requirements and leaves room for further scaling. Which setup will meet the requirements? 

   **[PROFESSIONAL]**

   1. Add an SQS queue to the ingestion layer to buffer writes to the RDS instance (RDS instance will not support data for 2 years)
   2. **Ingest data into a DynamoDB table and move old data to a Redshift cluster** (Handle 10K IOPS ingestion and store data into Redshift for analysis)
   3. Replace the RDS instance with a 6 node Redshift cluster with 96TB of storage (Does not handle the ingestion issue)
   4. Keep the current architecture but upgrade RDS storage to 3TB and 10K provisioned IOPS (RDS instance will not support data for 2 years)

10. Does Amazon DynamoDB support both increment and decrement atomic operations?

    1. No, neither increment nor decrement operations.
    2. Only increment, since decrement are inherently impossible with DynamoDB’s data model.
    3. Only decrement, since increment are inherently impossible with DynamoDB’s data model.
    4. **Yes, both increment and decrement operations.**

11. What is the data model of DynamoDB?

    1. “Items”, with Keys and one or more Attribute; and “Attribute”, with Name and Value.
    2. “Database”, which is a set of “Tables”, which is a set of “Items”, which is a set of “Attributes”.
    3. **“Table”, a collection of Items; “Items”, with Keys and one or more Attribute; and “Attribute”, with Name and Value.**
    4. “Database”, a collection of Tables; “Tables”, with Keys and one or more Attribute; and “Attribute”, with Name and Value.

12. In regard to DynamoDB, for which one of the following parameters does Amazon not charge you?

    1. Cost per provisioned write units
    2. Cost per provisioned read units
    3. Storage cost
    4. **I/O usage within the same Region**

13. Which statements about DynamoDB are true? Choose 2 answers.

    1. DynamoDB uses a pessimistic locking model
    2. **DynamoDB uses optimistic concurrency control**
    3. **DynamoDB uses conditional writes for consistency**
    4. DynamoDB restricts item access during reads
    5. DynamoDB restricts item access during writes

14. Which of the following is an example of a good DynamoDB hash key schema for provisioned throughput efficiency?

    1. **User ID, where the application has many different users.**
    2. Status Code where most status codes is the same.
    3. Device ID, where one is by far more popular than all the others.
    4. Game Type, where there are three possible game types.

15. You are inserting 1000 new items every second in a DynamoDB table. Once an hour these items are analyzed and then are no longer needed. You need to minimize provisioned throughput, storage, and API calls. Given these requirements, what is the most efficient way to manage these Items after the analysis?

    1. Retain the items in a single table
    2. Delete items individually over a 24 hour period
    3. **Delete the table and create a new table per hour**
    4. Create a new table per hour

16. When using a large Scan operation in DynamoDB, what technique can be used to minimize the impact of a scan on a table’s provisioned throughput?

    1. **Set a smaller page size for the scan** (Refer [link](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/QueryAndScanGuidelines.html))
    2. Use parallel scans
    3. Define a range index on the table
    4. Prewarm the table by updating all items

17. In regard to DynamoDB, which of the following statements is correct?

    1. An Item should have at least two value sets, a primary key and another attribute.
    2. **An Item can have more than one attributes**
    3. A primary key should be single-valued.
    4. An attribute can have one or several other attributes.

18. Which one of the following statements is NOT an advantage of DynamoDB being built on Solid State Drives?

    1. serve high-scale request workloads
    2. low request pricing
    3. **high I/O performance of WebApp on EC2 instance** (Not related to DynamoDB)
    4. low-latency response times

19. Which one of the following operations is NOT a DynamoDB operation?

    1. BatchWriteItem
    2. DescribeTable
    3. BatchGetItem
    4. **BatchDeleteItem** (DeleteItem deletes a single item in a table by primary key, but BatchDeleteItem doesn’t exist)

20. What item operation allows the retrieval of multiple items from a DynamoDB table in a single API call?

    1. GetItem
    2. **BatchGetItem**
    3. GetMultipleItems
    4. GetItemRange

21. An application stores payroll information nightly in DynamoDB for a large number of employees across hundreds of offices. Item attributes consist of individual name, office identifier, and cumulative daily hours. Managers run reports for ranges of names working in their office. One query is. “Return all Items in this office for names starting with A through E”. Which table configuration will result in the lowest impact on provisioned throughput for this query? 

    **[PROFESSIONAL]**

    1. Configure the table to have a hash index on the name attribute, and a range index on the office identifier
    2. **Configure the table to have a range index on the name attribute, and a hash index on the office identifier**
    3. Configure a hash index on the name attribute and no range index
    4. Configure a hash index on the office Identifier attribute and no range index

22. You need to migrate 10 million records in one hour into DynamoDB. All records are 1.5KB in size. The data is evenly distributed across the partition key. How many write capacity units should you provision during this batch load?

    1. 6667
    2. 4166
    3. **5556** ( 2 write units (1 for each 1KB) * 10 million/3600 secs, refer [link](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ProvisionedThroughput.html))
    4. 2778

23. A meteorological system monitors 600 temperature gauges, obtaining temperature samples every minute and saving each sample to a DynamoDB table. Each sample involves writing 1K of data and the writes are evenly distributed over time. How much write throughput is required for the target table?

    1. 1 write capacity unit
    2. **10 write capacity units** ( 1 write unit for 1K * 600 gauges/60 secs)
    3. 60 write capacity units
    4. 600 write capacity units
    5. 3600 write capacity units

24. You are building a game high score table in DynamoDB. You will store each user’s highest score for each game, with many games, all of which have relatively similar usage levels and numbers of players. You need to be able to look up the highest score for any game. What’s the best DynamoDB key structure?

    1. HighestScore as the hash / only key.
    2. **GameID as the hash key, HighestScore as the range key.** (hash (partition) key should be the GameID, and there should be a range key for ordering HighestScore. Refer [link](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GuidelinesForTables.html#GuidelinesForTables.Partitions))
    3. GameID as the hash / only key.
    4. GameID as the range / only key.

25. You are experiencing performance issues writing to a DynamoDB table. Your system tracks high scores for video games on a marketplace. Your most popular game experiences all of the performance issues. What is the most likely problem?

    1. DynamoDB’s vector clock is out of sync, because of the rapid growth in request for the most popular game.
    2. **You selected the Game ID or equivalent identifier as the primary partition key for the table.** (Refer [link](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GuidelinesForTables.html#GuidelinesForTables.UniformWorkload))
    3. Users of the most popular video game each perform more read and write requests than average.
    4. You did not provision enough read or write throughput to the table.

26. You are writing to a DynamoDB table and receive the following exception:” ProvisionedThroughputExceededException”. Though according to your Cloudwatch metrics for the table, you are not exceeding your provisioned throughput. What could be an explanation for this?

    1. You haven’t provisioned enough DynamoDB storage instances
    2. You’re exceeding your capacity on a particular Range Key
    3. **You’re exceeding your capacity on a particular Hash Key** (Hash key determines the partition and hence the performance)
    4. You’re exceeding your capacity on a particular Sort Key
    5. You haven’t configured DynamoDB Auto Scaling triggers

27. Your company sells consumer devices and needs to record the first activation of all sold devices. Devices are not activated until the information is written on a persistent database. Activation data is very important for your company and must be analyzed daily with a MapReduce job. The execution time of the data analysis process must be less than three hours per day. Devices are usually sold evenly during the year, but when a new device model is out, there is a predictable peak in activation’s, that is, for a few days there are 10 times or even 100 times more activation’s than in average day. Which of the following databases and analysis framework would you implement to better optimize costs and performance for this workload?

     

    **[PROFESSIONAL]**

    1. Amazon RDS and Amazon Elastic MapReduce with Spot instances.
    2. **Amazon DynamoDB and Amazon Elastic MapReduce with Spot instances.**
    3. Amazon RDS and Amazon Elastic MapReduce with Reserved instances.
    4. Amazon DynamoDB and Amazon Elastic MapReduce with Reserved instances



---



ElastiCache



1. What does Amazon ElastiCache provide?
   1. A service by this name doesn’t exist. Perhaps you mean Amazon CloudCache.
   2. A virtual server with a huge amount of memory.
   3. **A managed In-memory cache service**
   4. An Amazon EC2 instance with the Memcached software already pre-installed.
2. You are developing a highly available web application using stateless web servers. Which services are suitable for storing session state data? Choose 3 answers.
   1. Elastic Load Balancing
   2. **Amazon Relational Database Service (RDS)**
   3. Amazon CloudWatch
   4. **Amazon ElastiCache**
   5. **Amazon DynamoDB**
   6. AWS Storage Gateway
3. Which statement best describes ElastiCache?
   1. Reduces the latency by splitting the workload across multiple AZs
   2. A simple web services interface to create and store multiple data sets, query your data easily, and return the results
   3. **Offload the read traffic from your database in order to reduce latency caused by read-heavy workload**
   4. Managed service that makes it easy to set up, operate and scale a relational database in the cloud
4. Our company is getting ready to do a major public announcement of a social media site on AWS. The website is running on EC2 instances deployed across multiple Availability Zones with a Multi-AZ RDS MySQL Extra Large DB Instance. The site performs a high number of small reads and writes per second and relies on an eventual consistency model. After comprehensive tests you discover that there is read contention on RDS MySQL. Which are the best approaches to meet these requirements? (Choose 2 answers)
   1. **Deploy ElastiCache in-memory cache running in each availability zone**
   2. Implement sharding to distribute load to multiple RDS MySQL instances
   3. Increase the RDS MySQL Instance size and Implement provisioned IOPS
   4. **Add an RDS MySQL read replica in each availability zone**
5. You are using ElastiCache Memcached to store session state and cache database queries in your infrastructure. You notice in CloudWatch that Evictions and Get Misses are both very high. What two actions could you take to rectify this? Choose 2 answers
   1. **Increase the number of nodes in your cluster**
   2. Tweak the max_item_size parameter
   3. Shrink the number of nodes in your cluster
   4. **Increase the size of the nodes in the cluster**
6. You have been tasked with moving an ecommerce web application from a customer’s datacenter into a VPC. The application must be fault tolerant and well as highly scalable. Moreover, the customer is adamant that service interruptions not affect the user experience. As you near launch, you discover that the application currently uses multicast to share session state between web servers, In order to handle session state within the VPC, you choose to:
   1. **Store session state in Amazon ElastiCache for Redis** (scalable and makes the web applications stateless)
   2. Create a mesh VPN between instances and allow multicast on it
   3. Store session state in Amazon Relational Database Service (RDS solution not highly scalable)
   4. Enable session stickiness via Elastic Load Balancing (affects user experience if the instance goes down)
7. When you are designing to support a 24-hour flash sale, which one of the following methods best describes a strategy to lower the latency while keeping up with unusually heavy traffic?
   1. Launch enhanced networking instances in a placement group to support the heavy traffic (only improves internal communication)
   2. Apply Service Oriented Architecture (SOA) principles instead of a 3-tier architecture (just simplifies architecture)
   3. Use Elastic Beanstalk to enable blue-green deployment (only minimizes download for applications and ease of rollback)
   4. **Use ElastiCache as in-memory storage on top of DynamoDB to store user sessions** (scalable, faster read/writes and in memory storage)
8. You are configuring your company’s application to use Auto Scaling and need to move user state information. Which of the following AWS services provides a shared data store with durability and low latency?
   1. AWS ElastiCache Memcached (does not provide durability as if the node is gone the data is gone)
   2. Amazon Simple Storage Service
   3. Amazon EC2 instance storage
   4. **Amazon DynamoDB**
9. Your application is using an ELB in front of an Auto Scaling group of web/application servers deployed across two AZs and a Multi-AZ RDS Instance for data persistence. The database CPU is often above 80% usage and 90% of I/O operations on the database are reads. To improve performance you recently added a single-node Memcached ElastiCache Cluster to cache frequent DB query results. In the next weeks the overall workload is expected to grow by 30%. Do you need to change anything in the architecture to maintain the high availability for the application with the anticipated additional load and Why?
   1. **You should deploy two Memcached ElastiCache Clusters in different AZs because the RDS Instance will not be able to handle the load if the cache node fails.**
   2. If the cache node fails the automated ElastiCache node recovery feature will prevent any availability impact. (does not provide high availability, as data is lost if the node is lost)
   3. Yes you should deploy the Memcached ElastiCache Cluster with two nodes in the same AZ as the RDS DB master instance to handle the load if one cache node fails. (Single AZ affects availability as DB is Multi AZ and would be overloaded is the AZ goes down)
   4. No if the cache node fails you can always get the same data from the DB without having any availability impact. (Will overload the database affecting availability)
10. A read only news reporting site with a combined web and application tier and a database tier that receives large and unpredictable traffic demands must be able to respond to these traffic fluctuations automatically. What AWS services should be used meet these requirements?
    1. **Stateless instances for the web and application tier synchronized using ElastiCache Memcached in an autoscaling group monitored with CloudWatch and RDS with read replicas.**
    2. Stateful instances for the web and application tier in an autoscaling group monitored with CloudWatch and RDS with read replicas (Stateful instances will not allow for scaling)
    3. Stateful instances for the web and application tier in an autoscaling group monitored with CloudWatch and multi-AZ RDS (Stateful instances will allow not for scaling & multi-AZ is for high availability and not scaling)
    4. Stateless instances for the web and application tier synchronized using ElastiCache Memcached in an autoscaling group monitored with CloudWatch and multi-AZ RDS (multi-AZ is for high availability and not scaling)
11. You have written an application that uses the Elastic Load Balancing service to spread traffic to several web servers. Your users complain that they are sometimes forced to login again in the middle of using your application, after they have already logged in. This is not behavior you have designed. What is a possible solution to prevent this happening?
    1. Use instance memory to save session state.
    2. Use instance storage to save session state.
    3. Use EBS to save session state.
    4. **Use ElastiCache to save session state.**
    5. Use Glacier to save session slate.



---



VPC



1. You have a business-to-business web application running in a VPC consisting of an Elastic Load Balancer (ELB), web servers, application servers and a database. Your web application should only accept traffic from predefined customer IP addresses. Which two options meet this security requirement? Choose 2 answers

   1. Configure web server VPC security groups to allow traffic from your customers’ IPs (Web server is behind the ELB and customer IPs will never reach web servers)
   2. **Configure your web servers to filter traffic based on the ELB’s “X-forwarded-for” header** (get the customer IPs and create a custom filter to restrict access. Refer [link](https://aws.amazon.com/premiumsupport/knowledge-center/log-client-ip-load-balancer-apache/))
   3. **Configure ELB security groups to allow traffic from your customers’ IPs and deny all outbound traffic** (ELB will see the customer IPs so can restrict access, deny all is basically have no rules in outbound traffic, implicit, and its stateful so would work)
   4. Configure a VPC NACL to allow web traffic from your customers’ IPs and deny all outbound traffic (NACL is stateless, deny all will not work)

2. A user has created a VPC with public and private subnets using the VPC Wizard. The VPC has CIDR 20.0.0.0/16. The private subnet uses CIDR 20.0.0.0/24. Which of the below mentioned entries are required in the main route table to allow the instances in VPC to communicate with each other?

   1. Destination : 20.0.0.0/24 and Target : VPC
   2. Destination : 20.0.0.0/16 and Target : ALL
   3. Destination : 20.0.0.0/0 and Target : ALL
   4. **Destination : 20.0.0.0/16 and Target : Local**

3. A user has created a VPC with two subnets: one public and one private. The user is planning to run the patch update for the instances in the private subnet. How can the instances in the private subnet connect to the internet?

   1. Use the internet gateway with a private IP
   2. Allow outbound traffic in the security group for port 80 to allow internet updates
   3. The private subnet can never connect to the internet
   4. **Use NAT with an elastic IP**

4. A user has launched an EC2 instance and installed a website with the Apache webserver. The webserver is running but the user is not able to access the website from the Internet. What can be the possible reason for this failure?

   1. **The security group of the instance is not configured properly.**
   2. The instance is not configured with the proper key-pairs.
   3. The Apache website cannot be accessed from the Internet.
   4. Instance is not configured with an elastic IP.

5. A user has created a VPC with public and private subnets using the VPC wizard. Which of the below mentioned statements is true in this scenario?

   1. AWS VPC will automatically create a NAT instance with the micro size
   2. **VPC bounds the main route table with a private subnet and a custom route table with a public subnet**
   3. User has to manually create a NAT instance
   4. VPC bounds the main route table with a public subnet and a custom route table with a private subnet

6. A user has created a VPC with public and private subnets. The VPC has CIDR 20.0.0.0/16. The private subnet uses CIDR 20.0.1.0/24 and the public subnet uses CIDR 20.0.0.0/24. The user is planning to host a web server in the public subnet (port 80) and a DB server in the private subnet (port 3306). The user is configuring a security group of the NAT instance. Which of the below mentioned entries is not required for the NAT security group?

   1. For Inbound allow Source: 20.0.1.0/24 on port 80
   2. For Outbound allow Destination: 0.0.0.0/0 on port 80
   3. **For Inbound allow Source: 20.0.0.0/24 on port 80**
   4. For Outbound allow Destination: 0.0.0.0/0 on port 443

7. A user has created a VPC with CIDR 20.0.0.0/24. The user has used all the IPs of CIDR and wants to increase the size of the VPC. The user has two subnets: public (20.0.0.0/25) and private (20.0.0.128/25). How can the user change the size of the VPC?

   1. The user can delete all the instances of the subnet. Change the size of the subnets to 20.0.0.0/32 and 20.0.1.0/32, respectively. Then the user can increase the size of the VPC using CLI
   2. **It is not possible to change the size of the VPC once it has been created (NOTE – You can now increase the VPC size. Read [Post](https://aws.amazon.com/about-aws/whats-new/2017/08/amazon-virtual-private-cloud-vpc-now-allows-customers-to-expand-their-existing-vpcs/))**
   3. User can add a subnet with a higher range so that it will automatically increase the size of the VPC
   4. User can delete the subnets first and then modify the size of the VPC

8. A user has created a VPC with the public and private subnets using the VPC wizard. The VPC has CIDR 20.0.0.0/16. The public subnet uses CIDR 20.0.1.0/24. The user is planning to host a web server in the public subnet (port 80) and a DB server in the private subnet (port 3306). The user is configuring a security group for the public subnet (WebSecGrp) and the private subnet (DBSecGrp). Which of the below mentioned entries is required in the web server security group (WebSecGrp)?

   1. **Configure Destination as DB Security group ID (DbSecGrp) for port 3306 Outbound**
   2. Configure port 80 for Destination 0.0.0.0/0 Outbound
   3. Configure port 3306 for source 20.0.0.0/24 InBound
   4. Configure port 80 InBound for source 20.0.0.0/16

9. A user has created a VPC with CIDR 20.0.0.0/16. The user has created one subnet with CIDR 20.0.0.0/16 by mistake. The user is trying to create another subnet of CIDR 20.0.0.1/24. How can the user create the second subnet?

   1. There is no need to update the subnet as VPC automatically adjusts the CIDR of the first subnet based on the second subnet’s CIDR
   2. The user can modify the first subnet CIDR from the console
   3. **It is not possible to create a second subnet as one subnet with the same CIDR as the VPC has been created**
   4. The user can modify the first subnet CIDR with AWS CLI

10. A user has setup a VPC with CIDR 20.0.0.0/16. The VPC has a private subnet (20.0.1.0/24) and a public subnet (20.0.0.0/24). The user’s data centre has CIDR of 20.0.54.0/24 and 20.1.0.0/24. If the private subnet wants to communicate with the data centre, what will happen?

    1. It will allow traffic communication on both the CIDRs of the data centre
    2. It will not allow traffic with data centre on CIDR 20.1.0.0/24 but allows traffic communication on 20.0.54.0/24
    3. It will not allow traffic communication on any of the data centre CIDRs
    4. **It will allow traffic with data centre on CIDR 20.1.0.0/24 but does not allow on 20.0.54.0/24** (as the CIDR block would be overlapping)

11. A user has created a VPC with public and private subnets using the VPC wizard. The VPC has CIDR 20.0.0.0/16. The private subnet uses CIDR 20.0.0.0/24 . The NAT instance ID is i-a12345. Which of the below mentioned entries are required in the main route table attached with the private subnet to allow instances to connect with the internet?

    1. **Destination: 0.0.0.0/0 and Target: i-a12345**
    2. Destination: 20.0.0.0/0 and Target: 80
    3. Destination: 20.0.0.0/0 and Target: i-a12345
    4. Destination: 20.0.0.0/24 and Target: i-a12345

12. A user has created a VPC with CIDR 20.0.0.0/16 using the wizard. The user has created a public subnet CIDR (20.0.0.0/24) and VPN only subnets CIDR (20.0.1.0/24) along with the VPN gateway (vgw-12345) to connect to the user’s data centre. The user’s data centre has CIDR 172.28.0.0/12. The user has also setup a NAT instance (i-123456) to allow traffic to the internet from the VPN subnet. Which of the below mentioned options is not a valid entry for the main route table in this scenario?

    1. **Destination: 20.0.1.0/24 and Target: i-12345**
    2. Destination: 0.0.0.0/0 and Target: i-12345
    3. Destination: 172.28.0.0/12 and Target: vgw-12345
    4. Destination: 20.0.0.0/16 and Target: local

13. A user has created a VPC with CIDR 20.0.0.0/16. The user has created one subnet with CIDR 20.0.0.0/16 in this VPC. The user is trying to create another subnet with the same VPC for CIDR 20.0.0.1/24. What will happen in this scenario?

    1. The VPC will modify the first subnet CIDR automatically to allow the second subnet IP range
    2. It is not possible to create a subnet with the same CIDR as VPC
    3. The second subnet will be created
    4. **It will throw a CIDR overlaps error**

14. A user has created a VPC with CIDR 20.0.0.0/16 using the wizard. The user has created both Public and VPN-Only subnets along with hardware VPN access to connect to the user’s data centre. The user has not yet launched any instance as well as modified or deleted any setup. He wants to delete this VPC from the console. Will the console allow the user to delete the VPC?

    1. Yes, the console will delete all the setups and also delete the virtual private gateway
    2. No, the console will ask the user to manually detach the virtual private gateway first and then allow deleting the VPC
    3. **Yes, the console will delete all the setups and detach the virtual private gateway**
    4. No, since the NAT instance is running

15. A user has created a VPC with the public and private subnets using the VPC wizard. The VPC has CIDR 20.0.0.0/16. The public subnet uses CIDR 20.0.1.0/24. The user is planning to host a web server in the public subnet (port 80) and a DB server in the private subnet (port 3306). The user is configuring a security group for the public subnet (WebSecGrp) and the private subnet (DBSecGrp). Which of the below mentioned entries is required in the private subnet database security group (DBSecGrp)?

    1. **Allow Inbound on port 3306 for Source Web Server Security Group (WebSecGrp)**
    2. Allow Inbound on port 3306 from source 20.0.0.0/16
    3. Allow Outbound on port 3306 for Destination Web Server Security Group (WebSecGrp.
    4. Allow Outbound on port 80 for Destination NAT Instance IP

16. A user has created a VPC with a subnet and a security group. The user has launched an instance in that subnet and attached a public IP. The user is still unable to connect to the instance. The internet gateway has also been created. What can be the reason for the error?

    1. **The internet gateway is not configured with the route table**
    2. The private IP is not present
    3. The outbound traffic on the security group is disabled
    4. The internet gateway is not configured with the security group

17. A user has created a subnet in VPC and launched an EC2 instance within it. The user has not selected the option to assign the IP address while launching the instance. Which of the below mentioned statements is true with respect to the Instance requiring access to the Internet?

    1. The instance will always have a public DNS attached to the instance by default
    2. The user can directly attach an elastic IP to the instance
    3. The instance will never launch if the public IP is not assigned
    4. **The user would need to create an internet gateway and then attach an elastic IP to the instance to connect from internet**

18. A user has created a VPC with public and private subnets using the VPC wizard. Which of the below mentioned statements is not true in this scenario?

    1. **VPC will create a routing instance and attach it with a public subnet**
    2. VPC will create two subnets
    3. VPC will create one internet gateway and attach it to VPC
    4. VPC will launch one NAT instance with an elastic IP

19. A user has created a VPC with the public subnet. The user has created a security group for that VPC. Which of the below mentioned statements is true when a security group is created?

    1. It can connect to the AWS services, such as S3 and RDS by default
    2. It will have all the inbound traffic by default
    3. **It will have all the outbound traffic by default**
    4. It will by default allow traffic to the internet gateway

20. A user has created a VPC with CIDR 20.0.0.0/16 using VPC Wizard. The user has created a public CIDR (20.0.0.0/24) and a VPN only subnet CIDR (20.0.1.0/24) along with the hardware VPN access to connect to the user’s data centre. Which of the below mentioned components is not present when the VPC is setup with the wizard?

    1. Main route table attached with a VPN only subnet
    2. **A NAT instance configured to allow the VPN subnet instances to connect with the internet**
    3. Custom route table attached with a public subnet
    4. An internet gateway for a public subnet

21. A user has created a VPC with public and private subnets using the VPC wizard. The user has not launched any instance manually and is trying to delete the VPC. What will happen in this scenario?

    1. It will not allow to delete the VPC as it has subnets with route tables
    2. It will not allow to delete the VPC since it has a running route instance
    3. It will terminate the VPC along with all the instances launched by the wizard
    4. **It will not allow to delete the VPC since it has a running NAT instance**

22. A user has created a public subnet with VPC and launched an EC2 instance within it. The user is trying to delete the subnet. What will happen in this scenario?

    1. It will delete the subnet and make the EC2 instance as a part of the default subnet
    2. **It will not allow the user to delete the subnet until the instances are terminated**
    3. It will delete the subnet as well as terminate the instances
    4. Subnet can never be deleted independently, but the user has to delete the VPC first

23. A user has created a VPC with CIDR 20.0.0.0/24. The user has created a public subnet with CIDR 20.0.0.0/25 and a private subnet with CIDR 20.0.0.128/25. The user has launched one instance each in the private and public subnets. Which of the below mentioned options cannot be the correct IP address (private IP) assigned to an instance in the public or private subnet?

    1. **20.0.0.255**
    2. 20.0.0.132
    3. 20.0.0.122
    4. 20.0.0.55

24. A user has created a VPC with CIDR 20.0.0.0/16. The user has created public and VPN only subnets along with hardware VPN access to connect to the user’s datacenter. The user wants to make so that all traffic coming to the public subnet follows the organization’s proxy policy. How can the user make this happen?

    1. Setting up a NAT with the proxy protocol and configure that the public subnet receives traffic from NAT
    2. Setting up a proxy policy in the internet gateway connected with the public subnet
    3. It is not possible to setup the proxy policy for a public subnet
    4. **Setting the route table and security group of the public subnet which receives traffic from a virtual private gateway**

25. A user has created a VPC with CIDR 20.0.0.0/16 using the wizard. The user has created a public subnet CIDR (20.0.0.0/24) and VPN only subnets CIDR (20.0.1.0/24) along with the VPN gateway (vgw-12345) to connect to the user’s data centre. Which of the below mentioned options is a valid entry for the main route table in this scenario?

    1. Destination: 20.0.0.0/24 and Target: vgw-12345
    2. Destination: 20.0.0.0/16 and Target: ALL
    3. Destination: 20.0.1.0/16 and Target: vgw-12345
    4. **Destination: 0.0.0.0/0 and Target: vgw-12345**

26. Which two components provide connectivity with external networks? When attached to an Amazon VPC which two components provide connectivity with external networks? Choose 2 answers

    1. Elastic IPs (EIP) (Does not provide connectivity, public IP address will do as well)
    2. NAT Gateway (NAT) (Not Attached to VPC and still needs IGW)
    3. **Internet Gateway (IGW)**
    4. **Virtual Private Gateway (VGW)**

27. You are attempting to connect to an instance in Amazon VPC without success You have already verified that the VPC has an Internet Gateway (IGW) the instance has an associated Elastic IP (EIP) and correct security group rules are in place. Which VPC component should you evaluate next?

    1. The configuration of a NAT instance
    2. **The configuration of the Routing Table**
    3. The configuration of the internet Gateway (IGW)
    4. The configuration of SRC/DST checking

28. If you want to launch Amazon Elastic Compute Cloud (EC2) Instances and assign each Instance a predetermined private IP address you should:

    1. Assign a group or sequential Elastic IP address to the instances
    2. Launch the instances in a Placement Group
    3. **Launch the instances in the Amazon virtual Private Cloud (VPC)**
    4. Use standard EC2 instances since each instance gets a private Domain Name Service (DNS) already
    5. Launch the Instance from a private Amazon Machine image (AMI)

29. A user has recently started using EC2. The user launched one EC2 instance in the default subnet in EC2-VPC Which of the below mentioned options is not attached or available with the EC2 instance when it is launched?

    1. Public IP address
    2. Internet gateway
    3. **Elastic IP**
    4. Private IP address

30. A user has created a VPC with CIDR 20.0.0.0/24. The user has created a public subnet with CIDR 20.0.0.0/25. The user is trying to create the private subnet with CIDR 20.0.0.128/25. Which of the below mentioned statements is true in this scenario?

    1. It will not allow the user to create the private subnet due to a CIDR overlap
    2. **It will allow the user to create a private subnet with CIDR as 20.0.0.128/25**
    3. This statement is wrong as AWS does not allow CIDR 20.0.0.0/25
    4. It will not allow the user to create a private subnet due to a wrong CIDR range

31. A user has created a VPC with CIDR 20.0.0.0/16 with only a private subnet and VPN connection using the VPC wizard. The user wants to connect to the instance in a private subnet over SSH. How should the user define the security rule for SSH?

    1. **Allow Inbound traffic on port 22 from the user’s network**
    2. The user has to create an instance in EC2 Classic with an elastic IP and configure the security group of a private subnet to allow SSH from that elastic IP
    3. The user can connect to a instance in a private subnet using the NAT instance
    4. Allow Inbound traffic on port 80 and 22 to allow the user to connect to a private subnet over the Internet

32. A company wants to implement their website in a virtual private cloud (VPC). The web tier will use an Auto Scaling group across multiple Availability Zones (AZs). The database will use Multi-AZ RDS MySQL and should not be publicly accessible. What is the minimum number of subnets that need to be configured in the VPC?

    1. 1
    2. 2
    3. 3
    4. **4** (2 public subnets for web instances in multiple AZs and 2 private subnets for RDS Multi-AZ)

33. Which of the following are characteristics of Amazon VPC subnets? Choose 2 answers

    1. **Each subnet maps to a single Availability Zone**
    2. A CIDR block mask of /25 is the smallest range supported
    3. Instances in a private subnet can communicate with the Internet only if they have an Elastic IP.
    4. **By default, all subnets can route between each other, whether they are private or public**
    5. Each subnet spans at least 2 Availability zones to provide a high-availability environment

34. You need to design a VPC for a web-application consisting of an Elastic Load Balancer (ELB). a fleet of web/application servers, and an RDS database The entire Infrastructure must be distributed over 2 availability zones. Which VPC configuration works while assuring the database is not available from the Internet?

    1. One public subnet for ELB one public subnet for the web-servers, and one private subnet for the database
    2. One public subnet for ELB two private subnets for the web-servers, two private subnets for RDS
    3. **Two public subnets for ELB two private subnets for the web-servers and two private subnets for RDS**
    4. Two public subnets for ELB two public subnets for the web-servers, and two public subnets for RDS

35. You have deployed a three-tier web application in a VPC with a CIDR block of 10.0.0.0/28. You initially deploy two web servers, two application servers, two database servers and one NAT instance tor a total of seven EC2 instances. The web, application and database servers are deployed across two availability zones (AZs). You also deploy an ELB in front of the two web servers, and use Route53 for DNS Web traffic gradually increases in the first few days following the deployment, so you attempt to double the number of instances in each tier of the application to handle the new load unfortunately some of these new instances fail to launch. Which of the following could the root caused? (Choose 2 answers)

     

    [PROFESSIONAL]

    1. The Internet Gateway (IGW) of your VPC has scaled-up adding more instances to handle the traffic spike, reducing the number of available private IP addresses for new instance launches.
    2. AWS reserves one IP address in each subnet’s CIDR block for Route53 so you do not have enough addresses left to launch all of the new EC2 instances.
    3. AWS reserves the first and the last private IP address in each subnet’s CIDR block so you do not have enough addresses left to launch all of the new EC2 instances.
    4. **The ELB has scaled-up. Adding more instances to handle the traffic reducing the number of available private IP addresses for new instance launches**
    5. **AWS reserves the first four and the last IP address in each subnet’s CIDR block so you do not have enough addresses left to launch all of the new EC2 instances.**

36. A user wants to access RDS from an EC2 instance using IP addresses. Both RDS and EC2 are in the same region, but different AZs. Which of the below mentioned options help configure that the instance is accessed faster?

    1. **Configure the Private IP of the Instance in RDS security group** (Recommended as the data is transferred within the the Amazon network and not through internet – Refer [link](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html#USER_WorkingWithSecurityGroups.Authorizing\))
    2. Security group of EC2 allowed in the RDS security group
    3. Configuring the elastic IP of the instance in RDS security group
    4. Configure the Public IP of the instance in RDS security group

37. In regards to VPC, select the correct statement:

    1. **You can associate multiple subnets with the same Route Table.**
    2. You can associate multiple subnets with the same Route Table, but you can’t associate a subnet with only one Route Table.
    3. You can’t associate multiple subnets with the same Route Table.
    4. None of these.

38. You need to design a VPC for a web-application consisting of an ELB a fleet of web application servers, and an RDS DB. The entire infrastructure must be distributed over 2 AZ. Which VPC configuration works while assuring the DB is not available from the Internet?

    1. One Public Subnet for ELB, one Public Subnet for the web-servers, and one private subnet for the DB
    2. One Public Subnet for ELB, two Private Subnets for the web-servers, and two private subnets for the RDS
    3. **Two Public Subnets for ELB, two private Subnet for the web-servers, and two private subnet for the RDS**
    4. Two Public Subnets for ELB, two Public Subnet for the web-servers, and two public subnets for the RDS

39. You have an Amazon VPC with one private subnet and one public subnet with a Network Address Translator (NAT) server. You are creating a group of Amazon Elastic Cloud Compute (EC2) instances that configure themselves at startup via downloading a bootstrapping script from Amazon Simple Storage Service (S3) that deploys an application via GIT. Which setup provides the highest level of security?

    1. **Amazon EC2 instances in private subnet, no EIPs, route outgoing traffic via the NAT**
    2. Amazon EC2 instances in public subnet, no EIPs, route outgoing traffic via the Internet Gateway (IGW)
    3. Amazon EC2 instances in private subnet, assign EIPs, route outgoing traffic via the Internet Gateway (IGW)
    4. Amazon EC2 instances in public subnet, assign EIPs, route outgoing traffic via the NAT

40. You have launched an Amazon Elastic Compute Cloud (EC2) instance into a public subnet with a primary private IP address assigned, an internet gateway is attached to the VPC, and the public route table is configured to send all Internet-based traffic to the Internet gateway. The instance security group is set to allow all outbound traffic but cannot access the Internet. Why is the Internet unreachable from this instance?

    1. **The instance does not have a public IP address**
    2. The Internet gateway security group must allow all outbound traffic.
    3. The instance security group must allow all inbound traffic.
    4. The instance “Source/Destination check” property must be enabled.

41. You have an environment that consists of a public subnet using Amazon VPC and 3 instances that are running in this subnet. These three instances can successfully communicate with other hosts on the Internet. You launch a fourth instance in the same subnet, using the same AMI and security group configuration you used for the others, but find that this instance cannot be accessed from the internet. What should you do to enable Internet access?

    1. Deploy a NAT instance into the public subnet.
    2. **Assign an Elastic IP address to the fourth instance**
    3. Configure a publically routable IP Address in the host OS of the fourth instance.
    4. Modify the routing table for the public subnet.

42. You have a load balancer configured for VPC, and all back-end Amazon EC2 instances are in service. However, your web browser times out when connecting to the load balancer’s DNS name. Which options are probable causes of this behavior? Choose 2 answers

    1. **The load balancer was not configured to use a public subnet with an Internet gateway configured**
    2. The Amazon EC2 instances do not have a dynamically allocated private IP address
    3. **The security groups or network ACLs are not property configured for web traffic.**
    4. The load balancer is not configured in a private subnet with a NAT instance.
    5. The VPC does not have a VGW configured.

43. When will you incur costs with an Elastic IP address (EIP)?

    1. When an EIP is allocated.
    2. When it is allocated and associated with a running instance.
    3. **When it is allocated and associated with a stopped instance.**
    4. Costs are incurred regardless of whether the EIP is associated with a running instance.

44. A company currently has a VPC with EC2 Instances. A new instance being launched, which will host an application that works on IPv6. You need to ensure that this instance can initiate outgoing traffic to the Internet. At the same time, you need to ensure that no incoming connection can be initiated from the Internet on to the instance. Which of the following would you add to the VPC for this requirement?

    1. A NAT Instance
    2. A NAT Gateway
    3. An Internet Gateway
    4. **An egress-only Internet gateway**



---



CloudFront



1. Your company Is moving towards tracking web page users with a small tracking Image loaded on each page Currently you are serving this image out of US-East, but are starting to get concerned about the time It takes to load the image for users on the west coast. What are the two best ways to speed up serving this image? Choose 2 answers

   1. **Use Route 53’s Latency Based Routing and serve the image out of US-West-2 as well as US-East-1**
   2. **Serve the image out through CloudFront**
   3. Serve the image out of S3 so that it isn’t being served oft of your web application tier
   4. Use EBS PIOPs to serve the image faster out of your EC2 instances

2. You deployed your company website using Elastic Beanstalk and you enabled log file rotation to S3. An Elastic Map Reduce job is periodically analyzing the logs on S3 to build a usage dashboard that you share with your CIO. You recently improved overall performance of the website using Cloud Front for dynamic content delivery and your website as the origin. After this architectural change, the usage dashboard shows that the traffic on your website dropped by an order of magnitude. How do you fix your usage dashboard’? 

   **[PROFESSIONAL]**

   1. **Enable CloudFront to deliver access logs to S3 and use them as input of the Elastic Map Reduce job**
   2. Turn on Cloud Trail and use trail log tiles on S3 as input of the Elastic Map Reduce job
   3. Change your log collection process to use Cloud Watch ELB metrics as input of the Elastic Map Reduce job
   4. Use Elastic Beanstalk “Rebuild Environment” option to update log delivery to the Elastic Map Reduce job.
   5. Use Elastic Beanstalk ‘Restart App server(s)” option to update log delivery to the Elastic Map Reduce job.

3. An AWS customer runs a public blogging website. The site users upload two million blog entries a month. The average blog entry size is 200 KB. The access rate to blog entries drops to negligible 6 months after publication and users rarely access a blog entry 1 year after publication. Additionally, blog entries have a high update rate during the first 3 months following publication; this drops to no updates after 6 months. The customer wants to use CloudFront to improve his user’s load times. Which of the following recommendations would you make to the customer? 

   **[PROFESSIONAL]**

   1. Duplicate entries into two different buckets and create two separate CloudFront distributions where S3 access is restricted only to Cloud Front identity
   2. Create a CloudFront distribution with “US & Europe” price class for US/Europe users and a different CloudFront distribution with All Edge Locations for the remaining users.
   3. **Create a CloudFront distribution with S3 access restricted only to the CloudFront identity and partition the blog entry’s location in S3 according to the month it was uploaded to be used with CloudFront behaviors**
   4. Create a CloudFront distribution with Restrict Viewer Access Forward Query string set to true and minimum TTL of 0.

4. Your company has on-premises multi-tier PHP web application, which recently experienced downtime due to a large burst in web traffic due to a company announcement. Over the coming days, you are expecting similar announcements to drive similar unpredictable bursts, and are looking to find ways to quickly improve your infrastructures ability to handle unexpected increases in traffic. The application currently consists of 2 tiers a web tier, which consists of a load balancer, and several Linux Apache web servers as well as a database tier which hosts a Linux server hosting a MySQL database. Which scenario below will provide full site functionality, while helping to improve the ability of your application in the short timeframe required? 

   **[PROFESSIONAL]**

   1. **Offload traffic from on-premises environment Setup a CloudFront distribution and configure CloudFront to cache objects from a custom origin Choose to customize your object cache behavior, and select a TTL that objects should exist in cache.**
   2. Migrate to AWS Use VM Import/Export to quickly convert an on-premises web server to an AMI create an Auto Scaling group, which uses the imported AMI to scale the web tier based on incoming traffic Create an RDS read replica and setup replication between the RDS instance and on-premises MySQL server to migrate the database.
   3. Failover environment: Create an S3 bucket and configure it tor website hosting Migrate your DNS to Route53 using zone (lie import and leverage Route53 DNS failover to failover to the S3 hosted website.
   4. Hybrid environment Create an AMI which can be used of launch web serfers in EC2 Create an Auto Scaling group which uses the * AMI to scale the web tier based on incoming traffic Leverage Elastic Load Balancing to balance traffic between on-premises web servers and those hosted in AWS.

5. You are building a system to distribute confidential training videos to employees. Using CloudFront, what method could be used to serve content that is stored in S3, but not publically accessible from S3 directly?

   1. **Create an Origin Access Identity (OAI) for CloudFront and grant access to the objects in your S3 bucket to that OAI.**
   2. Add the CloudFront account security group “amazon-cf/amazon-cf-sg” to the appropriate S3 bucket policy.
   3. Create an Identity and Access Management (IAM) User for CloudFront and grant access to the objects in your S3 bucket to that IAM User.
   4. Create a S3 bucket policy that lists the CloudFront distribution ID as the Principal and the target bucket as the Amazon Resource Name (ARN).

6. A media production company wants to deliver high-definition raw video for preproduction and dubbing to customer all around the world. They would like to use Amazon CloudFront for their scenario, and they require the ability to limit downloads per customer and video file to a configurable number. A CloudFront download distribution with TTL=0 was already setup to make sure all client HTTP requests hit an authentication backend on Amazon Elastic Compute Cloud (EC2)/Amazon RDS first, which is responsible for restricting the number of downloads. Content is stored in S3 and configured to be accessible only via CloudFront. What else needs to be done to achieve an architecture that meets the requirements? Choose 2 answers 

   **[PROFESSIONAL]**

   1. **Enable URL parameter forwarding, let the authentication backend count the number of downloads per customer in RDS, and return the content S3 URL unless the download limit is reached.**
   2. Enable CloudFront logging into an S3 bucket, leverage EMR to analyze CloudFront logs to determine the number of downloads per customer, and return the content S3 URL unless the download limit is reached. (CloudFront logs are logged periodically and EMR not being real time, hence not suitable)
   3. Enable URL parameter forwarding, let the authentication backend count the number of downloads per customer in RDS, and invalidate the CloudFront distribution as soon as the download limit is reached. (Distribution are not invalidated but Objects)
   4. Enable CloudFront logging into the S3 bucket, let the authentication backend determine the number of downloads per customer by parsing those logs, and return the content S3 URL unless the download limit is reached. (CloudFront logs are logged periodically and EMR not being real time, hence not suitable)
   5. **Configure a list of trusted signers, let the authentication backend count the number of download requests per customer in RDS, and return a dynamically signed URL unless the download limit is reached.**

7. Your customer is implementing a video on-demand streaming platform on AWS. The requirements are to support for multiple devices such as iOS, Android, and PC as client devices, using a standard client player, using streaming technology (not download) and scalable architecture with cost effectiveness 

   **[PROFESSIONAL]**

   1. Store the video contents to Amazon Simple Storage Service (S3) as an origin server. Configure the Amazon CloudFront distribution with a streaming option to stream the video contents
   2. **Store the video contents to Amazon S3 as an origin server. Configure the Amazon CloudFront distribution with a download option to stream the video contents** (Refer [link](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web.html))
   3. Launch a streaming server on Amazon Elastic Compute Cloud (EC2) (for example, Adobe Media Server), and store the video contents as an origin server. Configure the Amazon CloudFront distribution with a download option to stream the video contents
   4. Launch a streaming server on Amazon Elastic Compute Cloud (EC2) (for example, Adobe Media Server), and store the video contents as an origin server. Launch and configure the required amount of streaming servers on Amazon EC2 as an edge server to stream the video contents

8. You are an architect for a news -sharing mobile application. Anywhere in the world, your users can see local news on of topics they choose. They can post pictures and videos from inside the application. Since the application is being used on a mobile phone, connection stability is required for uploading content, and delivery should be quick. Content is accessed a lot in the first minutes after it has been posted, but is quickly replaced by new content before disappearing. The local nature of the news means that 90 percent of the uploaded content is then read locally (less than a hundred kilometers from where it was posted). What solution will optimize the user experience when users upload and view content (by minimizing page load times and minimizing upload times)? 

   **[PROFESSIONAL]**

   1. Upload and store the content in a central Amazon Simple Storage Service (S3) bucket, and use an Amazon Cloud Front Distribution for content delivery.
   2. Upload and store the content in an Amazon Simple Storage Service (S3) bucket in the region closest to the user, and use multiple Amazon Cloud Front distributions for content delivery.
   3. Upload the content to an Amazon Elastic Compute Cloud (EC2) instance in the region closest to the user, send the content to a central Amazon Simple Storage Service (S3) bucket, and use an Amazon Cloud Front distribution for content delivery.
   4. **Use an Amazon Cloud Front distribution for uploading the content to a central Amazon Simple Storage Service (S3) bucket and for content delivery.**

9. To enable end-to-end HTTPS connections from the user‘s browser to the origin via CloudFront, which of the following options are valid? Choose 2 answers 

   **[PROFESSIONAL]**

   1. Use self signed certificate in the origin and CloudFront default certificate in CloudFront. (Origin cannot be self signed)
   2. Use the CloudFront default certificate in both origin and CloudFront (CloudFront cert cannot be applied to origin)
   3. **Use 3rd-party CA certificate in the origin and CloudFront default certificate in CloudFront**
   4. **Use 3rd-party CA certificate in both origin and CloudFront**
   5. Use a self signed certificate in both the origin and CloudFront (Origin cannot be self signed)

10. Your application consists of 10% writes and 90% reads. You currently service all requests through a Route53 Alias Record directed towards an AWS ELB, which sits in front of an EC2 Auto Scaling Group. Your system is getting very expensive when there are large traffic spikes during certain news events, during which many more people request to read similar data all at the same time. What is the simplest and cheapest way to reduce costs and scale with spikes like this? 

    **[PROFESSIONAL]**

    1. Create an S3 bucket and asynchronously replicate common requests responses into S3 objects. When a request comes in for a precomputed response, redirect to AWS S3
    2. Create another ELB and Auto Scaling Group layer mounted on top of the other system, adding a tier to the system. Serve most read requests out of the top layer
    3. **Create a CloudFront Distribution and direct Route53 to the Distribution. Use the ELB as an Origin and specify Cache Behaviors to proxy cache requests, which can be served late.** (CloudFront can server request from cache and multiple cache behavior can be defined based on rules for a given URL pattern based on file extensions, file names, or any portion of a URL. Each cache behavior can include the CloudFront configuration values: origin server name, viewer connection protocol, minimum expiration period, query string parameters, cookies, and trusted signers for private content.)
    4. Create a Memcached cluster in AWS ElastiCache. Create cache logic to serve requests, which can be served late from the in-memory cache for increased performance.

11. You are designing a service that aggregates clickstream data in batch and delivers reports to subscribers via email only once per week. Data is extremely spikey, geographically distributed, high-scale, and unpredictable. How should you design this system?

    1. Use a large RedShift cluster to perform the analysis, and a fleet of Lambdas to perform record inserts into the RedShift tables. Lambda will scale rapidly enough for the traffic spikes.
    2. **Use a CloudFront distribution with access log delivery to S3. Clicks should be recorded as query string GETs to the distribution. Reports are built and sent by periodically running EMR jobs over the access logs in S3.** (CloudFront is a Gigabit-Scale HTTP(S) global request distribution service and works fine with peaks higher than 10 Gbps or 15,000 RPS. It can handle scale, geo-spread, spikes, and unpredictability. Access Logs will contain the GET data and work just fine for batch analysis and email using EMR. Other streaming options are expensive as not required as the need is to batch analyze)
    3. Use API Gateway invoking Lambdas which PutRecords into Kinesis, and EMR running Spark performing GetRecords on Kinesis to scale with spikes. Spark on EMR outputs the analysis to S3, which are sent out via email.
    4. Use AWS Elasticsearch service and EC2 Auto Scaling groups. The Autoscaling groups scale based on click throughput and stream into the Elasticsearch domain, which is also scalable. Use Kibana to generate reports periodically.

12. Your website is serving on-demand training videos to your workforce. Videos are uploaded monthly in high resolution MP4 format. Your workforce is distributed globally often on the move and using company-provided tablets that require the HTTP Live Streaming (HLS) protocol to watch a video. Your company has no video transcoding expertise and it required you might need to pay for a consultant. How do you implement the most cost-efficient architecture without compromising high availability and quality of video delivery?

     

    **[PROFESSIONAL]**

    1. **Elastic Transcoder to transcode original high-resolution MP4 videos to HLS. S3 to host videos with lifecycle Management to archive original flies to Glacier after a few days. CloudFront to serve HLS transcoded videos from S3**
    2. A video transcoding pipeline running on EC2 using SQS to distribute tasks and Auto Scaling to adjust the number or nodes depending on the length of the queue S3 to host videos with Lifecycle Management to archive all files to Glacier after a few days CloudFront to serve HLS transcoding videos from Glacier
    3. Elastic Transcoder to transcode original high-resolution MP4 videos to HLS EBS volumes to host videos and EBS snapshots to incrementally backup original rues after a few days. CloudFront to serve HLS transcoded videos from EC2.
    4. A video transcoding pipeline running on EC2 using SQS to distribute tasks and Auto Scaling to adjust the number of nodes depending on the length of the queue. EBS volumes to host videos and EBS snapshots to incrementally backup original files after a few days. CloudFront to serve HLS transcoded videos from EC2



---



Route 53 



1. What does Amazon Route53 provide?
   1. A global Content Delivery Network.
   2. None of these.
   3. **A scalable Domain Name System**
   4. An SSH endpoint for Amazon EC2.
2. Does Amazon Route 53 support NS Records?
   1. Yes, it supports Name Service records.
   2. No
   3. It supports only MX records.
   4. **Yes, it supports Name Server records.** 
3. Does Route 53 support MX Records?
   1. **Yes**
   2. It supports CNAME records, but not MX records.
   3. No
   4. Only Primary MX records. Secondary MX records are not supported.
4. Which of the following statements are true about Amazon Route 53 resource records? Choose 2 answers
   1. **An Alias record can map one DNS name to another Amazon Route 53 DNS name.**
   2. A CNAME record can be created for your zone apex.
   3. **An Amazon Route 53 CNAME record can point to any DNS record hosted anywhere.**
   4. TTL can be set for an Alias record in Amazon Route 53.
   5. An Amazon Route 53 Alias record can point to any DNS record hosted anywhere.
5. Which statements are true about Amazon Route 53? (Choose 2 answers)
   1. Amazon Route 53 is a region-level service
   2. **You can register your domain name**
   3. **Amazon Route 53 can perform health checks and failovers to a backup site in the even of the primary site failure**
   4. Amazon Route 53 only supports Latency-based routing
6. A customer is hosting their company website on a cluster of web servers that are behind a public-facing load balancer. The customer also uses Amazon Route 53 to manage their public DNS. How should the customer configure the DNS zone apex record to point to the load balancer?
   1. Create an A record pointing to the IP address of the load balancer
   2. Create a CNAME record pointing to the load balancer DNS name.
   3. Create a CNAME record aliased to the load balancer DNS name.
   4. **Create an A record aliased to the load balancer DNS name**
7. A user has configured ELB with three instances. The user wants to achieve High Availability as well as redundancy with ELB. Which of the below mentioned AWS services helps the user achieve this for ELB?
   1. **Route 53**
   2. AWS Mechanical Turk
   3. Auto Scaling
   4. AWS EMR
8. How can the domain’s zone apex for example “myzoneapexdomain com” be pointed towards an Elastic Load Balancer?
   1. By using an AAAA record
   2. By using an A record
   3. By using an Amazon Route 53 CNAME record
   4. **By using an Amazon Route 53 Alias record**
9. You need to create a simple, holistic check for your system’s general availability and uptime. Your system presents itself as an HTTP-speaking API. What is the simplest tool on AWS to achieve this with?
   1. **Route53 Health Checks** (Refer [link](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-determining-health-of-endpoints.html))
   2. CloudWatch Health Checks
   3. AWS ELB Health Checks
   4. EC2 Health Checks
10. Your organization’s corporate website must be available on www.acme.com and acme.com. How should you configure Amazon Route 53 to meet this requirement?
    1. **Configure acme.com with an ALIAS record targeting the ELB. www.acme.com with an ALIAS record targeting the ELB.**
    2. Configure acme.com with an A record targeting the ELB. www.acme.com with a CNAME record targeting the acme.com record.
    3. Configure acme.com with a CNAME record targeting the ELB. www.acme.com with a CNAME record targeting the acme.com record.
    4. Configure acme.com using a second ALIAS record with the ELB target. www.acme.com using a PTR record with the acme.com record target.



---



Direct Connect



1. You are building a solution for a customer to extend their on-premises data center to AWS. The customer requires a 50-Mbps dedicated and private connection to their VPC. Which AWS product or feature satisfies this requirement?
   1. Amazon VPC peering
   2. Elastic IP Addresses
   3. **AWS Direct Connect**
   4. Amazon VPC virtual private gateway
2. Is there any way to own a direct connection to Amazon Web Services?
   1. You can create an encrypted tunnel to VPC, but you don’t own the connection.
   2. Yes, it’s called Amazon Dedicated Connection.
   3. No, AWS only allows access from the public Internet.
   4. **Yes, it’s called Direct Connect**
3. An organization has established an Internet-based VPN connection between their on-premises data center and AWS. They are considering migrating from VPN to AWS Direct Connect. Which operational concern should drive an organization to consider switching from an Internet-based VPN connection to AWS Direct Connect?
   1. AWS Direct Connect provides greater redundancy than an Internet-based VPN connection.
   2. AWS Direct Connect provides greater resiliency than an Internet-based VPN connection.
   3. **AWS Direct Connect provides greater bandwidth than an Internet-based VPN connection.**
   4. AWS Direct Connect provides greater control of network provider selection than an Internet-based VPN connection.
4. Does AWS Direct Connect allow you access to all Availabilities Zones within a Region?
   1. Depends on the type of connection
   2. No
   3. **Yes**
   4. Only when there’s just one availability zone in a region. If there are more than one, only one availability zone can be accessed directly.
5. A customer has established an AWS Direct Connect connection to AWS. The link is up and routes are being advertised from the customer’s end, however the customer is unable to connect from EC2 instances inside its VPC to servers residing in its datacenter. Which of the following options provide a viable solution to remedy this situation? (Choose 2 answers)
   1. Add a route to the route table with an IPSec VPN connection as the target (deals with VPN)
   2. **Enable route propagation to the Virtual Private Gateway (VGW)**
   3. Enable route propagation to the customer gateway (CGW) (route propagation is enabled on VGW)
   4. Modify the route table of all Instances using the ‘route’ command. (no route command available)
   5. **Modify the Instances VPC subnet route table by adding a route back to the customer’s on-premises environment.**
6. A company has configured and peered two VPCs: VPC-1 and VPC-2. VPC-1 contains only private subnets, and VPC-2 contains only public subnets. The company uses a single AWS Direct Connect connection and private virtual interface to connect their on-premises network with VPC-1. Which two methods increase the fault tolerance of the connection to VPC-1? Choose 2 answers
   1. Establish a hardware VPN over the internet between VPC-2 and the on-premises network. (Peered VPC does not support Edge to Edge Routing)
   2. **Establish a hardware VPN over the internet between VPC-1 and the on-premises network**
   3. Establish a new AWS Direct Connect connection and private virtual interface in the same region as VPC-2 (Peered VPC does not support Edge to Edge Routing)
   4. Establish a new AWS Direct Connect connection and private virtual interface in a different AWS region than VPC-1 (need to be in the same region as VPC-1)
   5. **Establish a new AWS Direct Connect connection and private virtual interface in the same AWS region as VPC-1**
7. Your company previously configured a heavily used, dynamically routed VPN connection between your on premises data center and AWS. You recently provisioned a Direct Connect connection and would like to start using the new connection. After configuring Direct Connect settings in the AWS Console, which of the following options will provide the most seamless transition for your users?
   1. Delete your existing VPN connection to avoid routing loops configure your Direct Connect router with the appropriate settings and verity network traffic is leveraging Direct Connect.
   2. Configure your Direct Connect router with a higher BGP priority than your VPN router, verify network traffic is leveraging Direct Connect and then delete your existing VPN connection.
   3. **Update your VPC route tables to point to the Direct Connect connection configure your Direct Connect router with the appropriate settings verify network traffic is leveraging Direct Connect and then delete the VPN connection.**
   4. Configure your Direct Connect router, update your VPC route tables to point to the Direct Connect connection, configure your VPN connection with a higher BGP priority. And verify network traffic is leveraging the Direct Connect connection
8. You are designing the network infrastructure for an application server in Amazon VPC. Users will access all the application instances from the Internet as well as from an on-premises network The on-premises network is connected to your VPC over an AWS Direct Connect link. How would you design routing to meet the above requirements?
   1. Configure a single routing Table with a default route via the Internet gateway. Propagate a default route via BGP on the AWS Direct Connect customer router. Associate the routing table with all VPC subnets (propagating default route would cause conflict)
   2. **Configure a single routing table with a default route via the internet gateway. Propagate specific routes for the on-premises networks via BGP on the AWS Direct Connect customer router. Associate the routing table with all VPC subnets.**
   3. Configure a single routing table with two default routes: one to the internet via an Internet gateway the other to the on-premises network via the VPN gateway use this routing table across all subnets in your VPC. (there cannot be 2 default routes)
   4. Configure two routing tables one that has a default route via the Internet gateway and another that has a default route via the VPN gateway Associate both routing tables with each VPC subnet. (as the instances has to be in public subnet and should have a single routing table associated with them)
9. You are implementing AWS Direct Connect. You intend to use AWS public service end points such as Amazon S3, across the AWS Direct Connect link. You want other Internet traffic to use your existing link to an Internet Service Provider. What is the correct way to configure AWS Direct Connect for access to services such as Amazon S3?
   1. Configure a public Interface on your AWS Direct Connect link. Configure a static route via your AWS Direct Connect link that points to Amazon S3. Advertise a default route to AWS using BGP.
   2. Create a private interface on your AWS Direct Connect link. Configure a static route via your AWS Direct connect link that points to Amazon S3 Configure specific routes to your network in your VPC.
   3. **Create a public interface on your AWS Direct Connect link. Redistribute BGP routes into your existing routing infrastructure advertise specific routes for your network to AWS**
   4. Create a private interface on your AWS Direct connect link. Redistribute BGP routes into your existing routing infrastructure and advertise a default route to AWS.
10. You have been asked to design network connectivity between your existing data centers and AWS. Your application’s EC2 instances must be able to connect to existing backend resources located in your data center. Network traffic between AWS and your data centers will start small, but ramp up to 10s of GB per second over the course of several months. The success of your application is dependent upon getting to market quickly. Which of the following design options will allow you to meet your objectives?
    1. Quickly create an internal ELB for your backend applications, submit a DirectConnect request to provision a 1 Gbps cross connect between your data center and VPC, then increase the number or size of your DirectConnect connections as needed.
    2. Allocate EIPs and an Internet Gateway for your VPC instances to use for quick, temporary access to your backend applications, then provision a VPN connection between a VPC and existing on -premises equipment.
    3. **Provision a VPN connection between a VPC and existing on -premises equipment, submit a DirectConnect partner request to provision cross connects between your data center and the DirectConnect location, then cut over from the VPN connection to one or more DirectConnect connections as needed.**
    4. Quickly submit a DirectConnect request to provision a 1 Gbps cross connect between your data center and VPC, then increase the number or size of your DirectConnect connections as needed.
11. You are tasked with moving a legacy application from a virtual machine running inside your datacenter to an Amazon VPC. Unfortunately this app requires access to a number of on-premises services and no one who configured the app still works for your company. Even worse there’s no documentation for it. What will allow the application running inside the VPC to reach back and access its internal dependencies without being reconfigured? (Choose 3 answers)
    1. **An AWS Direct Connect link between the VPC and the network housing the internal services** (VPN or a DX for communication)
    2. An Internet Gateway to allow a VPN connection. (Virtual and Customer gateway is needed)
    3. An Elastic IP address on the VPC instance (Don’t need a EIP as private subnets can also interact with on-premises network)
    4. **An IP address space that does not conflict with the one on-premises** (IP address cannot conflict)
    5. Entries in Amazon Route 53 that allow the Instance to resolve its dependencies’ IP addresses (Route 53 is not required)
    6. **A VM Import of the current virtual machine** (VM Import to copy the VM to AWS as there is no documentation it can’t be configured from scratch)



---



ELB



1. A user has configured an HTTPS listener on an ELB. The user has not configured any security policy which can help to negotiate SSL between the client and ELB. What will ELB do in this scenario?
   1. By default ELB will select the first version of the security policy
   2. **By default ELB will select the latest version of the policy**
   3. ELB creation will fail without a security policy
   4. It is not required to have a security policy since SSL is already installed
2. A user has configured ELB with SSL using a security policy for secure negotiation between the client and load balancer. The ELB security policy supports various ciphers. Which of the below mentioned options helps identify the matching cipher at the client side to the ELB cipher list when client is requesting ELB DNS over SSL
   1. Cipher Protocol
   2. Client Configuration Preference
   3. **Server Order Preference**
   4. Load Balancer Preference
3. A user has configured ELB with SSL using a security policy for secure negotiation between the client and load balancer. Which of the below mentioned security policies is supported by ELB?
   1. Dynamic Security Policy
   2. All the other options
   3. **Predefined Security Policy**
   4. Default Security Policy
4. A user has configured ELB with SSL using a security policy for secure negotiation between the client and load balancer. Which of the below mentioned SSL protocols is not supported by the security policy?
   1. **TLS 1.3**
   2. TLS 1.2
   3. SSL 2.0
   4. SSL 3.0
5. A user has configured ELB with a TCP listener at ELB as well as on the back-end instances. The user wants to enable a proxy protocol to capture the source and destination IP information in the header. Which of the below mentioned statements helps the user understand a proxy protocol with TCP configuration?
   1. **If the end user is requesting behind a proxy server then the user should not enable a proxy protocol on ELB**
   2. ELB does not support a proxy protocol when it is listening on both the load balancer and the back-end instances
   3. Whether the end user is requesting from a proxy server or directly, it does not make a difference for the proxy protocol
   4. If the end user is requesting behind the proxy then the user should add the “isproxy” flag to the ELB Configuration
6. A user has enabled session stickiness with ELB. The user does not want ELB to manage the cookie; instead he wants the application to manage the cookie. What will happen when the server instance, which is bound to a cookie, crashes?
   1. The response will have a cookie but stickiness will be deleted
   2. **The session will not be sticky until a new cookie is inserted**
   3. ELB will throw an error due to cookie unavailability
   4. The session will be sticky and ELB will route requests to another server as ELB keeps replicating the Cookie
7. A user has created an ELB with Auto Scaling. Which of the below mentioned offerings from ELB helps the user to stop sending new requests traffic from the load balancer to the EC2 instance when the instance is being deregistered while continuing in-flight requests?
   1. ELB sticky session
   2. ELB deregistration check
   3. **ELB connection draining**
   4. ELB auto registration Off
8. When using an Elastic Load Balancer to serve traffic to web servers, which one of the following is true?
   1. Web servers must be publicly accessible
   2. The same security group must be applied to both the ELB and EC2 instances
   3. ELB and EC2 instance must be in the same subnet
   4. **ELB and EC2 instances must be in the same VPC**
9. A user has configured Elastic Load Balancing by enabling a Secure Socket Layer (SSL) negotiation configuration known as a Security Policy. Which of the below mentioned options is not part of this secure policy while negotiating the SSL connection between the user and the client?
   1. SSL Protocols
   2. **Client Order Preference**
   3. SSL Ciphers
   4. Server Order Preference
10. A user has created an ELB with the availability zone us-east-1. The user wants to add more zones to ELB to achieve High Availability. How can the user add more zones to the existing ELB?
    1. It is not possible to add more zones to the existing ELB
    2. Only option is to launch instances in different zones and add to ELB
    3. The user should stop the ELB and add zones and instances as required
    4. **The user can add zones on the fly from the AWS console**
11. A user has launched an ELB which has 5 instances registered with it. The user deletes the ELB by mistake. What will happen to the instances?
    1. ELB will ask the user whether to delete the instances or not
    2. Instances will be terminated
    3. ELB cannot be deleted if it has running instances registered with it
    4. **Instances will keep running**
12. A Sys-admin has created a shopping cart application and hosted it on EC2. The EC2 instances are running behind ELB. The admin wants to ensure that the end user request will always go to the EC2 instance where the user session has been created. How can the admin configure this?
    1. Enable ELB cross zone load balancing
    2. Enable ELB cookie setup
    3. **Enable ELB sticky session**
    4. Enable ELB connection draining
13. A user has setup connection draining with ELB to allow in-flight requests to continue while the instance is being deregistered through Auto Scaling. If the user has not specified the draining time, how long will ELB allow inflight requests traffic to continue?
    1. 600 seconds
    2. 3600 seconds
    3. **300 seconds**
    4. 0 seconds
14. A customer has a web application that uses cookie Based sessions to track logged in users. It is deployed on AWS using ELB and Auto Scaling. The customer observes that when load increases Auto Scaling launches new Instances but the load on the existing Instances does not decrease, causing all existing users to have a sluggish experience. Which two answer choices independently describe a behavior that could be the cause of the sluggish user experience?
    1. ELB’s normal behavior sends requests from the same user to the same backend instance (its not by default)
    2. **ELB’s behavior when sticky sessions are enabled causes ELB to send requests in the same session to the same backend** 
    3. A faulty browser is not honoring the TTL of the ELB DNS name (DNS TTL would only impact the ELB instances if scaled and not the EC2 instances to which the traffic is routed)
    4. **The web application uses long polling such as comet or websockets. Thereby keeping a connection open to a web server tor a long time**
15. A customer has an online store that uses the cookie-based sessions to track logged-in customers. It is deployed on AWS using ELB and autoscaling. When the load increases, Auto scaling automatically launches new web servers, but the load on the web servers do not decrease. This causes the customers a poor experience. What could be causing the issue ?
    1. ELB DNS records Time to Live is set too high (DNS TTL would only impact the ELB instances if scaled and not the EC2 instances to which the traffic is routed)
    2. **ELB is configured to send requests with previously established sessions**
    3. Website uses CloudFront which is keeping sessions alive
    4. New Instances are not being added to the ELB during the Auto Scaling cool down period
16. You are designing a multi-platform web application for AWS. The application will run on EC2 instances and will be accessed from PCs, tablets and smart phones. Supported accessing platforms are Windows, MACOS, IOS and Android. Separate sticky session and SSL certificate setups are required for different platform types. Which of the following describes the most cost effective and performance efficient architecture setup?
    1. Setup a hybrid architecture to handle session state and SSL certificates on-prem and separate EC2 Instance groups running web applications for different platform types running in a VPC.
    2. Set up one ELB for all platforms to distribute load among multiple instance under it. Each EC2 instance implements all functionality for a particular platform.
    3. Set up two ELBs. The first ELB handles SSL certificates for all platforms and the second ELB handles session stickiness for all platforms for each ELB run separate EC2 instance groups to handle the web application for each platform.
    4. **Assign multiple ELBs to an EC2 instance or group of EC2 instances running the common components of the web application, one ELB for each platform type. Session stickiness and SSL termination are done at the ELBs.** (Session stickiness requires HTTPS listener with SSL termination on the ELB and ELB does not support multiple SSL certs so one is required for each cert)
17. You are migrating a legacy client-server application to AWS. The application responds to a specific DNS domain (e.g. www.example.com) and has a 2-tier architecture, with multiple application servers and a database server. Remote clients use TCP to connect to the application servers. The application servers need to know the IP address of the clients in order to function properly and are currently taking that information from the TCP socket. A Multi-AZ RDS MySQL instance will be used for the database. During the migration you can change the application code but you have to file a change request. How would you implement the architecture on AWS in order to maximize scalability and high availability?
    1. **File a change request to implement Proxy Protocol support In the application. Use an ELB with a TCP Listener and Proxy Protocol enabled to distribute load on two application servers in different AZs.** (ELB with TCP listener and proxy protocol will allow IP to be passed )**
       **
    2. File a change request to Implement Cross-Zone support in the application. Use an ELB with a TCP Listener and Cross-Zone Load Balancing enabled, two application servers in different AZs.
    3. File a change request to implement Latency Based Routing support in the application. Use Route 53 with Latency Based Routing enabled to distribute load on two application servers in different AZs.
    4. File a change request to implement Alias Resource support in the application Use Route 53 Alias Resource Record to distribute load on two application servers in different AZs.
18. A user has created an ELB with three instances. How many security groups will ELB create by default?
    1. 3
    2. 5
    3. **2** (One for ELB to allow inbound and Outbound to listener and health check port of instances and One for the Instances to allow inbound from ELB)
    4. 1
19. You have a web-style application with a stateless but CPU and memory-intensive web tier running on a cc2 8xlarge EC2 instance inside of a VPC The instance when under load is having problems returning requests within the SLA as defined by your business The application maintains its state in a DynamoDB table, but the data tier is properly provisioned and responses are consistently fast. How can you best resolve the issue of the application responses not meeting your SLA?
    1. **Add another cc2 8xlarge application instance, and put both behind an Elastic Load Balancer**
    2. Move the cc2 8xlarge to the same Availability Zone as the DynamoDB table (Does not improve the response time and performance)
    3. Cache the database responses in ElastiCache for more rapid access (Data tier is responding fast)
    4. Move the database from DynamoDB to RDS MySQL in scale-out read-replica configuration (Data tier is responding fast)
20. An organization has configured a VPC with an Internet Gateway (IGW). pairs of public and private subnets (each with one subnet per Availability Zone), and an Elastic Load Balancer (ELB) configured to use the public subnets. The applications web tier leverages the ELB, Auto Scaling and a Multi-AZ RDS database instance. The organization would like to eliminate any potential single points of failure in this design. What step should you take to achieve this organization’s objective?
    1. **Nothing, there are no single points of failure in this architecture.**
    2. Create and attach a second IGW to provide redundant internet connectivity. (VPC can be attached only 1 IGW)
    3. Create and configure a second Elastic Load Balancer to provide a redundant load balancer. (ELB scales by itself with multiple availability zones configured with it)
    4. Create a second multi-AZ RDS instance in another Availability Zone and configure replication to provide a redundant database. (Multi AZ requires 2 different AZ for setup and already has a standby)
21. Your application currently leverages AWS Auto Scaling to grow and shrink as load Increases/ decreases and has been performing well. Your marketing team expects a steady ramp up in traffic to follow an upcoming campaign that will result in a 20x growth in traffic over 4 weeks. Your forecast for the approximate number of Amazon EC2 instances necessary to meet the peak demand is 175. What should you do to avoid potential service disruptions during the ramp up in traffic?
    1. Ensure that you have pre-allocated 175 Elastic IP addresses so that each server will be able to obtain one as it launches (max limit 5 EIP and a service request needs to be submitted)
    2. **Check the service limits in Trusted Advisor and adjust as necessary so the forecasted count remains within limits.**
    3. Change your Auto Scaling configuration to set a desired capacity of 175 prior to the launch of the marketing campaign (Will cause 175 instances to be launched and running but not gradually scale)
    4. Pre-warm your Elastic Load Balancer to match the requests per second anticipated during peak demand (Does not need pre warming as the load is increasing steadily)
22. Which of the following features ensures even distribution of traffic to Amazon EC2 instances in multiple Availability Zones registered with a load balancer?
    1. Elastic Load Balancing request routing
    2. An Amazon Route 53 weighted routing policy (does not control traffic to EC2 instance)
    3. **Elastic Load Balancing cross-zone load balancing**
    4. An Amazon Route 53 latency routing policy (does not control traffic to EC2 instance)
23. Your web application front end consists of multiple EC2 instances behind an Elastic Load Balancer. You configured ELB to perform health checks on these EC2 instances, if an instance fails to pass health checks, which statement will be true?
    1. The instance gets terminated automatically by the ELB (it is done by Autoscaling)
    2. The instance gets quarantined by the ELB for root cause analysis.
    3. The instance is replaced automatically by the ELB. (it is done by Autoscaling)
    4. **The ELB stops sending traffic to the instance that failed its health check**
24. You have a web application running on six Amazon EC2 instances, consuming about 45% of resources on each instance. You are using auto-scaling to make sure that six instances are running at all times. The number of requests this application processes is consistent and does not experience spikes. The application is critical to your business and you want high availability at all times. You want the load to be distributed evenly between all instances. You also want to use the same Amazon Machine Image (AMI) for all instances. Which of the following architectural choices should you make?
    1. Deploy 6 EC2 instances in one availability zone and use Amazon Elastic Load Balancer. (Single AZ will not provide High Availability)
    2. Deploy 3 EC2 instances in one region and 3 in another region and use Amazon Elastic Load Balancer. (Different region, AMI would not be available unless copied)
    3. **Deploy 3 EC2 instances in one availability zone and 3 in another availability zone and use Amazon Elastic Load Balancer.**
    4. Deploy 2 EC2 instances in three regions and use Amazon Elastic Load Balancer. (Different region, AMI would not be available unless copied)
25. You are designing an SSL/TLS solution that requires HTTPS clients to be authenticated by the Web server using client certificate authentication. The solution must be resilient. Which of the following options would you consider for configuring the web server infrastructure? (Choose 2 answers)
    1. **Configure ELB with TCP listeners on TCP/443. And place the Web servers behind it.** (terminate SSL on the instance using client-side certificate)
    2. **Configure your Web servers with EIPs. Place the Web servers in a Route53 Record Set and configure health checks against all Web servers.** (Remove ELB and use Web Servers directly with Route 53)
    3. Configure ELB with HTTPS listeners, and place the Web servers behind it. (ELB with HTTPs does not support Client-Side certificates)
    4. Configure your web servers as the origins for a CloudFront distribution. Use custom SSL certificates on your CloudFront distribution (CloudFront [does not](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html#RequestCustomClientSideSslAuth) Client-Side ssl certificates)
26. You are designing an application that contains protected health information. Security and compliance requirements for your application mandate that all protected health information in the application use encryption at rest and in transit. The application uses a three-tier architecture where data flows through the load balancer and is stored on Amazon EBS volumes for processing, and the results are stored in Amazon S3 using the AWS SDK. Which of the following two options satisfy the security requirements? Choose 2 answers
    1. Use SSL termination on the load balancer, Amazon EBS encryption on Amazon EC2 instances, and Amazon S3 with server-side encryption. (connection between ELB and EC2 not encrypted)
    2. Use SSL termination with a SAN SSL certificate on the load balancer, Amazon EC2 with all Amazon EBS volumes using Amazon EBS encryption, and Amazon S3 with server-side encryption with customer-managed keys.
    3. **Use TCP load balancing on the load balancer, SSL termination on the Amazon EC2 instances, OS-level disk encryption on the Amazon EBS volumes, and Amazon S3 with server-side encryption.**
    4. Use TCP load balancing on the load balancer, SSL termination on the Amazon EC2 instances, and Amazon S3 with server-side encryption. (Does not mention EBS encryption)
    5. **Use SSL termination on the load balancer, an SSL listener on the Amazon EC2 instances, Amazon EBS encryption on EBS volumes containing PHI, and Amazon S3 with server-side encryption.**
27. A startup deploys its photo-sharing site in a VPC. An elastic load balancer distributes web traffic across two subnets. The load balancer session stickiness is configured to use the AWS-generated session cookie, with a session TTL of 5 minutes. The web server Auto Scaling group is configured as min-size=4, max-size=4. The startup is preparing for a public launch, by running load-testing software installed on a single Amazon Elastic Compute Cloud (EC2) instance running in us-west-2a. After 60 minutes of load-testing, the web server logs show the following:WEBSERVER LOGS | # of HTTP requests from load-tester | # of HTTP requests from private beta users || webserver #1 (subnet in us-west-2a): | 19,210 | 434 || webserver #2 (subnet in us-west-2a): | 21,790 | 490 || webserver #3 (subnet in us-west-2b): | 0 | 410 || webserver #4 (subnet in us-west-2b): | 0 | 428 |Which recommendations can help ensure that load-testing HTTP requests are evenly distributed across the four web servers? Choose 2 answers
    1. Launch and run the load-tester Amazon EC2 instance from us-east-1 instead.
    2. Configure Elastic Load Balancing session stickiness to use the app-specific session cookie.
    3. **Re-configure the load-testing software to re-resolve DNS for each web request.** (Refer [link](https://aws.amazon.com/articles/1636185810492479))
    4. Configure Elastic Load Balancing and Auto Scaling to distribute across us-west-2a and us-west-2b.
    5. **Use a third-party load-testing service which offers globally distributed test clients.** (Refer [link](https://aws.amazon.com/articles/1636185810492479))
28. To serve Web traffic for a popular product your chief financial officer and IT director have purchased 10 m1.large heavy utilization Reserved Instances (RIs) evenly spread across two availability zones: Route 53 is used to deliver the traffic to an Elastic Load Balancer (ELB). After several months, the product grows even more popular and you need additional capacity As a result, your company purchases two c3.2xlarge medium utilization RIs You register the two c3.2xlarge instances with your ELB and quickly find that the ml large instances are at 100% of capacity and the c3.2xlarge instances have significant capacity that’s unused Which option is the most cost effective and uses EC2 capacity most effectively?
    1. **Use a separate ELB for each instance type and distribute load to ELBs with Route 53 weighted round robin**
    2. Configure Autoscaling group and Launch Configuration with ELB to add up to 10 more on-demand mi large instances when triggered by CloudWatch shut off c3.2xlarge instances (increase cost as you still pay for the RI)
    3. Route traffic to EC2 m1.large and c3.2xlarge instances directly using Route 53 latency based routing and health checks shut off ELB (will not still use the capacity effectively)
    4. Configure ELB with two c3.2xlarge Instances and use on-demand Autoscailng group for up to two additional c3.2xlarge instances Shut on m1.large instances(Increases cost, as you still pay for the 10 m1.large RI)
29. Which header received at the EC2 instance identifies the port used by the client while requesting ELB?
    1. X-Forwarded-Proto
    2. X-Requested-Proto
    3. **X-Forwarded-Port**
    4. X-Requested-Port
30. A user has configured ELB with two instances running in separate AZs of the same region? Which of the below mentioned statements is true?
    1. **Multi AZ instances will provide HA with ELB** (ELB provides HA to route traffic to healthy instances only it does not provide scalability)
    2. Multi AZ instances are not possible with a single ELB
    3. Multi AZ instances will provide scalability with ELB
    4. The user can achieve both HA and scalability with ELB
31. A user is configuring the HTTPS protocol on a front end ELB and the SSL protocol for the back-end listener in ELB. What will ELB do?
    1. It will allow you to create the configuration, but the instance will not pass the health check
    2. Receives requests on HTTPS and sends it to the back end instance on SSL
    3. **It will not allow you to create this configuration** (Will give error “Load Balancer protocol is an application layer protocol, but instance protocol is not. Both the Load Balancer protocol and the instance protocol should be at the same layer. Please fix.”)
    4. It will allow you to create the configuration, but ELB will not work as expected
32. An ELB is diverting traffic across 5 instances. One of the instances was unhealthy only for 20 minutes. What will happen after 20 minutes when the instance becomes healthy?
    1. ELB will never divert traffic back to the same instance
    2. ELB will not automatically send traffic to the same instance. However, the user can configure to start sending traffic to the same instance
    3. **ELB starts sending traffic to the instance once it is healthy**
    4. ELB terminates the instance once it is unhealthy. Thus, the instance cannot be healthy after 10 minutes
33. A user has hosted a website on AWS and uses ELB to load balance the multiple instances. The user application does not have any cookie management. How can the user bind the session of the requestor with a particular instance?
    1. Bind the IP address with a sticky cookie
    2. Create a cookie at the application level to set at ELB
    3. Use session synchronization with ELB
    4. **Let ELB generate a cookie for a specified duration**
34. A user has configured a website and launched it using the Apache web server on port 80. The user is using ELB with the EC2 instances for Load Balancing. What should the user do to ensure that the EC2 instances accept requests only from ELB?
    1. Open the port for an ELB static IP in the EC2 security group
    2. **Configure the security group of EC2, which allows access to the ELB source security group**
    3. Configure the EC2 instance so that it only listens on the ELB port
    4. Configure the security group of EC2, which allows access only to the ELB listener
35. AWS Elastic Load Balancer supports SSL termination.
    1. For specific availability zones only
    2. False
    3. For specific regions only
    4. **For all regions**
36. User has launched five instances with ELB. How can the user add the sixth EC2 instance to ELB?
    1. **The user can add the sixth instance on the fly.**
    2. The user must stop the ELB and add the sixth instance.
    3. The user can add the instance and change the ELB config file.
    4. The ELB can only have a maximum of five instances.



---



CW



1. A company needs to monitor the read and write IOPs metrics for their AWS MySQL RDS instance and send real-time alerts to their operations team. Which AWS services can accomplish this? Choose 2 answers

   1. Amazon Simple Email Service (Cannot be integrated with CloudWatch directly)
   2. **Amazon CloudWatch**
   3. Amazon Simple Queue Service
   4. Amazon Route 53
   5. **Amazon Simple Notification Service**

2. A customer needs to capture all client connection information from their load balancer every five minutes. The company wants to use this data for analyzing traffic patterns and troubleshooting their applications. Which of the following options meets the customer requirements?

   1. Enable AWS CloudTrail for the load balancer.
   2. **Enable access logs on the load balancer.** (Refer [link](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html))**
      **
   3. Install the Amazon CloudWatch Logs agent on the load balancer.
   4. Enable Amazon CloudWatch metrics on the load balancer (does not provide Client connection information)

3. A user is running a batch process on EBS backed EC2 instances. The batch process starts a few instances to process Hadoop Map reduce jobs, which can run between 50 – 600 minutes or sometimes for more time. The user wants to configure that the instance gets terminated only when the process is completed. How can the user configure this with CloudWatch?

   1. **Setup the CloudWatch action to terminate the instance when the CPU utilization is less than 5%**
   2. Setup the CloudWatch with Auto Scaling to terminate all the instances
   3. Setup a job which terminates all instances after 600 minutes
   4. It is not possible to terminate instances automatically

4. A user has two EC2 instances running in two separate regions. The user is running an internal memory management tool, which captures the data and sends it to CloudWatch in US East, using a CLI with the same namespace and metric. Which of the below mentioned options is true with respect to the above statement?

   1. The setup will not work as CloudWatch cannot receive data across regions
   2. **CloudWatch will receive and aggregate the data based on the namespace and metric**
   3. CloudWatch will give an error since the data will conflict due to two sources
   4. CloudWatch will take the data of the server, which sends the data first

5. A user is sending the data to CloudWatch using the CloudWatch API. The user is sending data 90 minutes in the future. What will CloudWatch do in this case?

   1. **CloudWatch will accept the data**
   2. It is not possible to send data of the future
   3. It is not possible to send the data manually to CloudWatch
   4. The user cannot send data for more than 60 minutes in the future

6. A user is having data generated randomly based on a certain event. The user wants to upload that data to CloudWatch. It may happen that event may not have data generated for some period due to randomness. Which of the below mentioned options is a recommended option for this case?

   1. For the period when there is no data, the user should not send the data at all
   2. For the period when there is no data the user should send a blank value
   3. **For the period when there is no data the user should send the value as 0** (Refer [User Guide)](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/publishingMetrics.html#publishingZero)
   4. The user must upload the data to CloudWatch as having no data for some period will cause an error at CloudWatch monitoring

7. A user has a weighing plant. The user measures the weight of some goods every 5 minutes and sends data to AWS CloudWatch for monitoring and tracking. Which of the below mentioned parameters is mandatory for the user to include in the request list?

   1. Value
   2. **Namespace (**refer [put-metric request](http://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-data.html)**)**
   3. Metric Name
   4. Timezone

8. A user has a refrigerator plant. The user is measuring the temperature of the plant every 15 minutes. If the user wants to send the data to CloudWatch to view the data visually, which of the below mentioned statements is true with respect to the information given above?

   1. **The user needs to use AWS CLI or API to upload the data**
   2. The user can use the AWS Import Export facility to import data to CloudWatch
   3. The user will upload data from the AWS console
   4. The user cannot upload data to CloudWatch since it is not an AWS service metric

9. A user has launched an EC2 instance. The user is planning to setup the CloudWatch alarm. Which of the below mentioned actions is not supported by the CloudWatch alarm?

   1. **Notify the Auto Scaling launch config to scale up**
   2. Send an SMS using SNS
   3. Notify the Auto Scaling group to scale down
   4. Stop the EC2 instance

10. A user has a refrigerator plant. The user is measuring the temperature of the plant every 15 minutes. If the user wants to send the data to CloudWatch to view the data visually, which of the below mentioned statements is true with respect to the information given above?

    1. **The user needs to use AWS CLI or API to upload the data**
    2. The user can use the AWS Import Export facility to import data to CloudWatch
    3. The user will upload data from the AWS console
    4. The user cannot upload data to CloudWatch since it is not an AWS service metric

11. A user is trying to aggregate all the CloudWatch metric data of the last 1 week. Which of the below mentioned statistics is not available for the user as a part of data aggregation?

    1. **Aggregate**
    2. Sum
    3. Sample data
    4. Average

12. A user has setup a CloudWatch alarm on an EC2 action when the CPU utilization is above 75%. The alarm sends a notification to SNS on the alarm state. If the user wants to simulate the alarm action how can he achieve this?

    1. Run activities on the CPU such that its utilization reaches above 75%
    2. From the AWS console change the state to ‘Alarm’
    3. **The user can set the alarm state to ‘Alarm’ using CLI**
    4. Run the SNS action manually

13. A user is publishing custom metrics to CloudWatch. Which of the below mentioned statements will help the user understand the functionality better?

    1. The user can use the CloudWatch Import tool
    2. **The user should be able to see the data in the console after around 15 minutes**
    3. If the user is uploading the custom data, the user must supply the namespace, timezone, and metric name as part of the command
    4. The user can view as well as upload data using the console, CLI and APIs

14. An application that you are managing has EC2 instances and DynamoDB tables deployed to several AWS Regions. In order to monitor the performance of the application globally, you would like to see two graphs 1) Avg CPU Utilization across all EC2 instances and 2) Number of Throttled Requests for all DynamoDB tables. How can you accomplish this? 

    **[PROFESSIONAL]**

    1. Tag your resources with the application name, and select the tag name as the dimension in the CloudWatch Management console to view the respective graphs (CloudWatch metrics are regional)
    2. **Use the CloudWatch CLI tools to pull the respective metrics from each regional endpoint. Aggregate the data offline & store it for graphing in CloudWatch.**
    3. Add SNMP traps to each instance and DynamoDB table. Leverage a central monitoring server to capture data from each instance and table. Put the aggregate data into CloudWatch for graphing (Can’t add SNMP traps to DynamoDB as it is a managed service)
    4. Add a CloudWatch agent to each instance and attach one to each DynamoDB table. When configuring the agent set the appropriate application name & view the graphs in CloudWatch. (Can’t add agents to DynamoDB as it is a managed service)

15. You have set up Individual AWS accounts for each project. You have been asked to make sure your AWS Infrastructure costs do not exceed the budget set per project for each month. Which of the following approaches can help ensure that you do not exceed the budget each month?

     

    **[PROFESSIONAL]**

    1. Consolidate your accounts so you have a single bill for all accounts and projects (Consolidation will not help limit per account)
    2. Set up auto scaling with CloudWatch alarms using SNS to notify you when you are running too many Instances in a given account (many instances do not directly map to cost and would not give exact cost)
    3. Set up CloudWatch billing alerts for all AWS resources used by each project, with a notification occurring when the amount for each resource tagged to a particular project matches the budget allocated to the project. (as each project already has a account, no need for resource tagging)
    4. **Set up CloudWatch billing alerts for all AWS resources used by each account, with email notifications when it hits 50%. 80% and 90% of its budgeted monthly spend**

16. You meet once per month with your operations team to review the past month’s data. During the meeting, you realize that 3 weeks ago, your monitoring system which pings over HTTP from outside AWS recorded a large spike in latency on your 3-tier web service API. You use DynamoDB for the database layer, ELB, EBS, and EC2 for the business logic tier, and SQS, ELB, and EC2 for the presentation layer. Which of the following techniques will NOT help you figure out what happened?

    1. Check your CloudTrail log history around the spike’s time for any API calls that caused slowness.
    2. **Review CloudWatch Metrics graphs to determine which component(s) slowed the system down.** (Metrics data was available for 2 weeks before, however it has been extended now)
    3. Review your ELB access logs in S3 to see if any ELBs in your system saw the latency.
    4. Analyze your logs to detect bursts in traffic at that time.

17. You have a high security requirement for your AWS accounts. What is the most rapid and sophisticated setup you can use to react to AWS API calls to your account?

    1. Subscription to AWS Config via an SNS Topic. Use a Lambda Function to perform in-flight analysis and reactivity to changes as they occur.
    2. Global AWS CloudTrail setup delivering to S3 with an SNS subscription to the deliver notifications, pushing into a Lambda, which inserts records into an ELK stack for analysis.
    3. Use a CloudWatch Rule ScheduleExpression to periodically analyze IAM credential logs. Push the deltas for events into an ELK stack and perform ad-hoc analysis there.
    4. **CloudWatch Events Rules, which trigger based on all AWS API calls, submitting all events to an AWS Kinesis Stream for arbitrary downstream analysis.** (CloudWatch Events allow subscription to AWS API calls, and direction of these events into Kinesis Streams. This allows a unified, near real-time stream for all API calls, which can be analyzed with any tool(s). Refer [link](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/EventTypes.html#api_event_type))

18. To monitor API calls against our AWS account by different users and entities, we can use ____ to create a history of calls in bulk for later review, and use ____ for reacting to AWS API calls in real-time.

    1. AWS Config; AWS Inspector
    2. AWS CloudTrail; AWS Config
    3. **AWS CloudTrail; CloudWatch Events** (CloudTrail is a batch API call collection service, CloudWatch Events enables real-time monitoring of calls through the Rules object interface. Refer [link](https://aws.amazon.com/whitepapers/security-at-scale-governance-in-aws/))
    4. AWS Config; AWS Lambda

19. You are hired as the new head of operations for a SaaS company. Your CTO has asked you to make debugging any part of your entire operation simpler and as fast as possible. She complains that she has no idea what is going on in the complex, service-oriented architecture, because the developers just log to disk, and it’s very hard to find errors in logs on so many services. How can you best meet this requirement and satisfy your CTO?

     

    **[PROFESSIONAL]**

    1. Copy all log files into AWS S3 using a cron job on each instance. Use an S3 Notification Configuration on the <code>PutBucket</code> event and publish events to AWS Lambda. Use the Lambda to analyze logs as soon as they come in and flag issues. (is not fast in search and introduces delay)
    2. Begin using CloudWatch Logs on every service. Stream all Log Groups into S3 objects. Use AWS EMR cluster jobs to perform adhoc MapReduce analysis and write new queries when needed. (is not fast in search and introduces delay)
    3. Copy all log files into AWS S3 using a cron job on each instance. Use an S3 Notification Configuration on the <code>PutBucket</code> event and publish events to AWS Kinesis. Use Apache Spark on AWS EMR to perform at-scale stream processing queries on the log chunks and flag issues. (is not fast in search and introduces delay)
    4. **Begin using CloudWatch Logs on every service. Stream all Log Groups into an AWS Elasticsearch Service Domain running Kibana 4 and perform log analysis on a search cluster.** (ELK – Elasticsearch, Kibana stack is designed specifically for real-time, ad-hoc log analysis and aggregation)

20. Your EC2-Based Multi-tier application includes a monitoring instance that periodically makes application -level read only requests of various application components and if any of those fail more than three times 30 seconds calls CloudWatch to fire an alarm, and the alarm notifies your operations team by email and SMS of a possible application health problem. However, you also need to watch the watcher -the monitoring instance itself – and be notified if it becomes unhealthy. Which of the following is a simple way to achieve that goal?

     

    **[PROFESSIONAL]**

    1. Run another monitoring instance that pings the monitoring instance and fires a could watch alarm mat notifies your operations team should the primary monitoring instance become unhealthy.
    2. **Set a CloudWatch alarm based on EC2 system and instance status checks and have the alarm notify your operations team of any detected problem with the monitoring instance.**
    3. Set a CloudWatch alarm based on the CPU utilization of the monitoring instance and nave the alarm notify your operations team if C r the CPU usage exceeds 50% few more than one minute: then have your monitoring application go into a CPU-bound loop should it Detect any application problems.
    4. Have the monitoring instances post messages to an SOS queue and then dequeue those messages on another instance should the queue cease to have new messages, the second instance should first terminate the original monitoring instance start another backup monitoring instance and assume (the role of the previous monitoring instance and beginning adding messages to the SQS queue.



---



CF



1. What does Amazon CloudFormation provide?

   1. The ability to setup Autoscaling for Amazon EC2 instances.
   2. **A templated resource creation for Amazon Web Services.**
   3. A template to map network resources for Amazon Web Services
   4. None of these

2. A user is planning to use AWS CloudFormation for his automatic deployment requirements. Which of the below mentioned components are required as a part of the template?

   1. Parameters
   2. Outputs
   3. Template version
   4. **Resources**

3. In regard to AWS CloudFormation, what is a stack?

   1. Set of AWS templates that are created and managed as a template
   2. Set of AWS resources that are created and managed as a template
   3. **Set of AWS resources that are created and managed as a single unit**
   4. Set of AWS templates that are created and managed as a single unit

4. A large enterprise wants to adopt CloudFormation to automate administrative tasks and implement the security principles of least privilege and separation of duties. They have identified the following roles with the corresponding tasks in the company: (i) network administrators: create, modify and delete VPCs, subnets, NACLs, routing tables, and security groups (ii) application operators: deploy complete application stacks (ELB, Auto -Scaling groups, RDS) whereas all resources must be deployed in the VPCs managed by the network administrators (iii) Both groups must maintain their own CloudFormation templates and should be able to create, update and delete only their own CloudFormation stacks. The company has followed your advice to create two IAM groups, one for applications and one for networks. Both IAM groups are attached to IAM policies that grant rights to perform the necessary task of each group as well as the creation, update and deletion of CloudFormation stacks. Given setup and requirements, which statements represent valid design considerations? Choose 2 answers

    

   **[PROFESSIONAL]**

   1. **Network stack updates will fail upon attempts to delete a subnet with EC2 instances** (Subnets cannot be deleted with instances in them)
   2. Unless resource level permissions are used on the CloudFormation: DeleteStack action, network administrators could tear down application stacks (Network administrators themselves need permission to delete resources within the application stack & CloudFormation makes calls to create, modify, and delete those resources on their behalf)
   3. The application stack cannot be deleted before all network stacks are deleted (Application stack can be deleted before network stack)
   4. **Restricting the launch of EC2 instances into VPCs requires resource level permissions in the IAM policy of the application group** (IAM permissions need to be given explicitly to launch instances )
   5. Nesting network stacks within application stacks simplifies management and debugging, but requires resource level permissions in the IAM policy of the network group (Although stacks can be nested, Network group will need to have all the application group permissions)

5. Your team is excited about the use of AWS because now they have access to programmable infrastructure. You have been asked to manage your AWS infrastructure in a manner similar to the way you might manage application code. You want to be able to deploy exact copies of different versions of your infrastructure, stage changes into different environments, revert back to previous versions, and identify what versions are running at any particular time (development, test, QA, production). Which approach addresses this requirement?

   1. Use cost allocation reports and AWS Opsworks to deploy and manage your infrastructure.
   2. Use AWS CloudWatch metrics and alerts along with resource tagging to deploy and manage your infrastructure.
   3. Use AWS Beanstalk and a version control system like GIT to deploy and manage your infrastructure.
   4. **Use AWS CloudFormation and a version control system like GIT to deploy and manage your infrastructure.**

6. A user is usingCloudFormation to launch an EC2 instance and then configure an application after the instance is launched. The user wants the stack creation of ELB and AutoScaling to wait until the EC2 instance is launched and configured properly. How can the user configure this?

   1. It is not possible that the stack creation will wait until one service is created and launched
   2. The user can use the HoldCondition resource to wait for the creation of the other dependent resources
   3. The user can use the DependentCondition resource to hold the creation of the other dependent resources
   4. **The user can use the WaitCondition resource to hold the creation of the other dependent resources**

7. A user has created a CloudFormation stack. The stack creates AWS services, such as EC2 instances, ELB, AutoScaling, and RDS. While creating the stack it created EC2, ELB and AutoScaling but failed to create RDS. What will CloudFormation do in this scenario?

   1. CloudFormation can never throw an error after launching a few services since it verifies all the steps before launching
   2. It will warn the user about the error and ask the user to manually create RDS
   3. **Rollback all the changes and terminate all the created services**
   4. It will wait for the user’s input about the error and correct the mistake after the input

8. A user is planning to use AWS CloudFormation. Which of the below mentioned functionalities does not help him to correctly understand CloudFormation?

   1. **CloudFormation follows the DevOps model for the creation of Dev & Test**
   2. AWS CloudFormation does not charge the user for its service but only charges for the AWS resources created with it
   3. CloudFormation works with a wide variety of AWS services, such as EC2, EBS, VPC, IAM, S3, RDS, ELB, etc
   4. CloudFormation provides a set of application bootstrapping scripts which enables the user to install Software

9. A customer is using AWS for Dev and Test. The customer wants to setup the Dev environment with CloudFormation. Which of the below mentioned steps are not required while using CloudFormation?

   1. Create a stack
   2. **Configure a service**
   3. Create and upload the template
   4. Provide the parameters configured as part of the template

10. A marketing research company has developed a tracking system that collects user behavior during web marketing campaigns on behalf of their customers all over the world. The tracking system consists of an auto-scaled group of Amazon Elastic Compute Cloud (EC2) instances behind an elastic load balancer (ELB), and the collected data is stored in Amazon DynamoDB. After the campaign is terminated, the tracking system is torn down and the data is moved to Amazon Redshift, where it is aggregated, analyzed and used to generate detailed reports. The company wants to be able to instantiate new tracking systems in any region without any manual intervention and therefore adopted AWS CloudFormation. What needs to be done to make sure that the AWS CloudFormation template works in every AWS region? Choose 2 answers 

    **[PROFESSIONAL]**

    1. IAM users with the right to start AWS CloudFormation stacks must be defined for every target region. (IAM users are global)
    2. The names of the Amazon DynamoDB tables must be different in every target region. (DynamoDB names should be unique only within a region)
    3. **Use the built-in function of AWS CloudFormation to set the AvailabilityZone attribute of the ELB resource.**
    4. Avoid using DeletionPolicies for EBS snapshots. (Don’t want the data to be retained)
    5. **Use the built-in Mappings and FindInMap functions of AWS CloudFormation to refer to the AMI ID set in the ImageId attribute of the Auto Scaling::LaunchConfiguration resource.**

11. A gaming company adopted AWS CloudFormation to automate load -testing of their games. They have created an AWS CloudFormation template for each gaming environment and one for the load -testing stack. The load – testing stack creates an Amazon Relational Database Service (RDS) Postgres database and two web servers running on Amazon Elastic Compute Cloud (EC2) that send HTTP requests, measure response times, and write the results into the database. A test run usually takes between 15 and 30 minutes. Once the tests are done, the AWS CloudFormation stacks are torn down immediately. The test results written to the Amazon RDS database must remain accessible for visualization and analysis. Select possible solutions that allow access to the test results after the AWS CloudFormation load -testing stack is deleted. Choose 2 answers. 

    **[PROFESSIONAL]**

    1. **Define a deletion policy of type Retain for the Amazon QDS resource to assure that the RDS database is not deleted with the AWS CloudFormation stack.**
    2. **Define a deletion policy of type Snapshot for the Amazon RDS resource to assure that the RDS database can be restored after the AWS CloudFormation stack is deleted.**
    3. Define automated backups with a backup retention period of 30 days for the Amazon RDS database and perform point -in -time recovery of the database after the AWS CloudFormation stack is deleted. (as the environment is required for limited time the automated backup will not serve the purpose)
    4. Define an Amazon RDS Read-Replica in the load-testing AWS CloudFormation stack and define a dependency relation between master and replica via the DependsOn attribute. (read replica not needed and will be deleted when the stack is deleted)
    5. Define an update policy to prevent deletion of the Amazon RDS database after the AWS CloudFormation stack is deleted. (UpdatePolicy does not apply to RDS)

12. When working with AWS CloudFormation Templates what is the maximum number of stacks that you can create?

    1. 500
    2. 50
    3. **200** (Refer [link](https://aws.amazon.com/cloudformation/faqs/) – The limit keeps on changing to check for the latest)
    4. 10

13. What happens, by default, when one of the resources in a CloudFormation stack cannot be created?

    1. Previously created resources are kept but the stack creation terminates
    2. **Previously created resources are deleted and the stack creation terminates**
    3. Stack creation continues, and the final results indicate which steps failed
    4. CloudFormation templates are parsed in advance so stack creation is guaranteed to succeed.

14. You need to deploy an AWS stack in a repeatable manner across multiple environments. You have selected CloudFormation as the right tool to accomplish this, but have found that there is a resource type you need to create and model, but is unsupported by CloudFormation. How should you overcome this challenge? 

    **[PROFESSIONAL]**

    1. Use a CloudFormation Custom Resource Template by selecting an API call to proxy for create, update, and delete actions. CloudFormation will use the AWS SDK, CLI, or API method of your choosing as the state transition function for the resource type you are modeling.
    2. Submit a ticket to the AWS Forums. AWS extends CloudFormation Resource Types by releasing tooling to the AWS Labs organization on GitHub. Their response time is usually 1 day, and they complete requests within a week or two.
    3. Instead of depending on CloudFormation, use Chef, Puppet, or Ansible to author Heat templates, which are declarative stack resource definitions that operate over the OpenStack hypervisor and cloud environment.
    4. **Create a CloudFormation Custom Resource Type by implementing create, update, and delete functionality, either by subscribing a Custom Resource Provider to an SNS topic, or by implementing the logic in AWS Lambda.** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html))

15. What is a circular dependency in AWS CloudFormation?

    1. When a Template references an earlier version of itself.
    2. When Nested Stacks depend on each other.
    3. **When Resources form a DependOn loop.** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html#troubleshooting-errors-dependence-error), to resolve a dependency error, add a DependsOn attribute to resources that depend on other resources in the template. Some cases for e.g. EIP and VPC with IGW where EIP depends on IGW need explicitly declaration for the resources to be created in correct order)
    4. When a Template references a region, which references the original Template.

16. You need to run a very large batch data processing job one time per day. The source data exists entirely in S3, and the output of the processing job should also be written to S3 when finished. If you need to version control this processing job and all setup and teardown logic for the system, what approach should you use?

    1. Model an AWS EMR job in AWS Elastic Beanstalk. (cannot directly model EMR Clusters)
    2. **Model an AWS EMR job in AWS CloudFormation.** (EMR cluster can be modeled using CloudFormation. Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emr-cluster.html))
    3. Model an AWS EMR job in AWS OpsWorks. (cannot directly model EMR Clusters)
    4. Model an AWS EMR job in AWS CLI Composer. (does not exist)

17. Your company needs to automate 3 layers of a large cloud deployment. You want to be able to track this deployment’s evolution as it changes over time, and carefully control any alterations. What is a good way to automate a stack to meet these requirements? 

    **[PROFESSIONAL]**

    1. Use OpsWorks Stacks with three layers to model the layering in your stack.
    2. **Use CloudFormation Nested Stack Templates, with three child stacks to represent the three logical layers of your cloud.** (CloudFormation allows source controlled, declarative templates as the basis for stack automation and Nested Stacks help achieve clean separation of layers while simultaneously providing a method to control all layers at once when needed)
    3. Use AWS Config to declare a configuration set that AWS should roll out to your cloud.
    4. Use Elastic Beanstalk Linked Applications, passing the important DNS entries between layers using the metadata interface.

18. You have been asked to de-risk deployments at your company. Specifically, the CEO is concerned about outages that occur because of accidental inconsistencies between Staging and Production, which sometimes cause unexpected behaviors in Production even when Staging tests pass. You already use Docker to get high consistency between Staging and Production for the application environment on your EC2 instances. How do you further de-risk the rest of the execution environment, since in AWS, there are many service components you may use beyond EC2 virtual machines? 

    **[PROFESSIONAL]**

    1. **Develop models of your entire cloud system in CloudFormation. Use this model in Staging and Production to achieve greater parity.** (Only CloudFormation’s JSON Templates allow declarative version control of repeatedly deployable models of entire AWS clouds. Refer [link](https://blogs.aws.amazon.com/application-management/blog/category/Best+practices))
    2. Use AWS Config to force the Staging and Production stacks to have configuration parity. Any differences will be detected for you so you are aware of risks.
    3. Use AMIs to ensure the whole machine, including the kernel of the virual machines, is consistent, since Docker uses Linux Container (LXC) technology, and we need to make sure the container environment is consistent.
    4. Use AWS ECS and Docker clustering. This will make sure that the AMIs and machine sizes are the same across both environments.

19. Which code snippet below returns the URL of a load balanced web site created in CloudFormation with an AWS::ElasticLoadBalancing::LoadBalancer resource name “ElasticLoad Balancer”?

     

    [Developer]

    1. **“Fn::Join” : [“”, [ “http://”, {“Fn::GetAtt” : [ “ElasticLoadBalancer”,”DNSName”]}]]** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html))
    2. “Fn::Join” : [“”,[ “http://”, {“Fn::GetAtt” : [ “ElasticLoadBalancer”,”Url”]}]]
    3. “Fn::Join” : [“”, [ “http://”, {“Ref” : “ElasticLoadBalancerUrl”}]]
    4. “Fn::Join” : [“”, [ “http://”, {“Ref” : “ElasticLoadBalancerDNSName”}]]

20. For AWS CloudFormation, which stack state refuses UpdateStack calls? 

    [Developer]

    1. **`UPDATE_ROLLBACK_FAILED`** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-continueupdaterollback.html))
    2. <code>UPDATE_ROLLBACK_COMPLETE</code>
    3. <code>UPDATE_COMPLETE</code>
    4. <code>CREATE_COMPLETE</code>

21. Which of these is not a Pseudo Parameter in AWS CloudFormation? 

    [Developer]

    1. AWS::StackName
    2. AWS::AccountId
    3. **AWS::StackArn** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html))
    4. AWS::NotificationARNs

22. Which of these is not an intrinsic function in AWS CloudFormation? 

    [Developer]

    1. **Fn::SplitValue** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html))
    2. Fn::FindInMap
    3. Fn::Select
    4. Fn::GetAZs

23. Which of these is not a CloudFormation Helper Script? 

    [Developer]

    1. cfn-signal
    2. cfn-hup
    3. **cfn-request** (Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-helper-scripts-reference.html))
    4. cfn-get-metadata

24. What method should I use to author automation if I want to wait for a CloudFormation stack to finish completing in a script? 

    [Developer]

    1. Event subscription using SQS.
    2. Event subscription using SNS.
    3. **Poll using `ListStacks` / `list-stacks`.** (Only polling will make a script wait to complete. ListStacks / list-stacks is a real method. Refer [link](http://docs.aws.amazon.com/cli/latest/reference/cloudformation/list-stacks.html))
    4. Poll using <code>GetStackStatus</code> / <code>get-stack-status</code>. (GetStackStatus / get-stack-status does not exist)

25. Which status represents a failure state in AWS CloudFormation? 

    [Developer]

    1. <code>UPDATE_COMPLETE_CLEANUP_IN_PROGRESS</code> (UPDATE_COMPLETE_CLEANUP_IN_PROGRESS means an update was successful, and CloudFormation is deleting any replaced, no longer used resources)
    2. <code>DELETE_COMPLETE_WITH_ARTIFACTS</code> (DELETE_COMPLETE_WITH_ARTIFACTS does not exist)
    3. **`ROLLBACK_IN_PROGRESS`** (ROLLBACK_IN_PROGRESS means an UpdateStack operation failed and the stack is in the process of trying to return to the valid, pre-update state Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks.html))
    4. <code>ROLLBACK_FAILED</code> (ROLLBACK_FAILED is not a CloudFormation state but UPDATE_ROLLBACK_FAILED is)

26. Which of these is not an intrinsic function in AWS CloudFormation? 

    [Developer]

    1. Fn::Equals
    2. Fn::If
    3. Fn::Not
    4. **Fn::Parse** (Complete list of Intrinsic Functions: Fn::Base64, Fn::And, Fn::Equals, Fn::If, Fn::Not, Fn::Or, Fn::FindInMap, Fn::GetAtt, Fn::GetAZs, Fn::Join, Fn::Select, Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html))

27. You need to create a Route53 record automatically in CloudFormation when not running in production during all launches of a Template. How should you implement this? 

    [Developer]

    1. **Use a `Parameter` for `environment`, and add a `Condition` on the Route53 `Resource` in the template to create the record only when `environment` is not `production`.** (Best way to do this is with one template, and a Condition on the resource. Route53 does not allow null strings for Refer [link](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html))
    2. Create two templates, one with the Route53 record value and one with a null value for the record. Use the one without it when deploying to production.
    3. Use a <code>Parameter</code> for <code>environment</code>, and add a <code>Condition</code> on the Route53 <code>Resource</code> in the template to create the record with a null string when <code>environment</code> is <code>production</code>.
    4. Create two templates, one with the Route53 record and one without it. Use the one without it when deploying to production.



---



CloudTrail



1. You currently operate a web application in the AWS US-East region. The application runs on an auto-scaled layer of EC2 instances and an RDS Multi-AZ database. Your IT security compliance officer has tasked you to develop a reliable and durable logging solution to track changes made to your EC2, IAM and RDS resources. The solution must ensure the integrity and confidentiality of your log data. Which of these solutions would you recommend?
   1. **Create a new CloudTrail trail with one new S3 bucket to store the logs and with the global services option selected. Use IAM roles, S3 bucket policies and Multi Factor Authentication (MFA) Delete on the S3 bucket that stores your logs. (**Single New bucket with global services option for IAM and MFA delete for confidentiality**)
      **
   2. Create a new CloudTrail with one new S3 bucket to store the logs. Configure SNS to send log file delivery notifications to your management system. Use IAM roles and S3 bucket policies on the S3 bucket that stores your logs. (Missing Global Services for IAM)
   3. Create a new CloudTrail trail with an existing S3 bucket to store the logs and with the global services option selected Use S3 ACLs and Multi Factor Authentication (MFA) Delete on the S3 bucket that stores your logs. (Existing bucket prevents confidentiality)
   4. Create three new CloudTrail trails with three new S3 buckets to store the logs one for the AWS Management console, one for AWS SDKs and one for command line tools. Use IAM roles and S3 bucket policies on the S3 buckets that store your logs (3 buckets not needed, Missing Global services options)
2. Which of the following are true regarding AWS CloudTrail? Choose 3 answers
   1. **CloudTrail is enabled globally** (it can be enabled for all regions and also per region basis)
   2. CloudTrail is enabled by default (**was not enabled by default, however, it is enabled by default as per the latest [AWS enhancements](https://aws.amazon.com/blogs/aws/new-amazon-web-services-extends-cloudtrail-to-all-aws-customers/)**)
   3. **CloudTrail is enabled on a per-region basis** (it can be enabled for all regions and also per region basis)
   4. CloudTrail is enabled on a per-service basis (once enabled it is applicable for all the supported services, service can’t be selected)
   5. **Logs can be delivered to a single Amazon S3 bucket for aggregation**
   6. CloudTrail is enabled for all available services within a region. (is enabled only for CloudTrail supported services)
   7. Logs can only be processed and delivered to the region in which they are generated. (can be logged to bucket in any region)
3. An organization has configured the custom metric upload with CloudWatch. The organization has given permission to its employees to upload data using CLI as well SDK. How can the user track the calls made to CloudWatch?
   1. The user can enable logging with CloudWatch which logs all the activities
   2. **Use CloudTrail to monitor the API calls**
   3. Create an IAM user and allow each user to log the data using the S3 bucket
   4. Enable detailed monitoring with CloudWatch
4. A user is trying to understand the CloudWatch metrics for the AWS services. It is required that the user should first understand the namespace for the AWS services. Which of the below mentioned is not a valid namespace for the AWS services?
   1. AWS/StorageGateway
   2. **AWS/CloudTrail (**[CloudWatch supported namespaces](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/aws-namespaces.html)**)
      **
   3. AWS/ElastiCache
   4. AWS/SWF
5. Your CTO thinks your AWS account was hacked. What is the only way to know for certain if there was unauthorized access and what they did, assuming your hackers are very sophisticated AWS engineers and doing everything they can to cover their tracks?
   1. **Use CloudTrail Log File Integrity Validation**. (Refer [link](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-validation-intro.html))
   2. Use AWS Config SNS Subscriptions and process events in real time.
   3. Use CloudTrail backed up to AWS S3 and Glacier.
   4. Use AWS Config Timeline forensics.
6. Your CTO has asked you to make sure that you know what all users of your AWS account are doing to change resources at all times. She wants a report of who is doing what over time, reported to her once per week, for as broad a resource type group as possible. How should you do this?
   1. **Create a global AWS CloudTrail Trail. Configure a script to aggregate the log data delivered to S3 once per week and deliver this to the CTO.**
   2. Use CloudWatch Events Rules with an SNS topic subscribed to all AWS API calls. Subscribe the CTO to an email type delivery on this SNS Topic.
   3. Use AWS IAM credential reports to deliver a CSV of all uses of IAM User Tokens over time to the CTO.
   4. Use AWS Config with an SNS subscription on a Lambda, and insert these changes over time into a DynamoDB table. Generate reports based on the contents of this table.



----



AWS Config



1. One of the challenges in managing AWS resources is to keep track of changes in the resource configuration over time. Which one of the following statements provide the best solution?
   1. Use strict syntax tagging on the resources
   2. Create a custom application to automate the configuration management process
   3. **Use AWS Config for supported services and use an automated process via APIs for unsupported services**
   4. Use resource groups and tagging along with CloudTrail so that you can audit changes using the logs
2. Fill the blanks: ____ helps us track AWS API calls and transitions, ____ helps to understand what resources we have now, and ____ allows auditing credentials and logins.
   1. AWS Config, CloudTrail, IAM Credential Reports
   2. CloudTrail, IAM Credential Reports, AWS Config
   3. **CloudTrail, AWS Config, IAM Credential Reports**
   4. AWS Config, IAM Credential Reports, CloudTrail



----



OpsWorks



1. You are working with a customer who is using Chef configuration management in their data center. Which service is designed to let the customer leverage existing Chef recipes in AWS?
   1. Amazon Simple Workflow Service
   2. AWS Elastic Beanstalk
   3. AWS CloudFormation
   4. **AWS OpsWorks**
2. Your mission is to create a lights-out datacenter environment, and you plan to use AWS OpsWorks to accomplish this. First you created a stack and added an App Server layer with an instance running in it. Next you added an application to the instance, and now you need to deploy a MySQL RDS database instance. Which of the following answers accurately describe how to add a backend database server to an OpsWorks stack? Choose 3 answers
   1. **Add a new database layer and then add recipes to the deploy actions of the database and App Server layers.** (Refer [link](http://docs.aws.amazon.com/opsworks/latest/userguide/customizing-rds.html))
   2. Use OpsWorks’ “Clone Stack” feature to create a second RDS stack in another Availability Zone for redundancy in the event of a failure in the Primary AZ. To switch to the secondary RDS instance, set the [:database] attributes to values that are appropriate for your server which you can do by using custom JSON.
   3. **The variables that characterize the RDS database connection—host, user, and so on—are set using the corresponding values from the deploy JSON’s [:deploy][:app_name][:database] attributes.** (Refer [link](http://docs.aws.amazon.com/opsworks/latest/userguide/customizing-rds.html))
   4. Cookbook attributes are stored in a repository, so OpsWorks requires that the “password”: “your_password” attribute for the RDS instance must be encrypted using at least a 256-bit key.
   5. **Set up the connection between the app server and the RDS layer by using a custom recipe. The recipe configures the app server as required, typically by creating a configuration file. The recipe gets the connection data such as the host and database name from a set of attributes in the stack configuration and deployment JSON that AWS OpsWorks installs on every instance.** (Refer [link](http://docs.aws.amazon.com/opsworks/latest/userguide/customizing-rds.html))
3. You are tasked with the migration of a highly trafficked node.js application to AWS. In order to comply with organizational standards Chef recipes must be used to configure the application servers that host this application and to support application lifecycle events. Which deployment option meets these requirements while minimizing administrative burden?
   1. **Create a new stack within Opsworks add the appropriate layers to the stack and deploy the application**
   2. Create a new application within Elastic Beanstalk and deploy this application to a new environment (need to comply with chef recipes)
   3. Launch a Node JS server from a community AMI and manually deploy the application to the launched EC2 instance
   4. Launch and configure Chef Server on an EC2 instance and leverage the AWS CLI to launch application servers and configure those instances using Chef.
4. A web-startup runs its very successful social news application on Amazon EC2 with an Elastic Load Balancer, an Auto-Scaling group of Java/Tomcat application-servers, and DynamoDB as data store. The main web application best runs on m2.xlarge instances since it is highly memory- bound. Each new deployment requires semi-automated creation and testing of a new AMI for the application servers which takes quite a while and is therefore only done once per week. Recently, a new chat feature has been implemented in node.js and waits to be integrated in the architecture. First tests show that the new component is CPU bound Because the company has some experience with using Chef, they decided to streamline the deployment process and use AWS OpsWorks as an application life cycle tool to simplify management of the application and reduce the deployment cycles. What configuration in AWS OpsWorks is necessary to integrate the new chat module in the most cost-efficient and flexible way?
   1. Create one AWS Ops Works stack, create one AWS Ops Works layer, create one custom recipe
   2. **Create one AWS Ops Works stack, create two AWS Ops Works layers create one custom recipe** (Single environment stack, two layers for java and node.js application using built-in recipes and custom recipe for DynamoDB connectivity only as other configuration. Refer [link](http://docs.aws.amazon.com/opsworks/latest/userguide/other-services.html))
   3. Create two AWS Ops Works stacks, create two AWS Ops Works layers create one custom recipe
   4. Create two AWS Ops Works stacks, create two AWS Ops Works layers create two custom recipe
5. You company runs a complex customer relations management system that consists of around 10 different software components all backed by the same Amazon Relational Database (RDS) database. You adopted AWS OpsWorks to simplify management and deployment of that application and created an AWS OpsWorks stack with layers for each of the individual components. An internal security policy requires that all instances should run on the latest Amazon Linux AMI and that instances must be replaced within one month after the latest Amazon Linux AMI has been released. AMI replacements should be done without incurring application downtime or capacity problems. You decide to write a script to be run as soon as a new Amazon Linux AMI is released. Which solutions support the security policy and meet your requirements? Choose 2 answers
   1. Assign a custom recipe to each layer, which replaces the underlying AMI. Use AWS OpsWorks life-cycle events to incrementally execute this custom recipe and update the instances with the new AMI.
   2. **Create a new stack and layers with identical configuration, add instances with the latest Amazon Linux AMI specified as a custom AMI to the new layer, switch DNS to the new stack, and tear down the old stack.** (Blue-Green Deployment)
   3. Identify all Amazon Elastic Compute Cloud (EC2) instances of your AWS OpsWorks stack, stop each instance, replace the AMI ID property with the ID of the latest Amazon Linux AMI ID, and restart the instance. To avoid downtime, make sure not more than one instance is stopped at the same time.
   4. Specify the latest Amazon Linux AMI as a custom AMI at the stack level, terminate instances of the stack and let AWS OpsWorks launch new instances with the new AMI. (Will lead to downtime)
   5. **Add new instances with the latest Amazon Linux AMI specified as a custom AMI to all AWS OpsWorks layers of your stack, and terminate the old ones.**
6. When thinking of AWS OpsWorks, which of the following is not an instance type you can allocate in a stack layer?
   1. 24/7 instances (24/7 instances are supported and started manually and run until you stop them)
   2. **Spot instances** (Does not support spot instance directly but can be used with auto scaling Refer [link](https://forums.aws.amazon.com/thread.jspa?threadID=117372))
   3. Time-based instances (Time-based instances are run by AWS OpsWorks on a specified daily and weekly schedule)
   4. Load-based instances (Load-based instances are automatically started and stopped by AWS OpsWorks, based on specified load metrics, such as CPU utilization)
7. Which of the following tools does not directly support AWS OpsWorks, for monitoring your stacks?
   1. **AWS Config** (Refer [link](http://docs.aws.amazon.com/opsworks/latest/userguide/monitoring.html))
   2. Amazon CloudWatch Metrics (AWS OpsWorks uses CloudWatch to provide thirteen custom metrics with detailed monitoring for each instance in the stack)
   3. AWS CloudTrail (AWS OpsWorks integrates with CloudTrail to log every AWS OpsWorks API call and store the data in an S3 bucket)
   4. Amazon CloudWatch Logs (You can use Amazon CloudWatch Logs to monitor your stack’s system, application, and custom logs.)
8. When thinking of AWS OpsWorks, which of the following is true?
   1. **Stacks have many layers, layers have many instances.**
   2. Instances have many stacks, stacks have many layers.
   3. Layers have many stacks, stacks have many instances.
   4. Layers have many instances, instances have many stacks.



---



Trusted Advisor



The Trusted Advisor service provides insight regarding which four categories of an AWS account?

1. Security, fault tolerance, high availability, and connectivity
2. Security, access control, high availability, and performance
3. **Performance, cost optimization, security, and fault tolerance** (Note – Service limits is a latest addition)
4. Performance, cost optimization, access control, and connectivity



---



SQS



1. Which AWS service can help design architecture to persist in-flight transactions?

   1. Elastic IP Address
   2. **SQS**
   3. Amazon CloudWatch
   4. Amazon ElastiCache

2. A company has a workflow that sends video files from their on-premise system to AWS for transcoding. They use EC2 worker instances that pull transcoding jobs from SQS. Why is SQS an appropriate service for this scenario?

   1. SQS guarantees the order of the messages.
   2. SQS synchronously provides transcoding output.
   3. SQS checks the health of the worker instances.
   4. **SQS helps to facilitate horizontal scaling of encoding tasks**

3. Which statement best describes an Amazon SQS use case?

   1. Automate the process of sending an email notification to administrators when the CPU utilization reaches 70% on production servers (Amazon EC2 instances) (CloudWatch + SNS + SES)
   2. **Create a video transcoding website where multiple components need to communicate with each other, but can’t all process the same amount of work simultaneously** (SQS provides loose coupling)
   3. Coordinate work across distributed web services to process employee’s expense reports (SWF – Steps in order and might need manual steps)
   4. Distribute static web content to end users with low latency across multiple countries (CloudFront + S3)

4. Your application provides data transformation services. Files containing data to be transformed are first uploaded to Amazon S3 and then transformed by a fleet of spot EC2 instances. Files submitted by your premium customers must be transformed with the highest priority. How should you implement such a system?

   1. Use a DynamoDB table with an attribute defining the priority level. Transformation instances will scan the table for tasks, sorting the results by priority level.
   2. Use Route 53 latency based-routing to send high priority tasks to the closest transformation instances.
   3. **Use two SQS queues, one for high priority messages, and the other for default priority. Transformation instances first poll the high priority queue; if there is no message, they poll the default priority queue**
   4. Use a single SQS queue. Each message contains the priority level. Transformation instances poll high-priority messages first.

5. Your company plans to host a large donation website on Amazon Web Services (AWS). You anticipate a large and undetermined amount of traffic that will create many database writes. To be certain that you do not drop any writes to a database hosted on AWS. Which service should you use?

   1. Amazon RDS with provisioned IOPS up to the anticipated peak write throughput.
   2. **Amazon Simple Queue Service (SQS) for capturing the writes and draining the queue to write to the database**
   3. Amazon ElastiCache to store the writes until the writes are committed to the database.
   4. Amazon DynamoDB with provisioned write throughput up to the anticipated peak write throughput.

6. A customer has a 10 GB AWS Direct Connect connection to an AWS region where they have a web application hosted on Amazon Elastic Computer Cloud (EC2). The application has dependencies on an on-premises mainframe database that uses a BASE (Basic Available, Soft state, Eventual consistency) rather than an ACID (Atomicity, Consistency, Isolation, Durability) consistency model. The application is exhibiting undesirable behavior because the database is not able to handle the volume of writes. How can you reduce the load on your on-premises database resources in the most cost-effective way?

   1. Use an Amazon Elastic Map Reduce (EMR) S3DistCp as a synchronization mechanism between the onpremises database and a Hadoop cluster on AWS.
   2. **Modify the application to write to an Amazon SQS queue and develop a worker process to flush the queue to the on-premises database**
   3. Modify the application to use DynamoDB to feed an EMR cluster which uses a map function to write to the on-premises database.
   4. Provision an RDS read-replica database on AWS to handle the writes and synchronize the two databases using Data Pipeline.

7. An organization has created a Queue named “modularqueue” with SQS. The organization is not performing any operations such as SendMessage, ReceiveMessage, DeleteMessage, GetQueueAttributes, SetQueueAttributes, AddPermission, and RemovePermission on the queue. What can happen in this scenario?

   1. AWS SQS sends notification after 15 days for inactivity on queue
   2. **AWS SQS can delete queue after 30 days without notification**
   3. AWS SQS marks queue inactive after 30 days
   4. AWS SQS notifies the user after 2 weeks and deletes the queue after 3 weeks.

8. A user is using the AWS SQS to decouple the services. Which of the below mentioned operations is not supported by SQS?

   1. SendMessageBatch
   2. DeleteMessageBatch
   3. CreateQueue
   4. **DeleteMessageQueue**

9. A user has created a queue named “awsmodule” with SQS. One of the consumers of queue is down for 3 days and then becomes available. Will that component receive message from queue?

   1. **Yes, since SQS by default stores message for 4 days**
   2. No, since SQS by default stores message for 1 day only
   3. No, since SQS sends message to consumers who are available that time
   4. Yes, since SQS will not delete message until it is delivered to all consumers

10. A user has created a queue named “queue2” in US-East region with AWS SQS. The user’s AWS account ID is 123456789012. If the user wants to perform some action on this queue, which of the below Queue URL should he use?

    1. **http://sqs.us-east-1.amazonaws.com/123456789012/queue2**
    2. http://sqs.amazonaws.com/123456789012/queue2
    3. http://sqs. 123456789012.us-east-1.amazonaws.com/queue2
    4. http://123456789012.sqs.us-east-1.amazonaws.com/queue2

11. A user has created a queue named “myqueue” with SQS. There are four messages published to queue, which are not received by the consumer yet. If the user tries to delete the queue, what will happen?

    1. A user can never delete a queue manually. AWS deletes it after 30 days of inactivity on queue
    2. **It will delete the queue**
    3. It will initiate the delete but wait for four days before deleting until all messages are deleted automatically.
    4. I t will ask user to delete the messages first

12. A user has developed an application, which is required to send the data to a NoSQL database. The user wants to decouple the data sending such that the application keeps processing and sending data but does not wait for an acknowledgement of DB. Which of the below mentioned applications helps in this scenario?

    1. AWS Simple Notification Service
    2. AWS Simple Workflow
    3. **AWS Simple Queue Service**
    4. AWS Simple Query Service

13. You are building an online store on AWS that uses SQS to process your customer orders. Your backend system needs those messages in the same sequence the customer orders have been put in. How can you achieve that?

    1. It is not possible to do this with SQS
    2. **You can use sequencing information on each message**
    3. You can do this with SQS but you also need to use SWF
    4. Messages will arrive in the same order by default

14. A user has created a photo editing software and hosted it on EC2. The software accepts requests from the user about the photo format and resolution and sends a message to S3 to enhance the picture accordingly. Which of the below mentioned AWS services will help make a scalable software with the AWS infrastructure in this scenario?

    1. AWS Glacier
    2. AWS Elastic Transcoder
    3. AWS Simple Notification Service
    4. **AWS Simple Queue Service**

15. Refer to the architecture diagram of a batch processing solution using Simple Queue Service (SQS) to set up a message queue between EC2 instances, which are used as batch processors. Cloud Watch monitors the number of Job requests (queued messages) and an Auto Scaling group adds or deletes batch servers automatically based on parameters set in Cloud Watch alarms. You can use this architecture to implement which of the following features in a cost effective and efficient manner? 

    

    1. Reduce the overall time for executing jobs through parallel processing by allowing a busy EC2 instance that receives a message to pass it to the next instance in a daisy-chain setup.
    2. Implement fault tolerance against EC2 instance failure since messages would remain in SQS and worn can continue with recovery of EC2 instances implement fault tolerance against SQS failure by backing up messages to S3.
    3. Implement message passing between EC2 instances within a batch by exchanging messages through SOS.
    4. **Coordinate number of EC2 instances with number of job requests automatically thus Improving cost effectiveness**
    5. Handle high priority jobs before lower priority jobs by assigning a priority metadata field to SQS messages.

16. How does Amazon SQS allow multiple readers to access the same message queue without losing messages or processing them many times?

    1. By identifying a user by his unique id
    2. By using unique cryptography
    3. **Amazon SQS queue has a configurable visibility timeout**
    4. Multiple readers can’t access the same message queue

17. A user has created photo editing software and hosted it on EC2. The software accepts requests from the user about the photo format and resolution and sends a message to S3 to enhance the picture accordingly. Which of the below mentioned AWS services will help make a scalable software with the AWS infrastructure in this scenario?

    1. AWS Elastic Transcoder
    2. AWS Simple Notification Service
    3. **AWS Simple Queue Service**
    4. AWS Glacier

18. How do you configure SQS to support longer message retention?

    1. **Set the MessageRetentionPeriod attribute using the SetQueueAttributes method**
    2. Using a Lambda function
    3. You can’t. It is set to 14 days and cannot be changed
    4. You need to request it from AWS

19. A user has developed an application, which is required to send the data to a NoSQL database. The user wants to decouple the data sending such that the application keeps processing and sending data but does not wait for an acknowledgement of DB. Which of the below mentioned applications helps in this scenario?

    1. AWS Simple Notification Service
    2. AWS Simple Workflow
    3. AWS Simple Query Service
    4. **AWS Simple Queue Service**

20. If a message is retrieved from a queue in Amazon SQS, how long is the message inaccessible to other users by default?

    1. 0 seconds
    2. 1 hour
    3. 1 day
    4. forever
    5. **30 seconds**

21. Which of the following statements about SQS is true?

    1. Messages will be delivered exactly once and messages will be delivered in First in, First out order
    2. Messages will be delivered exactly once and message delivery order is indeterminate
    3. Messages will be delivered one or more times and messages will be delivered in First in, First out order
    4. **Messages will be delivered one or more times and message delivery order is indeterminate** (Before the introduction of FIFO queues)

22. How long can you keep your Amazon SQS messages in Amazon SQS queues?

    1. From 120 secs up to 4 weeks
    2. From 10 secs up to 7 days
    3. **From 60 secs up to 2 weeks**
    4. From 30 secs up to 1 week

23. When a Simple Queue Service message triggers a task that takes 5 minutes to complete, which process below will result in successful processing of the message and remove it from the queue while minimizing the chances of duplicate processing?

    1. **Retrieve the message with an increased visibility timeout, process the message, delete the message from the queue**
    2. Retrieve the message with an increased visibility timeout, delete the message from the queue, process the message
    3. Retrieve the message with increased DelaySeconds, process the message, delete the message from the queue
    4. Retrieve the message with increased DelaySeconds, delete the message from the queue, process the message

24. You need to process long-running jobs once and only once. How might you do this?

    1. Use an SNS queue and set the visibility timeout to long enough for jobs to process.
    2. Use an SQS queue and set the reprocessing timeout to long enough for jobs to process.
    3. **Use an SQS queue and set the visibility timeout to long enough for jobs to process.**
    4. Use an SNS queue and set the reprocessing timeout to long enough for jobs to process.

25. You are getting a lot of empty receive requests when using Amazon SQS. This is making a lot of unnecessary network load on your instances. What can you do to reduce this load?

    1. Subscribe your queue to an SNS topic instead.
    2. **Use as long of a poll as possible, instead of short polls.** (Refer [link](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html))
    3. Alter your visibility timeout to be shorter.
    4. Use <code>sqsd</code> on your EC2 instances.

26. You have an asynchronous processing application using an Auto Scaling Group and an SQS Queue. The Auto Scaling Group scales according to the depth of the job queue. The completion velocity of the jobs has gone down, the Auto Scaling Group size has maxed out, but the inbound job velocity did not increase. What is a possible issue?

    1. **Some of the new jobs coming in are malformed and unprocessable.** (As other options would cause the job to stop processing completely, the only reasonable option seems that some of the recent messages must be malformed and unprocessable)
    2. The routing tables changed and none of the workers can process events anymore. (If changed, none of the jobs would be processed)
    3. Someone changed the IAM Role Policy on the instances in the worker group and broke permissions to access the queue. (If IAM role changed no jobs would be processed)
    4. The scaling metric is not functioning correctly. (scaling metric did work fine as the autoscaling caused the instances to increase)

27. Company B provides an online image recognition service and utilizes SQS to decouple system components for scalability. The SQS consumers poll the imaging queue as often as possible to keep end-to-end throughput as high as possible. However, Company B is realizing that polling in tight loops is burning CPU cycles and increasing costs with empty responses. How can Company B reduce the number of empty responses?

    1. Set the imaging queue visibility Timeout attribute to 20 seconds
    2. **Set the Imaging queue ReceiveMessageWaitTimeSeconds attribute to 20 seconds** (Long polling. Refer [link](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html))
    3. Set the imaging queue MessageRetentionPeriod attribute to 20 seconds
    4. Set the DelaySeconds parameter of a message to 20 seconds



---



SNS



1. Which of the following notification endpoints or clients does Amazon Simple Notification Service support? Choose 2 answers
   1. **Email**
   2. CloudFront distribution
   3. File Transfer Protocol
   4. **Short Message Service**
   5. Simple Network Management Protocol
2. What happens when you create a topic on Amazon SNS?
   1. The topic is created, and it has the name you specified for it.
   2. **An ARN (Amazon Resource Name) is created**
   3. You can create a topic on Amazon SQS, not on Amazon SNS.
   4. This question doesn’t make sense.
3. A user has deployed an application on his private cloud. The user is using his own monitoring tool. He wants to configure that whenever there is an error, the monitoring tool should notify him via SMS. Which of the below mentioned AWS services will help in this scenario?
   1. None because the user infrastructure is in the private cloud/
   2. **AWS SNS**
   3. AWS SES
   4. AWS SMS
4. A user wants to make so that whenever the CPU utilization of the AWS EC2 instance is above 90%, the redlight of his bedroom turns on. Which of the below mentioned AWS services is helpful for this purpose?
   1. AWS CloudWatch + AWS SES
   2. **AWS CloudWatch + AWS SNS**
   3. It is not possible to configure the light with the AWS infrastructure services
   4. AWS CloudWatch and a dedicated software turning on the light
5. A user is trying to understand AWS SNS. To which of the below mentioned end points is SNS unable to send a notification?
   1. Email JSON
   2. HTTP
   3. AWS SQS
   4. **AWS SES**
6. A user is running a webserver on EC2. The user wants to receive the SMS when the EC2 instance utilization is above the threshold limit. Which AWS services should the user configure in this case?
   1. AWS CloudWatch + AWS SES
   2. **AWS CloudWatch + AWS SNS**
   3. AWS CloudWatch + AWS SQS
   4. AWS EC2 + AWS CloudWatch
7. A user is planning to host a mobile game on EC2 which sends notifications to active users on either high score or the addition of new features. The user should get this notification when he is online on his mobile device. Which of the below mentioned AWS services can help achieve this functionality?
   1. **AWS Simple Notification Service**
   2. AWS Simple Queue Service
   3. AWS Mobile Communication Service
   4. AWS Simple Email Service
8. You are providing AWS consulting service for a company developing a new mobile application that will be leveraging amazon SNS push for push notifications. In order to send direct notification messages to individual devices each device registration identifier or token needs to be registered with SNS, however the developers are not sure of the best way to do this. You advise them to: –
   1. Bulk upload the device tokens contained in a CSV file via the AWS Management Console
   2. Let the push notification service (e.g. Amazon Device messaging) handle the registration
   3. Implement a token vending service to handle the registration
   4. **Call the CreatePlatformEndpoint API function to register multiple device tokens.** (Refer [documentation](http://docs.aws.amazon.com/sns/latest/dg/mobile-push-send-devicetoken.html))**
      **
9. A company is running a batch analysis every hour on their main transactional DB running on an RDS MySQL instance to populate their central Data Warehouse running on Redshift. During the execution of the batch their transactional applications are very slow. When the batch completes they need to update the top management dashboard with the new data. The dashboard is produced by another system running on-premises that is currently started when a manually-sent email notifies that an update is required The on-premises system cannot be modified because is managed by another team. How would you optimize this scenario to solve performance issues and automate the process as much as possible?
   1. Replace RDS with Redshift for the batch analysis and SNS to notify the on-premises system to update the dashboard
   2. Replace RDS with Redshift for the batch analysis and SQS to send a message to the on-premises system to update the dashboard
   3. **Create an RDS Read Replica for the batch analysis and SNS to notify me on-premises system to update the dashboard**
   4. Create an RDS Read Replica for the batch analysis and SQS to send a message to the on-premises system to update the dashboard.
10. Which of the following are valid SNS delivery transports? Choose 2 answers.
    1. **HTTP**
    2. UDP
    3. **SMS**
    4. DynamoDB
    5. Named Pipes
11. What is the format of structured notification messages sent by Amazon SNS?
    1. An XML object containing MessageId, UnsubscribeURL, Subject, Message and other values
    2. An JSON object containing MessageId, DuplicateFlag, Message and other values
    3. An XML object containing MessageId, DuplicateFlag, Message and other values
    4. **An JSON object containing MessageId, unsubscribeURL, Subject, Message and other values**
12. Which of the following are valid arguments for an SNS Publish request? Choose 3 answers.
    1. **TopicAm**
    2. **Subject**
    3. Destination
    4. Format
    5. **Message**
    6. Language



---



Support Plans



1. Which AWS support plan has a dedicated technical account manager assigned for proactive guidance?
   1. AWS Basic support plan
   2. AWS Developer support plan
   3. AWS Business support plan
   4. **AWS Enterprise support plan**
2. Which feature is available for all the AWS support plans?
   1. Technical Account Manager
   2. Assigned Support Concierge
   3. **24×7 access to customer service**
   4. Access to Cloud Support resources



---



Orgz



1. An organization that is currently using consolidated billing has recently acquired another company that already has a number of AWS accounts. How could an Administrator ensure that all AWS accounts, from both the existing company and the acquired company, are billed to a single account?
   1. Merge the two companies, AWS accounts by going to the AWS console and selecting the “Merge accounts” option.
   2. **Invite the acquired company’s AWS account to join the existing company’s organization using AWS Organizations.**
   3. Migrate all AWS resources from the acquired company’s AWS account to the master payer account of the existing company.
   4. Create a new AWS account and set it up as the master payer. Move the AWS resources from both the existing and acquired companies’ AWS accounts to the new account.
2. Which of the following are the benefits of AWS Organizations? Choose the 2 correct answers:
   1. **Centrally manage access polices across multiple AWS accounts.**
   2. **Automate AWS account creation and management.**
   3. Analyze cost across all multiple AWS accounts.
   4. Provide technical help (by AWS) for issues in your AWS account.
3. A company has several departments with separate AWS accounts. Which feature would allow the company to enable consolidate billing?
   1. AWS Inspector
   2. AWS Shield
   3. **AWS Organizations**
   4. AWS Lightsail



---



C Billing



1. An organization is planning to create 5 different AWS accounts considering various security requirements. The organization wants to use a single payee account by using the consolidated billing option. Which of the below mentioned statements is true with respect to the above information?
   - Master (Payee) account will get only the total bill and cannot see the cost incurred by each account
   - **Master (Payee) account can view only the AWS billing details of the linked accounts**
   - It is not recommended to use consolidated billing since the payee account will have access to the linked accounts
   - Each AWS account needs to create an AWS billing policy to provide permission to the payee account
2. An organization has setup consolidated billing with 3 different AWS accounts. Which of the below mentioned advantages will organization receive in terms of the AWS pricing?
   - The consolidated billing does not bring any cost advantage for the organization
   - **All AWS accounts will be charged for S3 storage by combining the total storage of each account**
   - EC2 instances of each account will receive a total of 750*3 micro instance hours free
   - The free usage tier for all the 3 accounts will be 3 years and not a single year
3. An organization has added 3 of his AWS accounts to consolidated billing. One of the AWS accounts has purchased a Reserved Instance (RI) of a small instance size in the us-east-1a zone. All other AWS accounts are running instances of a small size in the same zone. What will happen in this case for the RI pricing?
   - Only the account that has purchased the RI will get the advantage of RI pricing
   - One instance of a small size and running in the us-east-1a zone of each AWS account will get the benefit of RI pricing
   - **Any single instance from all the three accounts can get the benefit of AWS RI pricing if they are running in the same zone and are of the same size**
   - If there are more than one instances of a small size running across multiple accounts in the same zone no one will get the benefit of RI
4. An organization is planning to use AWS for 5 different departments. The finance department is responsible to pay for all the accounts. However, they want the cost separation for each account to map with the right cost centre. How can the finance department achieve this?
   - **Create 5 separate accounts and make them a part of one consolidated billing**
   - Create 5 separate accounts and use the IAM cross account access with the roles for better management
   - Create 5 separate IAM users and set a different policy for their access
   - Create 5 separate IAM groups and add users as per the department’s employees
5. An AWS account wants to be part of the consolidated billing of his organization’s payee account. How can the owner of that account achieve this?
   - The payee account has to request AWS support to link the other accounts with his account
   - The owner of the linked account should add the payee account to his master account list from the billing console
   - **The payee account will send a request to the linked account to be a part of consolidated billing** (Check [Process](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/con-bill-tasks.html#conbill_addaccount))
   - The owner of the linked account requests the payee account to add his account to consolidated billing
6. You are looking to migrate your Development (Dev) and Test environments to AWS. You have decided to use separate AWS accounts to host each environment. You plan to link each accounts bill to a Master AWS account using Consolidated Billing. To make sure you keep within budget you would like to implement a way for administrators in the Master account to have access to stop, delete and/or terminate resources in both the Dev and Test accounts. Identify which option will allow you to achieve this goal.
   - Create IAM users in the Master account with full Admin permissions. Create cross-account roles in the Dev and Test accounts that grant the Master account access to the resources in the account by inheriting permissions from the Master account.
   - Create IAM users and a cross-account role in the Master account that grants full Admin permissions to the Dev and Test accounts.
   - **Create IAM users in the Master account. Create cross-account roles in the Dev and Test accounts that have full Admin permissions and grant the Master account access.**
   - Link the accounts using Consolidated Billing. This will give IAM users in the Master account access to resources in the Dev and Test accounts
7. When using consolidated billing there are two account types. What are they?
   - **Paying account and Linked account**
   - Parent account and Child account
   - Main account and Sub account.
   - Main account and Secondary account.
8. A customer needs corporate IT governance and cost oversight of all AWS resources consumed by its divisions. The divisions want to maintain administrative control of the discrete AWS resources they consume and keep those resources separate from the resources of other divisions. Which of the following options, when used together will support the autonomy/control of divisions while enabling corporate IT to maintain governance and cost oversight? Choose 2 answers
   - Use AWS Consolidated Billing and disable AWS root account access for the child accounts. (Need to link accounts and disabling root access is just a best practice)
   - **Enable IAM cross-account access for all corporate IT administrators in each child account.** (Provides IT goverance)
   - Create separate VPCs for each division within the corporate IT AWS account.
   - **Use AWS Consolidated Billing to link the divisions’ accounts to a parent corporate account** (Will provide cost oversight)
   - Write all child AWS CloudTrail and Amazon CloudWatch logs to each child account’s Amazon S3 ‘Log’ bucket (Preferred approach would be to store logs from multiple accounts to a single S3 bucket with CloudTrail for IT Goverance and CloudWatch alerts for Cost Oversight)
9. An organization has 10 departments. The organization wants to track the AWS usage of each department. Which of the below mentioned options meets the requirement?
   1. Setup IAM groups for each department and track their usage
   2. **Create separate accounts for each department, but use consolidated billing for payment and tracking**
   3. Create separate accounts for each department and track them separately
   4. Setup IAM users for each department and track their usage



---



Billing and Cost Management  



1. An organization is using AWS since a few months. The finance team wants to visualize the pattern of AWS spending. Which of the below AWS tool will help for this requirement?
   - AWS Cost Manager
   - **AWS Cost Explorer** (Check [Cost Explorer](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html))
   - AWS CloudWatch
   - AWS Consolidated Billing (Will not help visualize)
2. Your company wants to understand where cost is coming from in the company’s production AWS account. There are a number of applications and services running at any given time. Without expending too much initial development time, how best can you give the business a good understanding of which applications cost the most per month to operate?
   1. Create an automation script, which periodically creates AWS Support tickets requesting detailed intra-month information about your bill.
   2. Use custom CloudWatch Metrics in your system, and put a metric data point whenever cost is incurred.
   3. **Use AWS Cost Allocation Tagging for all resources, which support it. Use the Cost Explorer to analyze costs throughout the month.** (Refer [link](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html))
   4. Use the AWS Price API and constantly running resource inventory scripts to calculate total price based on multiplication of consumed resources over time.
3. You need to know when you spend $1000 or more on AWS. What’s the easy way for you to see that notification?
   1. AWS CloudWatch Events tied to API calls, when certain thresholds are exceeded, publish to SNS.
   2. Scrape the billing page periodically and pump into Kinesis.
   3. **AWS CloudWatch Metrics + Billing Alarm + Lambda event subscription. When a threshold is exceeded, email the manager.**
   4. Scrape the billing page periodically and publish to SNS.
4. A user is planning to use AWS services for his web application. If the user is trying to set up his own billing management system for AWS, how can he configure it?
   1. **Set up programmatic billing access. Download and parse the bill as per the requirement**
   2. It is not possible for the user to create his own billing management service with AWS
   3. Enable the AWS CloudWatch alarm which will provide APIs to download the alarm data
   4. Use AWS billing APIs to download the usage report of each service from the AWS billing console
5. An organization is setting up programmatic billing access for their AWS account. Which of the below mentioned services is not required or enabled when the organization wants to use programmatic access?
   1. Programmatic access
   2. AWS bucket to hold the billing report
   3. **AWS billing alerts**
   4. Monthly Billing report
6. A user has setup a billing alarm using CloudWatch for $200. The usage of AWS exceeded $200 after some days. The user wants to increase the limit from $200 to $400? What should the user do?
   1. Create a new alarm of $400 and link it with the first alarm
   2. It is not possible to modify the alarm once it has crossed the usage limit
   3. **Update the alarm to set the limit at $400 instead of $200** (Refer [link](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html#editing_billing_alarm))
   4. Create a new alarm for the additional $200 amount
7. A user is trying to configure the CloudWatch billing alarm. Which of the below mentioned steps should be performed by the user for the first time alarm creation in the AWS Account Management section?
   1. Enable Receiving Billing Reports
   2. **Enable Receiving Billing Alerts**
   3. Enable AWS billing utility
   4. Enable CloudWatch Billing Threshold



---





































