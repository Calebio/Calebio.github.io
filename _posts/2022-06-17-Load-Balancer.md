---
layout: post
title: Elastic Load Balancer
subtitle: EBS Overview
categories: Site
tags: [Application, Network, Classic]
---

**Elastic Load Balancer - EBL**
Elastic Load Balance is used to automatically distribute incoming traffic across multiple targets such as Amazon EC2 instances, across multiple AZs.


### Types of Load Balancers

- **Application Load Balancer:** are intelligent and best suited for load balancing of HTTP & HTTPS traffic. Application Load balancer operate at layer 7 and is "application aware". Makes use of the following to function properly: <br/>
    - Listeners: To check for connection requests from clients, using the protocol and port you configure.
    - Roles: To determine how the load balancer routes the request to it's registered target.
    - Target Group: Each target group routes requests to it's registered targets.

**Limitations of Application Load Balancer** <br/>
    - With Application Load Balancer, you can only use HTTP and HTTPS.
    - To must deploy at least one SSL/TLS server certificate.