# Serverless
## Lambda Basics
AWS Lambda is a compute service where you can upload your code and create a Lambda function.
* AWS Lambda takes care of provisioning and managing the servers that you use to run the code.
* You don't have to worry about operating systems, patching, scaling, etc.

You can use Lambda in the following ways:
* As an event-driven compute service where AWS Lambda runs your code in response to events.
  * These events could be changes to data in an Amazon S3 bucket or an Amazon DynamoDB table.
* As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls made using AWS SDKs.

### Traditional vs Serverless Architecture
* API Gateway will be at front end to serve the request
* Lambda will scale out automatically
* Next you may write to a serverless DB

### Languages Supported by Lambda
* Node.js
* Java
* Python
* C#
* Go
* Powershell

### How is Lambda Priced?
On two things:
1. Number of Requests
    * First 1 million requests are free
    * $0.20 per 1 million requests thereafter
2. Duration
    * Duration is calculated from the time code beings executing until it returns or terminates, rounded up to the nearest 100ms
    * The price depends on the amount of memory you allocate to your function
    * You are charged $0.000001667 for every GB-second used

### Why is it So Good?
* No servers
* Continuous Scaling
* Super cheap

## Lambda Exam Tips
Lambda scales out (not up) automatically.
* Which means if you have 5 invocations, it will scale out to 5 different lambda functions being executed at the same time.

Lambda events are independent, 1 event : 1 function.

Lambda is serverless.

Know what services are serverless.
* RDS for instance is not serverless
* S3, DynamoDB, API Gateway all are

Lambda functions can trigger other lambda functions.

Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening.

Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets for instance.

Know your triggers.

## Serverless Summary
#### Traditional Architecture:
Elastic Load Balancer -> EC2 Instances -> Storing data in a traditional database

* Still highly available and can tolerate failure if architected well.
* Is limited in how much it can scale, yet may be limited in its RDS instances

#### Serverless Architecture:
API Gateway -> Lambda -> Dynamo DB

* From both a high-availability point of view and a cost saving point of view, serverless architecture tends to be the better option

### Lambda Exam Tips
Lambda scales out (not up) automatically.
* Which means if you have 5 invocations, it will scale out to 5 different lambda functions being executed at the same time.

Lambda events are independent, 1 event : 1 function.

Lambda is serverless.

Know what services are serverless.
* RDS for instance is not serverless
* S3, DynamoDB, API Gateway all are

Lambda functions can trigger other lambda functions.

Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening.

Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets for instance.

Know your triggers.
