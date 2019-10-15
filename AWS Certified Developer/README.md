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

## 9 AWS CI/CD ###
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

### 9.1 Continuous Integration ###
- Developers push the code to a code repository often (GitHub / CodeCommit / Bitbucket / etc…)
- A testing / build server checks the code as soon as it’s pushed (CodeBuild / Jenkins CI / etc…)
- The developer gets feedback about the tests and checks that have passed / failed
- Find bugs early, fix bugs
- Deliver faster as the code is tested
- Deploy often
- Happier developers, as they’re unblocked    
![image](https://github.com/Sunmit/Notes/blob/master/AWS%20Certified%20Developer/images/ci-intro.png)

### 9.2 Continuous Delivery ###
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

### 9.3 CodeCommit ###
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
### 9.4 CodePipeline ###
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
### 9.5 CodeBuild ###
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
### 9.6 AWS CodeDeploy ###
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
### 9.7 CodeStar ###   
* CodeStar Overview   
    * CodeStar is an integrated solution that regroups: GitHub, CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CodePipeline, CloudWatch
    * Helps quickly create “CICD-ready” projects for EC2, Lambda, Beanstalk
    * Supported languages: C#, Go, HTML 5, Java, Node.js, PHP , Python, Ruby
    * Issue tracking integration with: JIRA / GitHub Issues
    * Ability to integrate with Cloud9 to obtain a web IDE (not all regions)
    * One dashboard to view all your components
    * Free service, pay only for the underlying usage of other services
    * Limited Customization
