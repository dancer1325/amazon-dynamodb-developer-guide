# DynamoDB local usage notes<a name="DynamoDBLocal.UsageNotes"></a>

* run applications -- against -- Amazon DynamoDB Local vs Amazon DynamoDB web service
  * configure an endpoint
* if you use Amazon DynamoDB Local, check:
  * if you use `-sharedDb` option -> DynamoDB creates "*shared-local-instance.db*"
    * database file
    * ALL program / connects to DynamoDB -> accesses this file
    * 👁️if you delete the file -> ANY data / you have stored | it is lost 👁️
  * if you omit `-sharedDb` -> DynamoDB creates "*myaccesskeyid_region.db*"
    * database file
    * AWS access key ID and AWS Region == they appear | your application configuration
    * 👁️if you delete the file -> ANY data / you have stored | it is lost 👁️
  * if you use `-inMemory` option -> DynamoDB does NOT write any database files ->
    * ALL data is written to memory
    * if you terminate DynamoDB -> data is lost
  * if you use `-optimizeDbBeforeStartup` option -> you must also specify `-dbPath` parameter
    * `-dbPath = pathToFindDatabaseFile`
  * AWS SDKs for DynamoDB requires your application configuration specify an AWS access key value & AWS Region 
    * NOT need to be valid AWS values
    * if you use valid AWS values -> if you want to run | Amazon DynamoDB web service -> ONLY change the endpoint 
  * DynamoDB local ALWAYS returns `null` for `billingModeSummary.`

