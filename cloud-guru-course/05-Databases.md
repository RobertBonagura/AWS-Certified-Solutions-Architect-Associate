# Databases on AWS
## Databases 101 
### What is a relational database?
Is structured similarly to an excel spreadsheet

Relational databases on AWS;
* SQL Server (microsoft)
* Oracle
* MySQL Server
* PostgreSQL
* Aurora
* MariaDB

RDS has two key features:
  * Multi-AZ -  For disaster recovery
  * Read Replicas - For performance

### Non Relational Databases
Consist of a Collection, which is like a table.

Inside a collection is a Document, which is simlar to a row.

Instead of using fields or columns, you have Key Value Pairs.

### What is Data Warehouseing?
Used for business intelligent, and pulling in very large complex data sets.

#### OLTP vs OLAP
Online Transaction Processing differs from Online Analytics Processing in terms of the types of queries you will run.

* OLTP for instance allows you to query information about a transaction from the purchaser
* For an OLAP example, the Net Profiit for EMEA and Pacific for the Digital Radio Product pulls in large numbers of records and performs arithmetic on the records to compute some value.
* Amazon's solution to Data Warehousing is Redshift

### ElastiCache
A web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud.

The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.

Supports two open-source in-memory caching engines:
  * Memcached
  * Redis

### Exam Tips
RDS (OLTP)
* SQL, MySQL, PostgreSQL, Oracle, Aurora, MariaDB

DynamoDB (NoSQL)

RedShift (OLAP)
* Used for Business Intelligence or Data Warehousing

Elasticache
* Memcahced
* Redis
* Used to speed up performance of existing databases by caching identical databases

## RDS
RDS runs on virtual machines (that you don't have access to)
* You cannot log into these
* Patching of the RDS Operating System and DB is Amazon's responsibility

RDS is no serverless
* Aurora Serverless IS serverless

## RDS - Back Ups, Multi-AZ & Read Replicas
### Back Ups with RDS
There are two different types of Backups for RDS:
* Automated Backups
* Database Snapshots

#### Automated Backups
Automated Backups allow you to recover your database to any point in time within a "retention period".
* The retention period can be between one and 35 days. 
* Automated Backups will take a full daily snapshot and will also store transaction logs throughout the day.
* When you do a recovery, AWS will first choose the most recent daily back up, and then apply transaction logs relevant to that day.
* This allows you to do a point in time recovery down to a second, within the retention period.

Automated Backups are enabled by default.
* The backup data is stored in S3 and you get free storage space equal to the size of your database

Backups are taken within a defined window.
* During the backup window, storage I/O may be suspended while your data is being backed up and you may experience elevated latency.

#### Databse Snapshots
Done manually, and are stored even after the DB is deleted unlike automated backups.

#### Restoring Backups
Whenever you resotre either an Automatic Backup or a manual Snapshot, the restpred version will be a new RDS instance with a new DNS endpoint.

### Encryption At Rest
Encryption is done using the AWS Key Management Service (KMS). 

Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, are are its automated backups, read replicas and snapshots.

### What is Multi-AZ
Allows you to have an exact copy of your production database in another AZ. 
* AWS handles the replication for you, so when your production database is written to, this write will automatically be synchronized to the stand by database.

In the event of planned database maintenance, DB Instance failure, or an AZ failure, Amazon RDS will automatically failover to the standyby so that database operations can resume quickly without administrative intervention.

It is used for Distaster Recovery only, does not improve performance.

Applies to all RDS except Aurora, which is inherently fault tolerant

### What is Read Replica?
Allow you to have a read-only copy of your production database.

This is achieved by using Asynchronous replication from the primary RDS instance to the read replica.
* You use read replicas primarily very read-heavy database workloads.

Available for all RDS except SQL Server

Things to know about Read Replicas:
* USed for scaling, not disaster recovery!
* Must have automatic backups turned on in order to deploy a read replica.
* You can have up to 5 read replica copies of any database
* You can have read replicas of read replicas (but watch out for latency)
* Each read replica will have its own DNS end point.
* You can have read replicas that have Multi-AZ.
* You can create read replicas of Multi-AZ source databases.
* Read replicas can be promoted to be their own databses. This breaks the replication.
* You can have a read replica in a second region.