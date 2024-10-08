# Setting up DynamoDB local \(downloadable version\)<a name="DynamoDBLocal"></a>

* included in downloadable version of Amazon DynamoDB
* allows
  * 👁️WITHOUT accessing the DynamoDB web service 👁️
    * develop
    * test applications
    * save money on
      * data storage
      * data transfer
    * NO need an internet connection 
* 👁️== database / self-contained | your computer 👁️
* once you're ready to deploy your application | production -> 
  * remove the local endpoint | code
  * -- points to the -- DynamoDB web service
* ways to get it 
  * [download](DynamoDBLocal.DownloadingAndRunning.md#DynamoDBLocal.DownloadingAndRunning.title)
    * requires JRE
  * [Apache Maven dependency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html#apache-maven)
  * [Docker image](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html#docker) 
* if you prefer to use the Amazon DynamoDB web service -> [Setting up DynamoDB \(web service\)](SettingUp.DynamoWebService.md)

**Topics**
+ [Deploying DynamoDB locally on your computer](DynamoDBLocal.DownloadingAndRunning.md)
+ [DynamoDB local usage notes](DynamoDBLocal.UsageNotes.md)