**Topics**
+ [Command line options](#DynamoDBLocal.CommandLineOptions)
+ [Setting the local endpoint](#DynamoDBLocal.Endpoint)
+ [Differences between downloadable DynamoDB and the DynamoDB web service](#DynamoDBLocal.Differences)

## Command line options<a name="DynamoDBLocal.CommandLineOptions"></a>

* CL options valid | Amazon DynamoDB Local
  * `-cors` `value`
    * -- enables support for -- cross\-origin resource sharing \(CORS\) for JavaScript
    * `value`
      * `domain1,domain2, ...`
        * comma\-separated "allow" list of specific domains
      * `*`
        * default 
        * == public access
  * `-dbPath` `value`
    * directory / DynamoDB writes its database file
    * if you do NOT specify it -> file is written | current directory
    * NOT possible to specify both `-dbPath` & `-inMemory` 
  * `-delayTransientStatuses`
    * TODO: — Causes DynamoDB to introduce delays for certain operations\. DynamoDB \(downloadable version\) can perform some tasks almost instantaneously, such as create/update/delete operations on tables and indexes\. However, the DynamoDB service requires more time for these tasks\. Setting this parameter helps DynamoDB running on your computer simulate the behavior of the DynamoDB web service more closely\. \(Currently, this parameter introduces delays only for global secondary indexes that are in either *CREATING* or *DELETING* status\.\)
  * `-help`
    * — Prints a usage summary and options\.
  * `-inMemory`
    * — DynamoDB runs in memory instead of using a database file\. When you stop DynamoDB, none of the data is saved\. You can't specify both `-dbPath` and `-inMemory` at once\.
  * `-optimizeDbBeforeStartup`
    * — Optimizes the underlying database tables before starting DynamoDB on your computer\. You also must specify `-dbPath` when you use this parameter\.
  * `-port` `value`
    * — The port number that DynamoDB uses to communicate with your application\. If you don't specify this option, the default port is `8000`\.
    **Note**  
    DynamoDB uses port 8000 by default\. If port 8000 is unavailable, this command throws an exception\. You can use the `-port` option to specify a different port number\. For a complete list of DynamoDB runtime options, including `-port` , type this command:  
    `java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -help`
  * `-sharedDb`
    * — If you specify `-sharedDb`, DynamoDB uses a single database file instead of separate files for each credential and Region\.

## Setting the local endpoint<a name="DynamoDBLocal.Endpoint"></a>

* AWS SDKs & tools
  * for the Amazon DynamoDB web service
    * by default,  -- use -- endpoints
  * for the downloadable version of DynamoDB
    * 👁️-> you MUST specify the local endpoint 👁️

        `http://localhost:8000`

### AWS Command Line Interface<a name="DynamoDBLocal.Endpoint.CLI"></a>

* uses
  * [Creating tables and loading data for code examples in DynamoDB](SampleData.md)
* `--endpoint-url` parameter
  * allows
    * accessing DynamoDB / running locally

    ```
    aws dynamodb list-tables --endpoint-url http://localhost:8000
    ```

### AWS SDKs<a name="DynamoDBLocal.Endpoint.SDK"></a>

* way to specify an endpoint
  * -- depends on the --
    * programming language
    * AWS SDK / you're using
  * check
    * [Java: Setting the AWS Region and endpoint](CodeSamples.Java.md#CodeSamples.Java.RegionAndEndpoint)
      * DynamoDB local supports the AWS SDK for Java V1 and V2
    + [\.NET: Setting the AWS Region and endpoint](CodeSamples.DotNet.md#CodeSamples.DotNet.RegionAndEndpoint)

## Differences between downloadable DynamoDB and the DynamoDB web service<a name="DynamoDBLocal.Differences"></a>

The downloadable version of DynamoDB is intended for development and testing purposes only\. By comparison, the DynamoDB web service is a managed service with scalability, availability, and durability features that make it ideal for production use\. 

The downloadable version of DynamoDB differs from the web service in the following ways:
+  DynamoDB local is capped at 100 items for transactions\. 
+ AWS Regions and distinct AWS accounts are not supported at the client level\.
+ Provisioned throughput settings are ignored in downloadable DynamoDB, even though the `CreateTable` operation requires them\. For `CreateTable`, you can specify any numbers you want for provisioned read and write throughput, even though these numbers are not used\. You can call `UpdateTable` as many times as you want per day\. However, any changes to provisioned throughput values are ignored\.
+ `Scan` operations are performed sequentially\. Parallel scans are not supported\. The `Segment` and `TotalSegments` parameters of the `Scan` operation are ignored\.
+ The speed of read and write operations on table data is limited only by the speed of your computer\. `CreateTable`, `UpdateTable`, and `DeleteTable` operations occur immediately, and table state is always ACTIVE\. `UpdateTable` operations that change only the provisioned throughput settings on tables or global secondary indexes occur immediately\. If an `UpdateTable` operation creates or deletes any global secondary indexes, then those indexes transition through normal states \(such as CREATING and DELETING, respectively\) before they become an ACTIVE state\. The table remains ACTIVE during this time\.
+ Read operations are eventually consistent\. However, due to the speed of DynamoDB running on your computer, most reads appear to be strongly consistent\.
+ Item collection metrics and item collection sizes are not tracked\. In operation responses, nulls are returned instead of item collection metrics\.
+ In DynamoDB, there is a 1 MB limit on data returned per result set\. Both the DynamoDB web service and the downloadable version enforce this limit\. However, when querying an index, the DynamoDB service calculates only the size of the projected key and attributes\. By contrast, the downloadable version of DynamoDB calculates the size of the entire item\.
+ If you're using DynamoDB Streams, the rate at which shards are created might differ\. In the DynamoDB web service, shard\-creation behavior is partially influenced by table partition activity\. When you run DynamoDB locally, there is no table partitioning\. In either case, shards are ephemeral, so your application should not be dependent on shard behavior\.
+  `TransactionConflictExceptions` are not thrown by downloadable DynamoDB for transactional APIs\. We recommend that you use a Java mocking framework to simulate `TransactionConflictExceptions` in the DynamoDB handler to test how your application responds to conflicting transactions\. 
+ In the DynamoDB web service, table names are case sensitive\. A table named `Authors` and one named `authors` can both exist as separate tables\. In the downloadable version, table names are case insensitive, and attempting to create these two tables would result in an error\.