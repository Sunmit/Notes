# Index #
* 1. [Section 9: AWS CI/CD](#l9)
* 2. [Section 10: AWS CloudFormation](#l10)
* 3. [Section 11: AWS Monitoring, Troubleshooting & Audit](#l11)
* 4. [Section 12: AWS Integration & Messaging](#l12)
* 5. [Section 13: AWS Lambda](#l13)
---
## Section 3: AWS Fundamentals: IAM + EC2<a name='l3'>
1. An IAM policy document must contain at least: (Select 3)   
  Id   
  Actions   
  Principle   
  Conditions   
  Resources    
  Effect   
  ***Answer:*** Actions, Resources, Effect   
2. Your company decides to adopt AWS S3 as a vault system for storing all the application secrets. For compliance purposes, this vault bucket must be encrypted. Your company wants to avoid the hassle of managing all encryption keys, what type of encryption suits your needs ?   
    Client-Side Encryption   
    Server-Side Encryption - SSE-KMS   
    Server-Side Encryption - SSE-C   
    Server-Side Encryption - SSE-S3   
    ***Answer:*** Server-Side Encryption - SSE-S3   
    ***Explanation:*** SSE-S3: AWS manages both data key and master key | SSE-KMS: AWS manages data key and you manage master key | SSE-C: You manage both data key and master key | Read more: http://amzn.to/2iVsGvM  
3. What is the relationship between an AZ (Availability Zone) and Region? (select two)   
    The Region is located in an AZ   
    The name of AZ is prefixed by the name of its Region   
    The name of the Region is prefixed by the name of its AZ   
    The AZ is located in a Region   
    ***Answer:*** Second & Forth   
    ***Explanation:*** AWS has many regions which are located in more than 10 countries. Each regions contains at least two availability zones. i.e: eu-west-3 is the name of Paris region and eu-west-3a, eu-west-3b and eu-west-3c are names of its AZs.   
---
## Section 6: AWS Fundamentals: Amazon S3<a name='l6'>
1. To have AWS S3 encrypt an object after it is uploaded, you need to add a header to the HTTP request called   
  x-amz-server-side-encryption   
  x-amz-sse   
  x-amz-kms   
  x-amz-encryption   
  ***Answer:*** x-amz-server-side-encryption 
  ***Explanation:*** It is a PUT http request with a set of headers. "x-amz-server-side-encryption" header identifies the type of encryption that should be applied.Read more: https://aws.amazon.com/blogs/security/how-to-prevent-uploads-of-unencrypted-objects-to-amazon-s3/   
2. The total size of all files on an S3 bucket can exceed 5TB   
    Yes
    No
    ***Answer:*** Yes   
    ***Explanation:*** The S3 bucket storage is unlimited. However, the maximum size of a single object (file) is 5TB   

---
## Section 9:<a name='l9'/> AWS CI/CD ##
1. which hook step should be used in appspec.yml file to ensure the application is properly running after being deployed?
  AfterInstall   
  ValidateService   
  Application   
  AllowTraffic      
  ***Answer:*** ValidateService
2. (hard question - think outside the box)   
    You would like to deploy static web files to Amazon S3 automatically. Which services should you use for this?   
    CodeCommit + CodePipeline   
    CodePipeline + CodeBuild   
    CodePipeline + CodeDeploy   
    CodeDeploy   
  ***Answer:*** CodePipeline + CodeBuild   
  ***Analysis:*** CodeBulid can run any commands, so you can use it to run commands and copy your static web files to Amazon S3.   
3. CodePipeline contains a set of actions and each action contains a set of stages   
  No   
  Yes   
  ***Answer:*** No   
  ***Explanation:*** each stage contains a set of actions   
4. Your are deploying your static website to an S3 bucket. You are planning to automate this deployment while reducing the cost of this automation as much as possible. Which AWS services should you consider in this case?   
    CodeBuild + CodePipeline   
    CodeBuild + CodeDeploy   
    CodeBuild + ECS
    CodeDeploy + ECS
    ***Answer:*** CodeBuild + CodePipeline      
    ***Explanation:*** Since it is a deployment to S3, AWS CodeBuild should be enough since static websites can be uploaded to s3 using the AWS CLI and without the need of provisioning an EC2 instance. CodeDeploy is only used to deploy to EC2 instances or Lambda functions
---
 ## Section 10:<a name='l10'/> AWS CloudFormation ##
 1. You need to specify the order in which your CloudFormation template should create resources   
    true   
    false   
    ***Answer:*** false   
    ***Analysis:*** CloudFormation creates them in the right order, so we no need to figure out ordering and orchestration.
2. A Developer at a company is working on a CloudFormation template to set up resources. Resources will be defined using code and provisioned based on conditions. Which section of a CloudFormation template does not allow for conditions?   
    Outputs
    Resources
    Conditions
    Parameters
    ***Answer:*** Parameters
    ***Explanation:***   
    * Correct answer - "Parameters" : Conditions cannot be used within the parameters section. Please visit https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html for more information on the parameter structure.   
    **Conditions can use parameters as an input, but parameters don't have conditions as an input**
    * Incorrect:
        * "Resources" - Associate conditions with the resources that you want to conditionally create
        * "Conditions" - You can define conditions in this section
        * "Outputs" - Associate conditions with the outputs that you want to conditionally create
        * For more information visit https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html
---
  ## Section 11: AWS Monitoring, Troubleshooting & Audit<a name='l11'/>
  1. High Resolution Custom Metrics can have a minimum resolution of   
    1 second   
    10 seconds   
    30 seconds   
    1 minute   
    ***Answer:*** 1 second   
   2. An Alarm on a High Resolution Metric can be triggered as often as    
    1 second   
    10 seconds   
    30 seconds   
    1 minute   
    ***Answer:*** 10 second
   3. My application traces appear in X-Ray when I run the application on my local laptop. When I deploy my application to my Elastic Beanstalk, the traces do not appear in X-Ray. Why?   
    A config file is missing in <span style="color=red">.ebextensions/</span> folder of your code   
    You need to authorize your application from the X-Ray console   
    Your code is wrong   
    ***Answer:*** First   
---
  ## Section 12: AWS Integration & Messaging<a name='l12'>
  1. You are preparing for the biggest day of sale of the year, where your traffic will increase by 100x. You have already setup SQS standard queue. What should you do?   
    Open a support ticket to pre-warm the SQS queue   
    Enable auto scaling in the SQS quequ   
    Increase the capacity of the SQS queue   
    Do nothing, SQS scales automatically   
    ***Answer:*** Forth   
---
  ## Section 13: AWS Lambda<a name='l13'>
---
  ## Section 14: AWS Serverless: DynamoDB<a name='l14'>
  1. Using which AWS API can you return one or more items from one or more DynamoDB tables?   
    Scan   
    BatchGetItem   
    BatchWriteItems   
    Query   
    ***Answer:*** BatchGetItem   
    ***Explanation:*** https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_BatchGetItem.html   
---
## Section 14: AWS Serverless: DynamoDB<a name='l14'>
1. With your team, you decided to migrate data of an application to a NoSQL database. You agreed that the NoSQL technology should be a managed service but also should have the capability to be running on a local environment offline. What is the suitable NoSQL system for this case?   
    MongoDB   
    Neo4j   
    EFS   
    DynamoDB   
    ***Answer:*** DynamoDB   
    ***Explanation:*** MongoDB and Neo4J are not a managed service. EFS is managed service but it is not about NoSQL storage. Besides, DynamoDB can run locally
---
## Section 15: AWS Serverless: API Gateway
& Cognito<a name='l15'>
1. You exposed a REST API of learning management system through AWS API Gateway. One of the resources is "chapters" and the stage is "dev". How will the URL look like ?   
  https://3ef6543d.dev.eu-west-3.api-gateway.com/chapters   
  https://3ef6543d.eu-west-3.api-gateway.com/dev   
  https://3ef6543d.eu-west-3.api-gateway.com/chapters/de   
  https://3ef6543d.eu-west-3.api-gateway.com/dev/chapters   
  ***Answer:*** https://3ef6543d.eu-west-3.api-gateway.com/dev/chapters   
  ***Explanation:*** You need to remember the pattern: https://.execute-api..amazonaws.com//   
2. Identity pools are the containers that Cognito Identity users to keep your apps' federated identities organized.   
  No   
  Yes   
  ***Answer:*** Yes   
3. API gateway methods are GET, POST, PUT and DELETE   
    No, there are more
    Yes
    ***Answer:*** No
    ***Explanation:*** All HTTP methods are supported including HEAD and OPTIONS,... so on. Also, ANY method which catches all cases   

---
## Section 17: ECS, ECR & Fargate - Docker in AWS<a name='l17'>   
1. Which commands must you run to push an existing Docker image to ECR? (select two)   
    docker login -u $AWS_ACCESS_KEY_ID -p $AWS_SECRET_ACCESS_KEY   
    awx docker push 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:lastest   
    docker build -t 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:lastest   
    docker push 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:lastest   
    $(aws ecr get-login --no-include-email)   
    ***Answer:*** Forth & Fifth   
    ***Explanation:*** You must know these two commands. You first run the login command, and then the docker push command.   
2. You deployed a Docker container registry system on an EC2 instance. The maintenance of this EC2 instance and its storage consumes your development time. Your manager asks you to come up with a solution that will save in time spent managing the application on the EC2 instance. What do you suggest?   
    Migrate to EKS   
    Migrate to ECR
    Migrate to Fargate   
    Migrate to ECS
    ***Answer:*** Migrate to ECR   
    ***Explanation:*** ECR is fully-managed services that stores container images   
---
## Section 18: AWS Security & Encryption: KMS, Encryption SDK, SSM Parameter Store, IAM&STS<a name='l18'>
1. Which of the following best describes how KMS Encryption works?
    
