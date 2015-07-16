---
layout: recipe
title: VPC
cloud-service: true
image: /images/aws-VPC.png
parent: aws.md
service_category: Networking
submenu:
  - { hook: "nat-high-availability", title: "NAT instance high availability." }
---


## NAT instance high availability<a name="nat-high-availability">&nbsp;</a> 

Date: 2015-07-16

### Summary
In Amazon Virtual Private Cloud (VPC), you can use private subnets for instances that you do not want to be directly addressable from the Internet. Instances in a private subnet can access the Internet without exposing their private IP address by routing their traffic through a Network Address Translation (NAT) instance in a public subnet. A NAT instance, however, can introduce a single point of failure to your VPC's outbound traffic.

### Impact
A single point of failure in your design.

### Workaround 1: Use public subnets 
Use public subnets (rather than private subnets) when EC2 instances require outbound access and leverage AWS security groups,  and host firewalls to protect your instances. This is by far the simplest approach.

**Drawback**: This approach has one less layer in your security-in-depth implementation compared to using private subnet for those instances. Note: You still have other protection mechanisms available to you (e.g. AWS security groups); 

### Workaround 2: Use AWS Auto Scaling to automatically heal the NAT instances
This solution has been published in [Cake Team Blogs: Making AWS NAT Instances Highly Available (without the compromises)](http://www.cakesolutions.net/teamblogs/making-aws-nat-instances-highly-available-without-the-compromises#)  
It uses built-in AWS auto-scale capability for automatic recovery of the NAT instances and therefore is more reliable than the solution originally [posted by Amazon](https://aws.amazon.com/articles/2781451301784570). (For example it does not require continuous running shell script to monitor availability and perform failover).   
<br/>  
    
The summary of steps is outlined below. For details please refer to excellent [Cake Team Blogs](http://www.cakesolutions.net/teamblogs/making-aws-nat-instances-highly-available-without-the-compromises#) article. They have done a great job documenting their approach; I had tested it and it worked like a charm.

#### Summary of steps:
    1. Create Elastic Network Interface (ENI) 
        (With attached Elastic IP address (EIP) and disabled Source / Destination Checking)
    2. Create an IAM Role to allow EC2 instance to attach ENI
    3. Create Auto Scaling Launch Configuration for NAT instance
        (Use the IAM Role previously created and user data script listed below. 
        I also used custom AMI instance to store the helper shell script.)
    4. Create an Auto Scaling Group in public subnet
        (Use launch configuration you have created and set min/max instances to 1)
    5. Update route tables in your private subnets to point to NAT ENI you have created for addresses outside of VPC.
  

#### Private Subnet route table 
All traffic outside of local VPC address range goes to your ENI attached to NAT instance
![NAT-HA-ENI-Private-Subnet-Route-table.png]({{ site.baseurl }}/images/AWS/NAT-HA-ENI-Private-Subnet-Route-table.png)
  
#### User data for NAT instance launch configuration
    {% highlight bash %}
    #!/bin/bash
    # store ENI ID
    echo eni-<your_eni_id> > /var/tmp/eni-id.txt
    # attach ENI
    /usr/local/bin/attach_eth1_eni.sh
    sleep 60
    ifdown eth0
    # Configure NAT forwarding
    iptables -t nat -A POSTROUTING -j MASQUERADE
    iptables -A FORWARD -j ACCEPT
    echo 1 > /proc/sys/net/ipv4/ip_forward
    {% endhighlight %}

#### /usr/local/bin/attach_eth1_eni.sh script (on NAT instance)
    {% highlight bash %}
    #!/bin/bash

    function log () {
      echo "$(date +"%b %e %T") $@"
      logger -- $(basename $0)" - $@"
    }

    my_instance_id=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
    my_eni_id=$(cat /var/tmp/eni-id.txt)

    export AWS_DEFAULT_REGION="<your-region>"

    /usr/bin/aws ec2 attach-network-interface --network-interface-id ${my_eni_id} --instance-id ${my_instance_id} --device-index 1
    retcode=$?

    [ "$retcode" -eq 0 ] && { log "eni attachment successful" ; exit 0 ; } || { log "eni attachment failed" ; exit 1 ; }
    {% endhighlight %}



