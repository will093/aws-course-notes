# Amazon SQS

* Producer sends messages to queue
* Consumer polls the queue for new messages

* Fully managed and used to decouple applications
* Unlimited throughout and unlimited messages in queue
* Default retention of messages: 4 days, max 14 days
* Low latency (10ms)
* 256KB per message sent

* Can have duplicate and out of order messages

* SQS producers
  * Produce messages to SQS using SDK (sendmessage)
  * Persisted until customer deletes it
* SQS consumers
  * Applications (EC2 services, lambdas, on premises server)
  * Poll SQS for up to 10 messages at a time
  * Multiple EC2 instance consumers
    * Consumers receive and process in parallel
    * At least once delivery
    * Best effort message ordering
    * Consumer deletes messages after processing
    * Can scale consumers horizontally to improve throughput
* Cloudwatch Metric - queue length, to autoscale group the EC2 instances

* SQS can decouple application tiers.
  * Eg. video processing
  * Frontend can upload video unprocessed instead of processing
  * Frontend could then add a message to the queue and a backend service could read the message from queue and process the video 

* Security - encryption in flight
  * At rest encryption using KMS keys
  * Client side cryption if the client wants to handle encryption/decryption itself
  * IAM policies to regulate access to SQS API

* SQS access policies
  * Similar to bucket policies, allows cross account access, or other services to write to an SQS queue

* Message visibility timeout
  * Period of time after polling in which the consumer must process the message and delete it. 
  * If not, then after that time it goes back to the queue. It will be processed twice
  * Consumer can call ChangeMessageVisibiliy to get more time, with a reasonable value (not too high or low)

# SQS long polling

* When a consumer requests messages from queue, it can wait (open tcp connection) for a message
* The consumer will get the message right away with low latency
* Reduces number of API calls to SQS by consumers while reducing latency
* Wait time can be 1-20 secs
* Always preferable to short polling

# SQS fifo queue

* fifo - first in first out (guarantee on ordering unlike standard SQS queue)

* SQS use case - buffer for DB writes
  * Large volume (spike) of DB writes could overload DB servers, so instead we write to a queue and let a service process the queue a more reasonable pace.

# Amazon SNS

* Pub/sub service, one (or many) publishers, many subscribers
* Event producer publishes events to a topic, subscribers subscribe to that topic
* Every subscriber receives all messages
* 12.5 million subscribers per topic

* How to publish
  * Topic publish (SDK)
  * Direct publish (mobile SDK)

* Security - same as SQS

* SQS + SNS
  * SNS topics can write to multiple SQS queues so that they receive those messages
  * SQS queues must have their policy allow SNS to write to them
  * Works cross-region
  * Use case: send one S3 event to multiple SQS queues

* SNS fifo
  * first in first out for SNS topics
  * can only have SQS fifo as subscribers

* SNS message filtering
  * JSON policy to filter out messages by specific criteria
  * It means SNS can send different messages to different places, eg. placed orders to one SQS, failed orders to another

# Amazon Kinesis

* Make it easy to collect, process and analyse streaming data in real time.
* Ingest real time data such as: application logs, metrics, website clickstreams, IoT telemetry data

* Kinesis data streams - capture, process and store data streams
* Kinesis data firehose - load data streams into AWS stores
* Kinesis data analytics - analyse data streams with SQL or Apache Flink
* Kinesis video streams - capture, process and store video streams

* Kinesis data streams
  * Split across shards, number of which can be scaled
  * Producers send data into streams
  * Data that is pushed is known as a record, made up of:
    * Partition key
    * Data blob
  * Consumers get records from the data stream, made up of:
    * Partition key
    * Sequence no
    * Data blob

* Retention 1-65 days
* Ability to reprocess data
* Once data is in kinesis it cant be deleted
* Data which shares partition key goes to same shard - key based ordering (data is fifo in this case)

# Kinesis Capacity modes

* Provisioned mode - choose number of shards provided, scale manually or with API
  * 1MB/s in, 2MB/s out per shard
  * Pay per shard per hour
* On-demand mode
  * No need to provision or manage capacity
  * Default capacity provisioned
  * Scales automatically based on last 30 days throughput
  * Pey per stream per hour and data in/out per GB.

* Security
  * Control access using IAM policies
  * Encryption in flight using https
  * Encryption at rest using KMS
  * Can implement your own client side encryption/decryption
  * VPC endpoints available
  * Monitor API calls using Cloudtrail

# Kinesis Data Firehose

* Takes data from sources (usually Kinesis data streams), transforms them with a lambda and then writes to a destination
* Writes to
  * Amazon S3
  * Amazon redshift DB (via S3)
  * Amazon opensearch
  * Custom and 3rd party destinations
* Can send all input data/failed input data into an S3 bucket
* Fully managed, no administration, auto scaling
* Pay for data going through Firehose
* Near realtime 
  * 60 seconds min for non full batches

* Streams vs firehose
  * Streams
    * Streaming for ingest at scale
    * Write custom code to produce/consume
    * Realtime
    * User manages scaling
    * Data storage for 1 to 365 days
    * Supports replay
  * Firehose
    * Load streaming data into S3/redshift/opensearch/other
    * Transform with a lambda
    * Near realtime
    * Fully managed
    * No state storage
    * No replay

# Other

Can use group IDs to ensure a single consumer handles a group of messages.

# Amazon MQ

* When migrating to the Cloud it can be easier to migrate to Amazon MQ than SQS/SNS
* Amazon MQ doesnt scale as well as SQS/SNS
* Managed message broker service for RabbitMQ and ActiveMQ
* Doesnt scale as much as SNS or SQS
* Runs on servers, can run multi AZ with failover (uses EFS for storage)
* Has both queue and topic features