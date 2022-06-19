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



![Application LB](/assets/images/banners/Application-Load-Balancer.jpg "Application-LB")

1. **Application Load Balancer - Inteligent:** are intelligent and best suited for load balancing of HTTP & HTTPS traffic. Application Load balancer operate at layer 7 and is "application aware". Makes use of the following to function properly: <br/>
    - Listeners: To check for connection requests from clients, using the protocol and port you configure.
    - Roles: To determine how the load balancer routes the request to it's registered target.
    - Target Group: Each target group routes requests to it's registered targets.

        **Limitations of Application Load Balancer** <br/>
        - With Application Load Balancer, you can only use HTTP and HTTPS.
        - To must deploy at least one SSL/TLS server certificate.



![Network LB](/assets/images/banners/Network-Load-Balancer.jpg "Network-LB")

2. **Network Load Balancer - Performance:** Operates at layer 4. When performance is a big factor to consider in your solution Network Load Balancer is the best choice because it is capable of handling millions of requests in seconds and maintaining ultra-low latency. It can decrypt traffic but you will need to install SSL certificate, and when need protocols that are not supported by Application Load Balancer.


![Application LB](/assets/images/banners/Classic-LB.jpg "Classic-LB")
3. **Classic Load Balancer:** is a classic legacy load balancer. You can balance HTTP/HTTPS application and use layer 7 specific features, such as **x-forwarded** and **sticky sessions**. You can also use strick layer 4 load balancing for applications that rely on TCP protocol.

    - **x-forworded-for:** The server access log contains the ip address of the proxy or load balancer only so when traffic is sent from a load balancer, x-forworded-for request header is used to see the ip address of the client.   
        You get erro _504 Gateway time out_ if your application is has issue and classic load balancer cannot reach your application.

    - **sticky session:** allows you to bind a user session to a specific EC2 instance. Ensures all the requests from the user during the session are sent to the same instance. You can also use sticky session for Application Load Balancer. But the traffic will be sent at the target group level.

### Health Checks.

All AWS load balancers can be configured with health checks. Health checks periodically sends request to load balancers registered instances to test their status, the status of healthy load balancers are 