# Section 3 - Identitiy Access Management & S3
## What is IAM?
Allows you to manage users and their level of access to the AWS console.

Offers:
* Centralized control of AWS account
* Shared access
* Granular permissions
* Identitiy federation (allows users to login with other third-party credentials)
* Multifactor authentication
* Provide temporary access for users/devices
* Allows you to set up a password rotation policy
* Supports PCI DSS compliance

## Key Terminology
**Users** - End users, such as people

**Groups** - A collection of users. Each user in the group will inherit the permissions of the group.

**Policies** - Made up of documents called Policy documents. These are in JSON and give permissions as to what a User/Group/Role is able to do.

**Roles** - You create roles and then assign them to AWS Resources to interact with other resources.

## What we learned from the IAM Lab
IAM is universal - it does not apply to specific regions.

The "root account" is simply the account created when the AWS account is first set up. It has complete Admin Access.

New users have no permissions when first created. 
* An assigned Access Key ID and Secret Access Key are used for programmatic access.
* These values are only viewable once and can not be regenerated.

Access Key ID and Secret Access Key can not be used to sign into the AWS Console.

Always set up Multifactor Authentification on your root account.

Can create your own custom password policies.

## Creating a Billing Alarm Lab

Set alarm to give notifications in the event that AWS account goes over some USD limit.

Use CloudWatch to create a Billing Alarm.
* Uses an SNS topic, which is a way to send an email.

---

## S3 101
Simple Storage Device

So what is it?
* A safe place to store your files
* Object-based storage
* Data is spread accross multiple devices and facilities.

Files can be 0 to 5 TB in size.

There is unlimited storage. Files are stored in Buckets.

Universal namespace, so bucket names must be unique globally. Used in web address, i.e.
`https://bucketname.s3.amazonaws.com`

When uploading to S3, you will receive a HTTP 200 status code sent to browser.

## S3 - Objects
S3 is object-based, where each file is an object that consists of:
* Key (The name of the object)
* Value - data
* Version ID
* Metadata
* Subresources (Access control lists, Torrent)

## Data Consistency Model for S3
How does it keep data consistent?
* Read after Write consistency for PUTS of new Objects
* Eventual Consistency for overwrite PUTS and DELETES

## S3 - Guarantees
Built for 99.99% availability

Amazon guarantees 99.9% availability

