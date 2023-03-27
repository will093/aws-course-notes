# Well architected framework pillars

1) Operational Excellence
2) Security
3) Reliability
4) Performance Efficiency
5) Cost Optimization
6) Sustainability

# Trusted advisor

* Analyse AWS accounts and provide recommendations
* Eg.
  * Cost optimisations
    * Resources which are underutiliszed
    * Reserved instances optimisations
  * Performance
    * High utilisation EC23 instances, CDN optimisations
  * Security
    * MFA enabled, IAM key rotation, exposed access keys
    * S3 bucket permissions, security groups with unrestricted ssh ports
  * Fault tolerance
    * EBS snapshots age, AZ balance
  * Service limits
    * Upgrade before you reach limits