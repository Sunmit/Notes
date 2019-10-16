# Index #
* 1. [Section 9: AWS CI/CD](#l9)
* 2. [Section 10: AWS CloudFormation](#l10)

## Section 1:<a name='l9'/> AWS CI/CD ##
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
---
  ## Section 10:<a name='l10'/> AWS CloudFormation ##
  1. You need to specify the order in which your CloudFormation template should create resources   
    true   
    false   
    ***Answer:*** false   
    ***Analysis:*** CloudFormation creates them in the right order, so we no need to figure out ordering and orchestration.
---
  