Amazon guarantees 99.999999999% durability for S3 information (11 x 9's)

## S3 - Features
* Tiered storage available
* Lifecycle Management
* Versioning
* Encryption
* MFA Delete
* Secure your data using Access Controls Lists and Buclet Policies

## S3 - Storage Classes
1. S3 Standard 
    * 99.99% availability
    * 99.999999999& durability
    * Stored redundantly across multiple devices in multiple facilities
    * Desgined to sustain loss of 2 facilities concurrently.
2. S3 - IA (Infrequently Accessed)
    * Accessed less frequently, but rapidly when needed.
    * Lower fee than S3 Standard, but includes a retrieval fee
3. S3 One Zone - IA
    * Lower cost for infrequently used data
    * Does not have multiple Availability Zone resistance
4. S3 - Intelligent Testing
    * Moves data to most cost-effective access tier
5. S3 Glacier
    * Secure, durable, low-cost storage for data archiving
    * Retrieval time configurable from minutes to hours
6. S3 Glacier Deep Archive
    * Lowest-cost storage class
    * Retrieval time of up to 12 hours is acceptable

First byte latency is milliseconds for each class, except for S3 Glacier which is based on retrieval time configurations.

## S3 - Charges
You are charged in the following ways:
* Storage
* Requests
* Storage Management Pricing
* Data Transfer Pricing
* Transfer Acceleration 
* Cross Region Replication Pricing  

**Cross Region Replication** - Anytime a bucket is updated, cross-region buckets are updated as well.

**Transfer Acceleration** - Enables end users to interact with buckets more faster by using CloudFront's edge location.

## S3 - Exam Tips
S3 is object-based, i.e. allows you to upload files as objects.

Files can be from 0 to 5 TB, stored in buckets.

There is unlimited storage.

S3 is a universal namespace. That is, each bucket name must be uniqie.

`https://acloudguru.s3.amazonaws.com/` uses default location East Virginia.

`https://acloudguru.eu-west-1.amazonaws.com/` uses a non-default location.

Not suitable for storing an OS or Database

Successful uploads will generate a HTTP 200 status code

You can turn on MFA Delete

The key fundamentals of S3 are:
* Key
* Value
* Version ID
* Metadata
* Subresources (Access Control Lists, Torrent)

Consistency Model:
* Read after write consistency for PUTS of new objects.
* Eventual consistency (can take time to propagate) for overwrite PUTS and DELETES

See different Storage Classes

Also, read S3 FAQ prior to taking exam!

## S3 Bucket Lab - Exam Tips
Control access to buckets using either a bucket ACL or using Bucket Policies

Buckets are a universal name space and return HTTP 200 status code on successful upload.

---

## S3 - Pricing Tiers
What makes up the cost of S3?
* Storage (by the GB)
* Requests and Data Retrievals
* Data Transfer
* Management and Replication

Looking at Storage Pricing:
* S3 Intelligent Tiering offers at most same cost of S3 Standard, while potentially offering IA pricing
* S3 Intelligent however does have a Monitoring and Automation fee of $0.0025/mo per 1,000 objects

Other options are much cheaper, with elevated retrieval costs.

![](./img/s3-pricing1.jpg)
![](./img/s3-pricing2.jpg)

## S3 - Pricing: Exam Tips
Understand how to get the best value out of S3 based on some given scenario.
* Typically, try to go with Intelligent Tiering
* If not worried about redundancy, the IA - One Zone may be best

---

## S3 - Security & Encryption
By default, all newly created buckets are PRIVATE. You can set up access control to your buckets using;
* Bucket Policies
* Access Control Lists

S3 buckets can be configured to create access logs, which will log all requests made to that bucket. This can be sent to another bucket and even another bucket in another account.

## S3 Encryption - The Basics
**Encryption In Transit** is when your computer and the server's traffic is encrypted (Think https)

Encryption In Transit is achieved by
* SSL/TLS

**Encryption At Rest** is encrypting that data that is stored server side.

Encryption At Rest is achieved by:
* S3 Managed Keys - SSE-S3 (Service-side encryption)
    * Where Amazon encrypts and decrypts keys for you.
* AWS Key Management Service, Managed Keys - SSE-KMS
    * Where you and Amazon manage the keys together
* Service Side Encryption With Customer Provided Keys - SSE-C
    * Where you give your keys to Amazon that you manage

There may also be Client Side Encryption where you encrypt the object and then upload it to S3.

To add encryption to an S3 bucket, just go into the AWS Console and select S3. Then select your S3 bucket and on the right-hand side of screen, click encryption to select a type.
* Interesting to think about, what this does is mean that if someone were to break in to the AWS server and take a hard disk why my bucket stored on it, it would still be encrypted without the appropriate AWS encryption key.

---

## S3 Versioning - Basics
Stores all versions of an object (including all writes and even deletes if you delete an object).

Once versioning is enabled on a bucket, **Versioning cannot be disabled**, only suspended (you would have to delete the bucket and create a new one to completely turn it off).

Integrates with **Lifecycle** rules, which will be covered in the next lecture to explain how to move things to Glacier.

Versioning's **MFA Delete* capability uses multi-factor authentication, which can be used to provided an additional layer of security.

## S3 Versioning - Lab

After going into bucket, clicking 'properties' allows you to enable Versioning. (Can only enable or suspend)

Then after uploading file to the bucket, you can make said file public. Then, after cicking object URL, you can access this file in a browser.

We can then make an update to the file, and re-upload it to the bucket. After uploading the file, what do you think will happen to the permissions of this file? Will it still be public? Well, if we click on the object URL again we will learn that by uploading a new version of the file, the permissions will update, requiring you to make public again.

Within the bucket, once versioning has been turned on, there is a *Versions* UI area that appears within the S3 bucket overview. If click Show, you will see different version ID's and Size. 
* Note, the bucket's total size is the sum of sizes of each version
* Enabling a Lifecycle policy will retire old version quickly

If using Actions->Delete (when Version->Hide is selected) to remove a file, note it will appear to have been deleted. However if we select Versions->Show, we will see all previous versions, including a new version containing a delete marker. We can delete this delete marker to restore the most recent version.

## S3 Version Lab - Exam Tips
Versioning stores all versions of an object (including all writes and even if you delete an object).

Great backup tool.

Once enabled, Versioning cannot be disabled, only suspended.

Integrates with Lifecycle rules.

Versioning's MFA Delete capability, which uses multi-factor authentication, can be used to provide an additional layer of security.

## S3 Lifecycle Management with S3 - Lab
Can go into a bucket and into 'Management', where we can add a 'lifecycle rule' to manage an object's lifecycle.

Lifecycle rules:
* Enable you to transition objects to the Standard - IA and/or the Amazon Glacier Storage class.
* Can automatically expire objects based on your retention needs.

Click on 'Add lifecycle rule', name it, and select 'Storage class transition'.
* This allows you to enable lifecycle rules on an object's current version, previous versions, or both.
* Can make transitions on curren version such as 'Transition to Standard-IA after 30 days' and 'Transition to Amazon Glacier after 60 days' 
* On previous versions may use something exactly the same.

Next, you can configure expiration:
* For both current and previous versions, may select something like 'Expire current version of object after 425 days from object creation' and 'Permanently delete previous versions after 425 days from begining of previous version'
* Can also clean up incomplete multipart uploads.

Finally, there is the Review page where we can verify Name, Scope, Transitions and Expirations of objects within the bucket.

## S3 Lifecycle Management - Exam Tips
Lifecycle management enables to you to automate moving your objects between different storage tiers.

Can be used in conjunction with versioning (Differentiate lifecycle activity between current and older versions).

---

## AWS Organizations & Consolidated Billing

Can have cross acount access in S3 buckets. Only way to enable this is to turn on AWS Organizations.

What is it?

An account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage.

Allows multiple AWS accounts to be centrally managed.

Can also do **consolidated billing**. The more storage you do, the less you pay. Therefore, allows you to aggregate all of your expenses.

Advantages of Consolidated Billing:
* One bill per AWS account
* Very easy to track charges and allocate costs
* Volume pricing discount

## AWS Organizations & Consolidated Billing - Exam Tips

Some Best Practices with AWS Organizations.
* Always enable MFA on root account.
* Always use a strong and complex password on the root account
* Paying account should be used for billing purposes only. Do not deploy resources into the paying account.
* Enable/Disable AWS services using Service Control Policies (SCP) either on Organizational Units (OU) or on individual accounts.
    * For instance if you don't want the finance department to spin up EC2 instances, you disable EC2 using SCP's.

## Sharing S3 Buckets Across Accounts

There are three different ways to share S3 buckets across accounts:
* Using Bucket Policies & IAM (applies across the entire bucket). Programmatic acccess only
* Using Bucket Access Control List's (ACL's) & IAM (applies tp individual  objects). Programmatic access only.
* Cross-account IAM Roles. Programmatic and Console access.

