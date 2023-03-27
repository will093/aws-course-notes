# DB types

* RDBMS
  * RDS
  * Aurora
* Object store
  * S3
  * S3 glacier (for archive)
* Data warehouse
  * Redshift
  * Athena
  * EMR
* Search
  * Opensearch
* Graphs
  * Amazon neptune - relationships between data
* Ledger
  * Amazon quantum ledger
* Time series
  * Amazon timestream

# RDS

* Managed Postgres / MySQL / Oracle / SQL server / MariaDB / Custom
* Provisioned RDS instance size and EBS volume type and size
* Auto scaling for storage
* Read replicas and multi AZ
* Security through IAM, security groups, KMS, SSL in transit
* Auto backup - 35 days
* Manual backup - long term backup

# Aurora

* Compatible API for Postgres/MySQL, separation of storage and compute
* Storage: data stored in 6 replicas across 3 AZ - available, self healing, auto-scaling
* Compute: cluster of DB instances across multiple AZ, auto scaling of read replicas
* Cluster: custom endpoints for writer and reader DB instances
* Same security/monitoring/maintenance features as RDS
* Know the backup and restore features for Aurora
* Aurora Serverless - unpredictable intermittent workloads, no capacity planning
* Aurora Multimaster - write failover
* Aurora Global - up to 16 DB read instances per region, 1s replication  
* Aurora Machine Learning - ML using SageMaker and comprehend on Aurora
* Aurora Database Cloning - new cluster from existing one
* Use case: same as RDS but more features and less maintenance

# DocumentDB

* AWS version of MongoDB

# Amazon Neptune

* Fully managed graph DB
* A popular graph dataset would be a social network

# Apache Cassandra

* Opensource NoSQL distributed DB
* Use cases
  * iot device info
  * time series data

# Amazon QLDB

* Quantum ledger database
* For recording financial transactions
* Fully managed, serverless, highly available, replication across 3AZ
* Review history of all changes made to application data over time
* Immutable system - no entry can be removed or modified, cryptographically verifiable.
* 2-3x better perf than common blockchain ledgers, can use SQL to manipulate
* No decentralised component

# Amazon timestream

* Faster and cheaper than relational DBs for timeseries data