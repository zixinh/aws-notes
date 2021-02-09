# aws-devops-study-notes

## Index

**Background**
**Compute**
- ***EC2***
  - [EC2 instance connect as an alternative to SSH ](#ec2-instance-connect-as-an-alternative-to-SSH)
  
**Storage**
- ***S3***

**Database**
- ***ElastiCache***

**Networking & Content Delivery**
- ***VPC***

**Application Integration**
- ***Step Function***
  - [vs Cloudwatch Events](#step-function-intro-&-comparison)

**Developer Tools**
- ***CodeCommit***
  
  
  
#### EC2 instance connect as an alternative to SSH 
- A browser based terminal 
- Where to access?
  - In AWS console, select an EC2 instance → select ‘Connect’ blue button at the top tabs → select connect with ‘EC2 instance connect’
- Only available for Amazon Linux 2 AMI

[ back to index ](#index)
[ back to topic ](#ec2)


#### EC2 user data
- common purpose: to automate boot tasks and the script would be executed when machine first starts
- user data is the data passed to EC2 machine and can be used by EC2 machine

[ back to index ](#index)
[ back to topic ](#ec2)


#### Step Function Intro & Comparison
- Purpose: Serverless orchestration tools using visual workflows
- Features
  - Visually manage components of orchestration
  - Manually manage different serverless service are difficult, step functions provide a workflow-based tool to better orchestrate serverless application
- Vs cloudwatch events
  - Cloudwatch events are not solely for serverless service
  - Not solely for orchestration, but simply a platform to connect each service together

[ back to index ](#index)
[ back to topic ](#step-function)