Roles will be covered later in course, however note it is a way of granting temporary access to an AWS resource, from another service or by another AWS account.

These can be created within IAM, which allow us to create a role for another AWS account by providing the account ID. Attaching policies to the role is what gives the role access to an AWS resource.

## Sharing S3 Buckets Across Accounts - Exam Tips

3 Different ways to share S3 buckets across accounts:
* Using Bucket Policies & IAM (applies across the entire bucket). Programmatic access only.
* Using Bucket ACL's & IAM (applies at the individual object level). Programmatic access only.
* Cross-account IAM Roles. Programmatic AND Console access.

## Cross Region Replication Lab

Need to go into bucket >> Management >> Replication and click add rule. Cross Region Replicatin requires versioning to be enabled on the bucket.

After clicking Add Rule, you can choose to replciate entire bucket OR prefix or tags.

Then set Destination Bucket, where you can choose a bucket in this account or another. Can also choose to create a new one, in which it will ask for you to select a name and region. 

May also change storage class and ownership for replicated bucket. 

Then under configuration options, you will have to select an IAM role, which wll automatically create a Role allowing for S3 Cross Region Replication. 

When we set up cross region replication, we will see it has the sinherited permissions as the bucket we replicated from but we can not see any objects in the bucket, and this is because when we set up cross region replication it is not going to transfer the items already in that bucket - these have to be uploaded manually.

If you create a delete marker in your original bucket, it will not be replicated in the replicated bucket.

## Cross Region Replication - Exam Tips
* Versioning must be enabled on both source and destination buckets.
* Files in an exisiting bucket are not replicated automatically.
* All subsequent updated files will be replicated automatically.
* Deleting individual versions or delete markers will not be replicated.
* Understand what Cross Region Replicatin is at a high level.

## Transfer Acceleration
What is S3 Transfer Acceleration?

