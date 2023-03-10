# AWS Cloudfront

* CDN
* Improves read performance, content cached at the edge
* Improves user experience
* 216 points of presence globally
* DDos protection, integration with shield, AWS web app firewall

# Cloudfront - origins

* S3 buckets
  * For distributing files and caching at the edge
  * Security - Cloudfront origin access control

* Custom origin (HTTP)
  * Application load balancer
  * EC2 instance
  * S3 website
  * Any HTTP backend that you like

# Cloudfront - alb or ec2 as a backend

* EC2 instance - must be public
  * Cloudfront has no private VPC integration
  * EC2 must allow public IPs of cloudfront edge locations
* Load balancer - must be public
  * EC2 instances can be private in this case

# Cloudfront - georestriction

* Allow users to access Cloudfront based on allow/deny countries
  * Uses 3rd party geo-ip database
  * Use case: copyright reasons

# Cloudfront - price classes

* Cloudfront endge locations all around the world
* Cost of data per edge location varies
* You can reduce the number of edge locations for cost reduction
  * 200 - the 200 cheapest regions
  * 100 - the 100 cheapest regions

# Cloudfront - cache invalidation

* You can force a full or partial invalidation in Cloudfront, thereby invalidating and clearing the cache

# AWS - global accelerator

* Unicast IP - 1 IP = 1 server
* Anycast IP - 1 IP maps to multiple servers, and client request gets routed to the closest

* Leverage AWS internal network to route to your application
* 2 anycast IP are created for your app
* Anycast IP send traffic to edge locations which send traffic to AWS application on AWS private network

* Works with Elastic IP, EC2 instances, ALB, NLB, public or private
* Consistent performance
* Health checks
* Security - 2 external IP + DDos protection

# AWS - global accelerator vs CLoudfront CDN

Cloudfront
  * Highly cacheable content served at the edge (eg. images)
  * Other cacheable HTTP content such as API calls, dynamic website files
  * Content served at edge, doesn't reach servers
Global accelerator
  * Improves perf for wide range of apps over TCP and UDP
  * Proxies packets at the edge to apps running across 1 or more regions
  * Good for non-http use cases, gaming, voice over IP etc...
  * Good for http use cases requiring static IP, or fast regional failover