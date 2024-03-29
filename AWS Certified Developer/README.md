# Index <a name="l0"/>
* 3. [Section 3: AWS Fundamentals-IAM&EC2](#l3)
* 4. [Section 4: AWS Fundamentals – Load Balancing, Auto Scaling Groups and EBS Volumes](#l4)
* 9. [Section 9: AWS CI/CD](#l9-aws-cicd)
    * 9.1 [Continuous Integration](#l9-1)
    * 9.2 [Continuous Delivery](#l9-2)
    * 9.3 [CodeCommit](#l9-3)
    * 9.4 [CodePipeline](#l9-4)
    * 9.5 [CodeBuild](#l9-5)
    * 9.6 [AWS CodeDeploy](#l9-6)
    * 9.7 [CodeStar](#l9-7)
* 10. [Section 10: AWS CloudFormation](#l10-aws-cloudformation)
    * 10.1 [CloudFormation Introduce](#l10-1)
    * 10.2 [YAML Crash Course](#l10-2)
    * 10.3 [CloudFormation Rollbacks](#l10-3)
* 11. [Section 11: AWS Monitoring, Troubleshooting & Audit](#l11)
    * 11.1 [Monitoring Overview in AWS](#l11-1)
    * 11.2 [AWS CloudWatch](#l11-2)
        * [Metrics](#l11-2-1)
        * [Logs](#l11-2-3)
        * [Events](#l11-2-4)
        * [Alarms](#l11-2-2)
    * 11.3 [AWS X-Ray](#l11-3)
    * 11.4 [AWS CloudTrail](#l11-4)
    * 11.5 [CloudTrail vs CloudWatch vs X-Ray](#l11-5)
* 12. [Section 12: AWS Integration & Messaging](#l12)
    * 12.1 [Section Introduction](#l12-1)
    * 12.2 [AWS SQS](#l12-2)
    * 12.3 [AWS SNS](#l12-3)
    * 12.4 [AWS Kinesis](#l12-4)
    * 12.5 [SQS vs SNS vs Kinesis](#l12-5)
* 13. [Section 13: AWS Lambda](#l13)
    * 13.1 [Section Overview](#l13-1)
    * 13.2 [Lambda Overview](#l13-2)
    * 13.3 [AWS Lambda Configuration](#l13-3)
    * 13.4 [AWS Lambda Concurrency and Throttling](#l13-4)
    * 13.5 [AWS Lambda Logging, Monitoring and Tracing](#l13-5)
    * 13.6 [AWS Lambda Limits to Know](#l13-6)
    * 13.7 [AWS Lambda Versions](#l13-7)
    * 13.8 [AWS Lambda Aliases](#l13-8)
    * 13.9 [Lambda Function Dependencies](#l13-9)
    * 13.10 [Lambda and CloudFormation](#l13-10)
    * 13.11 [Lambda Functions /tmp space](#l13-11)
    * 13.12 [AWS Lambda Best Practices](#l13-12)
    * 13.13 [Lambda@Edge](#l13-13)
* 14. [Section 14: DynamoDB](#l14)
    * 14.1 [Overview](#l14-1)
    * 14.2 [DynamoDB Intro](#l14-2)
    * 14.3 [DynamoDB – Primary Keys](#l14-3)
    * 14.4 [DynamoDB WCU&RCU-Provisioned Throughput](#l14-4)
    * 14.5 [DynamoDB Basic APIs](#l14-5)
    * 14.6 [DynamoDB – Indexs (GSI+LSI)](#l14-6)
    * 14.7 [DynamoDB Concurrency](#l14-7)
    * 14.8 [DynamoDB - DAX](#l14-8)
    * 14.9 [DynamoDB Streams](#l14-9)
    * 14.10 [DynamoDB -TTL (Time to Live)](#l14-10)
    * 14.11 [DynamoDB CLI – Good to Know](#l14-11)
    * 14.12 [DynamoDB Transactions](#l14-12)
    * 14.13 [DynamoDB – Security & Other Features](#l14-13)
---
## Exam  Details
- Deployment:CI/CD,BeanStalk,Serverless
- Security:deep dive + dedicate section
- Development with AWS Services:Serverless,API,SDK&CLI
- Refactoring:Understand all the AWS services for the best migration
- Monitoring and trouble shooting:CloudWatch, CloudTrail, X-Ray

---
## Section 3: AWS Fundamentals-IAM&EC2<a name="l3"/>
### 3.1 AWS Regions
* AWS has regions all around the world (us-east-1) 
* Each region has availability zones (us- east-1a, us-east-1b…) 
* Each availability zone is a physical data center in the region, but separate from the other ones (so that they’re isolated from disasters) 
* AWS Consoles are region scoped (except IAM, S3 & Route53)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/aws-regions-1.jpg)   
---
### 3.2 IAM 
### 3.2.1 IAM Introduction
* IAM (Identity and Access Management) 
* Your whole AWS security is there: 
    * Users 
    * Groups 
    * Roles 
* Root account should never be used (and shared) 
* Users must be created with proper permissions 
* IAM is at the center of AWS 
* Policies are written in JSON (JavaScript Object Notation)      
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/iam-1.jpg)    
* IAM has a **global** view 
* Permissions are governed by Policies (JSON) 
* MFA (Multi Factor Authentication) can be setup 
* IAM has predefined “managed policies”
* We’ll see IAM policies in details in the future 
* It’s best to give users the minimal amount of permissions they need to perform their job (least privilege principles)   
---
#### 3.2.2 IAM Federation
* Big enterprises usually integrate their own repository of users with IAM 
* This way, one can login into AWS using their company credentials 
* Identity Federation uses the SAML standard (Active Directory)
---
#### 3.2.3 IAM 101 Brain Dump
* One IAM **User** per PHYSICAL PERSON 
* One IAM **Role** per Application 
* IAM credentials should NEVER BE SHARED 
* Never, ever, ever, ever, write IAM credentials in code. EVER. 
* And even less, NEVER EVER EVER COMMIT YOUR IAM credentials 
* **Never use the ROOT account except for initial setup.**
* **Never use ROOT IAM Credentials**   
---
### 3.3 EC2
#### 3.3.1 EC2 Introduction
* EC2 is one of most popular of AWS offering 
* It mainly consists in the capability of : 
    * Renting virtual machines (EC2) 
    * Storing data on virtual drives (EBS) 
    * Distributing load across machines (ELB) 
    * Scaling the services using an auto-scaling group (ASG) 
---
#### 3.3.2 Introduction to Security Groups
* Security Groups are the fundamental of network security in AWS 
* They control how traffic is allowed into or out of our EC2 Machines.      
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/secutity-groups-1.jpg)   
* It is the most fundamental skill to learn to troubleshoot networking issues 
* In this lecture, we’ll learn how to use them to allow, inbound and outbound ports   

* Security groups are acting as a “firewall” on EC2 instances 
* They regulate: 
    * Access to Ports 
    * Authorised IP ranges – IPv4 and IPv6 
    * Control of inbound network (from other to the instance) 
    * Control of outbound network (from the instance to other)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/secutity-groups-2.jpg)     
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/secutity-groups-3.jpg)    
**Security Groups Good to know**   
* Can be attached to multiple instances 
* Locked down to a region / VPC combination 
* Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it 
* It’s good to maintain one separate security group for SSH access 
* If your application is not accessible (time out), then it’s a security group issue 
* If your application gives a “connection refused“ error, then it’s an application error or it’s not launched 
* All inbound traffic is blocked by default 
* All outbound traffic is authorised by default
---
#### 3.3.3 Elastic IP
##### 3.3.3.1 Private vs Public IP (IPv4)
* Networking has two sorts of IPs. IPv4 and IPv6: 
    * IPv4: 1.160.10.240 
    * IPv6: 3ffe: 1900:4545:3:200:f8ff:fe21:67cf 
* In this course, we will only be using IPv4. 
* IPv4 is still the most common format used online. 
* IPv6 is newer and solves problems for the Internet of Things (IoT).
* IPv4 allows for 3.7 billion different addresses in the public space 
* IPv4: [0-255].[0-255].[0-255].[0-255].   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/elastic-ip-1.jpg)   
* Public IP: 
    * Public IP means the machine can be identified on the internet (WWW)
    * Must be unique across the whole web (not two machines can have the same public IP). 
    * Can be geo-located easily 
* Private IP: 
    * Private IP means the machine can only be identified on a private network only 
    * The IP must be unique across the private network 
    * BUT two different private networks (two companies) can have the same IPs. 
    * Machines connect to WWW using an internet gateway (a proxy) 
    * Only a specified range of IPs can be used as private IP
##### 3.3.3.2 Elastic IPs
* When you stop and then start an EC2 instance, it can change its public IP . 
* If you need to have a fixed public IP for your instance, you need an Elastic IP 
* An Elastic IP is a public IPv4 IP you own as long as you don’t delete it
* You can attach it to one instance at a time
* With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account. 
* You can only have 5 Elastic IP in your account (you can ask AWS to increase that).
* Overall, try to avoid using Elastic IP: 
    * hey often reflect poor architectural decisions 
    * Instead, use a random public IP and register a DNS name to it 
    * Or, as we’ll see later, use a Load Balancer and don’t use a public IP
---
#### 3.3.4 Launching an Apache Server on EC2   
yum update -y  
yum install -y httpd.x86_64  
systemctl start httpd.service  
systemctl enable httpd.service  
更改安全组 添加http 80端口 inbound  
echo "hello world" > /var/www/html/index.html  
echo "Hello World from $(hostname -f)" > /var/www/html/index.html   
---
#### 3.3.5 EC2 User Data
* It is possible to bootstrap our instances using an EC2 User data script.
* bootstrapping means launching commands when a machine starts 
* That script is only run once at the instance first start 
* EC2 user data is used to automate boot tasks such as: 
    * Installing updates 
    * Installing software 
    * Downloading common files from the internet 
    * Anything you can think of 
* The EC2 User Data Script runs with the root user
---
#### 3.3.6 EC2 Instance Launch Types
* On Demand Instances: short workload, predictable pricing 
* Reserved Instances: long workloads (>= 1 year) 
* Convertible Reserved Instances: long workloads with flexible instances 
* Scheduled Reserved Instances: launch within time window you reserve 
* Spot Instances: short workloads, for cheap, can lose instances 
* Dedicated Instances: no other customers will share your hardware 
* Dedicated Hosts: book an entire physical server, control instance placement
##### 3.3.6.1 EC2 On Demand
* Pay for what you use (billing per second, after the first minute) 
* Has the highest cost but no upfront payment 
* No long term commitment 
* Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave.
##### 3.3.6.2 EC2 Reserved Instances
* Up to 75% discount compared to On-demand 
* Pay upfront for what you use with long term commitment 
* Reservation period can be 1 or 3 years 
* Reserve a specific instance type 
* Recommended for steady state usage applications (think database) 
* **Convertible Reserved Instance** 
    * can change the EC2 instance type 
    * Up to 54% discount 
* **Scheduled Reserved Instances** 
    * launch within time window you reserve 
    * When you require a fraction of day / week / month
##### 3.3.6.3 EC2 Spot Instances
* Can get a discount of up to 90% compared to On-demand 
* You bid a price and get the instance as long as its under the price 
* Price varies based on offer and demand 
* Spot instances are reclaimed with a 2 minute notification warning when the spot price goes above your bid 
* **Used for batch jobs, Big Data analysis, or workloads that are resilient to failures.**
* **Not great for critical jobs or databases**
##### 3.3.6.4 EC2 Dedicated Hosts
* Physical dedicated EC2 server for your use 
* Full control of EC2 Instance placement 
* Visibility into the underlying sockets / physical cores of the hardware
* Allocated for your account for a 3 year period reservation 
* More expensive 
* Useful for software that have complicated licensing model (BYOL –Bring Your Own License) 
* Or for companies that have strong regulatory or compliance needs
##### 3.3.6.5 EC2 Dedicated Instances
* Instances running on hardware that’s dedicated to you 
* May share hardware with other instances in same account 
* No control over instance placement (can move hardware after Stop / Start)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/ec2-1.jpg) 
##### 3.3.6.6 EC2 Instance Launch Types Compair
* On demand: coming and staying in resort whenever we like, we pay the full price 
* Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount.
* Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time 
* Dedicated Hosts: We book an entire building of the resort
#### 3.3.7 EC2 Pricing
* EC2 instances prices (per hour) varies based on these parameters: 
    * Region you’re in 
    * Instance Type you’re using 
    * On-Demand vs Spot vs Reserved vs Dedicated Host 
    * Linux vs Windows vs Private OS (RHEL, SLES, Windows SQL)
* You are billed by the second, with a minimum of 60 seconds. 
* You also pay for other factors such as storage, data transfer, fixed IP public addresses, load balancing 
* You do not pay for the instance if the instance is stopped   
**EC2 Pricing Example**
* t2.small in US-EAST -1 (VIRGINIA), cost $0.023 per Hour 
* If used for: 
    * 6 seconds, it costs $0.023/60 =  $0.000383 (minimum of 60 seconds)
    * 60 seconds, it costs $0.023/60 =  $0.000383 (minimum of 60 seconds) 
    * 30 minutes, it costs $0.023/2 =  $0.0115 
    * 1 month, it costs $0.023 * 24 * 30 = $16.56 (assuming a month is 30 days) 
    * X seconds (X > 60), it costs $0.023 * X / 3600 
* The best way to know the pricing is to consult the pricing page: https://aws.amazon.com/ec2/pricing/on-demand/
---
#### 3.3.8 AMI
##### 3.3.8.1 AMI Introduction
* As we saw, AWS comes with base images such as: 
    * Ubuntu 
    * Fedora 
    * RedHat 
    * Windows 
    * Etc… 
* These images can be customised at runtime using EC2 User data 
* But what if we could create our own image, ready to go? 
* That’s an AMI – an image to use to create our instances 
* AMIs can be built for Linux or Windows machines
##### 3.3.8.2 Why would you use a custom AMI?
* Using a custom built AMI can provide the following advantages:
    * Pre-installed packages needed 
    * Faster boot time (no need for long ec2 user data at boot time) 
    * Machine comes configured with monitoring / enterprise software
    * Security concerns – control over the machines in the network 
    * Control of maintenance and updates of AMIs over time 
    * Active Directory Integration out of the box 
    * Installing your app ahead of time (for faster deploys when auto-scaling) 
    * Using someone else’s AMI that is optimised for running an app, DB, etc… 
* AMI are built for a specific AWS region (!)
#### 3.3.9 EC2 Instances Overview
##### 3.3.9.1 Introduction
* Instances have 5 distinct characteristics advertised on the website: 
    * The RAM (type, amount, generation) 
    * The CPU (type, make, frequency, generation, number of cores) 
    * The I/O (disk performance, EBS optimisations) 
    * The Network (network bandwidth, network latency) 
    * The Graphical Processing Unit (GPU) 
* It may be daunting to choose the right instance type (there are over 50 of them) - https://aws.amazon.com/ec2/instance-types/
* https://ec2instances.info/ can help with summarizing the types of instances 
* R/C/P/G/H/X/I/F/Z/CR are specialised in RAM, CPU, I/O, Network, GPU 
* M instance types are balanced 
* T2/T3 instance types are “burstable”
##### 3.3.9.2 Burstable Instances (T2)
* AWS has the concept of burstable instances (T2 machines) 
* Burst means that overall, the instance has OK CPU performance. 
* When the machine needs to process something unexpected (a spike in load for example), it can burst, and CPU can be VERY good. 
* If the machine bursts, it utilizes “burst credits”
* If all the credits are gone, the CPU becomes BAD 
* If the machine stops bursting, credits are accumulated over time   

* Burstable instances can be amazing to handle unexpected traffic and getting the insurance that it will be handled correctly 
* If your instance consistently runs low on credit, you need to move to a different kind of non-burstable instance (all the ones described before).
---
3.3.10 EC2 – Checklist
* Know how to SSH into EC2 (and change .pem file permissions) 
* Know how to properly use security groups 
* Know the fundamental differences between private vs public vs elastic IP 
* Know how to use User Data to customize your instance at boot time 
* Know that you can build custom AMI to enhance your OS 
* EC2 instances are billed by the second and can be easily created and thrown away, welcome to the cloud!
---
## Section 4:AWS Fundamentals – Load Balancing, Auto Scaling Groups and EBS Volumes<a name="l4"/>
### 4.1 Load Balancing
#### 4.1.1 Load Balancing Introduction
##### 4.1.1.1 What is load balancing?
* Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream.   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lb-1.jpg) 
##### 4.1.1.2 Why use a load balancer?
* Spread load across multiple downstream instances 
* Expose a single point of access (DNS) to your application 
* Seamlessly handle failures of downstream instances 
* Do regular health checks to your instances 
* Provide SSL termination (HTTPS) for your websites 
* Enforce stickiness with cookies 
* High availability across zones 
* Separate public traffic from private traffic
##### 4.1.1.3 Why use an EC2 Load Balancer?
* An ELB (EC2 Load Balancer) is a managed load balancer 
    * AWS guarantees that it will be working 
    * AWS takes care of upgrades, maintenance, high availability 
    * AWS provides only a few configuration knobs 
* It costs less to setup your own load balancer but it will be a lot more effort on your end. 
* It is integrated with many AWS offerings / services
---
#### 4.1.2 Types of load balancer on AWS
* AWS has 3 kinds of Load Balancers 
* Classic Load Balancer (v1 - old generation) - 2009 
* Application Load Balancer (v2 - new generation) - 2016 
* Network Load Balancer (v2 - new generation) - 2017 
* Overall, it is recommended to use the newer / v2 generation load balancers as they provide more features 
* You can setup internal (private) or external (public) ELBs
---
#### 4.1.3 Health Checks
* Health Checks are crucial for Load Balancers 
* They enable the load balancer to know if instances it forwards traffic to are available to reply to requests 
* The health check is done on a port and a route (/health is common) 
* If the response is not 200 (OK), then the instance is unhealthy   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lb-2.jpg)   
---
#### 4.1.4 Application Load Balancer (v2)
##### 4.1.4.1 Application Load Balancer Intro
* Application load balancers (Layer 7) allow to do: 
    * Load balancing to multiple HTTP applications across machines (target groups) 
    * Load balancing to multiple applications on the same machine (ex: containers) 
    * Load balancing based on route in URL 
    * Load balancing based on hostname in URL
* Basically, they’re awesome for micro services & container-based application (example: Docker & Amazon ECS) 
* Has a port mapping feature to redirect to a dynamic port 
* In comparison, we would need to create one Classic Load Balancer per application before. That was very expensive and inefficient!
##### 4.1.4.2 Application Load Balancer (v2) HTTP Based Traffic
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lb-3.jpg)   
##### 4.1.4.3 Application Load Balancer v2 Good to Know
* Stickiness can be enabled at the target group level 
    * Same request goes to the same instance 
    * Stickiness is directly generated by the ALB (not the application) 
* ALB support HTTP/HTTPS & Websockets protocols 
* The application servers don’t see the IP of the client directly 
    * The true IP of the client is inserted in the header X-Forwarded-For 
    * We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lb-4.jpg)   
---
#### 4.1.5 Network Load Balancer (v2)
* Network load balancers (Layer 4) allow to do: 
    * Forward TCP traffic to your instances 
    * Handle millions of request per seconds 
    * Support for static IP or elastic IP 
    * Less latency ~100 ms (vs 400 ms for ALB) 
* Network Load Balancers are mostly used for extreme performance and should not be the default load balancer you choose 
* Overall, the creation process is the same as Application Load Balancers    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lb-5.jpg)   
---
#### 4.1.6 Load Balancer Good to Know
* Classic Load Balancers are Deprecated 
    * Application Load Balancers for HTTP / HTTPs & Websocket 
    * Network Load Balancer for TCP 
* CLB, ALB, NLB support SSL certificates and provide SSL termination 
* All Load Balancers have health check capability 
* ALB can route on based on hostname / path 
* ALB is a great fit with ECS (Docker)
* Any Load Balancer (CLB, ALB, NLB) has a static host name. Do not resolve and use underlying IP 
* LBs can scale but not instantaneously – contact AWS for a “warm-up”
* NLB directly see the client IP 
* 4xx errors are client induced errors 
* 5xx errors are application induced errors 
    * Load Balancer Errors 503 means at capacity or no registered target 
* If the LB can’t connect to your application, check your security groups!
---
### 4.2 Auto Scaling Group
#### 4.2.1 Auto Scaling Group Intro
* In real-life, the load on your websites and application can change 
* In the cloud, you can create and get rid of servers very quickly 
* The goal of an Auto Scaling Group (ASG) is to: 
    * Scale out (add EC2 instances) to match an increased load 
    * Scale in (remove EC2 instances) to match a decreased load 
    * Ensure we have a minimum and a maximum number of machines running
    * Automatically Register new instances to a load balancer
---
## Section 9:<a name='l9-aws-cicd'/> AWS CI/CD ###
What we’d like is to push our code “in a repository” and have it deployed onto the AWS
-  Automatically
-  The right way
-  Making sure it’s tested before deploying
-  With possibility to go into different stages (dev, test, pre-prod, prod)
-  With manual approval where needed

We’ll learn about
- AWS CodeCommit: storing our code
- AWS CodePipeline: automating our pipeline from code to ElasticBeanstalk
- AWS CodeBuild: building and testing our code
- AWS CodeDeploy: deploying the code to EC2 fleets (not Beanstalk)

### 9.1<a name='l9-1'/> Continuous Integration ###
- Developers push the code to a code repository often (GitHub / CodeCommit / Bitbucket / etc…)
- A testing / build server checks the code as soon as it’s pushed (CodeBuild / Jenkins CI / etc…)
- The developer gets feedback about the tests and checks that have passed / failed
- Find bugs early, fix bugs
- Deliver faster as the code is tested
- Deploy often
- Happier developers, as they’re unblocked    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/ci-intro.png)

### 9.2<a name='l9-2'/> Continuous Delivery ###
- Ensure that the software can be released reliably whenever needed.
- Ensures deployments happen often and are quick
- Shift away from “one release every 3 months” to ”5 releases a day”
- That usually means automated deployment
- - CodeDeploy
- - Jenkins CD
- - Spinnaker
- - Etc…  
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cd-intro.png)  
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/technology-stack-for-cicd.png)

### 9.3<a name='l9-3'/> CodeCommit ###
* CodeCommit Securty  
    * Interactions are done using Git (standard)
    * Authentication in Git:
        * SSH Keys: AWS Users can configure SSH keys in their IAM Console  
        * HTTPS: Done through the AWS CLI Authentication helper or Generating HTTPS credentials  
        * MFA (multi factor authentication) can be enabled for extra safety  
    * Authorization in Git:  
        * IAM Policies manage user / roles rights to repositories  
    * Encryption:
        * Repositories are automatically encrypted at rest using KMS  
        * Encrypted in transit (can only use HTTPS or SSH – both secure)  
    * Cross Account access:
        * Do not share your SSH keys  
        * Do not share your AWS credentials  
        * Use IAM Role in your AWS Account and use AWS STS (with AssumeRole API)  

* CodeCommit vs GitHub  
    * Similarities:
        * Both are git repositories
        * Both support code review (pull requests)
        * GitHub and CodeCommit can be integrated with AWS CodeBuild
        * Both support HTTPS and SSH method of authentication
    * Differences:
        * Security:
            * GitHub: GitHub Users
            * CodeCommit: AWS IAM users & roles,
        * Hosted:
            * GitHub: hosted by GitHub
            * GitHub Enterprise: self hosted on your servers
            * CodeCommit: managed & hosted by AWS
        * UI:
            * GitHub UI is fully featured
            * CodeCommit UI is minimal
* CodeCommit Notifications
    * You can trigger notifications in CodeCommit using **AWS SNS (Simple Notification Service)** or **AWS Lambda** or **AWS CloudWatch Event Rules**
    * Use cases for notifications SNS / AWS Lambda notifications:
        * Deletion of branches
        * Trigger for pushes that happens in master branch
        * Notify external Build System
        * Trigger AWS Lambda function to perform codebase analysis (maybe credentials got committed in the code?)
    * Use cases for CloudWatch Event Rules:
        * Trigger for pull request updates (created / updated / deleted / commented)
        * Commit comment events
        * CloudWatch Event Rules goes into an SNS topic
---
### 9.4<a name='l9-4'/> CodePipeline ###
* Continuous delivery
* Visual workflow
* Source: GitHub / CodeCommit / Amazon S3
* Build: CodeBuild / Jenkins / etc…
* Load Testing: 3rdparty tools
* Deploy: AWS CodeDeploy / Beanstalk / CloudFormation / ECS…
* Made of stages:
    * Each stage can have sequential actions and / or parallel actions
    * Stages examples: Build / Test / Deploy / Load Test / etc…
    * Manual approval can be defined at any stage
    
* AWS CodePipeline Artifacts
    * Each pipeline stage can create ”artifacts”
    * Artifacts are passed stored in Amazon S3 and passed on to the next stage  
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/pipeline-artifacts.png)  
   
* CodePipeline Troubleshooting  
    * CodePipeline state changes happen in AWS CloudWatch Events, which can in return create SNS notifications.
        * Ex: you can create events for failed pipelines
        * Ex: you can create events for cancelled stages
    * If CodePipeline fails a stage, your pipeline stops and you can get information in the console
    * AWS CloudTrail can be used to audit AWS API calls
    * If Pipeline can’t perform an action, make sure the “IAM Service Role” attached does have enough permissions (IAM Policy)
---
### 9.5<a name='l9-5'/> CodeBuild ###
* CodeBuild Overview  
    * Fully managed build service  
    * Alternative to other build tools such as Jenkins  
    * Continuous scaling (no servers to manage or provision – no build queue)
    * Pay for usage: the time it takes to complete the builds
    * Leverages Docker under the hood for reproducible builds
    * Possibility to extend capabilities leveraging our own base Docker images
    * Secure: Integration with KMS for encryption of build artifacts, IAM for build permissions, and VPC for network security, CloudTrail for API calls logging
    * Source Code from GitHub / CodeCommit / CodePipeline / S3…
    * Build instructions can be defined in code (buildspec.yml file)
    * Output logs to Amazon S3 & AWS CloudWatch Logs
    * Metrics to monitor CodeBuild statistics
    * Use CloudWatch Events to detect failed builds and trigger notifications
    * Use CloudWatch Alarms to notify if you need “thresholds” for failures
    * CloudWatch Events / AWS Lambda as a Glue
    * SNS notifications
    * Ability to reproduce CodeBuild locally to troubleshoot in case of errors
    * Builds can be defined within CodePipeline or CodeBuild itself
* How CodeBuild works  
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/how-code-build-works.png)  
   
* CodeBuild BuildSpec
    * ***buildspec.yml*** file must be at the root of your code
    * Define environment variables:
        * Plaintext variables
        * Secure secrets: use SSM Parameter store
    * Phases (specify commands to run):
        * Install: install dependencies you may need for your build
        * Pre build: final commands to execute before build
        * ***Build: actual build commands***
        * Post build: finishing touches (zip output for example)
    * Artifacts: What to upload to S3 (encrypted with KMS)
    * Cache: Files to cache (usually dependencies) to S3 for future build speedup
* CodeBuild Local Build
    * In case of need of deep troubleshooting beyond logs…
    * You can run CodeBuild locally on your desktop (after installing Docker)
    * For this, leverage the CodeBuild Agent
    * [aws docs](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)
---
### 9.6<a name='l9-6'/> AWS CodeDeploy ###
* CodeDeploy Overview
    * We want to deploy our application automatically to many EC2 instances
    * These instances are not managed by Elastic Beanstalk
    * There are several ways to handle deployments using open source tools (Ansible, Terraform, Chef, Puppet, etc…)
    * We can use the managed Service AWS CodeDeploy
* AWS CodeDeploy – Steps to make it work
    * Each EC2 Machine (or On Premise machine) must be running the CodeDeploy Agent
    * The agent is continuously polling AWS CodeDeploy for work to do
    * CodeDeploy sends appspec.yml file.
    * Application is pulled from GitHub or S3
    * EC2 will run the deployment instructions
    * CodeDeploy Agent will report of success / failure of deployment on the instance   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/codedeploy-overview.png) 
* AWS CodeDeploy – Other
    * EC2 instances are grouped by deployment group (dev / test / prod)
    * Lots of flexibility to define any kind of deployments
    * CodeDeploy can be chained into CodePipeline and use artifacts from there
    * CodeDeploy can re-use existing setup tools, works with any application, auto scaling integration
    * Note: Blue / Green only works with EC2 instances (not on premise)
    * Support for AWS Lambda deployments (we’ll see this later)
    * CodeDeploy does not provision resources
* AWS CodeDeploy Primary Components
    * ***Application:*** unique name
    * ***Compute platform:***  EC2/On-Premise or Lambda
    * ***Deployment configuration:***  Deployment rules for success / failures
        * EC2/On-Premise: you can specify the minimum number of healthy instances for the deployment.
        * AWS Lambda: specify how traffic is routed to your updated Lambda function versions.
    * ***Deployment group:*** group of tagged instances (allows to deploy gradually)
    * ***Deployment type:*** In-place deployment or Blue/green deployment:
    * ***IAM instance profile:*** need to give EC2 the permissions to pull from S3 / GitHub
    * ***Application Revision:*** application code + appspec.yml file
    * ***Service role:*** Role for CodeDeploy to perform what it needs
    * ***Target revision:*** Target deployment application version
* AWS CodeDeploy AppSpec
    * File section: how to source and copy from S3 / GitHub to filesystem
    * Hooks: set of instructions to do to deploy the new version (hooks can have timeouts). The order is:
        * ApplicationStop
        * DownloadBundle
        * BeforeInstall
        * AfterInstall
        * ApplicationStart
        * ValidateService: really important
* AWS CodeDeploy Deploy & Hooks Order   
  ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/codedeploy-hooksorder.png)
* AWS CodeDeploy Deployment Config
    * Configs:
        * One a time: one instance at a time, one instance fails => deployment stops
        * Half at a time: 50%
        * All at once: quick but no healthy host, downtime. Good for dev
        * Custom: min healthy host = 75%
    * Failures:
        * Instances stay in “failed state”
        * New deployments will first be deployed to “failed state” instances
        * To rollback: redeploy old deployment or enable automated rollback for failures
    * Deployment Targets:
        * Set of EC2 instances with tags
        * Directly to an ASG
        * Mix of ASG / Tags so you can build deployment segments
        * Customization in scripts with DEPLOYMENT_GROUP_NAME environment variables
* In Place Deployment – Half at a time  
  ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/deployment-half-at-a-time.png) 
* Blue Green Deployment
   ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/deployment-blue-green.png) 


* hands on steps
    * create two IAM roles
        * ***first role:*** create role - choose codeDeploy - choose codeDeploy - next:permission - next:tags - next:review - set a role name(such as CodeDeployServiceRole) - create role
        * ***second role:*** create role - choose EC2 - next:permission - policies search: AmazonS3ReadOnlyAccess - next:tags - next:review - set a role name(such as EC2InstanceRoleForCodeDeploy) - create role
    * create deploy
        * choose CodeDeploy service
        * Deploy - getting started -creat application - set Application name - set Compute platform (EC2/On-premises)
        * create a EC2 instance (configure IAM role the same as ***second role***)
        * set Security Group ssh and http
        * lanunch
    * install codedeploy agent
        * log in ec2 instance
        * excute cli in [shell](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/code/aws-cicd/codedeploy/commands.sh)
    * add tag to EC2 (Key Environment Value Dev)
    * create deployment group in CodeDeploy-Applications 
        * name the group (such as DevelopmentInstances)  
        * select service role (CodeDeployServiceRole)
        * Environment Configuration
            * choose Amazon EC2 Instance 
            * Key Environment Value Dev
            * unselect loadbalance
    * create deployment
        * create s3 bucket
        * update [application file](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/code/aws-cicd/codedeploy/SampleApp_Linux.zip) to s3 bucket
        * fill the s3 file address
        * create deployment
---
### 9.7<a name='l9-7'/> CodeStar ###   
* CodeStar Overview   
    * CodeStar is an integrated solution that regroups: GitHub, CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CodePipeline, CloudWatch
    * Helps quickly create “CICD-ready” projects for EC2, Lambda, Beanstalk
    * Supported languages: C#, Go, HTML 5, Java, Node.js, PHP , Python, Ruby
    * Issue tracking integration with: JIRA / GitHub Issues
    * Ability to integrate with Cloud9 to obtain a web IDE (not all regions)
    * One dashboard to view all your components
    * Free service, pay only for the underlying usage of other services
    * Limited Customization
---
## Section 10:<a name='l10-aws-cloudformation'/> AWS CloudFormation ###
### 10.1<a name='l10-1'/> CloudFormation Introduce ###
* Infrastruction as Code
    * Currently, we have been doing a lot of manual work
    * All this manual work will be very tough to reproduce:
        * In another region
        * in another AWS account
        * Within the same region if everything was deleted
    * Wouldn’t it be great, if all our infrastructure was… code?   
    * That code would be deployed and create / update / delete our infrastructure   
    
* What is CloudFormation
    * CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).
    * For example, within a CloudFormation template, you say:
        * I want a security group
        * I want two EC2 machines using this security group
        * I want two Elastic IPs for these EC2 machines
        * I want an S3 bucket
        * I want a load balancer (ELB) in front of these machines
    * Then CloudFormation creates those for you, in the ***right order***, with the ***exact configuration*** that you specify

   
* Benefits of AWS CloudFormation
    * Infrastructure as code
        * No resources are manually created, which is excellent for control
        * The code can be version controlled for example using git
        * Changes to the infrastructure are reviewed through code
    * Cost
        * Each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you
        * You can estimate the costs of your resources using the CloudFormation template
        * Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
    * Productivity
        * Ability to destroy and re-create an infrastructure on the cloud on the fly
        * Automated generation of Diagram for your templates!
        * Declarative programming (no need to figure out ordering and orchestration)
    * Separation of concern: create many stacks for many apps, and many layers. Ex:
        * VPC stacks
        * Network stacks
        * App stacks
    * Don’t re-invent the wheel
        * Leverage existing templates on the web!
        * Leverage the documentation
* How CloudFormation Works
    * Templates have to be uploaded in S3 and then referenced in CloudFormation
    * To update a template, we can’t edit previous ones. We have to re- upload a new version of the template to AWS
    * Stacks are identified by a name
    * Deleting a stack deletes every single artifact that was created by CloudFormation.
* Deploying CloudFormation templates
    * Manual way:
        * Editing templates in the CloudFormation Designer
        * Using the console to input parameters, etc
    * Automated way:
        * Editing templates in a YAML file
        * Using the AWS CLI (Command Line Interface) to deploy the templates
        * Recommended way when you fully want to automate your flow
* CloudFormation Building Blocks
    * Templates components (one course section for each):
        * 1. ***Resources: your AWS resources declared in the template (MANDATORY)***
        * 2. Parameters: the dynamic inputs for your template
        * 3. Mappings: the static variables for your template
        * 4. Outputs: References to what has been created
        * 5. Conditionals: List of conditions to perform resource creation
        * 6. Metadata
    * Templates helpers:
        * References
        * Functions

* Create Stack Hands On
    * create stack - Template is ready - Upload a [Template file](#https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/code/cloudformation/0-just-ec2.yaml) - set stack name(MyFirstCloudFormateTemplate) - next - create stack
    * update stack - action - update stack - upload a [template](#https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/code/cloudformation/1-ec2-with-sg-eip.yaml) to Amazon S3 - next - next - next - update
    * delete stack 
        * it will delete all the resources in the template
---
### 10.2<a name='l10-2'/> YAML Crash Course ###   
* **10.2.1 Overview**
    * YAML and JSON are the languages you can use for CloudFormation.
    * <span style="color:red">JSON is horrible for CF</span>   
    * <span style="color:green">YAML is great in so many ways</span>  
    * Key value Pairs
    * Nested objects
    * Support Arrays
    * Multi line strings
    * Can include comments!   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/yaml-crash.png)   
---
* 10.2.2 Resources 
    * Introduction     
        * Resources are the core of your CloudFormation template (MANDATORY)
        * They represent the different AWS Components that will be created and configured
        * Resources are declared and can reference each other
        * AWS figures out creation, updates and deletes of resources for us
        * There are over 224 types of resources  (!)
        * Resource types identifiers are of the form:    
    <span style="color:blue">AWS::aws-product-name::data-type-name</span>   

    * Resource Documents
        * [All the Documents](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
        * [EC2 instance Document](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html)   

    * FAQ
        * Can I create a dynamic amount of resources?   
        No, you can’t. Everything in the CloudFormation template has to be declared. You can’t perform code generation there.
        * Is every AWS Service supported?   
        Almost. Only a select few niches are not there yet   
        You can work around that using AWS Lambda Custom Resources.
---
* 10.2.3 Parameters 
    * Introduction
        * Parameters are a way to provide inputs to your AWS CloudFormation template
        * They’re important to know about if:
            * You want to **reuse** your templates across the company
            * Some inputs can not be determined ahead of time
        * Parameters are extremely powerful, controlled, and can prevent errors from happening in your templates thanks to types.

    * When to use a parameter?
        * Ask yourself this:
            * Is this CloudFormation resource configuration likely to change in the future?
            * If so, make it a parameter.
        * You won’t have to re-upload a template to change its content   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-parameters-1.png)   

    * Parameters Settings
        * Parameters can be controlled by all these settings:   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-parameters-2.png)   
    
    * How to Reference a Parameter
        * The <span style="color:blue">Fn::Ref</span> function can be leveraged to reference parameters
        * Parameters can be used anywhere in a template.
        * The shorthand for this in YAML is <span style="color:blue">!Ref</span>
        * The function can also reference other elements within the template   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-parameters-3.png)   

    * Concept: Pseudo Parameters
        * AWS offers us pseudo parameters in any CloudFormation template.
        * These can be used at any time and are enabled by default   
    
    Reference Value | Example Return Value
    ---|---
    AWS::AccountId       | 1234567890
    AWS::NotificationARNs| [arn:aws:sns:us-east-<br>1:123456789012:MyTopic]
    AWS::NotificationARNs|Does not return a value.
    AWS::Region          |us-east-2
    AWS::StackId         |arn:aws:cloudformation:us-east-</br>1:123456789012:stack/MyStack/1c2fa62</br> 0-982a-11e3-aff7-50e2416294e0
    AWS::StackName       |MyStack

---
* 10.2.4 Mapping 
    * Introduction
        * Mappings are fixed variables within your CloudFormation Template.
        * They’re very handy to differentiate between different environments (dev vs prod), regions (AWS regions), AMI types, etc
        * All the values are hardcoded within the template
        * Example:   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-mapping-1.png)   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-mapping-2.png)   

    * mappings vs parameters   
        * Mappings are great when you know in advance all the values that can be taken and that they can be deduced from variables such as   
            * Region
            * Availability Zone
            * AWS Account
            * Environment (dev vs prod)
            * Etc…
        * They allow safer control over the template.
        * Use parameters when the values are really user specific   

    * FindInMap
        * We use Fn::FindInMap to return a named value from a specific key
        * !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-mapping-3.png)     

---
* 10.2.5<a name="l10-6"/> Outputs   
    * What are outputs?
        * The Outputs section declares optional outputs values that we can import into other stacks (if you export them first)!
        * You can also view the outputs in the AWS Console or in using the AWS CLI
        * They’re very useful for example if you define a network CloudFormation, and output the variables such as VPC ID and your Subnet IDs
        * It’s the best way to perform some collaboration cross stack, as you let expert handle their own part of the stack
        * You can’t delete a CloudFormation Stack if its outputs are being referenced by another CloudFormation stack

    * Outputs Example
        * Creating a SSH Security Group as part of one template
        * We create an output that references that security group   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-outputs-1.png)      
---
* 10.2.6 Cross Stack Reference
    * Introduction
        * We then create a second template that leverages that security group
        * For this, we use the <span style="color:blue">Fn::ImportValue<span> function
        * You can’t delete the underlying stack until all the references are deleted too.   
            ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-csr-1.png)   

---
* 10.2.7 Conditions
    * Introduction
        * Conditions are used to control the creation of resources or outputs based on a condition.
        * Conditions can be whatever you want them to be, but common ones are:
            * Environment (dev / test / prod)
            * AWS Region
            * Any parameter value
        * Each condition can reference another condition, parameter value or mapping
    * How to define a condition?   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-condition-1.png)   
        * The logical ID is for you to choose. It’s how you name condition
        * The intrinsic function (logical) can be any of the following:
            * Fn::And
            * Fn::Equals
            * Fn::If
            * Fn::Not
            * Fn::Or
    * Using a Condition
        * Conditions can be applied to resources / outputs / etc…   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-condition-2.png)   

---
* 10.2.8 Intrisic Function
    * Overview
        * [Fn::Ref](#l10-2-8-1)
        * [Fn::GetAtt](#l10-2-8-2)
        * [Fn::FindInMap](#l10-2-8-3)
        * [Fn::ImportValue](#l10-2-8-4)
        * [Fn::Join](#l10-2-8-5)
        * [Fn::Sub](#l10-2-8-6)
        * [Condition Functions](#l10-2-8-7) (Fn::If, Fn::Not, Fn::Equals, etc…)

    * Fn::Ref<a name='l10-2-8-1'/>
        * The <span style="color:blue">Fn::Ref</span> function can be leveraged to reference
            * Parameters => returns the value of the parameter
            * Resources => returns the physical ID of the underlying resource (ex: EC2 ID)
        * The shorthand for this in YAML is <span style="color:blue">!Ref</span>   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-1.png)   
    * Fn::GetAtt<a name='l10-2-8-2'/>  
        * Attributes are attached to any resources you create
        * To know the attributes of your resources, the best place to look at is the documentation.
        * For example: the AZ of an EC2 machine!   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-2.png)   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-3.png)  

    * Fn::FindInMap<a name='l10-2-8-3'/> Accessing Mapping Values   
        * We use **Fn::FindInMap** to return a named value from a specific key
        * !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-4.png)   

    * Fn::ImportValue<a name='l10-2-8-4'/>
        * Import values that are exported in other templates
        * For this, we use the <span style="color:blue">Fn::ImportValue<span> function   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-5.png)   

    * Fn::Join<a name='l10-2-8-5'/>
        * Join values with a delimiter   
        <span style="color:red"> !join [ delimiter, [ comma-delimted list of values ] ]<span>   
        * This creates "a:b:c"   
            !join [ ":", [a, b, c] ]

    * Fn::Sub<a name='l10-2-8-6'/>
        * <span style="color:blue">Fn::Sub</span>, or <span style="color:blue">!Sub</span> as a shorthand, is used to substitute variables from a text. It’s a very handy function that will allow you to fully customize your templates.
        * For example, you can combine <span style="color:blue">Fn::Sub</span> with References or AWS Pseudo variables!
        * String must contain <span style="color:blue">${VariableName}</span> and will substitute them   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-intrisic-6.png)   

    * Condition Functions<a name='l10-2-8-7'/>   
        ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cf-condition-3.png) 
        * The logical ID is for you to choose. It’s how you name condition
        * The intrinsic function (logical) can be any of the following:
            * Fn::And
            * Fn::Equals
            * Fn::If
            * Fn::Not
            * Fn::Or
---
### 10.3<a name='l10-3'/> CloudFormation Rollbacks ###
* Stack Creation Fails:
    * Default: everything rolls back (gets deleted). We can look at the log
    * Option to disable rollback and troubleshoot what happened

* Stack Update Fails:
    * The stack automatically rolls back to the previous known working state
    * Ability to see in the log what happened and error messages

---
## Section 11: AWS Monitoring, Troubleshooting & Audit<a name="l11"> ##

### 11.1 Monitoring Overview in AWS<a name="l11-1"> ###
* Why Monitoring is Important
    * We know how to deploy applications   
        * Safely
        * Automatically
        * Using Infrastructure as Code
        * Leveraging the best AWS components!
    * Our applications are deployed, and our users don’t care how we did it…
    * Our users only care that the application is working!
        * Application latency: will it increase over time?
        * Application outages: customer experience should not be degraded
        * Users contacting the IT department or complaining is not a good outcome
        * Troubleshooting and remediation
    * Internal monitoring: 
        * Can we prevent issues before they happen? 
        * Performance and Cost 
        * Trends (scaling patterns) 
        * Learning and Improvement
* Monitoring in AWS 
    * AWS CloudWatch: 
        * [Metrics](#l11-2-1): Collect and track key metrics 
        * [Logs](#l11-2-3): Collect, monitor, analyze and store log files
        * [Events](#l11-2-4): Send notifications when certain events happen in your AWS
        * [Alarms](#l11-2-2): React in real-time to metrics / events
    * AWS X-Ray:
        * Troubleshooting application performance and errors
        * Distributed tracing of microservices 
    * AWS CloudTrail:
        * Internal monitoring of API calls being made 
        * Audit changes to AWS Resources by your users
---
### 11.2 AWS CloudWatch<a name="l11-2"> ###
#### 11.2.1 AWS CloudWatch Metrics <a name="l11-2-1"/>
##### 11.2.1.1 Metrics Overview #####
* CloudWatch provides metrics for every services in AWS
* **Metric** is a variable to monitor (CPUUtilization, NetworkIn…) 
* Metrics belong to **namespaces**
* **Dimension** is an attribute of a metric (instance id, environment, etc…).
* Up to 10 dimensions per metric
* Metrics have **timestamps**
* Can create CloudWatch dashboards of metrics

##### 11.2.1.2 AWS CloudWatch EC2 Detailed monitoring
* EC2 instance metrics have metrics “every 5 minutes”
* With detailed monitoring (for a cost), you get data “every 1 minute”
* Use detailed monitoring if you want to more prompt scale your ASG! 
* The AWS Free Tier allows us to have 10 detailed monitoring metrics 
* Note: EC2 Memory usage is by default not pushed (must be pushed from inside the instance as a custom metric)

##### 11.2.1.3 AWS CloudWatch Custom Metrics
* Possibility to define and send your own custom metrics to CloudWatch 
* Ability to use dimensions (attributes) to segment metrics 
    * Instance.id 
    * Environment.name
* Metric resolution:
    * Standard: 1 minute
    * High Resolution: up to 1 second (**StorageResolution** API parameter) – Higher cost 
* Use API call **PutMetricData** 
* Use exponential back off in case of throttle errors

#### 11.2.2 AWS CloudWatch Alarms<a name="l11-2-2"/>

* Alarms are used to trigger notifications for any metric 
* Alarms can go to Auto Scaling, EC2 Actions, SNS notifications 
* Various options (sampling, %, max, min, etc…) 
* Alarm States: 
    * OK 
    * INSUFFICIENT_DATA 
    * ALARM 
* Period:
    * Length of time in seconds to evaluate the metric 
    * High resolution custom metrics: can only choose 10 sec or 30 sec

#### 11.2.3 AWS CloudWatch Logs<a name="l11-2-3"/>

* Applications can send logs to CloudWatch using the SDK 
* CloudWatch can collect log from: 
    * Elastic Beanstalk: collection of logs from application 
    * ECS: collection from containers 
    * AWS Lambda: collection from function logs 
    * VPC Flow Logs: VPC specific logs 
    * API Gateway 
    * CloudTrail based on filter 
    * CloudWatch log agents: for example on EC2 machines 
    * Route53: Log DNS queries 
* CloudWatch Logs can go to: 
    * Batch exporter to S3 for archival 
    * Stream to ElasticSearch cluster for further analytics
* CloudWatch Logs can use filter expressions 
* Logs storage architecture: 
    * Log groups: arbitrary name, usually representing an application 
    * Log stream: instances within application / log files / containers 
* Can define log expiration policies (never expire, 30 days, etc..) 
* Using the AWS CLI we can tail CloudWatch logs 
* To send logs to CloudWatch, make sure IAM permissions are correct!
* Security: encryption of logs using KMS at the Group Level

#### 11.2.4 AWS CloudWatch Events<a name="l11-2-4"/>

* Schedule: Cron jobs 
* Event Pattern: Event rules to react to a service doing something 
    * Ex: CodePipeline state changes! 
* Triggers to Lambda functions, SQS/SNS/Kinesis Messages 
* CloudWatch Event creates a small JSON document to give information about the change

---
### 11.3 AWS X-Ray <a name="l11-3"/>
#### 11.3.1 AWS X-Ray Overview
* Debugging in Production, the good old way: 
    * Test locally 
    * Add log statements everywhere 
    * Re-deploy in production
* Log formats differ across applications using CloudWatch and analytics is hard. 
* Debugging: monolith “easy”, distributed services “hard”
* No common views of your entire architecture! 
* Enter… AWS X-Ray!

**Visual analysis of our applications**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cw-xray-1.png)   

#### 11.3.2 AWS X-Ray Advantages
* Troubleshooting performance (bottlenecks) 
* Understand dependencies in a microservice architecture 
* Pinpoint service issues 
* Review request behavior 
* Find errors and exceptions 
* Are we meeting time SLA? 
* Where I am throttled? 
* Identify users that are impacted

#### 11.3.3 X-Ray compatibility
* AWS Lambda 
* Elastic Beanstalk 
* ECS 
* ELB 
* API Gateway 
* EC2 Instances or any application server (even on premise)

#### 11.3.4 AWS X-Ray Leverages Tracing   
* Tracing is an end to end way to following a “request”
* Each component dealing with the request adds its own “trace”
* Tracing is made of segments (+ sub segments) 
* Annotations can be added to traces to provide extra-information 
* Ability to trace: 
    * Every request 
    * Sample request (as a % for example or a rate per minute) 
* X-Ray Security: 
    * IAM for authorization 
    * KMS for encryption at rest
    
#### 11.3.5 AWS X-Ray How to enable it?
1.  Your code (Java, Python, Go, Node.js, .NET) must import the AWS X-Ray SDK
    * Very little code modification needed 
    * The application SDK will then capture: 
        * Calls to AWS services 
        * HTTP / HTTPS requests 
        * Database Calls (MySQL, PostgreSQL, DynamoDB) 
        * Queue calls (SQS) 
2. Install the X-Ray daemon or enable X-Ray AWS Integration 
    * X-Ray daemon works as a low level UDP packet interceptor (Linux / Windows / Mac…) 
    * AWS Lambda / other AWS services already run the X-Ray daemon for you
    * Each application must have the IAM rights to write data to X-Ray   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cw-xray-2.png)   

#### 11.3.6 The X-Ray magic
* X-Ray service collects data from all the different services 
* Service map is computed from all the segments and traces 
* X-Ray is graphical, so even non technical people can help troubleshoot   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/cw-xray-1.png) 

#### 11.3.7 AWS X-Ray Troubleshooting
* If X-Ray is not working on EC2 
    * Ensure the EC2 IAM Role has the proper permissions 
    * Ensure the EC2 instance is running the X-Ray Daemon 
* To enable on AWS Lambda: 
    * Ensure it has an IAM execution role with proper policy (AWSX-RayWriteOnlyAccess) 
    * Ensure that X-Ray is imported in the code
   
#### 11.3.8 X-Ray Additional Exam Tips
* The X-Ray daemon / agent has a config to send traces cross account: 
    * make sure the IAM permissions are correct – the agent will assume the role
    * This allows to have a central account for all your application tracing 
* Segments: each application / service will send them 
* Trace: segments collected together to form an end-to-end trace 
* Sampling: decrease the amount of requests sent to X-Ray, reduce cost
* Annotations: Key Value pairs used to **index** traces and use with **filters**   
* Metadata: Key Value pairs, **not indexed**, not used for searching   
* Code must be instrumented to use the AWS X-Ray SDK (interceptors, handlers, http clients) 
* IAM role must be correct to send traces to X-Ray    
* X-Ray on EC2 / On-Premise: 
    * Linux system must run the X-Ray daemon 
    * IAM instance role if EC2, other AWS credentials on on-premise instance 
* X-Ray on Lambda: 
    * Make sure X-Ray integration is ticked on Lambda (Lambda runs the daemon) 
    * IAM role is lambda role 
* X-Ray on Beanstalk: 
    * Set configuration on EB console 
    * Or use a beanstalk extension (.ebextensions/xray-daemon.config) 
* X-Ray on ECS / EKS / Fargate (Docker): 
    * Create a Docker image that runs the Daemon / or use the official X-Ray Docker image 
    * Ensure port mappings & network settings are correct and IAM task roles are defined
---
### 11.4 AWS CloudTrail<a name="l11-4"/>
* Provides governance, compliance and audit for your AWS Account 
* CloudTrail is enabled by default! 
* Get an history of events / API calls made within your AWS Account by:
    * Console 
    * SDK 
    * CLI 
    * AWS Services 
* Can put logs from CloudTrail into CloudWatch Logs 
* If a resource is deleted in AWS, look into CloudTrail first!
---
### 11.5 CloudTrail vs CloudWatch vs X-Ray<a name="l11-5"/>
* CloudTrail: 
    * Audit API calls made by users / services / AWS console 
    * Useful to detect unauthorized calls or root cause of changes 
* CloudWatch: 
    * CloudWatch Metrics over time for monitoring 
    * CloudWatch Logs for storing application log 
    * CloudWatch Alarms to send notifications in case of unexpected metrics 
* X-Ray: 
    * Automated Trace Analysis & Central Service Map Visualization 
    * Latency, Errors and Fault analysis 
    * Request tracking across distributed systems
---
## Section 12: AWS Integration & Messaging<a name="l12"/>
### 12.1 Section Introduction<a name="l12-1"/>
* When we start deploying multiple applications, they will inevitably need to communicate with one another 
* There are two patterns of application communication   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-intro-1.png)   
* Synchronous between applications can be problematic if there are sudden spikes of traffic 
* What if you need to suddenly encode 1000 videos but usually it’s 10? 
* In that case, it’s better to decouple your applications, 
    * using SQS: queue model 
    * using SNS: pub/sub model 
    * using Kinesis: real-time streaming model
* These services can scale independently from our application!
---
### 12.2 AWS SQS<a name="l12-2"/>   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-1.png)    
#### 12.2.1 Standard Queue
* Oldest offering (over 10 years old) 
* Fully managed • Scales from 1 message per second to 10,000s per second
* Default retention of messages: 4 days, maximum of 14 days 
* No limit to how many messages can be in the queue 
* Low latency (<10 ms on publish and receive) 
* Horizontal scaling in terms of number of consumers 
* Can have duplicate messages (at least once delivery, occasionally) 
* Can have out of order messages (best effort ordering) 
* Limitation of 256KB per message sent

#### 12.2.2 Delay Queue
* Delay a message (consumers don’t see it immediately) up to 15 minutes
* Default is 0 seconds (message is available right away) 
* Can set a default at queue level 
* Can override the default using the **DelaySeconds** parameter
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-2.png) 
#### 12.2.3 Producing Messages
* Define Body 
* Add message attributes (metadata – optional) 
* Provide Delay Delivery (optional) 
* Get back 
* Message identifier 
* MD5 hash of the body   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-3.png)
#### 12.2.4 Consuming Messages
* Consumers…
* Poll SQS for messages (receive up to 10 messages at a time) 
* Process the message within the visibility timeout 
* Delete the message using the message ID & receipt handle   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-4.png)   

#### 12.2.5 Visibility timeout
* When a consumer polls a message from a queue, the message is “invisible” to other consumers for a defined period… the **Visibility Timeout**: 
    * Set between 0 seconds and 12 hours (default 30 seconds) 
    * If too high (15 minutes) and consumer fails to process the message, you must wait a long time before processing the message again 
    * If too low (30 seconds) and consumer needs time to process the message (2 minutes), another consumer will receive the message and the message will be processed more than once
* **ChangeMessageVisibility** API to change the visibility while processing a message
* **DeleteMessage** API to tell SQS the message was successfully processed

#### 12.2.6 Dead Letter Queue
* If a consumer fails to process a message within the Visibility Timeout…the message goes back to the queue! 
* We can set a threshold of how many times a message can go back to the queue – it’s called a “redrive policy”
* After the threshold is exceeded, the message goes into a dead letter queue (DLQ) 
* We have to create a DLQ first and then designate it dead letter queue 
* Make sure to process the messages in the DLQ before they expire!    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-5.png) 
#### 12.2.7 Long Polling
* When a consumer requests message from the queue, it can optionally “wait” for messages to arrive if there are none in the queue 
* This is called Long Polling 
* **LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application.**
* The wait time can be between 1 sec to 20 sec (20 sec preferable) 
* Long Polling is preferable to Short Polling 
* Long polling can be enabled at the queue level or at the API level using **WaitTimeSeconds**    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-6.png)   
#### 12.2.8 SQS Message consumption flow diagram   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-7.png)   
#### 12.2.9 AWS SQS – FIFO Queue
* Newer offering (First In - First out) – not available in all regions! 
* Name of the queue must end in .fifo 
* Lower throughput (up to 3,000 per second with batching, 300/s without) 
* Messages are processed in order by the consumer 
* Messages are sent exactly once 
* No per message delay (only per queue delay)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-8.png) 

#### 12.2.10 FIFO - Features   
* Deduplication: (not send the same message twice) 
    * Provide a MessageDeduplicationId with your message 
    * De-duplication interval is 5 minutes 
    * Content based duplication: the MessageDeduplicationId is generated as the SHA-256 of the message body (not the attributes) 
* Sequencing: 
    * To ensure strict ordering between messages, specify a MessageGroupId
    * Messages with different Group ID may be received out of order 
    * E.g. to order messages for a user, you could use the ”user_id” as a group id 
    * Messages with the same Group ID are delivered to one consumer at a time

#### 12.2.11 SQS Extended Client
* Message size limit is 256KB, how to send large messages? 
* Using the SQS Extended Client (Java Library)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sqs-9.png)   

#### 12.2.12 AWS SQS Security
* Encryption in flight using the HTTPS endpoint 
* Can enable SSE (Server Side Encryption) using KMS 
    * Can set the CMK (Customer Master Key) we want to use 
    * Can set the data key reuse period (between 1 minute and 24 hours) 
        * Lower and KMS API will be used often 
        * Higher and KMS API will be called less 
    * SSE only encrypts the body, not the metadata (message ID, timestamp, attributes) 
* IAM policy must allow usage of SQS 
* SQS queue access policy 
    * Finer grained control over IP 
    * Control over the time the requests come in

#### 12.2.13 SQS – Must know API
* CreateQueue, DeleteQueue 
* PurgeQueue: delete all the messages in queue 
* SendMessage, ReceiveMessage, DeleteMessage 
* ChangeMessageVisibility: change the timeout 
* Batch APIs for SendMessage, DeleteMessage, ChangeMessageVisibility helps decrease your costs
 
#### 12.2.14 AWS SQS Use Cases
* Decouple applications (for example to handle payments asynchronously)
* Buffer writes to a database (for example a voting application) 
* Handle large loads of messages coming in (for example an email sender) 
* SQS can be integrated with Auto Scaling through CloudWatch!

### 12.3 AWS SNS<a name="l12-3"/>
#### 12.3.1 AWS SNS Overview   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sns-1.png)   
* The “event producer” only sends message to one SNS topic 
* As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications 
* Each subscriber to the topic will get all the messages (note: new feature to filter messages) 
* Up to 10,000,000 subscriptions per topic 
* 100,000 topics limit 
* Subscribers can be: 
    * SQS • HTTP / HTTPS (with delivery retries – how many times) 
    * Lambda 
    * Emails 
    * SMS messages 
    * Mobile Notifications
#### 12.3.2 SNS integrates with a lot of Amazon Products
* Some services can send data directly to SNS for notifications 
* CloudWatch (for alarms) 
* Auto Scaling Groups notifications 
* Amazon S3 (on bucket events) 
* CloudFormation (upon state changes => failed to build, etc) 
* Etc…

#### 12.3.3 AWS SNS – How to publish
* Topic Publish (within your AWS Server – using the SDK) 
    * Create a topic 
    * Create a subscription (or many) 
    * Publish to the topic 
* Direct Publish (for mobile apps SDK) 
    * Create a platform application 
    * Create a platform endpoint 
    * Publish to the platform endpoint 
    * Works with Google GCM, Apple APNS, Amazon ADM…
#### 12.3.4 SNS + SQS: Fan Out    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-sns-2.png)   
* Push once in SNS, receive in many SQS 
* Fully decoupled 
* No data loss 
* Ability to add receivers of data later 
* SQS allows for delayed processing 
* SQS allows for retries of work 
* May have many workers on one queue and one worker on the other queue
---
### 12.4 AWS Kinesis<a name="l12-4"/>
#### 12.4.1 AWS Kinesis Overview
* **Kinesis** is a managed alternative to Apache Kafka 
* Great for application logs, metrics, IoT, clickstreams 
* Great for “real-time” big data 
* Great for streaming processing frameworks (Spark, NiFi, etc…) 
* Data is automatically replicated to 3 AZ 

* **Kinesis Streams**: low latency streaming ingest at scale 
* **Kinesis Analytics**: perform real-time analytics on streams using SQL 
* **Kinesis Firehose**: load streams into S3, Redshift, ElasticSearch…   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-1.png)   

#### 12.4.2 Kinesis Streams 
##### 12.4.2.1 Kinesis Streams Overview
* Streams are divided in ordered Shards / Partitions   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-2.png)   
* Data retention is 1 day by default, can go up to 7 days 
* Ability to reprocess / replay data 
* Multiple applications can consume the same stream 
* Real-time processing with scale of throughput 
* Once data is inserted in Kinesis, it can’t be deleted (immutability)

##### 12.4.2.2 Kinesis Streams Shards
* One stream is made of many different shards 
* 1MB/s or 1000 messages/s at write PER SHARD 
* 2MB/s at read PER SHARD 
* Billing is per shard provisioned, can have as many shards as you want
* Batching available or per message calls. 
* The number of shards can evolve over time (reshard / merge) 
* **Records are ordered per shard**    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-3.png)   
#### 12.3.3 AWS Kinesis API
##### 12.3.3.1 AWS Kinesis API – Put records
* PutRecord API + Partition key that gets hashed 
* The same key goes to the same partition (helps with ordering for a specific key) 
* Messages sent get a “sequence number”
* Choose a partition key that is highly distributed (helps prevent “hot partition”) 
    * user_id if many users 
    * Not country_id if 90% of the users are in one country 
* Use Batching with PutRecords to reduce costs and increase throughput 
* **ProvisionedThroughputExceeded** if we go over the limits 
* Can use CLI, AWS SDK, or producer libraries from various frameworks   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-4.png)    
##### 12.4.3.2 AWS Kinesis API – Exceptions
* ProvisionedThroughputExceeded Exceptions 
    * Happens when sending more data (exceeding MB/s or TPS for any shard
    * Make sure you don’t have a hot shard (such as your partition key is bad and too much data goes to that partition) 
* Solution: 
    * Retries with backoff 
    * Increase shards (scaling) 
    * Ensure your partition key is a good one
##### 12.4.3.3 AWS Kinesis API – Consumers
* Can use a normal consumer (CLI, SDK, etc…) 
* Can use Kinesis Client Library (in Java, Node, Python, Ruby, .Net) 
    * KCL uses DynamoDB to checkpoint offsets 
    * KCL uses DynamoDB to track other workers and share the work amongst shards   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-5.png)   

##### 12.4.3.4 Kinesis KCL in Depth
* Kinesis Client Library (KCL) is Java library that helps read record from a Kinesis Streams with distributed applications sharing the read workload 
* **Rule: each shard is be read by only one KCL instance** 
* Means 4 shards = max 4 KCL instances 
* Means 6 shards = max 6 KCL instances 
* Progress is checkpointed into DynamoDB (need IAM access) 
* KCL can run on EC2, Elastic Beanstalk, on Premise Application 
* **Records are read in order at the shard level**
KCL Example: 4 shards    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-6.png)   
KCL Example: 4 shards, scaling KCL app   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-7.png)   
KCL Example: 6 shards, scaling Kinesis   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-8.png)    
KCL Example: 6 shards, scaling KCL   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-kinesis-9.png)   
##### 12.3.3.5 Kinesis Security
* Control access / authorization using IAM policies 
* Encryption in flight using HTTPS endpoints 
* Encryption at rest using KMS 
* Possibility to encrypt / decrypt data client side (harder) 
* VPC Endpoints available for Kinesis to access within VPC   
#### 12.4.4 AWS Kinesis Data Analytics
* Perform real-time analytics on Kinesis Streams using SQL 
* Kinesis Data Analytics: 
* Auto Scaling 
* Managed: no servers to provision 
* Continuous: real time 
* Pay for actual consumption rate 
* Can create streams out of the real-time queries   
#### 12.4.5 AWS Kinesis Firehose
* Fully Managed Service, no administration 
* Near Real Time (60 seconds latency) 
* Load data into Redshift / Amazon S3 / ElasticSearch / Splunk 
* Automatic scaling 
* Support many data format (pay for conversion) 
* Pay for the amount of data going through Firehose
### 12.5 SQS vs SNS vs Kinesis<a name="l12-5"/>   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/im-vs-1.png)    
---
## Section 13 AWS Lambda<a name="l13"/>
### 13.1 Section Overview <a name="l13-1"/>
**What’s serverless?** 
* Serverless is a new paradigm in which the developers don’t have to manage servers anymore… 
* They just deploy code 
* They just deploy… functions ! 
* Initially... Serverless == FaaS (Function as a Service) 
* Serverless was pioneered by AWS Lambda but now also includes anything that’s managed: “databases, messaging, storage, etc.”   
* **Serverless does not mean there are no servers…**   
   it means you just don’t manage / provision / see them

**Serverless in AWS**
* AWS Lambda & Step Functions 
* DynamoDB 
* AWS Cognito 
* AWS API Gateway 
* Amazon S3 
* AWS SNS & SQS 
* AWS Kinesis 
* Aurora Serverless   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-1.jpg)   
---
### 13.2 Lambda Overview<a name="l13-2"/>
**Why AWS Lambda**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-2.jpg)   

**Benefits of AWS Lambda**
* Easy Pricing: 
* Pay per request and compute time 
* Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
* Integrated with the whole AWS Stack 
* Integrated with many programming languages 
* Easy monitoring through AWS CloudWatch 
* Easy to get more resources per functions (up to 3GB of RAM!) 
* Increasing RAM will also improve CPU and network!

**AWS Lambda language support**
* Node.js (JavaScript) 
* Python 
* Java (Java 8 compatible) 
* C# (.NET Core) 
* Golang 
* C# / Powershell

**AWS Lambda Integrations Main ones**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-3.jpg)   
**Example: Serverless Thumbnail creation**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-4.jpg)   
**Example: Serverless CRON Job**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-5.jpg)    
**AWS Lambda Pricing (as of June 2018, us-east-1 region)**
* You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/ 
* Pay per **calls**: 
    * First 1,000,000 requests are free 
    * $0.20 per 1 million requests thereafter ($0.0000002 per request) 
* Pay per **duration**: (in increment of 100ms) 
    * 400,000 GB-seconds of compute time per month if FREE 
    * == 400,000 seconds if function is 1GB RAM 
    * == 3,200,000 seconds if function is 128 MB RAM 
    * After that $1.00 for 600,000 GB-seconds 
* It is usually very cheap to run AWS Lambda so it’s very popular
---
### 13.3 AWS Lambda Configuration<a name= "l13-3"/>
* Configuration
    * Timeout: default 3 seconds, max of 300s (Note: new limit 15 minutes) 
    * Environment variables 
    * Allocated memory (128M to 3G) 
    * Ability to deploy within a VPC + assign security groups 
    * IAM execution role must be attached to the Lambda function  
---
### 13.4 AWS Lambda Concurrency and Throttling<a name= "l13-4"/>
* Concurrency   
    * Concurrency: up to 1000 executions (can be increased through ticket)   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-config-1.jpg)   
    * Can set a “reserved concurrency” at the function level 
    * Each invocation over the concurrency limit will trigger a “Throttle”
    * Throttle behavior: 
        * If synchronous invocation => return ThrottleError - 429 
        * If asynchronous invocation => retry automatically and then go to DLQ
* AWS Lambda Retries and DLQ
    * If a lambda function asynchronous invocation fails, it will be retried twice
    * After all retries, unprocessed events go to the Dead Letter Queue 
    * DLQ can be a SNS topic or SQS queue    
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-config-2.jpg)
    * The original event payload is sent to the DLQ 
    * This is an easy way to debug what’s wrong with your functions in production without changing the code 
    * Make sure the IAM execution role is correct for your Lambda function asynchronous trigger Retry loop
---
### 13.5 AWS Lambda Logging, Monitoring and Tracing<a name= "l13-5"/>
* CloudWatch: 
    * AWS Lambda execution logs are stored in AWS CloudWatch Logs 
    * AWS Lambda metrics are displayed in AWS CloudWatch Metrics 
    * Make sure your AWS Lambda function has an execution role with an IAM policy that authorizes writes to CloudWatch 
* X-Ray: 
    * It’s possible to trace Lambda with X-Ray 
    * Enable in Lambda configuration (runs the X-Ray daemon for you) 
    * Use AWS SDK in Code 
    * Ensure Lambda Function has correct IAM Execution Role
---
### 13.6 AWS Lambda Limits to Know<a name= "l13-6"/>
* Execution: 
    * Memory allocation: 128 MB – 3008 MB (64 MB increments) 
    * Maximum execution time: 300 seconds (5 minutes), now 15 minutes but 5 for exam 
    * Disk capacity in the “function container” (in /tmp): 512 MB
    * Concurrency limits: 1000 
* Deployment: 
    * Lambda function deployment size (compressed .zip): 50 MB 
    * Size of uncompressed deployment (code + dependencies): 250 MB 
    * Can use the /tmp directory to load other files at startup 
    * Size of environment variables: 4 KB
---
### 13.7 AWS Lambda Versions<a name= "l13-7"/>
* When you work on a Lambda function, we work on **$LATEST** 
* When we’re ready to publish a Lambda function, we create a version
* Versions are immutable
* Versions have increasing version numbers 
* Versions get their own ARN (Amazon Resource Name) 
* Version = code + configuration (nothing can be changed - immutable) 
* Each version of the lambda function can be accessed   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-version-1.jpg)   

### 13.8 AWS Lambda Aliases<a name= "l13-8"/>
* Aliases are ”pointers” to Lambda function versions 
* We can define a “dev”, ”test”, “prod” aliases and have them point at different lambda versions 
* Aliases are mutable 
* Aliases enable Blue / Green deployment by assigning weights to lambda functions 
* Aliases enable stable configuration of our event triggers / destinations 
* Aliases have their own ARNs   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-aliases-1.jpg)  
---
### 13.9 Lambda Function Dependencies<a name= "l13-9"/>
* If your Lambda function depends on external libraries: for example AWS X-Ray SDK, Database Clients, etc…
* **You need to install the packages alongside your code and zip it together**
    * For Node.js, use npm & “node_modules” directory 
    * For Python, use pip --target options 
    * For Java, include the relevant .jar files 
* Upload the **zip** straight to Lambda if less than 50MB, else to S3 first 
* Native libraries work: they need to be compiled on Amazon Linux
---
### 13.10 Lambda and CloudFormation<a name= "l13-10"/>
* You must store the Lambda zip in S3 
* You must refer the S3 zip location in the CloudFormation code   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-cf-1.jpg)   

### 13.11 Lambda Functions /tmp space<a name= "l13-11"/>
* If your Lambda function needs to download a big file to work…
* If your Lambda function needs disk space to perform operations…
* You can use the /tmp directory • Max size is 512MB 
* The directory content remains when the execution context is frozen, providing transient cache that can be used for multiple invocations (helpful to checkpoint your work) 
* For permanent persistence of object (non temporary), use S3

### 13.12 AWS Lambda Best Practices<a name= "l13-12"/>
* **Perform heavy-duty work outside of your function handler** 
    * Connect to databases outside of your function handler
    * Initialize the AWS SDK outside of your function handler 
    * Pull in dependencies or datasets outside of your function handler 
* **Use environment variables for:** 
    * Database Connection Strings, S3 bucket, etc… do not put these values directly in your code 
    * Passwords, sensitive values… they can be encrypted using KMS
* **Minimize your deployment package size to its runtime necessities.**
    * Break down the function if need be 
    * Remember the AWS Lambda limits
* **Avoid using recursive code, never have a Lambda function call itself**
* **Don't put your Lambda function in a VPC unless you have to**
---
### 13.13 Lambda@Edge<a name= "l13-13"/>
* You have deployed a CDN using CloudFront 
* What if you wanted to run a global AWS Lambda alongside? 
* Or how to implement request filtering before reaching your application? 
* For this, you can use Lambda@Edge: deploy Lambda functions alongside your CloudFront CDN 
    * Build more responsive applications 
    * You don’t manage servers, Lambda is deployed globally 
    * Customize the CDN content 
    * Pay only for what you use
* You can use Lambda to change CloudFront requests and responses: 
    * After CloudFront receives a request from a viewer (viewer request)
    * Before CloudFront forwards the request to the origin (origin request)
    * After CloudFront receives the response from the origin (origin response) 
    * Before CloudFront forwards the response to the viewer (viewer response)   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-edge-1.jpg)   
* You can also generate responses to viewers without ever sending the request to the origin   
**Global application**   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-edge-2.jpg)  
**Use Cases**
* Website Security and Privacy 
* Dynamic Web Application at the Edge 
* Search Engine Optimization (SEO) 
* Intelligently Route Across Origins and Data Centers 
* Bot Mitigation at the Edge 
* Real-time Image Transformation 
* A/B Testing 
* User Authentication and Authorization 
* User Prioritization 
* User Tracking and Analytics
---
## Section 14: DynamoDB<a name="l14"/>
### 14.1 Overview<a name="l14-1"/>
**Traditional Architecture**   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-intro-1.jpg)   
* Traditional applications leverage RDBMS databases 
* These databases have the SQL query language 
* Strong requirements about how the data should be modeled 
* Ability to do join, aggregations, computations 
* Vertical scaling (means usually getting a more powerful CPU / RAM / IO)   
**NoSQL databases**
* NoSQL databases are non-relational databases and are distributed 
* NoSQL databases include MongoDB, DynamoDB, etc.
* NoSQL databases do not support join 
* All the data that is needed for a query is present in one row 
* NoSQL databases don’t perform aggregations such as “SUM”
* NoSQL databases scale **horizontally** 
* There’s no “right or wrong” for NoSQL vs SQL, they just require to model the data differently and think about user queries differently
---
### 14.2 DynamoDB Intro<a name="l14-2"/>
* Fully Managed, Highly available with replication across 3 AZ 
* NoSQL database - not a relational database 
* Scales to massive workloads, distributed database 
* Millions of requests per seconds, trillions of row, 100s of TB of storage
* Fast and consistent in performance (low latency on retrieval) 
* Integrated with IAM for security, authorization and administration 
* Enables event driven programming with DynamoDB Streams 
* Low cost and auto scaling capabilities   
**Basics**   
* DynamoDB is made of tables 
* Each table has a primary key (must be decided at creation time) 
* Each table can have an infinite number of items (= rows) 
* Each item has attributes (can be added over time – can be null) 
* Maximum size of a item is 400KB 
* Data types supported are: 
    * Scalar Types: String, Number, Binary, Boolean, Null 
    * Document Types: List, Map 
    * Set Types: String Set, Number Set, Binary Set   
