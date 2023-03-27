# Disaster recovery overview

* On premise => On premise
* On premise => AWS Cloud (hybrid)
* AWS Cloud Region A => AWS Cloud Region B

* Recovery Point objective (RPO)
  * How often you run backups
* Recovery time objective (RTO)
  * Amount of downtime between disaster and recovery

* Strategies
  * Backup

* Backup and restore
  * Stick data in Glacier for archive, restore after disaster
* Pilot light
  * Small version of app is always running in Cloud with only critical parts of system
  * Critical systems (eg. database) always running
  * After a disaster the non critical systems are started and scaled up to production load
* Warm standby
  * Full system up and running but at minimum size
  * After a disaster we can scale to production load
* Multi site / Hot site
  * Very low RTO
  * Full production scale is running AWS and On Premise
  * Very fast recover/failover in case of disaster
* All AWS multi region

# DMS - database migration service

* Quickly and securely migrate databases to AWS, resilient, self healing
* Supports
  * Homogenous migrations: Oracle to Oracle
  * Heterogenous migrations: Microsft SQL Server to Aurora
* Continuous replication using CDC (continous data change)
* Must create EC2 instance to perform replication tasks

* AWS schema conversion tool
  * Convert DB schema from one engine to another
  * Not needed if source and target engines the same

# RDS and Aurora Migrations

* RDS MySQL to Aurora MySQL
  * Option 1 - create snapshot and restore as Aurora
  * Option 2 - create a read replica as Aurora, then promote it as its own cluster
* External SQL to Aurora
  * option 1:
    * Percona XtraBackup to create file backup in S3
    * Create an Aurora MySQL DB from S3
  * option 2
    * Create Aurora MySQL DB
    * Use mysqldump to migrate MySQL into Aurora
* Use DMS (database migration service) if both databases are up and running (continuous replication)

# On premise strategy with AWS

* Ability to download Amazon Linux 2 as VM
* VM import/export
  * Migrate existing VMs into EC2
  * Create disaster recovery for your on premise instances on AWS
* AWS Application Discovery Service
  * Gather info on on premise servers to plan a migration
* AWS Database Migration Service
  * replicate on premise to AWS, AWS to AWS, AWS to on premise
* AWS Server Migration Service
  * Incremental replication of on premise live servers to AWS   

# AWS backup

* Fully managed service
* Centrally manage and automate backups across AWS services
* No need to create custom scripts and manual processes
* supported services
  * Amazon EC2 / EBS
  * Amazon S3
  * RDS/Aurora
  * And more...
* Cross region backups
* Cross accouint backups
* Backup policies known as Backup Plans
* Vault Lock - WORM (write once read many) state means backups cannot be deleted

# Application Discovery Service

* Plan migration projects by gathering information about on premise data centers
* Server utilisation data and dependency mapping are important for migrations

* Agentless Discovery (AWS Agentless Discovery Connector)
  * VM inventory, configuration and performance history
* Agent based Discovery (Aws Application Discovery Agent)
  * System configuration, system performance, running processes
* Resulting data can be viewed with AWS migration Hub

* Application Migration Service
  * Simplify migrating applications to AWS
  * Converts your servers to run on AWS

# Transfering large amounts of data to AWS

* Over internet/site to site VPN
  * Immediate setup
  * Slow
* Direct connect 1Gbps
  * Long 1 time setup
* Snowball
  * Time to order snowballs
  * 1 week end to end transfer
  * Can be combined with DMS (database migration service)

# VMware Cloud on AWS

* Some customers use VMWare Cloud to manage on premises data center
* VMware Cloud allows users to extend their data center capacity using AWS, and access AWS services
* VMware Cloud on AWS allows them to do this