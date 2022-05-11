---
layout: post
title: S3 Encryption
subtitle: How to encrypt file and enforce in S3
categories: Site
tags: [S3, Storage, Encryption]
---

Securing files and keeping them from unathorized access is very important. As you know in the shared responsibility model, you are responsible for the security in the cloud. Encryption in S3 is a very crutial security mechanism used in making sure your bucket and files are secure.

## Types of Encryption 

### Encryption in Transit
This means encryption of data when sending to and from your bucket and you can achieve this with <br/>
- SSL/TLS
- HTTPS

### Encryption at rest
- SSE-S3 <br/>
S3 Managed keys using AES256-bit Encryption `x-amz-server-side-encryption : AES256`

- SSE-KMS <br/>
Key Management Service `x-amz-server-side-encryption : kms : aws`

- SSE-C <br/>
Customer provided keys


## How to add Server-side Encryption

There are two  ways to add server-side encryption: <br/>

- ### Console
Select encryption setting on your S3 bucket, the easiest way is to check a box on the console.

**Adding ecryption to a bucket at creation.**
- Go to AWS Management Console
- Go to storage and click on S3
- Click Create Bucket, Scroll down and find where you have default encryption
- Enable default encryption and select **Amazon Key SSE-S3**
- Be sure to block all public access 
- Click on Create to create a bucket.

**Now let's upload a file to be sure the encryption is active** <br/>

- Click on upload
- Add file (Select file and upload)
- After the upload is successful
- Click on the created file and scroll down to **Server-side**
- Encryption is **Successful!**


- ### Bucket Policy
You can enforce encryption using bucket policy.
A bucket policy can deny all S3 `PUT` requests that don't include the `x-amz-server-side-encryption` parameter in the request header.