---
### 14.3 DynamoDB – Primary Keys<a name="l14-3"/>
* **Option 1: Partition key only (HASH)** 
* Partition key must be unique for each item 
* Partition key must be “diverse” so that the data is distributed 
* Example: user_id for a users table   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-pk-1.jpg)   
* **Option 2: Partition key + Sort Key** 
* The combination must be unique 
* Data is grouped by partition key 
* Sort key == range key 
* Example: users-games table 
    * user_id for the partition key 
    * game_id for the sort key   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-pk-2.jpg)    
**Partition Keys exercise**
* We’re building a movie database 
* What is the best partition key to maximize data distribution? 
    * movie_id • producer_name 
    * leader_actor_name • movie_language 
* movie_id has the highest cardinality so it’s a good candidate
* movie_language doesn’t take many values and may be skewed towards English so it’s not a great partition key
---
### 14.4 DynamoDB WCU&RCU-Provisioned Throughput<a name="l14-4"/>
**Provisioned Throughput**
* Table must have provisioned read and write capacity units 
* **Read Capacity Units (RCU)**: throughput for reads 
* **Write Capacity Units (WCU)**: throughput for writes 
* Option to setup auto-scaling of throughput to meet demand 
* Throughput can be exceeded temporarily using “burst credit”
* If burst credit are empty, you’ll get a “ProvisionedThroughputException”.
* It’s then advised to do an exponential back-off retry   
**Write Capacity Units**   
* One write capacity unit represents one write per second for an item up to **1 KB** in size. 
* If the items are larger than **1 KB**, more WCU are consumed 
* **Example 1**: we write 10 objects per seconds of 2 KB each. 
    * We need 2 * 10 = 20 WCU 
