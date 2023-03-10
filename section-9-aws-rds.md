# Amazon RDS

* Managed DB service for DB, using SQL
* Supports
  * Postgres
  * MySQL
  * MariaDB
  * Oracle
  * Microsoft SQL Server
  * Aurora (AWS proprietary DB)

* Managed Service
  * Auto provisioning, OS patching
  * Continuous backups and point in time restore
  * Monitoring dashboards
  * Read replicas
  * Multi AZ setup for disaster recovery
  * Maintenance windows for upgrades
  * Scaling capability
  * Storage backed by EBS
  * Cannot SSH in, or access underlying instance

# Storage autoscaling

* Helps increase storage on RDS DB dynamically, RDS detects you are running out of space and scales automatically
* Maximum Storage Threshold must be set

# Read replicas

* Up to 5
* Within AZ, cross AZ or cross region
* Replication is async, so reads are eventually consistent
* Replicas can be promoted to their own DB
* Applications must update connection string to use read replicas
* Use cases
  * Reporting application - read DB without overloading original DB load
* RDS read replica fees:
  * Free within same region
  * Cross region replica, will incur replication fee

# RDS - multi AZ (disaster recovery)

* SYNC replication
* One DNS name - automatic failover to standby
* Increase availability
* Failover in case of loss of AZ
* No manual intervention in apps
* Not used for scaling - no reads or writes to this
* Read replicas can be set up as multi AZ

# RDS custome

* Access OS an more customisation
* Oracle and Microsoft SQL with OS and DB customisation
* Access underlying EC2 instance using ssh

# Amazon Aurora

* Proprietary AWS technology
* Aurora is AWS CLoud optimised and claims better performance than other solutions
* Aurora automatically grows in increments of 10GB as required
* Postgres and MySQL supported as aurora
* High availability and read scaling
  * 6 copies of your data across 3 AZ
  * 4 copies needed for write
  * 3 copies needed for read
  * self healing - p2p replication
  * Storage is striped across 100s of volumes
  * Cross region replication
  * Master + up to 15 read replicas

* Aurora DB cluster
  * Writer endpoint - points to master
  * Reader endpoint - load balancer spread across read replicas
  * Load balancing happens at connection level

* Aurora autoscaling - read replicas get auto-scaled according to load
* Custom endpoint - allows you to point traffic to specific read replicas
  * Reader endpoint generally not used after a custom endpoint is defined

* Aurora serverless - automatic database instantiation and auto scaling based on usage
  * No capacity planning needed and pay per second

* Aurora multi master
  * Failover for write node to another node
  * All nodes can read and write

* Global Aurora
  * 1 primary region
  * 5 secondary read only regions - replication lag is less than 1 second
  * Typical cross region replication is less than 1 second

* Aurora machine learning
  * ML based based predications for your applicaton
  * SImple imtegration with AWS ML
  * Use cases:
    * fraud detection, ad targeting, sentiment analysis, product recommendation
  * Supported services:
    * Amazon Sagemaker
    * Amazon comprehend


# RDS backups

* Automated backups
  * Daily full back up (during backup window)
  * Transaction logs backed up every 5 mins (can do a restore from this)
  * 1-35 days retention
* Manual snapshots
  * Retention for as long as you want

* Snapshot + restore of DB cheaper than stopping it

* Restore RDS backup:
  * From snapshot
  * RDS from s3 (upload your backup)
  * Aurora from s3

* Aurora DB cloning
  * Create new aurora DB cluster from existing one
  * Faster than snapshot and restore
  * Uses same cluster volume as original
  * Fast + cost effective
  * Create a staging DB from a prod DB without impacting the prod DB

# RDS and Aurora security

* At rest encryption 
* Master and replicas encrypted using AWS KMS
  * If master is not encrypted, read replicas cant be encrypted
  * To encrypt an existing DB, create a snapshot and restore as encrypted
* You can use IAM roles to connect to db, instead of username/password
* Security groups: control network access to aurora or RDS

* SSH only available on RDS custom
* Audit logs can be enabled and sent to cloud watch for longer retention

# RDS database proxy

* Fully managed db proxy for RDS
* Allow apps to pool and share DB connections established with DB - great for lambdas
* Improve DB efficiency by reducing stress and the DB and minimising open connections
* Reduce RDS and aurora failover time by 66%
* Enforce IAM authentication and store credentials in AWS Secrets Manager
* RDS proxy is never publicly accessible (accessed over VPC only, ie by other services)

# Amazon ElastiCache

* ElastiCache is for managed Redis/Memcached
* Caches are in memory DBs with high performance and low latency
* Reduce load of DBs for read intensive workloads
* Helps make application stateless
* Requires heavy application code changes to use ElastiCache

Redis
  * Multi AZ with auto-failover
  * Read replicas to scale reads and have high availability
  * Data durability using AOF (append only file) persistance
  * Backup and restore features
  * Supports sets and sort sets

Memcached
  * Multi node for partitioning of data
  * No high availability
  * No backup and restore
  * Non-persistant
  * Multi-threaded architecture

Advanced

* Supports IAM authentication for Redis
* IAM policies on ElastiCahe used for AWS API-level security
* Redis Auth
  * password/token when creating redis cluster
  * password/token is extra, on top of security group
  * Supports SSL in flight encryption
* Memcached
  * Supports SASL based auth

ElastiCache patterns

* Lazy Loading - all read data cached, data cacn become stale in cache
* Write through - add or update data in cache when written to a DB (no stale data)
* Sessions store - store temp session data in a cache (using TTL feature)

Redis use case

* Gaming leaderboards complex
* Sorted Sets guarantee uniqueness + element ordering
* Whenever an element is added to the cache it is ranked and ordered