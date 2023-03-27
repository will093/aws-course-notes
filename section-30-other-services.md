# CloudFormation

* Declarative way of outlining AWS infrastructure for any resources
* Infrastructure as code
  * No need to create resources manual
  * All written as config/code
  * Estimate costs based on CloudFormation template

# Amazon SES

* Fully managed service to send emails securely, globally and at scale.
* Allow inbound/outbound emails

# Amazon Pinpoint

* 2 way marketing service
* Email, SMS, push, voice, in-app messaging
* Segment and personalise messages for customrs
* Use cases: run campaigns, sending emails, SMS etc...
* Versus SNS or SES
  * In Pinpoint you can create schedules, message templates, full marketing campaigns etc...

# SSM Session Manager

* Start a secure shell on EC2 or on-premises servers
* No SSH, bastion hosts or SSH keys, or port 22 needed
  * Instead a SSM agent is installed on the EC2 instance

# Systems manager

* Run command
  * Run commands/scripts on groups of EC2 instances using SSM
* Patch manager
  * Automate the process of patching managed instances
  * On demand or on schedule
*  Maintenance window
  *  Define a schedule for when to perform actions on your instances
*  Automation
   *  Simplifies common maintenance tasks for EC2 instances and AWS resources
   *  Runbook - defines actions to be performed

# AWS cost explorer

* Visualize and understand AWS costs over time
* Choose an optimal savings plan based on costs
  * Alternative to reserved instances

# Amazon Elastic Transcoder

* Convert media files stored in S3 into media files in formats required be devices, phones etc...
* ie convert video to iPhone compatible format

# AWS batch

* Fully managed batch processing at any scale
* Efficiently run 100,000s of batch jobs on AWS
* Dynamically launch EC2 instances or Spot Instances
* Uses Docker Images and runs on ECS

# Amazon appflow

* Securely transfer data between 3rd part SAAS applications and AWS
* Save time by not needing to write integrations with the 3rd party services