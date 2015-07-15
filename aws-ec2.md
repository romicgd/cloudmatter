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
Example: to change RDP ports for EC2 Windows instance one can update Windows registry shown below.  
<br/>    
    
<font color="red">Please remember to update AWS security group and Windows firewall settings for the EC2 instance to align.</font>   
<br/>

{% highlight registry %}
    [HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TerminalServer\WinStations\RDP-Tcp]
    PortNumber=<your_port_number>
{% endhighlight %}

### Impact/drawback
Using alternative port-mapping facilities requires extra effort as well as being error-prone/risky (e.g. see [AWS  forum thread: reset RDP Port](https://forums.aws.amazon.com/thread.jspa?threadID=75501)) compared to a smooth UX offered by Microsoft Azure. 

### References
**TODO: List AWS forum reference here**
[AWS forum]().

### Related 
It would be great if AWS implemented a port mapping capability similar to Mixrosoft Azure (Azure portal screenshot is displayed below).  
<br/>
   
![Port Mapping in Microsoft Azure.]({{ site.baseurl }}/images/Azure/azure-port-mapping.png)
