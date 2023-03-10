# Amazon Route 53

* Highly available, scalable, fully managed and Authoritative DNS
  * Authoratitive = you the customer can update the DNS records

* Also a domain registrar
* Ability to check health of your resources
* Only AWS servie which provides 100% availability SLA
* Route 53 is a references to the traditional DNS port

# Records

How you wnt to route traffic for a domain

* each record contains
  * Domain/subdomain name
  * Record Type
  * Value
  * Routing Poicy
  * TTL

* Route 53 supports following DNS types
  * A / AAAA / CNAME / NS
  * Some others which are mor advanced

Records:
  * A - hostname to ipv4
  * AAAA - hostname to ipv6
  * CNAME - hostame to another hostname
    * Target is a domain name which must have an A/AAAA
    * Cant create CNAME for top node, eg example.com
  * NS - Name Servers for the hosted zone
    * Control how traffic is routed for a domain

Hosted zones
  * Container for records that defines how to route traffic to a domain/subdomain
  * Public hosted zone - contains records specifying how to route traffic from the public internet
  * Private hosted zone- contains records specifying how to route traffic within a private VPC

CNAME vs Alias

* CNAME
  * Points hostname to another hostname
  * Only for non-root domain

* Alias
  * Point hostname to an AWS resources
  * Works for root domain and non root domain
  * Free of charge
  * Native health check
  * Alias is an extension to DNS
  * Auto recognises changes in the resource's IP
  * Alias record is of type A/AAAA
  * You can't set TTL
  * Cannot set a record for an EC2 DNS name, but can point it to most other things on AWS

# Routing policies

* Simple
  * Route traffic to single resources
  * Specify multiple values in single record
  * If multiple values returned, a random one is chosen by the client

* Weighted - control the % of requests which go to each resource
* Latency - based on traffic between users and AWS regions
  * EG. Germany users may be directy to US (if that is the lowest latency)
  * Can be associated with health checks

* Failover
  * Route traffic to the healthy resource (based on health check results)

* Geolocation
  * Routing based on user location
  * Use cases: website localisation, restrict content distribution, load balancing
  * Should have a default record in case there is no location info

* Geoproximity
  * Route traffic based on geographic location of users and resources
  * Shift more traffic to a resources based on bias value
  * You must use route 53 traffic flow
  * Resources
    * AWS resources
    * Non AWS resources (specify lat/long)

* Multi value
  * Use when routing traffic to multiple resources
  * ROute 53 return multiple values/resources
  * Can be associated with health checks
  * Up to 8 healthy records are returned for each multi-value query
  * Not a substitute for ELB load balancing

Health checks
  * Http health checks only for public resources
  * 15 health checkers will monitor the endpoint health
  * If > 18% say its healthy then you are good
  * Health check pass when endpoint responds 2xx or 3xx
  * Health checks can be set to pass/fail based of first 5120 bytes of response

Calculated health checks

* Calculate results of multiple health checks into a single health check
* OR, AND, NOT
* Monitor up to 256 child health checks
* Specify how many children must pass checks to make the parent pass
* Use Case: perform maintenance to site with causing all health checks to fail
  
Private health checks

* Route 53 health checkers live on public web
* Create a public Cloudwatch alarm which can watch a private EC2 instance
* Point the health checker to the public Cloudwatch alarm

Using a different domain registrar

* Update nameservers on the domain registrar to point to AWS route 53