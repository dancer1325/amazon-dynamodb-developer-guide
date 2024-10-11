# Enabling Time to Live \(TTL\)<a name="time-to-live-ttl-how-to"></a>

* ways to enable TTL
  * Amazon DynamoDB console
  * AWS CLI
  * [Amazon DynamoDB API Reference](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/)

**Topics**
+ [Enable Time to Live \(console\)](#time-to-live-ttl-how-to-enable-console)
+ [Enable Time to Live \(AWS CLI\)](#time-to-live-ttl-how-to-enable-cli-sdk)

## Enable Time to Live \(console\)<a name="time-to-live-ttl-how-to-enable-console"></a>

* TODO:
* steps
  1. Sign in to the AWS Management Console and open the DynamoDB console at [https://console\.aws\.amazon\.com/dynamodb/](https://console.aws.amazon.com/dynamodb/)\.

  1. Choose **Tables**, and then choose the table that you want to modify\.

  1. In the **Additional settings** tab, in the **Time to Live \(TTL\)** section, choose **Enable**\.  
  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/images/ttl_table.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)

  1. In the **Enable Time to Live \(TTL\)** page, enter the **TTL attribute name**\.  
  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/images/ttl_enable.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)

  1. \(Optional\) To preview some of the items that will be deleted when TTL is enabled, choose **Run preview**\.
  **Warning**  
  This provides you with a sample list of items\. It does not provide you with a complete list of items that will be deleted by TTL\.

  1. Choose **Enable TTL** to save the settings and enable TTL\.

Now that TTL is enabled, the TTL attribute is marked **TTL** when you view items on the DynamoDB console\. You can view the date and time that an item expires by hovering your pointer over the attribute\.

## Enable Time to Live \(AWS CLI\)<a name="time-to-live-ttl-how-to-enable-cli-sdk"></a>

1. Enable TTL | table

   ```
   aws dynamodb update-time-to-live --table-name TTLExample --time-to-live-specification "Enabled=true, AttributeName=ttl"
   ```

1. Describe TTL | table

   ```
   aws dynamodb describe-time-to-live --table-name TTLExample
   {
       "TimeToLiveDescription": {
           "AttributeName": "ttl",
           "TimeToLiveStatus": "ENABLED"
       }
   }
   ```

1. Add an item | table / TTL attribute is set -- via -- BASH shell & AWS CLI 

   ```
   EXP=`date -d '+5 days' +%s`    # +5 days, -- to create an -- expiration time
   aws dynamodb put-item --table-name "TTLExample" --item '{"id": {"N": "1"}, "ttl": {"N": "'$EXP'"}}'
   ```


**Note**  
* get the current time | epoch time format
  * Linux Terminal: `date +%s`
  * Python: `import time; int(time.time())`
  * Java: `System.currentTimeMillis() / 1000L`
  * JavaScript: `Math.floor(Date.now() / 1000)`