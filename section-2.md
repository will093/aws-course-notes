* AWS Regions 
  * A region is a cluster of data centers
  * How do you choose an AWS region? 
    * 1) Compliance - if data should not leave a specific country/location
    * 2) Proximity - close proximity to end user will reduce latency
    * 3) Available services - not all services available in all regions.
    * 4) Pricing - varies between regions

* AWS availability zones
  * Each region has 3-6 availability zones (AZs)
  * Each AZ is 1 or more discreet isolated data centers
  * AZs connected with high bandwidth, ultra-low latency networking

* AWS points of presence
  * These are edge locations and regional caches (whats the difference between these 2?)
  * These reduce latency/can utilise a CDN.

Some AWS services are global, but most are region scoped