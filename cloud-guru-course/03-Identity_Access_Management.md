# Section 3 - Identitiy Access Management
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

Universal namespac, so bucket names must be unique globally. Used in web address, i.e.
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

## S3 - Security & Encryption
By default, all newly created buckets are PRIVATE.
