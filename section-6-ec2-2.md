# Elastic IP

* Fixed public IP address, you own it until you delete it. Not recommended in most cases.
* Can have a max of 5 Elastic IPs

# Placement groups

If you want to control how instances are placed then you can use these.

3 strategies:

* Cluster - low latency group in a single AZ
  * Use case: Big data (fast job) and application needing low latency and high throughput
* Spread - spreads instances across a region (max 7 instances per group per AZ)
  * Can spread across AZs
  * Reduced risk of simultanesous failure
  * EC2 isntances on different physical hardware
  * Con - max 7 instances per AZ per placement group
  * Use case: application where availability is critical
  
* Partition - spread instances across many partitions (hardware racks) within an AZ. Scales to 100s of EC2 instances per group.
  * Up to 7 partitions per AZ
  * Multiple AZs in the same region
  * 100s of EC2 instances
  * Instances in one partition don't share racks with ohter partitions.
  * EC2 instances get access to the partition information
  * Use case: good for big data (Hadoop, Cassandra, Kafka)

# Elastic Network Interfaces

* Logical component in a VPC which respresents a virtual network card
* ENI has following properties
  * 1 private primary IPv4, 1 or more secondary IPv4
  * 1 Elastic IP per private IPv4
  * 1 Public IPv4
  * One or more security groups
  * A MAC address
* Create ENI independently from EC2 instances, move them to EC2 instances for failover (backup)
* Bound to a specific AZ


# EC2 Hibernate

* Stop - data on disk (EBS - Elastic Block Store) kept in tact next start
* Terminate - any EBS volumes set up are lost.
* Start - OS boots, EC2 user data script runs, app starts, cache warms up...

* Hibernate 
  * Whatever was in RAM is preserved
  * Instance boot is much faster on restart
  * Under the hood - RAM state written to a file in the root EBS
  * Root EBS volume must be encrypted
  * Use cases:
    * Long running processes
    * Saving the RAM state
    * Services that take time to initialise