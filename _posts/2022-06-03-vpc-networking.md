---
layout: post
title: Amazon VPC (Networking)
subtitle: Virtual Private Cloud
categories: Site
tags: [Subnet, VPN, route-table, VPC]

---

**What is VPC?**
VPC means Virtual Private Cloud, youn can think of **VPC** as a virtual data centre in the cloud. It is a logically iisolated part of AWS Cloud where can define your own network and have complete control of the virtual networm including your own ip address subnets, route tables and network gateways. <br/>

**What can you do with VPC??** <br/>
You can : <br/>
- Lunch EC2 instance into the subnet of your choice
- Have custom ip addresses 
- Have route tables
- Have an internet gateway
- Access Control List (ACL)
- More control over the network

Access Control List can be used to block specific ip addresses  from accessing the network for security purposes.
One subnet is always available in one availability zone by default.
CIDR.xyz is used to calculate the number of ip addresses within a subnet and comes in handy with calculating ip addresses for your network.

**What is VPN**
VPN means Virtual Private Network. In a case where you already have a physical corporate datacenter and you want to leverage the AWS cloud services as an extension in the cloud, you will need the VPN hardware to ensure a secure connection between the datacenter and the cloud.

There are mainly two types of VPC networks : <br/>
1. **Default VPC :**
- default VPC is user friendly
- All subnets in default VPC have route out to the internet 
- Each EC2 instance has both public and Private IP addresses.

2. **Custom VPC**
- Is fully customized 
- Takes time to setup


**these notes were taking on A Cloud Guru**
