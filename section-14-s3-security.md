# S3 security

4 methods for encrypting objects in S3
HTTPS is recommended

* Server side encryption
  * Amazon S3 managed keys (enabled by default for new objects)
    * Encrypts S3 objects using keys handled and owned by AWS
    * Encrytion type is AES-256
    * Upload client must set header 'x-amz-server-side-encryption': 'aes256'
  * KMS keys stored in AWS KMS
    * Leverage AWS key management service to manage keys
    * KMS Advantages: user control + audit key uasge using cloudtrail
    * 'x-amz-server-side-encryption': 'aws:kms'
    * KMS disadvantages - quota per second for downloads as each one calls the KMS API, you may experience throttling
  * Custom provided keys
    * When you want to manage your own encryption keys
    * Amazon wont store the provided key
    * https must be used
    * Encryption key must be provided in HTTP headers for each upload/download request
* Client side encryption
  * Use client libs, eg. Amazon S3 client side encryption lib
  * Clients must encrypt data before uploading
  * Clients must decrypt data after downloading

You can force encryption on uploaded objects using bucket policies

# MFA delete

Force users to generate a code on a mobile device in order to do important operations on S3.

* Permanently delete an object version
* Suspend versioning on a bucket

Only root account can enable/disable MFA

# S3 Access logs

* For audit purposes you may want to log all access to S3 buckets
* Any request to S3 from any account, auth or denied, will be logged into another S3 bucket, to be analysed using data analysis tools
* Target logging bucket must be in the same region
  * Don't log to the bucket you are monitoring (it will create a loop lol)

# S3 - pre-signed URLs

* Generate pre-signed URLs using S3 console, AWS CLI or SDK
* URL Expiration
  * S3 console - 1 - 720 mins
  * AWS CLI - up to 168 hours
* Users given a presigned URL inherit the perminssions of the user who generated it for GET/PUT

# S3 - glacier vault lock

* Adopt a WORM (Write once read many) model
* Create a vault lock policy
* Lock the policy for future edits
* Helpful for compliance and data retention

# S3 object lock

* Adopt a WORM (Write once read many) model
* Block an object version deletion for a specific amount of time
* Retention mode - compliance
  * Version can't be overwritten or deleted by any user (including root)
  * Retention modes cannot be changed, retention period cannot be shortened
* Retention mode - governance
  * Most users can't change or overwrite
  * Some users have special permissions to override
* Both have retnention period: amount of time to protect the object for
* Legal hold - protect the object indefinitely, indpenedent of retention period
  * can be placed and removed using s3:putobjectlegalhold IAM permission


# S3 - Access points

* Provide permissions for a specific group of users to access a specific S3 prefix
* Simplify security management for S3 buckets
* Each access point has its own DNS name (VPC or internet) and policy

VPC origin

* We can define the access point to be accessible only within the VPC
* Must create VPC endpoint to access the access point
* VPC endpoint must allow access to the target bucket and access point


# S3 - Object lambda

* Use AWS lambda to change the object before retrieved by the caller
* Only 1 S3 bucket needed, on top we create
  * S3 access point
  * S3 object lambda access points