It utilizes the CloudFront Network to accelerate your uploads to S3. 
* Instead of uploading directly to your S3 bucket, you can use a distinct URL to upload directly to an edge location which will then transfer that file to S3. You will get a distinct URL to upload to.

Amazon has built a tool to test that acceleration.

---

## CloudFront - Overview
What is CloudFront?

A content delivery network (CDN), which is a system of distributed servers (network) that delivers web pages and other web content to a user based on the geographical locations of the user, the origin of the webpage, and a content delivery network.

## CloudFront - Key Terminology
**Edge Location** - The location where content will be cached. This is seperate from an AWS Region/Availabiliy Zone.

**Origin** - The origin of all of the files that the CDN will distribute. This can be an S3 Bucket, an EC2 instance, an Elastic Load Balancer, or Route53.

**Distribution** - This is the name given to the CDN which consists of a collection of Edge Locations.

There are two different types of Distributions:
* **Web Distribution** - Typically used for Websites.
* **RTMP** - Used for Adobe and Media Streaming.

## CloudFront - How It Works

Can be used to to deliver your entire website, including dynamic, static, streaming, and interactive content using a global network of edge locations. 

Requests for your content are automatically routed to the nearest edge location, so content is delivered with the best possible performance.

## CloudFront - Exam Tips
Know the Key Terminology above.

Edge locations are not just READ only - you can write to them to (i.e. put an object on to them). This is done during Transfer Acceleration.

Objects are cached for the life of the **TTL (Time To Live)**.

You can clear cached objects, you can invalidate an object, but you will be charged.

## Create a CloudFront Distribution - Lab

Listed under Network Content and Delivery. It is global, not under a specific region.

After selecting a delivery method (web or RTMP), you go to create dstribution section where you set the Origin Domain Name, which in this lab is a bucket. Nearly everything in this lab is kept as default.

TTL can be customized (Time to Live). Can also restrict Viewer Access as well using signed URLs or signed Cookies (think about only wanting subscribed users)

After creating distribution, it can take up to 15 - 20 minutes. Same time span for disbling, which must be done before completely deleting.

Once complete, copy the distribution domain name into browser, followed by whatever domain name that has been connected.

Within setitngs, you can create invalidations to prevent some resource to being held within the CDN.

---

## Snowball 
What is it?

A petabyte-scale data transport solution that uses secure appliances to transfer large amounts of data into andn out of AWS. 

Using Snowball addresses common challenges with large-scale data tansfers including high network costs, long transfer times, and security concers. Transfering data with Snowball is simple, fast, secure and can be as little as one-fifth the cost of high-speed internet.

Snowball comes in either 50TB or 80TB sizes.

Snowball uses multiple layers of security dsigned to protect your data including tamper-resistant enclosures, 256-bit encryption and an industry-standard Trust Platform Module (TPM) designed to ensure both security and full chain-of-custody of your data.

Once the data transfer job has been processed and verified, AWS performs a software erasure of the Snowball appliance.

## Snowball Edge

AWS Snowball Edge is a 100TB data transfer device with on-board storage and compute capabilities. You can use Snowball Edge to move large amounts of data into and out of AWS, as a temporary storage tier for large local datasets or to support local workloads in remote or offline locations.

It is like having a portable AWS.

AWS Snowmobile is even more insane. Exa-byte scale data transfer service used to move extremely large services. A 45-foot long truck.

## Snowball - Exam Tips

Understand what Snowball is.

Understand it can import and export from S3.

If you have a question about moving a large amount of data into the cloud, just remember you would use snowball.

---

## Storage Gateway

A service that connects an on-premise software appliance with cloud based storage to provide seamless and secure integration between an organization's on-premises IT environment and AWS's storage infrastructure.

This enables you to securely store data to the AWS cloud for scalable and cost-effective storage.

It si available for download as Virtual Machine image that you install on a host datacenter, it supports either VMware ESXi or Microsoft Hyper-V. 

Then, you can connect to your AWS account through its activation process.

There are three different types of Storage Gateway:
* File Gateway (NFS & SMB) - basically a way of storing files in S3
* Volume Gateway (iSCSI) - A way of storing copies of your hard disk drives or virtual hard disk drives
    * Stored Volumes
    * Cached Volumes
* Tape Gateway (VTK) - which is a virtual tape library

### File Gateway
File are stored as objects in your S3 buckets.

