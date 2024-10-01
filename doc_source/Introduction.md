# What is Amazon DynamoDB?<a name="Introduction"></a>

* Amazon DynamoDB
  * := database service
  * characteristics
    * fully managed 
      * == NOT have to worry about
        * hardware provisioning
        * setup
        * configuration
        * replication
        * software patching
        * cluster scaling / NO
          * downtime
          * performance degradation
    * NoSQL
    * possibility to [encryption at rest](EncryptionAtRest.md)
  * allows
    * getting
      * ANY amount of data
      * ANY level of request traffic
    * creating on-demand backup capability
      * == FULL backups of your tables
      * uses
        * long\-term retention
        * regulatory compliance needs
      * [Using On\-Demand backup and restore for DynamoDB](BackupRestore.md)
    * point-in-time recovery -- for your -- Amazon DynamoDB tables
      * [Point\-in\-time recovery: How it works](PointInTimeRecovery_Howitworks.md)
      * uses
        * accidental write or delete operations
      * 👁️ valid | last 35 days 👁️
    * deleting automatically expired items | tables
      * [Expiring items by using DynamoDB Time to Live \(TTL\)](TTL.md)
      * Reason: reduce
        * storage usage
        * cost
  * provides
    * fast & predictable performance
    * seamless scalability
    *  
* AWS Management Console
  * allows
    * monitoring
      * resource utilization
      * performance metrics

## High availability and durability<a name="ddb_highavailability"></a>

* high availability
  * 👁️data and traffic for your tables -- is automatically spread over a -- sufficient number of servers 👁️/ 
    * handle your
      * throughput
      * storage requirements
    * maintain performance
      * consistent
      * fast
* high durability
  * data is 
    * stored | solid-state disks \(SSDs\)
    * replicated -- across -- multiple Availability Zones | 1 AWS Region
* global tables
  * [Global tables](GlobalTables.md)
  * allows
    * 👁️DynamoDB tables | AWS Region -- keep in sync with -- DynamoDB tables | another AWS Region 👁️

## Getting started with DynamoDB<a name="ddb_getstarted"></a>

* **[Amazon DynamoDB: How it works](HowItWorks.md)—**
  * essential DynamoDB concepts
* **[Setting up DynamoDB](SettingUp.md)—**
* **[Accessing DynamoDB](AccessingDynamoDB.md)—**
  * via
    * AWS Console
    * AWS CLI
    * AWS API
* [Getting started with DynamoDB](GettingStartedDynamoDB.md)
* [Getting started with DynamoDB and the AWS SDKs](GettingStarted.md)
* about application development
  * [Programming with DynamoDB and the AWS SDKs](Programming.md)
  * [Working with tables, items, queries, scans, and indexes](WorkingWithDynamo.md)
* [Best practices for designing and architecting with DynamoDB / maximize performand & minimize costs](best-practices.md)
* [Adding tags and labels to DynamoDB resources](Tagging.md)
* [Amazon DynamoDB resources: guides, tools](https://aws.amazon.com/dynamodb/resources/) 
* [AWS Database Migration Service User Guide](https://docs.aws.amazon.com/dms/latest/userguide/)
  * allows
    * migrating data from a relational database or MongoDB -- to a -- DynamoDB table 
  * uses
    * [Amazon DynamoDB database -- as a -- migration target](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.DynamoDB.html)


## DynamoDB tutorials<a name="ddb_tutorials"></a>

* goal
  * complete E2E procedures -- to familiarize with -- DynamoDB
* requirements
  * AWS Account
    * even with free tier
* [Build an Application Using a NoSQL Key\-Value Data Store](http://aws.amazon.com/getting-started/guides/build-an-application-using-a-no-sql-key-value-data-store/?ref=docs_gateway/dynamodb/Introduction.html) 
* [Create and Query a NoSQL Table with Amazon DynamoDB](http://aws.amazon.com/getting-started/hands-on/create-nosql-table/?ref=docs_gateway/dynamodb/Introduction.html) 