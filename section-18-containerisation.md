# Docker

Images stored in Docker repos.

* Docker hub
  * Public repo with base images for many technologies
* Amazon ECR (elastic container registry)
  * Private repository
  * Public repository (Amazon ECR public gallery)

Docker container management:

* Amazon elastic container service (ECS)
  * Amazions own container platform
* Amazon elastic kubernetes service (EKS)
  * Amazon's managed kubernetes
* AWS fargate
  * Amazons own serverless container platform
  * Works with ECS and EKS
* Amazon ECR
  * Store container images

## Docker vs VMs

* Sort of a virtualisation technology, but not entirely
* Resources shared with the host, many containers on one host/server.

## Amazon ECS

* Elastic container service
* Launch Docker containers on AWS - launch ECS tasks on ECS clusters
* EC2 launch type
  * must provision and maintain the EC2 instances
  * AWS takes care of starting and stopping containers
  * Each EC2 instance must run the ECS agent to register in the ECS cluster
* Fargate launch type
  * You do not need to manage the EC2 instances
  * All serverless
  * Create tasks and AWS runs them based on the CPU/RAM you need

# IAM roles for ECS

* EC2 instance profile
  * Used by ECS agent
  * Makes API calls to ECS service
  * Send container logs to Cloudwatch logs
  * Pull docker image from ECR
  * Reference sensitive data in secrets manager or SSM parameter store
* ECS task role
  * Allows each task to have a specific role
  * Use different roles for the different ECS services you run
  * Task role defined in task definition

# ECS - load balancer integrations

* ALB supported for most use cases
* Network Load Balancer
  * Recommend only for high throughput/high perf use cases, or to pair with AWS private link

# Amazon ECS - data volumes (EFS)

* Mount EFS file systems onto ECS tasks
* Work for both EC2 and fargate
* Tasks in same AZ will share the same filesystem
* Fargate + EFS = serverless

# Amazon ECS - auto scaling

* Auto increase/decrease the desired no of ECS tasks
* Amazon ECS auto scaling uses AWS application auto scaling
  * ECS service average CPU utilisation
  * ECS service RAM utilisation
  * ALB request count per target (metric from ALB)
* TargetTracking - scale based on a CloudWatch metric
* Step Scaling - scale based on a CLoudWatch alarm
* Scheduled scaling - scale based on specified date/time

* ECS scaling not the same as EC2 auto scaling
* Fargate auto scaling very easy to set up
* EC2 launch type
  * Scale with ASG scaling
  * Scale with ECS cluster capacity provider
    * Auto provision EC2 instances needed for EC2 tasks

# Amazon ECR (elastic container registry)

* Store and manage Docker images on AWS
* Private + public repository (Amazon ECR public gallery)

# Amazon EKS overview (Amazon elastic Kubernetes service)

* A way to launch managed Kubernetes on AWS
* Kubernetes is an open-source system for auto deployment, scaling and management of containerised (usually Docker) apps.
* Alternative to ECS, similar goal but different API.
* Use case: you already use kubernetes and want to migrate to the Cloud


# Amazon EKS - node types

* managed node groups
  * creates and manages nodes/ec2 instances for you
  * nodes are part of an ASG managed by EKS
  * Supports On demand or spot instances
* self managed nodes
  * Nodes created by you, registered to EKS cluster and managed by an ASG
  * Use prebuilt AMI optimised for EKS
  * Supports on demand or spot instances  
* AWS fargate
  * no maintenance

# Amazon EKS - data volumes

* Specify a storageclass manifest on your EKS cluster
* Leverages a Container Storage Interface (CSI) compliant driver
  

# AWS app runner

* Fully managed service that makes it easy to deploy web applications and APIs at scale
* No infrastructure experience required
* Start with source code or docker container image
* Auto build and deploy app
* Auto scaling, load balancer, highly available, encryption
* VPC access support
* Connect to db, cach, message queue services
* Use cases: web apps, APIs, microservices