* **Example 2**: we write 6 objects per second of 4.5 KB each 
    * We need 6 * 5 = 30 WCU  (4.5 gets rounded to the upper KB) 
* **Example 3**: we write 120 objects per minute of 2 KB each 
    * We need 120 / 60 * 2 = 4 WCU    
**Strongly Consistent Read vs Eventually Consistent Read**
* **Eventually Consistent Read**: If we read just after a write, it’s possible we’ll get unexpected response because of replication 
* **Strongly Consistent Read**: If we read just after a write, we will get the correct data 
* **By default**: DynamoDB uses Eventually Consistent Reads, but GetItem, Query & Scan provide a “ConsistentRead” parameter you can set to True   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-rcu-1.jpg)      
**Read Capacity Units**
* One read capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. 
* If the items are larger than 4 KB, more RCU are consumed 
* **Example 1**: 10 strongly consistent reads per seconds of 4 KB each 
    * We need 10 * 4 KB / 4 KB = 10 RCU 
* **Example 2**: 16 eventually consistent reads per seconds of 12 KB each 
    * We need (16 / 2) * ( 12 / 4 ) = 24 RCU 
* **Example 3**: 10 strongly consistent reads per seconds of 6 KB each 
    * We need 10 * 8 KB / 4 = 20 RCU (we have to round up 6 KB to 8 KB)   
