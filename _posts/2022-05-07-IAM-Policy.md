---
layout: post
title: IAM policy
subtitle: How to control user access using IAM policy
categories: Site
tags: [AWS, IAM Policy, User-Access-Management]
---

We are going to talk about Resource based policies
 

## What is a policy document? 

A policy is an object in AWS that, when associated with an entity or resource, defines their permissions. AWS evaluates these policies when a principal, such as a user, makes a request. Permissions in the policies determine whether the request is allowed or denied. Most policies are stored in AWS as JSON documents.

## What is IAM Policy?

IAM policies define permissions for the entities you attach them to. For example, to grant access to an IAM role, attach a policy to the role. The permissions defined in the policy determine whether requests are  allowed or denied. You also can attach policies to some resources, such as Amazon S3 buckets, to grant direct, cross-account access. And you can attach policies to an AWS organization or organizational unit to restrict access across multiple accounts. AWS evaluates these policies when an IAM role makes a request. 




### Types of Policies

-  Identity-based policies 
-  Resource-based policies
- Permissions boundaries  
- Organizations SCPs 
- Access control lists (ACLs)  
- Session policies


Today we will learn about the **Resource-Policies**
 


## Why do we need control user access access
It further helps in ensuring that users get the least priviledge and access required for their job


## How can you control user access with IAM policy?
You can control access by assigning permissions through policy documents which are made up of JSON

### Here's an example of what Policy Document looks like
```json
{
    "Version": "2012-10-17",
     "Statement": {
         {
             "Effect": "Allow",
             "Action": "*",
             "Resource": "*"
         }
     }
}
``` 
`*` means All or Everything <br/>
`Allow` means permit

So  the above policy document statement says, you have the permission to perform every `*`  `Action` on  every `Resource`. This grants Admin access to the resource that this resource.

We can assign policy documents to Groups, Users and Roles
