# Using DynamoDB Time to Live \(TTL\)<a name="time-to-live-ttl-before-you-start"></a>

 
* goal
  * comprehend TTL
    * 👁️optional to use it👁
    * MOST of the hard work is done behind the scenes by DynamoDB on your behalf️
    * -> your implementation proceed smoothly

**Topics**
+ [Formatting an item’s TTL attribute](#time-to-live-ttl-before-you-start-formatting)
+ [Usage notes](#time-to-live-ttl-before-you-start-notes)
+ [Troubleshooting TTL](#time-to-live-ttl-before-you-start-troubleshooting)

## Formatting an item’s TTL attribute<a name="time-to-live-ttl-before-you-start-formatting"></a>

* optional / table
  * == you need to enable
* requirements
  * identify a specific attribute name
    * if an item does NOT have that attribute -> TTL processes ignores it
  * TTL attribute’s value -- must be a -- 
    * top-level `Number` data type
      * otherwise, TTL processes ignore the item -- _Example:_ `String` --
    * timestamp / [Unix epoch time format](https://en.wikipedia.org/wiki/Unix_time) in *seconds*
      * otherwise, TTL processes ignore the item
      * [Epoch Converter](https://epochconverter.com/)
    * dateTimeStamp / expiration < 5 years in the past
      * otherwise, TTL processes NOT expire that item
* uses
  * determine if an item is eligible for expiration


## Usage notes<a name="time-to-live-ttl-before-you-start-notes"></a>

* TODO:
When using TTL, consider the following:
+ Enabling, disabling, or changing TTL settings on a table can take approximately one hour for the settings to propagate and to allow the execution of any further TTL related actions\.
+ You cannot reconfigure TTL to look for a different attribute\. You must disable TTL, and then reenable TTL with the new attribute going forward\.
+ When you use AWS CloudFormation, you can enable TTL when creating a DynamoDB table\. For more information, see the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html)\.
+ You can use AWS Identity and Access Management \(IAM\) policies to prevent unauthorized updates to the TTL attribute on an item or the configuration of TTL\. If you allow access to only specified actions in your existing IAM policies, ensure that your policies are updated to allow `dynamodb:UpdateTimeToLive` for roles that need to enable or disable TTL on tables\. For more information, see [Using Identity\-Based Policies \(IAM Policies\) for Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/using-identity-based-policies.html)\.
+ Consider whether you need to do any post processing of deleted items via Amazon DynamoDB Streams, such as archiving items to an Amazon S3 data lake\. The streams records of TTL deletes stand out in a stream as they are marked as system deletes instead of normal deletes\. You can filter for these system deletes and do some post processing on them by using a combination of AWS Lambda event filters and a AWS Lambda function\. For more information about the additions to the streams records, see [Amazon DynamoDB Streams and Time to Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/time-to-live-ttl-streams.html)\.
+ If data recovery is a concern, we recommend that you back up your tables\.
  + For fully managed table backups, use DynamoDB [on\-demand backups](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html) or [continuous backups with point\-in\-time recovery](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html)\.
  + For a 24\-hour recovery window, you can use Amazon DynamoDB Streams\. For more information, see [Amazon DynamoDB Streams and Time to Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/time-to-live-ttl-streams.html)\.

## Troubleshooting TTL<a name="time-to-live-ttl-before-you-start-troubleshooting"></a>

* things to check 
  * DynamoDB console's *Overview* tab
    * TTL | table is enabled
    * name of the attribute for TTL == your code is writing into items
  * DynamoDB console's Amazon CloudWatch metrics | *Metrics* tab
    * TTL is deleting items -- as -- you expect them to be deleted
  * TTL attribute value is [properly formatted](#time-to-live-ttl-before-you-start-formatting)