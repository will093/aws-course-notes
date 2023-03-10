# EC2 - Elastic Compute Cloud

This consists of:

* Renting virtual machines (EC2)
* Storing on virtual drives (EBS)
* Distributing load across machines (ELB)
* Scalking services using Auto Scaling Group (ASG)

# Ec2 instance types:

* General Instances
  * Good for web servers, code repositories
  * Balance between Compute (CPU), Memory, Networking

* Compute Optimised
  * Good for game servers, machine learning, batch processing, etc...

* Memory Optimised
  * High performance relational/non-relational DBs
  * Distributed web scale cache stores

* Storage optimised
  * Good for cache for in memory DBs, relational/non-relational DBs
  * Distributed file systems
  * High volume OLTP application

# Security Groups

* Control how traffic allowed into or out of EC2 instances
* Only contain allow rules
* Rules xan refrence by IP or by security group

Security groups act as a firewall on EC2 instances, regulate:

* Access to ports
* Authorised IP ranges (IPv4 + IPv6)
* Control of inbound network
* Control of outbound network

More info:

* Can be attached to multiple instances
* Tied to a specific region/VPC combination
* Lives outside of EC2, EC2 won't see blocked traffic.
* Good to create a separate security group for ssh access.
* If an application is not accessible/request times out, it could be a security group issue.
* If you get a connection refused error then it is likely an application error.
* Inbound traffic is all blocked by default
* Outbound traffic is all allowed by default

# AWS Classic ports

22 = ssh - log into a Linux EC2 instance
21 = FTP - upload files to fileshare
22 = SFTP - upload files using SSH
80 = HTTP - access unsecured websites
443 = HTTPS - access secure websites
3389 = RDP (Remote desktop protocol) - log into a Windows instance

Don't log in as a user inside of EC2 (or other services), apply permissions directly to the service ising IAM roles.

# EC2 purchasing options

* On demand - pay per second
* Reserved - 1 or 3 years
  * Reserved instances - long workloads
  * Convertible reserved instances - long workloads + flexible instances
* Savings plans - 1 or 3 years
  * Commitment to usage in $, can change instances etc but must pay the commited amount, long workload
* Spot Instances
  * short workloads, very cheap, can lose instances
  * Not good for databases or critical processes
* Dedicated Hosts
  * An entire physical server
  * You have visibility of low level hardware, cores, sockets etc...
  * You control instance placement yourself
* Dedicated instances
  * No customers will share your hardware
  * No visibility of low level hardware, or control of instance placement
* Capacity reservations 
  * Reserve capacity in a specific AZ (availablity zone) for any duration
  * Great for short term workloads which need to run uninterupted in a specific AZ
  * If you don't use the reserved capacity then you will still pay for it

## Spot instances

* Discount of up to 90% vs on demand
* Define a max spot price, you get the instance while the spot price is below this amount
* If the spot  price goes over max you have a 2 minute grace period to stop/terminate the instance
* Spot block - get a spot instance for 1-6 hours uninterupted

Spot request object contains:
  * Max prices
  * Desired no of instances
  * Launch specification
  * Request type - 1 time/persistant
    * 1 time - will not restart instance after instance is stopped for whatever reason
    * Persistant - will continue to start instances if some get stopped and the request is still valid
  * Validity... from - until

Spot fleets

* Set of spot instances + on demand instances

* The spot fleet will try to meet target capacity with price constraints
  * Define launch pools, eg. instance type, OS, AZ
  * Can have multiple launch pools and let the fleet choose
  * Spot fleet stops launching instances when it reaches capacity or cost constraint
* Strategies to allocate spot instances
  * lowestPrice: pool with lowest price (cost optimisation, short load)
  * diversified: distributed across pools (availability, long workloads)
  * capacityOptimized: pool with optimal capacity for number of instances