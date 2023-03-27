# IAM - identity and access management

* IAM is a global service.
* Root user create by default, shouldn't be used or shared.
* Users are people (in an organisation) which can be organised into groups, users can be in multiple groups.
* Users and groups are given permissions through a JSON document called a policy.
* Policies usually applied to groups, a policy for an individual user is known as an inline policy.

* IAM policy structure
  * Version - policy language verions
  * ID - Optional ID for the policy
  * Statement
    * Sid - id for statement
    * Effect - allow/deny
    * Principal - user/role to whom the statement applies
    * Action - list of actions (eg. API calls) that the statement is concerned with
    * Resource - list of resources that the statement is concerned with
    * Condition (optional) - conditions under which the policy will be applied

* IAM 
  * Password policy - specific password requirements
  * MFA - password you know + security device you own
    * Virtual MFA device - authenticator app
    * Universal 2 factor security key
    * Hardware key fob MFA device
    * Hardware key fob MFA device for AWS Govcloud

* 3 ways to access AWS
  * Management console - via web browser (password + MFA)
  * CLI - via command line (access keys)
  * SDK - via code (access keys)

* AWS Cloud Shell - a terminal within AWS management console which acts like AWS CLI with current user credentials.

* IAM Roles - a way of defining permissions to be applied to AWS services.

* IAM security tools
  * Credentials Report - account's users and their credential's statuses.
  * Access Advisor - permissions granted to a user and when services were last accessed.

* IAM best practices
  * Only use root account for AWS setup, nothing else
  * One physical user = 1 AWS user
  * Assign users to groups and apply permissions to groups
  * Strong password policy
  * User MFA
  * User roles for giving permissions to AWS services
  * Use access keys for programmatic access to AWS
  * Audit permissions with IAM credentials report
  * Never share AWS IAM users or keys