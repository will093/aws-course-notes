# Encryption overview

* Encryption in flight (SSL)
  * SSL certificates help with encryption
  * Protects againts man in the middle attack
* Server encryption at rest
  * Data encrypted after being received
  * Decrypted before being sent
  * Encryption keys must be managed somewhere that the server has access to

# Client side encryption

* Client encrypts data before sending and decrypts it after recieving.
* Server does not encrypt or decrypt

# AWS KMS (Key Management Service)

* "encryption" on an AWS service usually uses KMS
* AWS manages encryption keys for us
* Fully integrated with IAM for auth
* Able to audit KMS key usage with CloudTrail
* Seamlessly integrate into AWS services

# KMS key types

* Symettric keys
  * Single encryption key used to encrypt/decrypt
  * AWS services integrated with KMS use symmetric keys
  * No access to unencrypted KMS key

* Asymettric keys
  * Public (encrypt) and private (decrypt) key pair
  * Used for encrypt/decrypt, sign/verify operations
  * Public key only is downloadable
  * Use case: encryption outside of AWS by users who can't call KMS API

* Key types
  * AWS owned keys (default key)
  * AWS managed key: free 
  * Customer managed keys imported: $1 per month
  * Pay for API calls to KMS

* Automatic key rotation
  * AWS managed key - auto every 1 year
  * Customer managed KMS key - auto every year
  * Imported KMS key - manual rotation using alias

* Copying snapshots across regions
  * KMS keys regions specific
  * When copying, AWS will decrypt and then encrypt in the new region

* KMS key policies
  * Control access to KMS keys, similar to S3 bucket policies
  * Cannot control access without them
  * Default KMS Key Policy
    * Created if you don't provide one
    * Complete access to the key to route user and entire AWS account
  * Custom KMS Key Policy
    * Define users, roles, that can access the KMS key
    * Define who can administer the key
    * Useful for cross-account access to the key'

* Copying snapshots across accounts
  * Create a snapshot encrypted with KMS key (custome managed key)
  * Attach a KMS key policy to authorise cross-account access
  * Share the encrypted snapshot with target account
  * Create a copy of the snapshot in target account, encrypt with a new CMK

* KMS multi region keys
  * Identical KMS keys in different AWS regions, that can be used interchangeably
  * Multi-region keys have the same key ID, key material, automatic rotation
  * Encrypt in one region and decrpyt in another region
  * No need to re-encrypt or make cross-region API calls
  * KMS Multi Region keys are not global (primary + replicas)
  * Each multi region key is managed independently
  * Use cases:
    * Global client side encryption, encrypt Global DynamoDB, Global Aurora

* S3 replication encryption
  * Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
  * Objects encrypted with SSE-C (customer provided) never replicated

* Objects encrypted with SSE-KMS
  * Specify the KMS key to encrypt with
  * Adapt KMS key policy for target key
  * IAM role with kms:Decrypt for the source KMS key and kms:Encrypt for target KMS key
  * May get KMS throttling errors (one request per object?)
* You can use multi-region AWS KMS keys, but they are treated as independent keys by S3 (ie. object will be decrypted and then encrypted when copying between regions)

# SSM Parameter store

* Secure storage for configuration and secrets
* Optional encryption using KMS
* Serverless
* Version tracking
* Security through IAM
* Notifications with Amazon EventBridge
* Integration with CloudFormation

* SSM Parameter store - hierarchy
  * organised in nested folders
  * Can access secrets of secrets manager

* Parameter policies
  * Allow to assign a TTL to a parameter to force updating or deleting sensitive data such as passwords
  * Can assign multiple policies at a time

# AWS Secrets Manager

* Newer service meant for storing secrets
* Force rotation of secrets every X days, automate generation of secrets
* Integrate with Amazon RDS
* Secrets encrypted with KMS
* Mostly for RDS integration
* Supports multi-region secrets
  * Automatically keeps secrets in sync across regions
  * Use cases:
    * Multi region apps
    * Disaster recovery
    * Multi region DB 

# AWS Certificate Manager (ACM)

* Easily provision, manage and deploy TLS certificates
* Provide in flight encryption
* Supports public and private TLS certs
* Auto renewal
* Integrations with:
  * Elastic Load Balancer
  * CloudFront Distributions
  * APIs on API gateway
* Cannot use ACM with EC2

# ACM - requesting public certificates

* List domains to be included in the certificate
  * Fully qualified domain name
  * Wildcard domain
* Select Validation: DNS validation or email validation
* Will take a few hours to get verified
* Public certificate will be enrolled for auto renewal

# ACM - importing public certificate

* No auto renewal - must import a new cert before expiry
* ACM sends daily expiration events from 45 days before expiry
* AWS config has a managed rule to check for expiring certificates (acm-certificate-expiration-check)

# ACM - integrate with ALB

* ALB can redirect from http to https

# ACM - integrate with API gateway

* Create a custom domain name in API gateway
* Edge-optimized: for global clients
  * Request routed through CloudFront Edge locations
  * API gateway still lives in only 1 region
  * TLS cert must be in the same region as CloudFront, in us-east-1
  * The setup CNAME or A-Alias in Route 53
* Regional
  * For clients in the same region
  * TLS cert must be imported on API Gateway, in same region as the API stage
  * The setup CNAME or A-Alias in Route 53

# AWS WAF - web application firewall

* Protects your web application from Layer 7 (HTTP) exploits

* Deploy on
  * Application load balancer
  * API gateway
  * CloudFront
  * AppSync GraphQL API
  * Cognito User Pool

* Define Web ACL (web access control list) rules:
  * IP set: up to 10,000 IP addresses - multiple rules for more IPs
  * HTTP headers, HTTP body, URI strings protect from commons attacks - SQL injection and XSS
  * Size constraints, geo-match (block countries)
  * Rate-based rules - DDOS protection

* Web ACL are regional apart from Cloudfront
* Rule group - reusable set of rules to add to a web ACL

# AWS WAF - fixed IP while using WAF with a load balancer

* Does not support network load balancer (layer 4)
* We can use Global Accelerator for fixed IP and WAF on the ALB

# AWS shield - DDoS protection

* AWS shield standard 
  * Free service with basic DDoS protected
* AWS Shield advanced
  * Response team to help you
  * Auto creates AWS WAF rules againts layer 7 attacks

# AWS Firewall Manager - manage Shield and WAF in 1 place

* Manage rules in all accounts of an AWS organisation
  
* Security policy - common set of security rules
  * WAF rules
  * AWS Shield advanced
  * Security Groups for EC2, ALB and ENI resources from VPC
  * AWS network firewall (VPC level)
  * Amazon Route 53 DNS firewall
  * Policies created at region level

* Rules applied to new resources as they are created across all accounts in AWS organisation

# Amazon Guard Duty - ai powered threat discovery

* Intelligent threat discovery
* Machine Learning algorithms, anomaly detections, 3rd party data
* Input data:
  * CloudTrail event logs
    * CloudTrail management events
    * CloudTrail S3 Data Events
  * VPC flow logs
  * DNS logs
  * Kubernetes audit logs
* Can setup EventBridge rules to be notified about findings
* Can protect against CryptoCurrency attacks

# Amazon Inspector - security assessment for application instances

* Run automated security assessments

* For EC2 instances
* For container Images push to Amazon ECR
* For Lambda functions

* Only evaluates running EC2 instances, container images, lambda functions
  * Package vulnerabilities
  * Network reachability

# Amazon Macie - avoid leaks in personal information

* Data security and data privacy service
* Alerts you to sensitive information such as personally identifiable information (PII)