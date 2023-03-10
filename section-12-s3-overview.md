# Amazon S3 Use Case

One of the main building blocks of AWS - 'infinitely scaling storage'.

* Many AWS services use S3 as an integration

Use cases:

* Backup and storage
* Disaster recover
* Archive
* Hybrid Cloud Storage
* Aoplication Hosting
* Media Hosting
* Data lakes and big data analytics
* Software delivery

## Buckets

* Store objects (files) in buckets (directories)
* Buckets have a globally unique name
* Buckets defined at the region level
* S3 looks like a global service but buckets created in a region

* Objects
  * They have a key, which is the full path
    * key is prefix + object name (s3://my-bucket/my_folder/my_folder_2/my_file.txt)
    * prefix = folder path (my_folder/my_folder_2/)
    * object name = file name (my_file.txt)
  * No concept of directories within buckets
    * The UI can make you think there is, but these are actually just keys with slashes in the name
  * Object values are the content of the body
    * Max size 5TB
    * If uploading more than 5GB, must do multi-part upload
  * Other fields - metadata, tags, version ID

## Security

* User based
  * IAM policies - which API calls allowed for a specific IAM user
* Resource based
  * Bucket policies - bucket wide rules from S3 console - allows cross account
  * Object Access Control List (ACL) - fine grain
  * Bucket Access Control List (ACL) - less common
* An IAM principal can access an S3 object if:
  * (IAM permissions ALLOW it OR resources policy allows it) AND there is no explicit DENY.

## S3 Bucket Policies

* JSON based policies
  * Resources: buckets and objects
  * Effect: allow/deny
  * Actions: set of API allow/deny
  * Principal: account or user to apply the policy to

* Use S3 bucket for policy to:
  * Grant public access to the bucket
  * Force objects to be encrpyted at upload
  * Grant access to another account (cross account)

## S3 Static Website hosting

* S3 can host static websites and have them accessible on the internet
  * Bucket must allow public reads

## S3 versioning

* Enabled at bucket level
* Best practice to version buckets

## S3 replication

* Must enable versioning in source + destination
* Cross region replication (CRR)
* Same region replication (SRR)
* Buckets can be in different AWS accounts
* Must give IAM permissions to S3
* After enabling replication, only new objects replicated
  * Replicate existing objects with S3 batch replication
* No chaining of replication, replicating from a-> b and b-> c does not mean a-> c replication occurs
* Can replicate delete markers from source to target (but cannot delete specific versions for security reasons)

## S3 storage classes

* S3 standard - general purpose
  * Default - frequently accessed data
  * Low latency, high throughput
  * Sustain 2 concurrent facility failures
* S3 standard - inrequent access
  * Infrequent access, rapid access when needed
* S3 one zone-infrequent access
  * Lost if zone destroyed, less available
  * Use for secondary backups etc
* S3 glacier instant retrieval
  * Low cost storage for archiving/backup
  * min storage 90 days
  * Cost to get data back, but it comes instantly
* S3 glacier flexible retrieval
  * Low cost storage for archiving/backup
  * min storage 90 days
  * Cost to get data back, but it takes mins/hours to access depending on chosen tier
* S3 deep archive
  * For long term storage
  * Access (standard 12 hours, bulk 48 hours)
  * Min storage 180 days
* S3 intelligent tiering
  * Monthly monitoring and auto tiering fee
  * Moves objects between its own tiers based on usage
  * No retrieval charges

## S3 durability and availability

* Durability:
  * high durability (99.9999999%) of objects across multiple AX
  * If you store 10,000,000 objects with S3, you will on averga lose 1 every 10000 yeras
  * Same for all storage classes

* Availability
  * How much of the time a service is available (S3 standard is 99.99%, so unavailable 53 mins per year on average)