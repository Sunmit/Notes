# Index #
* 1. [Section 9: AWS CI/CD](#9-AWS-CI/CD)

## Section 1:<a name='9-AWS-CI/CD'/> AWS CI/CD ##
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
  
