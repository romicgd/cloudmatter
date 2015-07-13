## Summary
The purpose of this site is to make deployment in off-premise cloud(e.g. AWS, Azure) easier and more robust. 

Constraints, bugs, and potential pitfallsare listed with suggestions and workarounds.  

Most of limitations and bugs are documented by cloud providers. This site will not repeat those. Rather it focuses on following scenarios: 

* When no documentation has been found describing the encountered issue. 
* When existing documentation does not adequately reflect potential impact of the issue.

## Use
Images on the left side of this page allow to navigate to information for the respective cloud provider.

For each cloud provider:

* Issues are grouped by service offered by the provider.
* Each issue will include the date when it has been encountered.

## Example
If you are considering setting up your database as AWS RDS instance you need to take into account the fact that you can not copy snapshots of RDS database instances from AWS account. Thus the security compromise of a single AWS account may  result in loss of your database **as well as all of your backups** (which is probably not what you intended). See [RDS]({{ site.baseurl }}/aws-rds.html) page for details and possible workarounds.

## Scope
So far: 

* Amazon AWS
* Microsoft Azure (WIP)

## Disclaimer
1. This site reflects personal experience. The information will be added/updated incrementally as new issues are discovered/fixed. If you would like to share your experience by adding information or find that posted information is incorrect please see **Contribution** section below.
2. I'm not a front-end developer by any stretch of imagination: used github pages to setup basic templates and will improve UX time permitting but overall focus is on performing research, running tests and adding information (currently in form of markdown) - not on developing a fancy website. 

## Contribution
Please submit issues and pull requests. [See the code for this site on Github.](https://github.com/romicgd/cloudmatter).

