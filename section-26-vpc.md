# CIDR

* Classless Inter-Domain Routing - method for allocating IP addresses
* Used in security groups and AWS networking

* They help to define an IP address range
  * We've seen ww.xx.yy.xx/32 which is 1 IP
  * 0.0.0.0/0 is all IPs

* CIDR consists of 2 components
* Base IP
  * Represents IP in the range XX.XX.XX.XX
  * Eg. 10.0.0.0, 192.168.0.0
* Subnet mask
  * Defines how many bits can change in the IP
  * Eg. /0, /24, /32
  * Can take 2 forms:
    * /8 255.0.0.0
    * /16 255.255.0.0
    * /24 255.255.255.0
    * /32 255.255.255.255

# Understanding CIDR - Subnet mask

* Subnet mask allows part of underlying IP to get additional next values from the base IP

# Understanding CIDR - Exercise

Private IP can only have a few ranges

* 10.x.x.x - big networks
* 172.16.x.x - AWS default
* 192.168.x.x - home networks

All other IP are public

# Default VPC overview

* All new AWS accounts have default VPC
* New EC2 instances launched into VPC if no subnet specified
* Default VPC has internet connectivity and all EC2 instances have public IPs
* We get a public and a private IPv4 DNS name

# VPC (virtual private cloud)

* You can have up to 5 VPC per region (soft limit)
* Max CIDR per VPC is 5. For each CIDR:
  * Min size is /28 (16 IPs)
  * Max size is /16 (65536 IPs)
* Because VPC is private, only private IPv4 allowed
* Your VPC CIDR should not overlap with your other networks (in case you want to merge them)

# Subnet

* Subnet is a range of IP addresses within a VPC
* AWS reserves 5 IPs in each subnet
* These 5 IPs are not available and cannot be assigned

# Internet Gateway (IQW)

* Allows resources in a VPC to connect to the internet
* Scales horizontally and highly available/redundant
* Must be created separately from a VPC
* One VPC can only be attached to one IGW and vice versa

* Internet Gatweays alone do not allow internet access
* Route tables must also be edited

# Bastion Host

* Bastion Host allows us to SSH into a private instance
* Bastion is in the public subnet, which is connected to all private subnets
* Bastion Host Security Group must allow inbound from public internet on port 22 from restricted CIDR, eg. public CIDR of your corporation
* Security group of the EC2 instances must allow Security Group of the Bastion Host, or private IP of Bastion Host

# NAT instances

* NAT = network address translation
* Allows EC2 instances in private subnets to connect to the internet
* Must be launched in a public subnet
* Must disable EC2 Source/destination check
* Must have Elastic IP attached
* Route Tables must be configurated to route traffic from private subnets to NAT instance

# NAT Gateway

* AWS managed NAT, higher bandwidth, high availability, no admin
* Pay per hour for usage and bandwidth
* NATGW is created in a specific AZ, uses an Elastic IP
* Can't be used by EC2 instance in same subnet
* Requires an IGW... private subnet => NATGW => IGW
* 5 GBPS of bandwidth scaling to 45 GBPS
* No security groups needed

# Nat Gateway with High Availability

* NAT gateway is resilient within a single AZ
* Create multiple NAT gateways in multiple AZs for fault tolerance
* No cross AZ failover needed as if AZ goes down it doesn't need NAT

# Security groups and NACLs

* Network Access Control List
* A sort of firewall which controls traffic to and from subnets
* 1 NACL per subnet, new subnets assigned to the default NACL
* You define NACL rules:
  * Rules have a number, lower number = higher precedence
  * First rule match drives the decision
  * Allow/deny specific IP ranges
  * Last rule is a * DENY to deny all unmatched traffic
  * AWS recommends adding rules by increment of 100
* Newly created NACL denies everything
* NACL great to block specific IP address at subnet level

* Default NACL
  * Accepts everything inbound/outbound for associated subnets

* Ephemeral Ports
  * Any 2 endpoints must use ports to establish a connection
  * Clients connect to a defined port and expect a response on an ephemeral port
  * Different Operating Systems use different port range

# VPC Peering

* Privately connect 2 VPCs using AWS network
* Must not have overlapping CIDRs
* Behave as in same network
* VPC Peering is not transitive a and b, b and c, does not mean a and c

# VPC endpoints (AWS private link)

