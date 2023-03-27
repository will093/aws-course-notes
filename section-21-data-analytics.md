# Athena

* Serverless query service to analyse data stored in S3
* SQL to query files  
* Commonly used with Amazon quicksight for reporting/dashboards
* Use cases
  * Business intelligence
  * analytics
  * reporting
  * analyze VPC flow logs
  * ELB logs
  * Cloudtrail Trails
* Exam Question: analyse data in S3 using serverless SQL = Athena

# Athena - performance improvement

* Columnar data for cost savings
  * Parquet or ORC format recommended
  * Glue to convert to Parquet and ORC
* Compress data for smaller retrievals
* Partition datasets in S3 for easy querying on virtual columns
* Use larger files to minimise overheads  

# Athena - federated query

* Run SQL queries across relational, non-relational, object, and custom data sources.
* Uses Data Source Connectors on AWS lambda to run federated queries 
* Stores results back in Amazon S3

# Redshift overview

* Based on Portgres, but not OLTP (online transaction processing)
* Its OLAP (online analytics processing) - data warehousing
* 10x better performance than other data warehouses, scales to PBs of data
* Columnar storage of data (instead of row based) & parralel query engine
* SQL for queries
* Pay as you go
* For serious data warehousing this is better than Athena (faster queries/joins/aggregations)

# Redshift cluster

* Leader node - query planning, results aggregation
* Compute node - perform queries, send results to leader
* Provison node size in advance
* Reserve instances for cost savings

# Redshift cluster - snapshots and DR

* Multi AZ for some clusters
* Snapshots are point in time backups, incremental
* Restore into new cluster
* Can be automated
* Manual snapshot retained until deleted
* Can configure Redshift to auto copy snapshots of a cluster to another AWS region

# Redshift spectrum

* Query data already in S3 without loading it
* Must have a redshift cluster to start query
* Thousands of nodes will perform the query

# Amazon Opensearch (Elasticsearch)

* In opensearch you can search any field in a DB, even partial matches
* Complement to another DB (ie. copied from a db by a lambda when a record is added)
* Requires a cluster of instances (not serverless)
* Own query language (not SQL)

# Amazon EMR

* Amazon MapReduce
* EMR helps creating hadoop clusters (big data) to analyse and process vast amounts of data.
* Clusters can be 100s of EC2 instances
* EMR comes bundled with BigData tools
* Auto-scaling, integrated with spot instances
* Use cases: data processing, machine learning, big data

# Amazon EMR - node types and purchases

* Master node - manage cluster, co-ordinate, manage health - long running
* Core node - run tasks and data - long running
* Task nodes - just to run tasks (spot instances)
* Purchasing options
  * On demand
  * Reserved (min 1 year): cost savings (EMR will use automatically if available)
  * Spot instances: cheaper, can be terminated, less reliable

* Can have long running, or transient (temporary) cluster

# Amazon QuickSight

* Serverless machine learning business intelligence service to create interactive dashboards
* Fast, auto-scalable, embeddable, per session pricing
* Use cases:
  * Business analytics
  * Building visualisations
  * Perform ad-hoc analysis
  * Get business insights using data
* Integrated with RDS, Aurora, Redshift, S3
* In memory computation using SPICE engine if data imported into QuickSight
* Possibility to set up column level security (CLS)

# QuickSight - dashboards and analysis

* Define users and groups (internal to quicksight)
  * Not related to IAM users
* A dashboard is:
  * Read only snapshot of an analysis that you can share
  * preserves configuration of the analysis (filters etc...)
* You can share an analysis with users or groups.

# AWS glue

* Managed ETL (extract, form and load) service
* Prepare and transform data for analytics
* Fully serverless
* Convert data to parquet format (eg.convert csv to parquet for use by Athena)
* Glue data catalog
  * Data Crawlers crawl various databases and extract metadata and store in the glue catalog
* Glue job bookmarks: prevent re-processing old data
* Glue elastic views: 
  * Combine and replicate data across multiple stores using SQL
  * No custom code, glue monitors for changes in the source data, and is serverless
  * Leverages a virtual table
* Glue DataBrew: clean and normalise data using pre-built transformation
* Glue studio: new GUI to create, run and monitor ETL jobs in glue.
* Glue streaming ETL:
  * Built on Apache Spark Structured Streaming
  * Compatible with Kinesis Data Streaming, Kafka, MSK

# AWS lake formation

* Data lake: central place to have all data for analytics purposes
* Fully managed service - makes it easy to setup a data lake in days
* Discover, cleanse, transform and ingest data into data lake
* Out-of-the-box source blueprints: S3, RDS, NoSQL db...
* Fine-grained access control for your applications (row and column level)
* Built on top of AWS glue
* Other services like Redshift, athena and EMR can utilise the data lake for analysis and dashboards
* Security
  * Access control
  * Column and row level security

# Kinesis Data Analytics for SQL

* Read from Streams or Firehose
* Run SQL (reference data in S3)
* Write back to Kinesis Streams or Firehose
* Fully managed, pay for consumption rate
* Use cases:
  * time stream analytics
  * real time dashboards
  * real time metrics

# Kinesis Data Analytics for Apache Flink

* Use flink to process and analyse streaming data
* Read from Kinesis streams or Amazon MSK
* Run any Apache Flink app on a managed cluster on AWS
  * Does not read from Firehose

# Amazon managed streaming for Apache Kafka (MSK)

* Alternative to Amazon Kinesis
* Fully managed Apache Kafka on AWS
  * Allow you to create, update, delete clusters
* MSK Serverless
  * Run Kafka without managing the capacity
  * MSK auto provisions resources and scales compute/storage

# Apache Kafka at a high level

* Message size, 1MB default but can be higher.

# Big data ingestion pipeline

* We want ingestion pipeline to be fully serverless
* Collect data in real time
* IoT core allows you to harvest data from IoT
* Firehose helps with selivery to S3 in near realtime



