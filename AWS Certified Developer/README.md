# Index <a name="l0"/>   
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
---
## Exam  Details
- Deployment:CI/CD,BeanStalk,Serverless
- Security:deep dive + dedicate section
- Development with AWS Services:Serverless,API,SDK&CLI
- Refactoring:Understand all the AWS services for the best migration
- Monitoring and trouble shooting:CloudWatch, CloudTrail, X-Ray

## Install Apache
### command
yum update -y  
yum install -y httpd.x86_64  
systemctl start httpd.service  
systemctl enable httpd.service  
更改安全组 添加http 80端口 inbound  
echo "hello world" > /var/www/html/index.html  
echo "Hello World from $(hostname -f)" > /var/www/html/index.html  

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
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-1.png)   
---
### 13.2 Lambda Overview<a name="l13-2"/>
**Why AWS Lambda**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-2.png)   

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
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-3.png)   
**Example: Serverless Thumbnail creation**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-4.png)   
**Example: Serverless CRON Job**   
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-intro-5.png)    
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
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-config-1.png)   
    * Can set a “reserved concurrency” at the function level 
    * Each invocation over the concurrency limit will trigger a “Throttle”
    * Throttle behavior: 
        * If synchronous invocation => return ThrottleError - 429 
        * If asynchronous invocation => retry automatically and then go to DLQ
* AWS Lambda Retries and DLQ
    * If a lambda function asynchronous invocation fails, it will be retried twice
    * After all retries, unprocessed events go to the Dead Letter Queue 
    * DLQ can be a SNS topic or SQS queue    
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-config-2.png)
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
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-version-1.png)   

### 13.8 AWS Lambda Aliases<a name= "l13-8"/>
* Aliases are ”pointers” to Lambda function versions 
* We can define a “dev”, ”test”, “prod” aliases and have them point at different lambda versions 
* Aliases are mutable 
* Aliases enable Blue / Green deployment by assigning weights to lambda functions 
* Aliases enable stable configuration of our event triggers / destinations 
* Aliases have their own ARNs   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-aliases-1.png)  
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
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-cf-1.png)   

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
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-edge-1.png)   
* You can also generate responses to viewers without ever sending the request to the origin   
**Global application**   
    ![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/lambda-edge-2.png)  
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
## [BACK TO TOP](#l0)
---
