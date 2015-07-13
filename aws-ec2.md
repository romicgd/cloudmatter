---
layout: recipe
title: EC2
cloud-service: true
image: /images/aws-EC2.png
parent: aws.md
service_category: Compute
submenu:
  - { hook: "EC2-instance-port-mapping", title: "Port mapping for EC2 instances." }
---

## Port mapping for EC2 instances <a name="EC2-instance-port-mapping">&nbsp;</a> 

Date: 2015-07-14

### Summary
Thre is no built-in port-mapping facilities for EC2 instance services (e.g. RDP, Remote PowerShell).

### Workaround: use alternative port mapping facilities
Example: to change RDP ports for EC2 Windows instance one can update Windows registry shown below (remembering to update the security group for the instance to align).


    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TerminalServer\WinStations\RDP-Tcp\PortNumber


### Impact/drawback
Using alternative port-mapping facilities requires extra effort as well as being error-prone/risky compared to a smooth UX offered by Microsoft Azure. 

### References
**TODO: List AWS forum reference here**
[AWS forum]().

### Related 
It would be great if AWS could do something like displayed in Azure portal screenshot below.

   
![Port Mapping in Microsoft Azure.]({{ site.baseurl }}/images/Azure/azure-port-mapping.png)
