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
---
## Section 6: AWS Fundamentals: Amazon S3<a name='l6'>
1. To have AWS S3 encrypt an object after it is uploaded, you need to add a header to the HTTP request called   
  x-amz-server-side-encryption   
  x-amz-sse   
  x-amz-kms   
  x-amz-encryption   
  ***Answer:*** x-amz-server-side-encryption 
  ***Explanation:*** It is a PUT http request with a set of headers. "x-amz-server-side-encryption" header identifies the type of encryption that should be applied.Read more: https://aws.amazon.com/blogs/security/how-to-prevent-uploads-of-unencrypted-objects-to-amazon-s3/
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
---
  ## Section 10:<a name='l10'/> AWS CloudFormation ##
  1. You need to specify the order in which your CloudFormation template should create resources   
    true   
    false   
    ***Answer:*** false   
    ***Analysis:*** CloudFormation creates them in the right order, so we no need to figure out ordering and orchestration.
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
  
