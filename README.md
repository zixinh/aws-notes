# aws-devops-study-notes

## Index

**Background**
#### **Compute**
- ***EC2***
  - [EC2 instance connect as an alternative to SSH ](#ec2-instance-connect-as-an-alternative-to-SSH)
  
#### **Storage**
- ***S3***

#### **Database**
- ***ElastiCache***

#### **Networking and Content Delivery**
- ***VPC***
- ***API Gateway***
  - [Mapping Template](#api-gateway-mapping-template)
  - [API Canary Deployment](#api-canary-deployment)
  - [Integration with AWS services](#api-gateway-integration-with-aws-services)

#### **Application Integration**
- ***Step Function***
  - [vs Cloudwatch Events](#step-function-intro-and-comparison)

#### **Developer Tools**
- ***CodeCommit***
  
#### **Containers**
- ***ECS***
  - [Docker workflow](#docker-workflow)
  - [Port Binding for container and host](#port-binding-for-container-and-host)
  - [service vs task vs container instance vs ECS instance vs cluster](#service-vs-task-vs-container-instance-vs-ECS-instance-vs-cluster)

## End of Index


  
#### EC2 instance connect as an alternative to SSH 
- A browser based terminal 
- Where to access?
  - In AWS console, select an EC2 instance → select ‘Connect’ blue button at the top tabs → select connect with ‘EC2 instance connect’
- Only available for Amazon Linux 2 AMI

[ back to topic ](#compute)


#### API gateway mapping template
- purpose: Transform content before sending to designated service e.x lambda or before sending back to client
- workflow: Client → method request → integration request e.x. Mapping → aws service e.x. lambda → integration response → method response → client 

[ back to topic ](#networking-and-content-delivery)


#### API canary deployment
- Admins can set canary deployment for an API endpoint with same name
- Example  
  - Given existing stage A and canary deployment is enabled
  - when a new change is deployed to stage A, API gateway automatically shift designated percentage of traffic to new API
  - If new change is working well, click ‘promote canary’ to shift all traffic to new API

[ back to topic ](#networking-and-content-delivery)


#### API Gateway integration with AWS services
- steps
  - create a api gateway resource
  - create a method upon newly created resource
  - select AWS service as integration type e.x. Step function
  - configure selected AWS service
  
[ back to topic ](#networking-and-content-delivery)


#### Step Function Intro and Comparison
- Purpose: **Serverless** orchestration tools using visual workflows
- Features
  - **Visually** manage components of orchestration
  - Manually manage different serverless service are difficult, step functions provide a workflow-based tool to better orchestrate serverless application
- Vs cloudwatch events
  - Cloudwatch events are not solely for serverless service
  - Not solely for orchestration, but simply a platform to connect each service together

[ back to topic ](#application-integration)


#### Docker Workflow
- build & run: Dockerfile --build--> Docker Image --run--> Docker Container
- storage: Docker Image <--pull/push--> Docker Hub(for common container)/Amazon ECR(for customized container)
- management:
  - ECS: AWS own platform
  - Fargate: Serverless platform
  - EKS: AWS managed Kubernetes platform
  
[ back to topic ](#containers)

#### Port Binding for container and host
- purpose: a host has many ports for networking, container also has many ports for networking. for external traffics, container need to use host's port to send/receive traffics -- need to bind a host's port.
- steps
  - create a task definition
  - add a container
  - add distinct port mappings for each container
  - if leave host port as empty, host would **randomly** assign a port to the container
  
[ back to topic ](#containers)

#### service vs task vs container instance vs ECS instance vs cluster
- cluster vs service vs task vs container instance
  - cluster is a set of services
  - service is a set of tasks that has the **same task definition**
  - task is a set of container instances e.x.ECS agent
- ECS instance is EC2 instance that containers run upon
  - service and tasks are run upon ECS instance
  
[ back to topic ](#containers)