* Every AWS service is publicly exposed
* VPC endpoints allow you to connect to AWS services using a private network instead of using the public internet
* Redundant and scale horizontally
* Remove need for IGW, NATGW to access AWS services
* In case of issues:
  * Check DNS setting resolution in your VPC
  * Check route tables

* Types of Endpoints
  * Interface Endpoints
    * Provisions an ENI as an entry point
    * Supports most AWS services
    * Cost per hour and per GB data processed
  * Gateway Endpoints
    * Provisions a gateway and must be used as a target in a route table (no security groups)
    * Supports both S3 and DynamoDB
    * Free

# VPC Flow Logs

* Capture information about IP traffic goin into your interfaces:
  * VPC Flow logs
  * Subnet Flow logs
  * Elastic Network Interface Flow Logs
* Help to monitor and troubleshoot connectivity issues
* Flow logs data can go to S3/CloudWatch logs
* Captures Network information from AWS managed services: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway

# VPC Flow Logs Syntax

* srcaddre and dstaddr - help identify problem IP
* srcport and dstport - help identify problem ports
* Action - success or failure of request (due to Security Group / NACL)

# AWS Site to Site VPN

* Virtual Private Gateway (VGW)
  * VPN concentrator on the AWS side of VPN connection
  * VGW is created and attached to the VPC from which you want to create the site to site connection
* Customer Gateway (CGW)
  * Software Application or physical device on customer side of VPN connection

* CGD - use public IP of the CGD, or of the NAT if it is behind one
* You must enable Route propagation for the VPG in the route table for your subnets
* Ping - enable ICMP protocol on security groups

* AWS VPN CloudHub
  * Allows CGD devices to talk to each other via the VPG
  * ie. you essentially create a VPN between VPC and customer networks

# Direct Connect (DX)

* Provides dedicated private connection from remote network to your VPC
* Dedicated connections must be set up between your DC and AWS direct connect locations
* You must set up a VIrtual Private Gateway on your VPC
* Access public and private resources on the same connection
* Use cases:
  * Increase bandwidth throughput
  * More consistent network experience for real time data
  * Hybrid environments (on prem and cloud)
* Supports IPv4 and IPv6
* Takes 1 month to set up, must set up physical AWS Direct Connect Location
* Traffic won't have to go over public internet
* Direct connect gateway
  * Connect to multiple VPCs in different regions, but on the same AWS account

* Dedicated Connection  - 1GBPS, 10GBPS, 100GBPS
  * Physical ethernet port dedicated to a customer
* Hosted Connection
  * Connection made via AWS Direct Connect Partner
  * Capacity can be added or removed on demand

* Data in transit not encrypted but is on a private network
  * You can add a VPN to make sure the data is encrypted if you want

* Resiliency
  * High resiliency - one connection at each of multiple locations
  * Max resiliency - two connections at each of multiple locations

# Transit Gateway

* Transitive peering between thousands of VPC and on-premises, hub-and-spoke connection.
* Regional resource, can work cross region
* You can peer transit gateways across regions
* Route tables: limit which VPC can talk with other VPC
* Works with Direct Connect Gateway and VPN connections
* Only service which supports IP multicast

* ECMP - equal cost multi path routing
* Use multiple VPN to transit gateway (this multiplies the bandwidth, with each tunnel being 2.5GBPS)

# VPC traffic mirroring

* Capture and inspect network traffic in your VPC
* Route traffic to security appliances that you manage
* Capture the traffic
  * From (source) ENI
  * To (targets) ENI or network load balancer
* Capture all packets or the packets of your interest
* Source and target could be in same VPC or peered VPCs
* Use cases:
  * Content inspection
  * Threat monitoring
  * troubleshooting

# IPv6

* Successor of IPv4
* All are public and internet routable (no private range)
* Format x.x.x.x.x.x.x.x (x is hexadecimal 0000 to ffff)

* IPv4 cannot be disabled for VPC or subnets, but can us IPv6 to run in dual stack mode

# Egress only internet gateway

* Allows outbound IPv6 but not inbound or IPv4
* You must update the route tables

# AWS network firewall

* Layer 3 to layer 7 protection
* Internally it uses the AWS gateway load balancer
* Supports 1000s of rules
  * Filter by IP, port, protocol, domain
  * Allow, drop or get alerted based on rules
  * Send logs to S3, Cloudwatch logs, Kinesis firehose