**Partitions Internal**
* Data is divided in partitions 
* Partition keys go through a hashing algorithm to know to which partition they go to 
* To compute the number of partitions: 
    * By capacity: (TOTAL RCU / 3000) + (TOTAL WCU / 1000) 
    * By size: Total Size / 10 GB 
    * Total partitions = CEILING(MAX(Capacity, Size)) 
* **WCU and RCU are spread evenly between partitions**   

**DynamoDB -Throttling**   
* If we exceed our RCU or WCU, we get   
**ProvisionedThroughputExceededExceptions**
* Reasons: 
    * Hot keys: one partition key is being read too many times (popular item for ex) 
    * Hot partitions: 
    * Very large items: remember RCU and WCU depends on size of items 
* Solutions: 
    * Exponential back-off when exception is encountered (already in SDK)
    * Distribute partition keys as much as possible 
    * If RCU issue, we can use DynamoDB Accelerator (DAX)
---
### 14.5 DynamoDB Basic APIs<a name="l14-5"/>
**DynamoDB – Writing Data**   
* **PutItem** - Write data to DynamoDB  (create data or full replace)
    * Consumes WCU 
* **UpdateItem** – Update data in DynamoDB (partial update of attributes)
    * Possibility to use Atomic Counters and increase them 
* **Conditional Writes**: 
    * Accept a write / update only if conditions are respected, otherwise reject 
    * Helps with concurrent access to items 
    * No performance impact   