### Volume Gateway
Follows iSCSI protocol

Data written into these volumes can be asychronously backed up as point-in-time snapshots of your volumes, and stored in the cloud as Amazon EBS snapshots.

Snapshots are incremental backups that capture only changed blocks. All snapshot storage is also compressed to minimize your storage charges.

Basically a way of storing virtual hard drives in S3, while looking like EBS snapshots.

#### Stored Volumes
Allows you to store primary data locally, while backing that data to AWS.

Note it is the entire data set. ....
...

#### Cached Volumes
Doestn do entuere date set locally, only using frequently used data ...
...

Come back and review Volume Gateway if necessary.

## Tape Gateway
Back up tape cartridges... like VHS and cassette tapes?

## Exam Tips
Did not appear at all on Cloud Guru's recent exam version.

File Gateway
* File Gateway - For flat files, stored directly on S3

Volume Gateway - using iSCSI.
* Stored Volumes - Entire dataset is stored on site and is asynchronously backed up to S3.
* Cached Volumes - Entiore dataset is stored on S3 and the most frequently accessed data is cached on site.

Gateway Virtual Tape Library

---

## Athena vs Macie

The following two services are often confused for one another. Though they share some similarities, they ar quite different.

### Athena
An interactive query service which enables you to analyze and query data located in S3 using standard SQL
* Serverless, nothing to provision, pay per query / per TB scanned.
* No need to set up complex Extract/Transform/Load (ETL) process

Basically lets you turn S3 into a giant database

What can Athena be used for?
* Can be sued to query log files stored in S3, e.g. ELB logs, S3 access logs, etc
* Generate business reports on data stored in S3
* Analyse AWS cost and usage reports
* Run queries on click-stream data

### Macie
What is PII (Personally Identifiable Information)?
* Personal data used to establish an individual's identity.
* Can include SSN, email, Home address, etc

Macie is a security service that uses Machine Learning and NLP (Natural Language PRocessing) to discover, classify and protect sensitive data stored in S3.

* Uses AI to recognize if your S3 objects contain sensitive data such as PII
* Dashboards, reporting and alerts
* Works directly with data stored innS3
* Can also analyze CloudTrail logs
* Great for PCI-DCS (if your taking credit cardpayments) and preventing ID theft.

## Exam Tips
### Athena
* Athena is an interactive query service
* It allows you to query data located in S3 using standard SQL
* Serverless
* Commonly used to analyse log data stored in S3

### Macie
* Macie uses AI to analyse data in S3 and helps identify PII
* Can also be used to analyse CloudTrail logs for suspicous API activity
* Includes Dashboards, Reports and Alerting
* Great for PCI-DSS compliance and preventing ID theft

---

## S3 & IAM Review

### IAM
Identity Access Management consists of the following:
* Users
* Groups
* Roles
* Policies

Policies are applied to these three. Applying a policy to a group makes all users within that group inherit said polict automatically.

IAM is universal. It does not apply to regions at this time.

The root account is simply the account created when you first set up your AWS account, it has complete Admin access.
* By default, new Users have NO permissions untill granted. Much like a new S3 bucket.

New Users are assigned  Access Key ID & Seret Access Keys when first created. You can not use this to log into the console, however they can be used to to access AWS via APIs and Command Line.
    * You can only view these once and must save them. If you lose them, you have to regenerate them so secure them in a safe place.

Always use MFA on root account.

Also, you can set custom rotating password policies.

### S3

Remember S3 us object based, allowing for files up to 5TB in size. There is unlimited storage and these files are stored in what are known as buckets.

S3 is a universal namespace, so bucket names must be unique globally.

It is object-based storing, so it is not suitable to store an OS or DBMS on.

Successfull uploads will generate a HTTP 200 status code.

By default, all newly credated buckets are PRIVTATE. You setup access control to your bucket using:
* Bucket Policies
* Access Control Lists

S3 can be configured to create access logs, which will log all requests made to the S3 bucket. This can be sent to another bucket or even another bucket in another AWS account.

The Key Fundamentals of S3 Are:
* Key - the name of the object
* Value - the data of teh file
* Version ID - important for versioning
* Metadata - Data about the data, such as tags
* Subresources - such as Access Control Lists of Torrents

Consistency model for S3
* Read after Write consistency for PUTS of new Objects
* Eventual Consistency for overwrite PUTS and DELETES (can take some time to propagate)

