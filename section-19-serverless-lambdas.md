# Lambdas

* Virtual functions - no servers to manage
* Limited by time - short executions
* Run on-demand
* Scaling is automated

# Benefits of AWS lambda

* Easy pricing
  * Pay per request/compute time
  * Generous free tier
* Integrated with whole AWS suite
* Integrated with many programming languages
* Monitoring with CloudWatch
* Can easily increase RAM, which increases CPU and network too

* Lambda container image
  * Container image must implement lambda runtime API
  * ECS/fargate is preferred for running arbitrary docker images

* Example use case - serverless thumbnail generation
  * S3 bucket image upload triggers a lambda which can generate a thumbnail from an image and store it in another S3 bucket, insert metadata in dynamodb
* Example use case - cron job
  * Cloudwatch events eventbridge - trigger a lambda to run every 1 hour

# AWS lambda limits

* Execution
  * Memory allocation: 128MB - 10GB
  * Max execution time: 900 seconds
  * Env vars: 4KB
  * Disk capacity in function container: 512MB to 10GB
  * Concurrent executions: 1000
* Deployment
  * Lambda function deployment size: 50MB compressed, 250MB uncompressed
  * Can use /tmp directory to load files at startup
  * Env vars: 4KB

# Customization at the edge

* Edge functions
  * Attach to Cloudfront distributions
  * 2 types: CloudFront functions and Lambda@edge
  * Use cases: 
    * customise CDN content
    * dynamic web app at edge
    * SEO
    * Real time image transformation
    * A/B testing
    * etc...
* Cloudfront functions
  * Lightweight JS functions
  * High scale, latency sensitive CDN customizations
  * sub-ms startup times, millions of reqs/s
  * Used to change viewer requests/responses
* Lambda@edge
  * Lambda functions in NodeJS or python
  * Scales to 1000s of reqs/second
  * Used to changer Cloudfront requests and responses
    * Viewer request - after Cloudfront receives it from viewer.
    * Origin request - before Cloudfront forwards it to origin server
    * Viewer response
    * Origin response
* Author in 1 region, Cloudfront replicates to all locations

* Cloudfront functions
  * Cache key normalization
  * Header manipulation
  * URL rewrites or redirects
  * Request authentication

* Lambda@edge
  * Longer execution time (several ms)
  * Adjustable CPU/memory
  * Code depends of 3rd parties
  * Network access to external services
  * Filesystem access or http request body access
  
* VPC
  * By default, lambda is not inside your VPC
  * You can define VPC ID and configure your lambda to run inside of it using an elastic network interface
    * Use case: run inside VPC to connect to DB - must use RDS proxy to pool connections and not overlaod DB as lambdas scale.

* Invoking lambda from RDS and Aurora
  * Invoke lambda functions from within DB instance
  * Allows you to process data events from within a DB
  * Supported for RDS for PostgreSQL and Aurora
  * Must allow outbound traffic to your Lambda function from with your DB instance

* RDS event notifications vs lambda invocation from RDS
  * Info about the instance itself and not the data being manipulated.

# Amazon DynamoDB

* Fully manage, highly available DB, replication across multiple AZs
* NoSQL database - not relational
* Scales to massive workloads, distributed
* Millions of reqs/second, 100s of TBs of storage
* Fast and consistent performance
* Integrated with IAM for security and auth
* Standard and infrequent access table class

* Dynamo basics
  * DynamoDB is made of Tables
  * Each table has a primary key
  * Each table can have infinite number of items/rows
  * Each item as attributes (can be added over time)
  * Max item size is 400kB
  * In DynamoDB you can rapidly evolve schemas
  * Can have a TTL to delete items after a certain amount of time

* Capacity modes (read/write throughput)
  * Provisioned mode
    * You specify reds/writes per second
    * No capacity planning needed
    * Pay for provisioned read/write units
    * Possibility to add auto scaling mode
  * On-Demand mode
    * Reads/writes scale with workload
    * No capacity planning needded
    * Pay for what you use (more expensive)
    * Great for unpredictable workoads, steep sudden spikes

