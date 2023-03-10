# EBS (Elastic Block Store) volume

Elastic Block Store volume is a network drive that you can attach to instances while they run.

* Allows you to persist data after instance termination.
* Mounted to one instance at a time
* Bound to a specific AZ

Think of it as a 'network USB stick' - an SSD type thing which the EC2 instance communicates with over the network.

* Network drive (not physical drive)
  * It can be quickly detached from one EC2 instance and attached to another
  * Has a provisioned capacity (GB and IOPS) that you will be billed for.

* Delete on termination:
  * By default, root EBS volume is deleted
  * Any other EBS volume is not deleted.'

# EBS Snapshots

* Make a backup of your EBS volume at a point in time
* Not necessary to detach volume to do this, but recommended
* Can copy snapshots across AZ or region

Features:

* EBS Snapshot Archive
  * Move snapshot to an archive tier which is 75% cheaper
  * Takes 24-72 hours to restore the archive
* Recycle bin for EBS Snapshots
  * Setup rules to retain deleted snapshots to recover them after accidental deletion
  * Specify retention - 1 day to 1 year

* Fast Snapshot restore (FSR)
  * Full initialisation of snapshot, so no latency on first use (this is expensive)

# AMI - Amazon Machine Image

* Customisation of an EC2 Instance
* Add your own software, configuration, OS, monitoring
* Faster boot/config time because software is pre-packaged
* AMI are built for a specific region (can be copied across regions)
* You can launch EC2 from
  * A public AMI - AWS provided
  * Your own AMI - made and maintained yourself
  * AWS marketplace AMI - an AMI someone else made (and sells)

# EC2 Instance Store

* EBS volumes are network drives with good but 'limited' performance - for high performance, use EC2 instance store
  * Better I/O performance
  * Instance Stores lose their storage if stopped (ephemeral)
  * Use cases:
    * Buffer, cache, temporary content
    * Risk of data loss if hardware fails - backups and replication are your responsibility

# EBS Volume Types

* gp2/gp3 (SSD) - general purpose SSD, balances price and performance
  * Cost effective, low-latency
  * System boot voumes, virtual desktops, development, test environments
  * 1GiB - 16TiB
  * gp3:
    * Baseline of 3000 IOPS, throughput 125MiB/s
    * Can increase IOPS to  16000, throughput to 1000MiB/s
  * gp2
    * Small gp2 can husrt IOPS to 3000
    * Size of volume and IOPS are linked, max IOPS is 16000
    * 3 IOPS per GB, means at 5334 GB we have max IOPS
* ios1/io2 (SSD) - highest performance SSD volume for mission critical low-latency or high-throughput workloads.
  * Provisioned IOPS (POIPS) SSD
  * Critical business applications with sustained IOPS performance
  * Applications needing more than 16000 IOPS
  * Great for database workloads
  * io1/io2 (4GiB - 16TiB)
    * High Performance
  * io2 block express
    * Realy high performance
  * Supports EBS multi attach

* HDD
  * Cannot be boot volume
  * 125GiB to 16TiB 
  * st1 (HDD): low cost HDD designed for frequently accessed, throughput intensive loads
    * Big Data, Data warehouses, log processing
    * Max throughput 500MiB/s - max IOPS 500 
  * ac1 (HDD): lowest cost HDD volume designed for less frequently accessed workloads.
    * Infrequently accessed data
    * Lowest cost important
  
* EBS volumes are characterised in size/throughout/IOPS
* Only gp2/gp3 and io1/io2 can be used as boot volumes

# EBS Multi attach

* Attach an EBS volume to multiple EC2 instances in the same AZ
* Each instance has fulll read and write permissions to high performance volume
* Use case:
  * Higher application availability in clustered linux applications
  * Applications must manage concurrent write operations

* **Max of 16 EC2 instances can be attached at a time**

# EBS Encryption

* When you create encrypted volume, all data etc... is encrypted
  * You dont have to do anything
  * Effect on latency is minimal
* To encrypt existing volume, create a snapshot, then copy it with encryption, and then create a new volume from encrypted snapshot.

# Amazon EFS - elastic file system

* Managed NFS (network file system) that can be mounted on many EC2
* EFS works with EC2 instances in multiple AZs
* Highly available, scalable, very expensive, pay per use.
* Use cases:
  * Content management
  * Web serving
  * Data sharing
  * Wordpress
* Uses security group to control access
* Compatiable with Linux AMI, but not windows.
* Encryption at rest using KMS
* Scales automatically, pay per use.

Performance/storage classes:

* EFS scale
  * 1000s of concurrent NFS clients, 10GB+/s throughput
  * Grow to petabyte scale netwrok file system automatically
* Performance mode (set at EFS creation time)
  * General Purpose - latency sensitive (web server, CMS)
  * Max I/O - higher latency, highly parallel (big data)
* Throughput mode
  * Bursting - 1TB  50MiB/s + burst of up to 100MiB/s
  * Provisioned - set throughput regardless of storage size
  * Elastic - ato scale throughput based on workload
    * Good for unpredictable workloads
* Storage tiers
  * Standard - frequently accessed files
  * Infrequent access (EFS-IA) - cost to retrieve files, lower cost to store. Enable it with a lifecycle policy.
* Availability and durability
  * Standard - multi AZ (good for production)
  * One Zone - 1 AZ, great for dev, backup enabled by default (Over 90% cost savings)