**DynamoDB – Deleting Data**
* **DeleteItem** 
    * Delete an individual row 
    * Ability to perform a conditional delete 
* **DeleteTable** 
    * Delete a whole table and all its items 
    * Much quicker deletion than calling DeleteItem on all items     
    
**DynamoDB – Batching Writes**
* **BatchWriteItem** 
    * Up to 25 PutItem and / or DeleteItem in one call 
    * Up to 16 MB of data written 
    * Up to 400 KB of data per item 
* Batching allows you to save in latency by reducing the number of API calls done against DynamoDB 
* Operations are done in parallel for better efficiency 
* It’s possible for part of a batch to fail, in which case we have the try the failed items (using exponential back-off algorithm)   
**DynamoDB – Reading Data**   
* **GetItem**: 
    * Read based on Primary key 
    * Primary Key = HASH or HASH-RANGE 
    * Eventually consistent read by default 
    * Option to use strongly consistent reads (more RCU - might take longer)
    * ProjectionExpression can be specified to include only certain attributes 
* **BatchGetItem**: 
    * Up to 100 items 
    * Up to 16 MB of data 
    * Items are retrieved in parallel to minimize latency   
**DynamoDB – Query**
* Query returns items based on: 
    * PartitionKey value (must be = operator) 
    * SortKey value (=, <, <=, >, >=, Between, Begin) – optional
    * FilterExpression to further filter (client side filtering)
