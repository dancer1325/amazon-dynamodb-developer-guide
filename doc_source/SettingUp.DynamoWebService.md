# Setting up DynamoDB \(web service\)<a name="SettingUp.DynamoWebService"></a>


1. [Sign up for AWS](#SettingUp.DynamoWebService.SignUpForAWS)
2. if you want to interact with DynamoDB != AWS Management Console (_Example:_ via AWS API or AWS CLI) -> 
   1. [Get an AWS access key](#SettingUp.DynamoWebService.GetCredentials)
   2. [Configure your credentials](#SettingUp.DynamoWebService.ConfigureCredentials) \(used to access DynamoDB programmatically\)\. 

## Signing up for AWS<a name="SettingUp.DynamoWebService.SignUpForAWS"></a>

1. create an AWS account
   1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup), to sign up
   2. by default *AWS account root user* is created
      1. has access to ALL AWS services & resources | account
2. [follow best practices](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) & [use root user ONLY | specific tasks](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)

## Granting programmatic access<a name="SettingUp.DynamoWebService.GetCredentials"></a>

* TODO:
Before you can access DynamoDB programmatically or through the AWS Command Line Interface \(AWS CLI\), you must have programmatic access\. You don't need programmatic access if you plan to use the DynamoDB console only\. 

Users need programmatic access if they want to interact with AWS outside of the AWS Management Console\. The way to grant programmatic access depends on the type of user that's accessing AWS:
+ If you manage identities in IAM Identity Center, the AWS APIs require a profile, and the AWS Command Line Interface requires a profile or an environment variable\.
+ If you have IAM users, the AWS APIs and the AWS Command Line Interface require access keys\. Whenever possible, create temporary credentials that consist of an access key ID, a secret access key, and a security token that indicates when the credentials expire\.

To grant users programmatic access, choose one of the following options\.


****  

| Which user needs programmatic access? | To | By | 
| --- | --- | --- | 
|  Workforce identity \(Users managed in IAM Identity Center\)  | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. |  Following the instructions for the interface that you want to use: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SettingUp.DynamoWebService.html)  | 
| IAM | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. | Following the instructions in [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the IAM User Guide\. | 
| IAM | Use long\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\.\(Not recommended\) | Following the instructions in [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## Configuring your credentials<a name="SettingUp.DynamoWebService.ConfigureCredentials"></a>

Before you can access DynamoDB programmatically or through the AWS CLI, you must configure your credentials to enable authorization for your applications\.

 There are several ways to do this\. For example, you can manually create the credentials file to store your access key ID and secret access key\. You also can use the `aws configure` command of the AWS CLI to automatically create the file\. Alternatively, you can use environment variables\. For more information about configuring your credentials, see the programming\-specific AWS SDK developer guide\.

 To install and configure the AWS CLI, see [Using the AWS CLI](Tools.CLI.md)\. 