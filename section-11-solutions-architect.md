Instantiating applications quickly

EC2 instances

* Golden AMI - AMI prepped with the stuff you need for instances
* Bootstrap with User Data is slower, but could be used for dynamic config - eg. fetching some data from somewhere
* Golden AMI + User Data (Elastic Beanstalk)

RDS Databases + EBS Volumes

* Restore from a snapshot AMI.. is just an AMI. It's a container, which links to snapshots of the EBS volumes of an EC2 instance, it has a config for the volumes and it has permissions.

What makes a Golden AMI different (at least how i use the word) is that a golden AMI is prepared for others to use. Certain operating systems let you prepare them for imagine for example sysprep was a way you would prepare windows operating systems for imaging .. it would ready it for setup on a new machine.

# Elastic Beanstalk

Elastic beanstalk is a managed service for deploying applications using various AWS components in an easy way (uses EC2, ASG, ELB, RDS, etc...)

* Components
  * Application - coolection of Elastic Beanstalk components - environment, versions, configuration
  * Version - an iteration of you application code
  * Environment
    * Collection of AWS resources running an application version
    * Tiers - web server environment + worker environment tier
    * You can create multiple environments (dev, test, prod)
      * Single instance is great for dev
      * High availability with load balancer is great for prod



