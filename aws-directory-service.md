---
layout: recipe
title: Directory Service
cloud-service: true
image: /images/aws-directory-service.png
parent: aws.md
service_category: Administration_and_Security
submenu:
- { hook: "Instances-subnet-same-as-AWS-DS", title: "Instances need to be in the same VPC subnets as AWS Directory Service" }
- { hook: "Simple-AD-incompatible-Win2012R2-AD-tools", title: "Simple AD incompatible with Windows Server 2012 R2 Users and Computers" }

---
## Instances need to be in the same VPC subnets as AWS Directory Service<a name="Instances-subnet-same-as-AWS-DS">&nbsp;</a> 
Date: 2015-08-11

### Summary
To use AWS Directory Service Windows EC2 instances must be launched into the same VPC subnets as your AWS Directory Service directory. This issue is documented in [AWS Directory Service FAQs](http://aws.amazon.com/directoryservice/faqs/). The reason to include it here is the impact.

### Impact
To me this restriction essentially prevents the use of AWS Directory Service in any scenario beyond very basic. Even for simple scenarions I typically need to setup multiple VPCs and subnets.

### Workaround
None at this moment.

## Simple AD incompatible with Windows Server 2012 R2 Users and Computers<a name="Simple-AD-incompatible-Win2012R2-AD-tools">&nbsp;</a> 
Date: 2015-08-11

### Summary
Incompatibility between the Simple AD directory and the Active Directory Users and Computers tool on Windows Server 2012 R2 that causes user creation to fail.

### Impact
Obvious. The reason that this is documented here is that the information is not listed in [AWS Directory Service Limits](http://docs.aws.amazon.com/directoryservice/latest/adminguide/limits.html). I only found it in troubleshooting section, which I read after I tried 2012R2 AD tools...

### Workaround
As per AWS documentation until this issue is resolved, create users in Simple AD from a Windows Server 2008 R2 instance.

### References
[AWS Directory Service Admin Guide - Troubleshooting AWS Directory Service](http://docs.aws.amazon.com/directoryservice/latest/adminguide/admin_troubleshooting.html#win2012r2_user_create_fails).


