# AWS Snow family

* Collect and process data at the edge, and migrate data in and out of AWS
* Data migrations - very large amounts of data eg. 10TB+ can take more than 1 week to download/upload
  * Instead AWS can send you a physical device with your data on it (AWS Snowball)

* AWS snowcone is smaller and lighter
* AWS snowmobile - a literal truck which stores up to 10pb ata
* Import to glacier - first import from snowball to S3, then lifecycle policy to glacier

# Amazon FSx

High performance file system

* FSx for Windows (file server)
  * It can also be mounted on Linux instances
  * Supports Microsoft DFS File System
* Amazon FSx for Lustre
  * High performance computing, machine learning
  * Seamless integration with S3
* Amazon FSX for Netapp ONTAP
  * File system compatible with NFS, SMB
  * Point in time instant cloning
* Amazon FSx for OpenZFS
  * Point in time instant cloning

* Scratch filesystem
  * Temp storage
  * Data not replicated in case of server failure
  * Use case: optimise cost, short term processing
* Persistant filesystem
  * Long term processing
  * Data replicated in same AZ
  * Replaced failed files within minutes

Comparison

* EFS: This is standard NFS optimized for cloud as a managed service
* FSx for Windows: SMB / CIFS fileshare as a service for Windows nodes.
* FSx for Lustre: High performance shared filesystem for HPC workloads. You probably know if you need this one.
* FSx for ONTAP: You want to use a NetApp but AWS won't let you stuff one into us-east-1. This is a managed service that starts at 1TB of disk. It's expensive but awesome.
* FSx for OpenZFS: You've been asleep for a decade or are willfully deluded into believing that ZFS is going to come roaring in "any month now" as an industry best practice. Dream on, you beautiful diamond.

# AWS storage gateway

* AWS is pushing for hybrid cloud
  * part of infrastructure on cloud, part on premises
* S3 is a proprietary storage technology, how to expose it on premises?

* storage gateway is the bridge between on premise and cloud
  * sends with nfs or smb on premises to s3 on the cloud
* FSx storage gateway
  * sends with nfs or smb on premises to FSx on Cloud
* Volume storage gateway
  * Converts between block storage on premises and S3 on Cloud
* Tape gateway
  * Physical tapes backed up on S3

# AWS transfer family

* file transfers in/out of AWS using ftp, sftp, ftps

# AWS datasync

* Move large amount of data
  * On premises/on other Cloud, into AWS
  * Sync to Amazon S3, EFS, FSx
  * File system metadata and permissions preserved

# AWS storage compared

* S3: object storage
* S3 glacier: archiving object storage
* EBS volumes: network storage to attach to 1 EC2 instance at a time
* Instance storage: high performance physical storage for an EC2 instance
* EFS: network storage which can be attached to multiple EC2
* FSx for Windows: filesystem for windows servers
* FSx for Lustre: filesystem for high performance computing linux
* FSx for NetApp: high OS compatibility filesystem
* FSx for OpenZFS: zfs compatible filesystem
* Storage gateway: bridge between on premise storage and S3 buckets in cloud
* Transfer family: FTP, SFTP, FTPS interface on top of Amazon S3 or Amazon EFS
* DataSync: schedule data sync on premise to AWS, or cloud to AWS, or between AWS storage types
* Snowball family: transfer large amounts of data to/from AWS on a physical device
* Database: specific workloads with need for indexing and querying