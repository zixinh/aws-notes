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
- ***API Gateway***
  - [Mapping Template](#api-gateway-mapping-template)
  - [API Canary Deployment](#api-canary-deployment)
  - [Integration with AWS services](#api-gateway-integration-with-aws-services)

**Application Integration**
- ***Step Function***
  - [vs Cloudwatch Events](#step-function-intro-and-comparison)

**Developer Tools**
- ***CodeCommit***
  
  
## End of Index




  
#### EC2 instance connect as an alternative to SSH 
- A browser based terminal 
- Where to access?
  - In AWS console, select an EC2 instance → select ‘Connect’ blue button at the top tabs → select connect with ‘EC2 instance connect’
- Only available for Amazon Linux 2 AMI

[ back to index ](#index)


#### API gateway mapping template
- purpose: Transform content before sending to designated service e.x lambda or before sending back to client
- workflow: Client → method request → integration request e.x. Mapping → aws service e.x. lambda → integration response → method response → client 

[ back to index ](#index)


#### API canary deployment
- Admins can set canary deployment for an API endpoint with same name
- Example  
  - Given existing stage A and canary deployment is enabled
  - when a new change is deployed to stage A, API gateway automatically shift designated percentage of traffic to new API
  - If new change is working well, click ‘promote canary’ to shift all traffic to new API

[ back to index ](#index)


#### API Gateway integration with AWS services
- steps
  - create a api gateway resource
  - create a method upon newly created resource
  - select AWS service as integration type e.x. Step function
  - configure selected AWS service
  
[ back to index ](#index)


#### Step Function Intro and Comparison
- Purpose: **Serverless** orchestration tools using visual workflows
- Features
  - **Visually** manage components of orchestration
  - Manually manage different serverless service are difficult, step functions provide a workflow-based tool to better orchestrate serverless application
- Vs cloudwatch events
  - Cloudwatch events are not solely for serverless service
  - Not solely for orchestration, but simply a platform to connect each service together

[ back to index ](#index)