* Returns: 
    * Up to 1 MB of data 
    * Or number of items specified in Limit
* Able to do pagination on the results
* Can query table, a local secondary index, or a global secondary index   
**DynamoDB - Scan**   
* **Scan** the entire table and then filter out data (inefficient) 
* Returns up to 1 MB of data – use pagination to keep on reading 
* Consumes a lot of RCU 
* Limit impact using Limit or reduce the size of the result and pause
* For faster performance, use parallel scans: 
    * Multiple instances scan multiple partitions at the same time
    * Increases the throughput and RCU consumed
    * Limit the impact of parallel scans just like you would for Scans
* Can use a ProjectionExpression + FilterExpression (no change to RCU)
---   
### 14.6 DynamoDB – Indexs (GSI+LSI)<a name="l14-6"/>
**DynamoDB – LSI (Local Secondary Index)**
* Alternate range key for your table, local to the hash key 
* Up to five local secondary indexes per table.
* The sort key consists of exactly one scalar attribute.
* The attribute that you choose must be a scalar String, Number, or Binary
* **LSI must be defined at table creation time**      
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-index-1.jpg)    
**DynamoDB – GSI (Global Secondary Index)**   
* To speed up queries on non-key attributes, use a Global Secondary Index
* GSI = partition key + optional sort key 
* The index is a new “table” and we can project attributes on it 
    * The partition key and sort key of the original table are always projected (KEYS_ONLY) 
    * Can specify extra attributes to project (INCLUDE) 
    * Can use all attributes from main table (ALL) 
