---
layout: recipe
title: RDS
cloud-service: true
image: /images/aws-rds.png
parent: aws.md
service_category: Database
submenu:
  - { hook: "rds-snapshot-copy-different-account", title: "RDS snapshots can not be copied from the AWS account" }
---


## RDS snapshots can not be copied from the AWS account<a name="rds-snapshot-copy-different-account">&nbsp;</a> 

Date: 2015-07-11

### Summary
RDS service does not provide capability to copy snapshots of RDS database instances from AWS account (not even between diferent AWS accounts).

### Impact
The compromise of one account can result in the complete destruction of all data including backups. No capability to recover in this scenario. 

### Mitigation
Setup multi-factor authentication ([MFA](http://aws.amazon.com/iam/details/mfa/)) to reduce the risk of breach. 

**Drawback**: While this reduces the risk of security compromize in my opinion it does not completely address the issue: single AWS account remains a single point-of-failure.

### Workaround: database export/import
Use database export/import capabilities for backup as opposed to AWS snapshots.

**Drawback**: This approach is typically slower that snapshots (e.g. can not use Oracle transportable tablespaces with RDS) and becomes provibitvely slow for large databases.

### Suggested alternative approach
Instantiate database on your own EC2 instance, which allows to copy snapshots of EBS volumes between different AWS accounts.
This seems to be the best solution currently available when the database export/import approach exceeds your recovery time objective (RTO). 

### References
[AWS forum](https://forums.aws.amazon.com/thread.jspa?threadID=124038).

### Related History 
Code Spaces - according to   published information they have used Elastic Block Store volumes and therefore they had more flexibility in data protection compared to RDS. Unfortunately they have chosen to rely on a single AWS account which led to catastrophic results.     
[Code Spaces closes shop after attackers destroy Amazon-hosted customer data.](http://arstechnica.com/security/2014/06/aws-console-breach-leads-to-demise-of-service-with-proven-backup-plan/)
