# AWS Organisations

* Global service
* Allows to manage multiple AWS accounts
* Main account is the management account
* Other accounts are member accounts
* Consolidated billing across acounts
* Share reserved instances and discounts across accounts

* Organisational Units (OU)
  * Breakdown by business unit
    * Sales
    * Retail
    * Finance
  * Environment
    * Prod
    * Staging
    * Dev
  * Project/product

* Advantages
  * Multi account vs 1 account multi VPC
  * Tagging standards for billing
  * Enable Cloudtrail on all accounts, store logs on central S3
  * Send CloudWatch logs to central S3
  * Cross Account Roles for admin purposes
  * Security: service control policies
    * IAM policies applied to OU or accounts to restrict users/roles
    * Do not apply to management account

# IAM conditions

* Restrict IP addresses from which API calls can be made
* Restrict access to some resources from some regions

# IAM roles vs Resource base Policies

* When you assume a role (user, application or service) you give up your original permissions and take those assigned to the role
* Resource base policies supported by S3 buckets, lambdas, SQS, SNS, etc...

* Eventbridge - when a rule runs it needs permissions on the target
  * Resource based policy - Lambda, SNS, SQS, Cloudwatch logs, API gateway
  * IAM role - Kinesis Stream, ECS task, etc...

# Permission boundary

* Set the maximum permissions for a user or role

# IAM identity center

* One login for 
  * all your AWS accounts in AWS organisations - its mostly useful for this
  * Business Cloud applications
  * EC2 Windows instances

* Permission set
  * Access specific OU 
  
# Microsoft active directory

* Found on any windows server with AD domain services
* Database of objects: user accounts, computers, printers etc...
* Centralised security management
* Objects organised in trees
* A group of trees is a forest

# AWS Directory Services

* AWS managed Microsoft AD
  * Create own AD in AWS
  * Supports multi MFA
* AD Connector
  * Users are managed on the On-Premise AD 
  * Proxy
* Simple AD
  * AD compatible managed directory on AWS
  * Cannot connect to on premise AD

# AWS Control Tower

* Easy way to set up secure and compliant multi account AWS
* AWS Control Tower uses AWS organisations to create accounts

* Benefits:
  * Automate environment setup in a few clicks
  * Detect policy violoateions and remediate them
  * Monitor compliance through interactive dashboard  

* Guardrails
  * Ongoing governence for all accounts
  * Preventative guardrails (SCPs) eg. restrict regions accross accounts
  * Detective Guardrails - using AWS Config (eg. identify untagged resources)