* Must define RCU / WCU for the index 
* Possibility to add / modify GSI (not LSI)
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-index-2.jpg)    
**DynamoDB Indexes and Throttling**
* GSI: 
    * **If the writes are throttled on the GSI, then the main table will be throttled!** 
    * Even if the WCU on the main tables are fine 
    * Choose your GSI partition key carefully! 
    * Assign your WCU capacity carefully! 
* LSI: 
    * Uses the WCU and RCU of the main table 
    * No special throttling considerations   
---
### 14.7 DynamoDB Concurrency<a name="l14-7"/>
* DynamoDB has a feature called “Conditional Update / Delete”
* That means that you can ensure an item hasn’t changed before altering it
* That makes DynamoDB an optimistic locking / concurrency database   
 ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-concurrency-1.jpg)   
---
### 14.8 DynamoDB - DAX<a name="l14-8"/>
* DAX = DynamoDB Accelerator 
* Seamless cache for DynamoDB, no application re- write 
* Writes go through DAX to DynamoDB 
* Micro second latency for cached reads & queries 
* Solves the Hot Key problem (too many reads) 
* 5 minutes TTL for cache by default 
* Up to 10 nodes in the cluster 
* Multi AZ (3 nodes minimum recommended for production) 
* Secure (Encryption at rest with KMS, VPC, IAM, CloudTrail…)   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-dax-1.jpg)   
---
### 14.9 DynamoDB Streams<a name="l14-9"/>
* Changes in DynamoDB (Create, Update, Delete) can end up in a DynamoDB Stream 
* This stream can be read by AWS Lambda, and we can then do: 
    * React to changes in real time (welcome email to new users) 
    * Analytics 
    * Create derivative tables / views 
    * Insert into ElasticSearch 
