# Amazon S3 - moving between storage classes

* Transition actions - configure objects to transition to another storage class
* Expiration actions - configure objects to delete after some time

* Rules can be created for a certain prefic
* Rules can be created for certain object tags

# Amazon S3 - requester pays

* You can set a bucket up so that the owner pays for storage and requester pays for downloads
* Obvs the requester must be authenticated

# Amazon S3 - event notifications

* S3 object created, S3 object removed, S3 object restore, S3 replication
* Use case: generate thumbnails of images uploaded
* Can create as many S3 events as required, eg. to different targets
* Typically deliver events in seconds, but can take 1 min or longer

* Advanced filtering options with JSON rules (metadata, object size, name...)
* Multiple Destinations - ex Step Functions, Kinesis Stream / Firehose...
* EventBridge capabilities - Archive, Replay Events, Reliable delivery

# Amazon S3 - performance

* S3 auto scales to high request rates.
* Application can achieve 3500 PUT/COPY/POST/DELELTE ot 5500 GET/HEAD per prefix, per second.
* Multi-part upload - recommended for files > 100MB, mandatory over 5GB
* S3 transfer acceleration - speed up transfer by transferring to an AWS edge location which forwards it to S3 bucket in the target region.
* Parallelise gets by specifying byte ranges
  * Can be used for speeding up downloads, or fetching only part of a file

# S3 - select and glacier select

* Retrieve less data using SQL by performing server side filtering (eg. on a CSV)
  * Can filter by rows/columns
  * Less network transfer, less CPU cost client-side

# S3 - batch operations

* Bulk operations on existing S3 objects with single request.
  * Modify object metadata
  * Encrypt unencryped objects
  * Modify ACLs, tags
  * Invoke lambda to perform custom action on each object
  * Can use S3 Inventory to get oject list and S3 select to filter objects