# DynamoDB - advanced features (DAX caching)

* DAX caching - Fully managed, highly available, in-memory cache for dynamoDB
* Help solve read congestion by caching
* Microsecond latency for cached data
* 5 min TTL cache by default
* No application code changes

Can use DAX in combo with elasticache, ie. using elasticache for caching aggregate data.


# DynamoDB Stream processing

* Order stream of item level modifications in a table
  * React to changes in real time (welcome email)
  * Real time usage analytics
  * Cross region replication
  * Invoke AWS lambda on changes to the table

* Alternatively you can integrate with Kinesis data streams 
  * longer retention
  * Write to S3
  * etc...

# DynamoDB global tables

* Make DynamoDB table accessible with low latency in multiple regions
* Active-active replication (apps can read and right to tables in any regions)
* Must enable Dynamo Streams

# DynamoDB - backup for disaster recovery

* Continuous backups with point in time recovery
  * Optionally enabled for last 35 days
* On demand backups
  * Full backups for long term retention until deleted

# Integration with S3

* You can export to S3 as a JSON or ION (must enable PITR recovery)
* You can import from S3 (JSON or ION)

# API Gateway

* Proxy server for a lambda
* API gateway + AWS lambda = no infrastructure to manage
* Support for websocket
* Handle API versioning
* Handle different environments Handle security
* Create API keys, handle request throttling
* Swagger / Open API import to quickly define APIs
* Transform and validate requests and responses
* Generate SDK and API specifications
* Cache API responses

* Integrations - high level
  * Lambda function
    * Easy way to expose REST api backed by lambda
  * HTTP
    * Any http endpoint in backend
  * AWS service
    * Expose any AWS API through API Gateway
    * Eg. start an AWS step function workflow, post message to SQS
      * API gateway allows you to add authentication, but expose on public internet, rate limit etc...

* API gateway endpoint types
  * Edge optimised (default)
    * Requests routed through CloudFront edge locations
    * API gateway still only lives in one region
  * Regional
    * For clients in the same region
  * Private - can only be accessed from your VPC
    * User resource policy to define access

* API gateway security
  * User authentication
    * IAM roles (for internal applications)
    * Cognito (identity for external users)
    * Custom authorizer (your own logic)
  * Custom domain name HTTPS security through integration with AWS Certificate Manager
    * If using edge optimized endpoint, certificate must be us-east-1
    * If using regional endpoint, certificate must be in API gateway region
    * Must setup CNAME or A-alias record in Route 53

# AWS Step functions

* Serverless visual worklow to orchestrate Lambda functions (flow chart type thing??)

# Amazon Cognito

* Gives users an identity to intereact with our web or mobile application
* Cognito User Pools
  * Sign in functionality for app users
  * Integrate with API gateway and application load balancer
* Cognito Identity Pools (Federated Identity)
  * Provide AWS credentials to users so they can Access AWS resources directly
  * Integrate with Cognito User Pools as an identity provider
* Cognito vs IAM
  * Cognito - Web and mobile users outside of AWS

# Cognito User Pools (CUP) - User Features

* Create a serverless DB of users for your web and mobile apps
* Simple login
  * User/password
  * Password reset
* Email and phone verification
* MFA
* Social login

# CUP - integrations 

* Integrates with API gateway and application load balancer

# Cognito Identity Pools (federated identities)

* Get identities for 'users' so they obtain temp AWS credentials
* User source can be Cognito User Pools, 3rd party logins, etc...
* Users can the access AWS services directly or through API gateway
* The IAM policies applied to the credentials are defined in Cognito
* Can be customised based on user_id for fine grained control`
* Default IAM roles for authenticated and guest users
* Fine grained access - Enable Row level security for dynamo DB