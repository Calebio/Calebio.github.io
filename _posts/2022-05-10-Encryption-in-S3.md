---
layout: post
title: S3 Encryption
subtitle: How to encrypt S3 bucket and files
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
**Server-side Encryption**
- SSE-S3 <br/>
S3 Amazon Managed keys using AES256-bit Encryption `x-amz-server-side-encryption : AES256`

- SSE-KMS <br/>
Key Management Service `x-amz-server-side-encryption : kms : aws`

- SSE-C <br/>
Customer provided keys

**Client-Side Encryption**<br/>
This is a case where you as a user encrypt your files before you upload them to your S3 bucket. 




Everytime a user uploads a file to S3, a `PUT` request is sent,
Below it a `PUT` Request when encryption is not enabled. <br/>

```json
PUT /Key+ HTTP/1.1
Host: Bucket.s3.amazonaws.com
Date: Tue, 09 May 2022  
Authorization: authorization string
Content-Type: text/plain
Content-Length: 27364
x-amz-meta-author : Caleb
Expect: 100-continue
[27363 bytes of object data]
```

**Use Case**<br/>
Your company might have a policy where all files going into a particular bucket has to be protected from unathorized access. The solution is applying encryption to the bucket. <br/>


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

 When encryption is enabled, the `PUT` request looks like

 ```json 
  
PUT /Key+ HTTP/1.1
Host: Bucket.s3.amazonaws.com
Date: Tue, 09 May 2022  
Authorization: authorization string
Content-Type: text/plain
Content-Length: 27364
x-amz-meta-author : Caleb
Expect: 100-continue
x-amz-server-side-encryption : AES256
[27363 bytes of object data]

 ```


- ### Bucket Policy
You can enforce encryption using bucket policy.
A bucket policy can deny all S3 `PUT` requests that has no encryption parameter in the request header just like `x-amz-server-side-encryption : AES256`. A more detailed way of enforcing bucket policy  [Bucket-Policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)

