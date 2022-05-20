---
layout: post
title: Networking EC2 intances
subtitle: Different networking options for your EC2 instance
categories: Site
tags: [EC2, Networking, ENI, EN, EFA, SR-IOV]

---

There are different networking options for EC2 instances and these options are dependent on a user's **Cost** and **Purpose**.<br/>
You can attach any of the 3 different virtual networking cards to your EC2 instance. Now let's explore the networking options. 

- ## Elastic Network Interface - ENI
This virtual networking card is used for basic day-to-day networking. It allows private IPV4 address, IPV4 public address, many IPV6 addresses, MAC Address, and 1 or more Security groups.<br/>

**ENI Use Case** <br/>
- ENI is mostly used to create management networks
- Enables network and security appliances in your VPC.
- You can use ENI to create dual-homed instances with workloads/roles on distinct subnets.
- ENI is a way to create a low-budget, high-performance solution.



- ## Enhanced Network - EN

**What is Enhanced Network?** <br/>
As the term implies, Enhanced network card is an higher networking option that provides high performance networking between 10Gbps - 100Gbp susing **Single root I/O virtualization(SR-IOV)** that enables high network performance and lower CPU utilization.

- **Performance :** provides higher bandwidth, higher packet per second(PPS)
- **CPU utilization :** Consistently lower inter-instance latencies


**Enhanced Network(EN) comes with two options and you use it with any of the two options below** <br/>

- Elastic Network Adapter - ENA :  
Supports network speed of up to 100Gbps for supported instance types.
ENA can be used for almost any option you choose

- Intel 82599 Virtual Function(VF)interface : 
Suppoorts network speed of up to 10Gbps for supported instance types. Typically used for older instances

I'd personally recommend ENA over VF Interface because it's modern and a lot faster.

- ## Elastic Fabric Adapter - EFA 
What is Elastic Fabric Adapter EFA?<br/>
EFA is a network device you can be attached to your Amazon EC2 instance that accelerates high performance computing (HPC) and Machine Learning applications, It provides lower and more consistent latency and higher network throughput than the traditional TCP used in Cloud-based HPC systems.<br/>
EFA can use **OS-Bypass**. OS-Bypass makes it a lot faster because it enables HPC and Machine learning appliactions to bypass OS kernel and communicate directly with the EFA device. OS-Bypass is only supported on Linux OS and is not supported on windows as at the time this article was published.



<p>
These notes were taking from A cloud guru.
<p/>