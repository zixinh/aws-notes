# aws-study-notes

## Index

#### **Background**
- ****networking****
  - [OSI key takeaways](#osi-key-takeaways)
  - [Purpose of subnet mask](#purpose-of-subnet-mask)
  - [Network Address Translation (NAT)](#network-address-translation)
  - [IPv4 public private addressing](#ipv4-public-private-addressing)
  - [TLS and SSL mechanism](#tls-and-ssl-mechanism)

#### **Compute**
- ***EC2***
  - [EC2 instance connect as an alternative to SSH ](#ec2-instance-connect-as-an-alternative-to-SSH)
  
#### **Storage**
- ***S3***

#### **Database**
- ***ElastiCache***

#### **Security and Identity**
- ***IAM***
  - [IAM roles in container services](#iam-roles-in-container-services)
- ***STS***

#### **Management and Governance**
- ***CloudTrail***
  - [CloudTrail vs Cloudwatch Logs](#cloudtrail-vs-cloudwatch-logs)
  - [CloudTrail Validation](#cloudtrail-validation)
- ***OpsWorks***
  - [OpsWorks vs Cloudformation](#opsworks-vs-cloudformation)
  - [OpsWorks Lifecycle Events](#opsworks-lifecycle-events)

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
  - [IAM roles in container services](#iam-roles-in-container-services)
  - [ECS services auto scaling](#ecs-services-auto-scaling)


## End of Index


#### OSI key takeaways
- 3 main groups
  - group 1 - 2 nodes transmission: layer 1(bit),2(frame) 
    - layer 1 is physical layer -- mechanisms of physical transmission and reception of bits
    - layer 2 is Data link -- manage layer 1 between **2 nodes** -- collision detection, unicast transmission, able to use switches etc.(using MAC)
  - group 2 - multi-nodes transmission: layer 3(packet),4(segment/datagram)
    - layer 3 is network layer -- mechanisms of **multi-nodes** transmission -- routing, traffic control, for multi-node transmission of **single packet**; multiple packets multi-node transmission are managed by layer 4 (using IP protocol)
    - layer 4 is transport layer -- manage layer 3 between multiple nodes for **multiple packets** to ensure reliability and integrity -- acknowledgement, segmentation etc.
  - group 3 - application data: layer 5,6,7 (all data)
    - layer 5 for maintaining a session
    - layer 6 for encode/decode, decrypt/encrypt
    - layer 7 for high-level APIs 
- note: OSI is conceptual, many stuff lays between each layer. e.x. ARP between layer 2 and layer 3 to resolve IP address to MAC address

[ back to topic ](#background)


#### Network Address Translation
- purpose: overcome ipv4 shortage
- type
  - static NAT (e.x. internet gateway): 1 private to 1 fixed public 
  - dynamic NAT: 1 private to first available public 
  - port address translation (e.x. AWS NAT gateway): many private to 1 public using ports to identify
- port address translation mechanism
  - NAT gateway maintains a NAT translation table which maps client's private ipv 4 & port to NAT's public ipv4 & port
  - during connection, NAT translate received packets on a port to corresponding client in private network
  - limit: for outbound traffic only. initial inbound traffic to private device cannot be established if specific port of public ipv4 is not provided. even a specific port is provided, NAT table needs to have a entry to a private device given that public port 

[ back to topic ](#background)

#### Purpose of subnet mask
- appeared purpose: to differentiate network and host 
- real purpose: 
  - to let router know if packet is for local or remote so router can send them correspondingly
  - router can determine next hop target more efficiently given packet destination ip --> reduce the size of route table

[ back to topic ](#background)

#### ipv4 public private addressing
- public addressing
  - Class addressing
    - Class A: first 8 bits as network, rest bits as hosts
    - Class B: first 16 bits as network, rest bits as hosts
    - Class C: first 24 bits as network, rest bits as hosts
    - Class D & E: special usage
  - Classless addressing
    - each first bits as network, rest bits as hosts
    - good for IP subnetting
    - size of /n = 2 * /n+1    
- private addressing
  - why standardize? - to ensure accurate execution of public & private address translation 
  - 10.0.0.0 to 10.255.255.255 (size of 1 class A network)
  - 172.16.0.0 to 172.31.255.255 (size of 16 class B networks)
  - 192.168.0.0 to 192.168.255.255 (size of 256 class C networks)

[ back to topic ](#background)

#### TLS and SSL mechanism
- key players
  - certificte authority (CA) -- independent org that browsers and servers trust
  - browser
  - server
- Basic encryption principles
  - Public keys can be verified as a key from a valid source from anywhere
  - Any message encrypted by a public key can only be decrypted by corresponding private key
- Key phases
  1 client trusts server with help of CA certificates
  2 master key creation and synchronization
  3 Connection is established using master key
- mechanism
  - client sends a SSL/TLS request to server
  - server generates a public & private key pairs and a certificate signing request(CSR) to CA
  - CA returns a signed certificate to server
  - server sends back the signed certificate to client
  - client validates the signed certificate 
  - phase 1 ends ---- server is trusted by client
  - client creates a master key and encrypts it using public key of signed certificate
  - client sends the encrypted master key to server
  - server decrypts the encrypted master key using private key of the key pair
  - phase 2 ends ---- client and server both know the master key for encryption and decryption of future data
  - SSL/TLS connection is established 
  - phase 3 ends

[ back to topic ](#background)


#### EC2 instance connect as an alternative to SSH 
- A browser based terminal 
- Where to access?
  - In AWS console, select an EC2 instance → select ‘Connect’ blue button at the top tabs → select connect with ‘EC2 instance connect’
- Only available for Amazon Linux 2 AMI

[ back to topic ](#compute)


#### CloudTrail vs Cloudwatch Logs
- Difference 1
  - CloudTrail is for detailed API logging, including
    - who made the API call
    - what's the API call request
    - what's the response of API call
  - Cloudwatch logs may capture general info of API calls, but would not capture in great details **if CloudTrail is not enabled** 
- Difference 2 
  - CloudTrail provides search & filter tools to find specific API records in console
  - Cloudwatch does not have such tool and may need to manually set up a database or other programs to do search & filter

[ back to topic ](#management-and-governance)

#### CloudTrail Validation
- digest file vs log file
  - log file: all API records stored in S3 in a timely manner
  - digest file: **hashed** version of log file, stored in S3 once in an hour
- validation
  - aws would compare digest file and log file and see if hashed version is matched with log file by using following CLI command 
```
aws cloudtrail validate-logs --start-time XXXX --trail-arn XXXX --profile XXX
```

[ back to topic ](#management-and-governance)


#### OpsWorks vs Cloudformation
- Difference
  - OpsWorks is for configuration management -- to manage softwares on existing servers
  - Cloudformation is for infrastructure provisioning -- to manage hardwares
- Similarities
  - Both use infrastructure-as-code
  - Both can do a little bit of other's job. Cloudformation can manage configuration as well but not in-depth; OpsWorks can also provision servers but not in-depth.

[ back to topic ](#management-and-governance)

#### OpsWorks lifecycle events
- each layer has a set of 5 lifecycle event and each lifecycle event has associated with a set of Chef recipes
- 5 lifecycle events
  - setup: when a server is booted, run recipes to set up server
  - configure: occurs on **all instances** when
    - an instance enters or leaves an online state, 
    - EIP is associated or disassociated from an instance
    - new load balancer is attached or detached to/from a layer
  - deploy
  - undeploy
  - shutdown
- instance-specific vs instance-universal
  - for setup, deploy, undeploy and shutdown -- instance-specific, that is, recipes are only run upon specific instance that triggered lifecycle event
  - for configure event -- instance-universal, that is, when a single instance has a configrue lifecycle event, the set of recipes are run upon all instances
  
[ back to topic ](#management-and-governance)


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
- storage: Docker Image <--pull/push--> Docker Hub(for common container)/Amazon ECR(for customized/private container)
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

#### IAM roles in container services
- EC2 IAM roles
  - for EC2 communicates to ECS and ECS to EC2 (not for fargate)
- Task IAM roles
  - for containers in tasks to communicate to AWS service
- Service auto scaling IAM role
  - for ECS to use ecsAutoscaleRole

[ back to topic - containers ](#containers)

[ back to topic - security and identity ](#security-and-identity)


#### IAM POLICY trust vs permission and resource based vs identity based
- trust policy vs permission policy
  - each IAM role has 2 policy, one is trust policy and the other is permission policy
  - trust policy is to define who (called principals) can use this role. principals could be roles/users/accounts/services
    - e.x. principals: EKS -- EKS can use this role and able to access to resources and actions by its permission policy
  - permission policy is to define which resource & actions a role can use. resource is a specific resource of an aws service and action refers to specific action of a resource can do
    - e.x. resource: S3 bucket aws-test-bucket-67321 and action is read S3 bucket

- resource-based vs identity-based 
  - both are permission policy, not trust policy


#### ECS services auto scaling
- purpose: if task reaches some limits, auto scale-in or scale-out to accommodate demands
- EC2 vs fargate vs docker-based elastic beanstalk
  - EC2: create auto scaling for both ECS service and EC2 instances
  - fargate: create auto scaling for service only
  - EB: create auto scaling for EC2 only -- tasks automatically created when a new EC2 instance is deployed
- scaling policy:
  - step scaling: more advanced and fine-tuned config when certain alarm is triggered --- able to create detailed steps to adjust alarmed situation
  - target tracking: scale in and out to meet a value of a metric
  
[ back to topic - containers ](#containers)
