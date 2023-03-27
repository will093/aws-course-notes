# Amazon Cloudwatch metrics

* Provides metrics for every service in AWS
* Metric is a variable to monitor
* Metrics belong to namespaces
* Dimension is an attirbute of a metric
* Can create custom metrics

# Cloudwatch logs

* Log groups: arbitrary name, using representing an application
* Log stream: instances within an application
* Log expiration policies (never expire, 30 days, etc...)
* Can send logs to:
  * Amazon S3
  * Kinesis Data Streams
  * Kinesis Data Firehose
  * AWS Lambda
  * OpenSearch

# Cloudwatch logs - sources

* SDK, Cloudwatch logs agent, CloudWatch unified agent
* Elastic Beanstalk: collection of logs from application
* ECS: collection from containers
* AWS Lambda: collection from function logs
* VPC flow logs: VPC specific logs
* API gateway
* Cloudtrail based on filter
* Route53: log DNS queries

# Cloudwatch logs metrics filter and insights

* Can filter logs based on various criteria - metric filters
* Metric filters can be used to trigger CloudWatch alarms
* Cloudwatch logs insights can be used to query logs and add queries to Cloudwatch Dashboards

# Cloudwatch logs - S3 export

* Log data can take up to 12 hours to be available for export
* The API call is CreateExportTask

# Cloudwatch logs subscriptions

* Filter on the logs and send to a destionation (eg. lambda or firehose)

# Cloudwatch logs for EC2

* By default no application logs sent to CloudWatch
* Must run CloudWatch agent on EC2 to push log files
* Cloudwatch agent can be set up on premises too

# Cloudwatch logs agent and unifeied agent

* CloudWatch logs agent
  * Old version of agent
  * Can only send to CloudWatch logs
* CloudWatch Unified agent
  * Collect additional system metrics such as RAM, CPU, etc...
  * Collect logs to send to CloudWatch logs
  * Centralised config using SSM parameter store

# CloudWatch unified agent (metrics)

* Collected directly on Linux server / EC2 instance
  * CPU
  * Disk Metrics
  * RAM
  * Netstat (TCP and UDP connections)
  * Processes
  * Swap space

# Cloudwatch Alarms

* Trigger notifications for any metric
* Various options
* Alarm states
  * Ok
  * Insufficient data
  * Alarm
* Period
  * Length of time in secs to evaluate the metric
  * High resolution custom metrics (10s, 30s, 60s)
* Alarm targets
  * Stop, terminate, reboot, recover an EC2 instance
  * Trigger auto scaling
  * Send notification to SNS
* Composite alarms
  * Monitoring states of other Alarms
  * And/Or conditions
  * Reduce alarm noise

# Instance recovery

* Checks:
  * Instance status = Check the EC2 VM
  * System status = check the underlying hardware

* Recovery
  * Same private, public Elastic IP, placement group

# Amazon Eventbridge

* Schedule: cron jobs
* Event pattern: event rules to react to a service doing something
* Trigger lambda functions, send SQS/SNS messages

* Eventbridge
  * Gets events from event buses
  * Default event bus (AWS service events)
  * Partner event bus (3rd party service events)
  * Custom event bus (your own applications can send events to event bridge)

* You can archive and replay events

* Eventbridge schema registry
  * Analyse events in bus and generate a schema

* Security - Resource based policy
  * Allow/deny events from another AWS account, or particular regions

# Cloudwatch container insights

* Collect and summarise metrics and logs from containers
* Available for containers on:
  * Amazon ECS
  * Amazon EKS
  * Kubernetes on EC2
  * Fargate (both for ECS and EKS)

# Cloudwatch lambda insights

* Monitoring and troubleshooting for AWS lambda
* Collects and aggregates system metrics

# Cloudwatch contributer insights

* Analyse log data and create time series for contributor data
  * See metric about top N contributors (eg. services using most resources, making most requests)
* Understand what is impacting system performance
* Works for any AWS generated logs
* Identify bad hosts, heaviest network users, etc...
* Find bad hosts, heaviest network users, build rules from scratch, etc...

# CLoudwatch application insights

* Automated dashboards to show potential problems with monitored applications, to isolate ongoing issues.
* Application must run on EC2 with specific technologies and languages

# AWS Cloudtrail

* Governance, compliance and audit for AWS account
* Enabled by default
* History of all events and API calls within your AWS account
* Logs can by placed in CloudWatch or S3
* A trail can be applied to all regions or a single region
* If a resources is unexpectedly deleted, check CloudTrail

# Cloudtrail events

* Management events
  * Events performned on resources in AWS account
    * Configuring security
    * Setting up logging
    * Configuring routing
  * By default, trails are configured to log management events
  * Can separate read and write events

* Data events
  * By default, not logged (because of high volume)
  * Eg. S3 object level activity
  * Can separate read and write events
  * Eg. Lambda exeution activity

* Cloudtrail insight events
  * Detect unusual activity in your account
  * Continuously analyse events to detect unusual patterns
    * Anomalies appear in CloudTrail console
    * EventBridge event is generated

* Cloudtrail event retention
  * Stored for 90 days in CloudTrail
  * Send events to S3 and then analyse with Athena

* Additional feature:
  * Intercept API calls with cloudtrail, send to eventbridge.

# AWS config

* Helps with auditing and recording compliance
* Helps with record configuration and changes over time
* Questions which can be answered by this service:
  * Is there unrestricted SSH access to my security groups
  * Do my buckets have public access
  * How has my ALB configuration changed over time
* Receive SNS notification for any changes
* Per region service
* Can be aggregated across regions and accounts
* Can transfer config data to S3

* Config rules
  * AWS managed config rules
  * Custom config rules
  * AWS config rules does not prevent actions from occuring, just for compliance

# AWS config resource

* View compliance of a resource over time
* View configuration of a resource over time
* View CloudTrail API calls of a resource over time

# Config rules - remediatons

* Automate remediation of non-compliant resources using SSM automation document

# Config rules - notifications

* Use EventBridge to trigger notifications when AWS resources are non-compliant
* Ability to send config changes and compliance state notifications to SNS