You HAVE to understand the different S3 classes of storage:
1. S3 Standard - What most people use
2. S3 - IA (Infrequently Accessed) 
    * For data that is accessed less frequently. Lower fee than S3, but you are charged a retrieval fee.
3. S3 One Zone - IA
    * For when you don't require multiple Availability Zone resilience.
4. S3 - Intelligent Tiering
    * Uses machine learning to choose the most cost-effective access tier.
5. S3 Glacier - for data archival
    * Can configure retrieval time from minutes to hours.
6. S3 Glacier Deep Archive
    * Configure retrieval time of 12 hours.

Understand how to get the best value out of S3
* S3 - Intelligent Tiering is typically best solution
* S3 One Zone - IA is good only for data where data is not crucial because if you lose that zone that's it, youve lost your data.

The order in which tiers are most to least expensive:
1. S3 Standard
2. S3 - IA
3. S3 - Intelligent Tiering
4. S3 One Zone - IA
5. S3 Glacier
6. S3 Glacier Deep Archive

#### S3 Encryption 
Encryption In Transit is achieved by SSL/TLS
* Whenever you go into the console to upload files to S3, you will be connected via https.

Encryption at Rest (Server Side) is achieved by
    * S3 Managed Keys - SSE-S3
    * AWS Key Management Service, Managed Keys - SSE-KMS    
    * Server Side Encrypton with Customer Provided Keys - SSE-C

Client Side Encryption exists where the user encrypts the object before uploading them to S3

#### AWS Organizations
Some best practices with AWS Organizations
* Always enable MFA on root account.
* Always use a strong anf complex password on root acount.
* Paying account should be for billing purposes only. Do not deploy resources into the paying account.
* Enable/Disable AWS services using Service Control Policies (SCP) either on OU (Organizational Units) or on individual accounts.

Three different ways to share S3 buckets across accounts:
1. Using Bucket Policies & IAM (applies accross entire bucket). Programmatic Access Only.
2. Using Bucket ACLs & IAM (individual objects). Programmatic Access Only.
3. Cross-account IAM Roles. Programmatic AND Cosnole roles.

Cross-Region Replication - Just need to know what it is: A way of replicating objects accross regions.

In order for this to work:
* Versioning must be enabled on both the source and destination buckets.
* Files in existing buckets are not replicated automatically.
* However, all subsesequent updated files will be replicated automatically.
* Delete markers are not replicated though.
* Deleting individual versions or delete markers will not be replicated.
* Understand what it is at a high-level

#### Lifecycle Policies
Automates movement between different storage tiers.

Can be used in conjunction with versioning.

Can be applied to current versions and previous versions.

#### S3 Transfer Acceleration
Users will upload files to edge locations and then to a bucket
* Used if you need to increaase performance of users being able to upload files to S3

#### CloudFront
**Edge Location** - This is the location where content will be cached. This is seperate to an AWS Region/Availability Zone.

**Origin** - This the origin of the files that the CDN will distribute. This can be either an S3 Bucket, EC2 Instance, an Elastic Load Balancer, or Route 53.

**Distribution** - This is the name given to the CDN which consists of a collection of Edge Locations.
* Web Distribution -  Used typically for Websites
* RTMP - Used for Adobe Media Streaming

Edge Locations are not READ only, you can write to them to.

CloudFront objects are cached for the TTL. You can clear cached objects by invalidating them but you will be charged.

#### Snowball
Just understand what it is: A big disk used to move data in and out of the AWS cloud. Can be used to import/export to S3

#### Storage Gateway
File Gateway - used for flat files stored directly on S3

Volume Gateway:
* Stored Volumes - Entire dataset is stored on site and is asynchronously backed up to S3
* Cached Volumes - Entire dataset is stored on S3 and only most frequently access data is cahced on site.

Gateway Virtual Tape Library - Used for backing up tape

#### Athena
Just remember what it is and what it allows you to do:
* An interactive query service that
* Allows you to query data located in S3 using standard SQL
* Serverless
* Commonly used to analyse log stored in S3

#### Macie
* Uses AI to analyze data in S3 and helps identity PII (Personall Identifiable Information)
* Can also be used to analye CloudTrail logs for suspcious API activity
* Includees Dashboards, Reports and Alerting
* Great for PCI-DSS compliance and ID theft

#### Finally
Read the S3 FAQs before taking the exam. S3 comes up A LOT.