* Could implement cross region replication using Streams • Stream has 24 hours of data retention   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-stream-1.jpg)   
---
### 14.10 DynamoDB -TTL (Time to Live)<a name="l14-10"/>
* TTL = automatically delete an item after an expiry date / time 
* TTL is provided at no extra cost, deletions do not use WCU / RCU 
* TTL is a background task operated by the DynamoDB service itself 
* Helps reduce storage and manage the table size over time 
* Helps adhere to regulatory norms 
* TTL is enabled per row (you define a TTL column, and add a date there) 
* DynamoDB typically deletes expired items within 48 hours of expiration 
* Deleted items due to TTL are also deleted in GSI / LSI 
* DynamoDB Streams can help recover expired items
### 14.11 DynamoDB CLI – Good to Know<a name="l14-11"/>
* --projection-expression : attributes to retrieve 
* --filter-expression : filter results 
* General CLI pagination options including DynamoDB / S3: 
    * Optimization: 
        * --page-size : full dataset is still received but each API call will request less data (helps avoid timeouts) 
    * Pagination: 
        * --max-items : max number of results returned by the CLI. Returns NextToken 
        * --starting-token: specify the last received NextToken to keep on reading
---
### 14.12 DynamoDB Transactions<a name="l14-12"/>
* New feature from November 2018 
* Transaction = Ability to Create / Update / Delete multiple rows in different tables at the same time 
* It’s an “all or nothing” type of operation. 
* Write Modes: Standard, Transactional 
* Read Modes: Eventual Consistency, Strong Consistency, Transactional 
* Consume 2x of WCU / RCU   
AccountBalanceTable     
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-transaction-1.jpg)  
BankTransactionsTable
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/dynamodb-transaction-2.jpg)   
A transaction is a write to both table, or none!
---
### 14.13 DynamoDB – Security & Other Features<a name="l14-13"/>
* Security: 
    * VPC Endpoints available to access DynamoDB without internet 
    * Access fully controlled by IAM 
    * Encryption at rest using KMS 
    * Encryption in transit using SSL / TLS 
* Backup and Restore feature available 
    * Point in time restore like RDS 
    * No performance impact 
* Global Tables 
    * Multi region, fully replicated, high performance 
* Amazon DMS can be used to migrate to DynamoDB (from Mongo, Oracle, MySQL, S3, etc…) 
* You can launch a local DynamoDB on your computer for development purposes
---
## Section 15 API Gateway
### 15.1 AWS API Gateway Overview
**Building a Serverless API**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/api-overview-1.jpg)   
* AWS Lambda + API Gateway: No infrastructure to manage 
* Handle API versioning (v1, v2…) 
* Handle different environments (dev, test, prod…) 
* Handle security (Authentication and Authorization) 
* Create API keys, handle request throttling 
* Swagger / Open API import to quickly define APIs 
* Transform and validate requests and responses 
* Generate SDK and API specifications 
* Cache API responses   
**API Gateway Integrations**
* Outside of VPC: 
    * AWS Lambda (most popular / powerful) 
    * Endpoints on EC2 
    * Load Balancers 
    * Any AWS Service 
    * External and publicly accessible HTTP endpoints
* Inside of VPC: 
    * AWS Lambda in your VPC 
    * EC2 endpoints in your VPC
### 15.2 AWS API Gateway Stages and Deployment   
####15.2.1 Deployment Stages
* Making changes in the API Gateway does not mean they’re effective 
* You need to make a “deployment” for them to be in effect 
* It’s a common source of confusion 
* Changes are deployed to “Stages” (as many as you want) 
* Use the naming you like for stages (dev, test, prod) 
* Each stage has its own configuration parameters 
* Stages can be rolled back as a history of deployments is kept
---
#### 15.2.2 API Gateway – Stage Variables
* Stage variables are like environment variables for API Gateway 
* Use them to change often changing configuration values 
* They can be used in: 
    * Lambda function ARN 
    * HTTP Endpoint 
    * Parameter mapping templates 
* Use cases: 
    * Configure HTTP endpoints your stages talk to (dev, test, prod…) 
    * Pass configuration parameters to AWS Lambda through mapping templates 
* Stage variables are passed to the ”context” object in AWS Lambda
---
## [BACK TO TOP](#l0)
---
