--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Refreshing objects in your Amazon S3 bucket<a name="refresh-cache"></a>

As your NFS or SMB client performs file system operations, your gateway maintains an inventory of the objects in the Amazon S3 object cache associated with your file share\. Your gateway uses this cached inventory to reduce the latency and frequency of Amazon S3 requests\. This operation does not import files into the S3 File Gateway cache storage\. It only updates the cached inventory to reflect changes in the inventory of the objects in the Amazon S3 object cache\.

To refresh the S3 bucket object cache for your file share, select the method that best fits your use case from the following list, then complete the corresponding procedure below\.

**Topics**
+ [Configure an automated cache refresh schedule using the Storage Gateway console](#auto-refresh-console-procedure)
+ [Configure an automated cache refresh schedule using AWS Lambda with an Amazon CloudWatch rule](#auto-refresh-lambda-procedure)
+ [Perform a manual cache refresh using the Storage Gateway console](#manual-refresh-console-procedure)
+ [Perform a manual cache refresh using the Storage Gateway API](#manual-refresh-api-procedure)

## Configure an automated cache refresh schedule using the Storage Gateway console<a name="auto-refresh-console-procedure"></a>

**To configure an automated cache refresh schedule using the Storage Gateway console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**\.

1. Choose the file share for which you want to configure the refresh schedule\.

1. For **Actions**, choose **Edit file share settings**\.

1. For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh after which access to the directory would cause the File Gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. Choose **Save changes**\.

## Configure an automated cache refresh schedule using AWS Lambda with an Amazon CloudWatch rule<a name="auto-refresh-lambda-procedure"></a>

**To configure an automated cache refresh schedule using AWS Lambda with an Amazon CloudWatch rule**

1. Identify the S3 bucket used by the S3 File Gateway\.

1. Check that the *Event* section is blank\. It populates automatically later\.

1. Create an IAM role, and allow Trust Relationship for Lambda `lambda.amazonaws.com`\.

1. Use the following policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "StorageGatewayPermissions",
               "Effect": "Allow",
               "Action": "storagegateway:RefreshCache",
               "Resource": "*"
           },
           {
               "Sid": "CloudWatchLogsPermissions",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Create a Lambda function from the Lambda console\.

1. Use the following function for your Lambda task\.

   ```
   import json
   import boto3
   client = boto3.client('storagegateway')
   def lambda_handler(event, context):
       print(event)
       response = client.refresh_cache(
           FileShareARN='arn:aws:storagegateway:ap-southeast-2:672406474878:share/share-E51FBS9C'
       )
       print(response)
       return 'Your FileShare cache has been refreshed'
   ```

1. For **Execution role**, choose the IAM role you created\.

1. Optional: add a trigger for Amazon S3 and select the event **ObjectCreated** or **ObjectRemoved**\.
**Note**  
`RefreshCache` needs to complete one process before starting another\. When you create or delete many objects in a bucket, performance might degrade\. Therefore, we recommend against using S3 triggers\. Instead, use the Amazon CloudWatch rule described following\.

1. Create a CloudWatch rule on the CloudWatch console and add a schedule\. Generally, we recommend a *fixed rate* of 30 minutes\. However, you can use 1–2 hours on large S3 bucket\.

1. Add a new trigger for CloudWatch events and choose the rule you just created\.

1. Save your Lambda configuration\. Choose **Test**\.

1. Choose **S3 PUT** and customize the test to your requirements\.

1. The test should succeed\. If not, modify the JSON to your requirements and retest\.

1. Open the Amazon S3 console, and verify that the event you created and the Lambda function ARN are present\.

1. Upload an object to your S3 bucket using the Amazon S3 console or the AWS CLI\.

   The CloudWatch console generates a CloudWatch output similar to the following\.

   ```
   {
       u'Records': [
           {u'eventVersion': u'2.0', u'eventTime': u'2018-09-10T01:03:59.217Z', u'requestParameters': {u'sourceIPAddress': u'MY-IP-ADDRESS'}, 
           u's3': {u'configurationId': u'95a51e1c-999f-485a-b994-9f830f84769f', u'object': {u'sequencer': u'00549CC2BF34D47AED', u'key': u'new/filename.jpeg'}, 
           u'bucket': {u'arn': u'arn:aws:s3:::MY-BUCKET', u'name': u'MY-GATEWAY-NAME', u'ownerIdentity': {u'principalId': u'A3OKNBZ72HVPP9'}}, u's3SchemaVersion': u'1.0'}, 
           u'responseElements': {u'x-amz-id-2': u'76tiugjhvjfyriugiug87t890nefevbck0iA3rPU9I/s4NY9uXwtRL75tCyxasgsdgfsq+IhvAg5M=', u'x-amz-request-id': u'651C2D4101D31593'}, 
           u'awsRegion': u'MY-REGION', u'eventName': u'ObjectCreated:PUT', u'userIdentity': {u'principalId': u'AWS:AROAI5LQR5JHFHDFHDFHJ:MY-USERNAME'}, u'eventSource': u'aws:s3'}
       ]
   }
   ```

   The Lambda invocation gives you output similar to the following\.

   ```
   {
       u'FileShareARN': u'arn:aws:storagegateway:REGION:ACCOUNT-ID:share/MY-SHARE-ID', 
           'ResponseMetadata': {'RetryAttempts': 0, 'HTTPStatusCode': 200, 'RequestId': '6663236a-b495-11e8-946a-bf44f413b71f', 
               'HTTPHeaders': {'x-amzn-requestid': '6663236a-b495-11e8-946a-bf44f413b71f', 'date': 'Mon, 10 Sep 2018 01:03:59 GMT', 
                   'content-length': '90', 'content-type': 'application/x-amz-json-1.1'
           }
       }
   }
   ```

   Your NFS share mounted on your client will reflect this update\.
**Note**  
For caches updating large object creation or deletion in large buckets with millions of objects, updates may take hours\.

1. Delete your object manually using the Amazon S3 console or AWS CLI\.

1. View the NFS share mounted on your client\. Verify that your object is gone \(because your cache refreshed\)\.

1. Check your CloudWatch logs to see the log of your deletion with the event `ObjectRemoved:Delete`\.

   ```
   {
       u'account': u'MY-ACCOUNT-ID', u'region': u'MY-REGION', u'detail': {}, u'detail-type': u'Scheduled Event', u'source': u'aws.events', 
       u'version': u'0', u'time': u'2018-09-10T03:42:06Z', u'id':  u'6468ef77-4db8-0200-82f0-04e16a8c2bdb', 
       u'resources': [u'arn:aws:events:REGION:MY-ACCOUNT-ID:rule/FGw-RefreshCache-CW']
   }
   ```
**Note**  
For cron jobs or scheduled tasks, your CloudWatch log event is `u'detail-type': u'Scheduled Event'`\.

## Perform a manual cache refresh using the Storage Gateway console<a name="manual-refresh-console-procedure"></a>

**To perform a manual cache refresh using the Storage Gateway console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share for which you want to perform the refresh\.

1. For **Actions**, choose **Refresh cache**\.

   The time that the refresh process takes depends on the number of objects cached on the gateway and the number of objects that were added to or removed from the S3 bucket\.

## Perform a manual cache refresh using the Storage Gateway API<a name="manual-refresh-api-procedure"></a>

**To perform a manual cache refresh using the Storage Gateway API**
+ Send an HTTP POST request to invoke the `RefreshCache` operation with your desired parameters through the Storage Gateway API\. For more information, see [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) in the *AWS Storage Gateway API Reference*\.
**Note**  
Sending the `RefreshCache` request only initiates the cache refresh operation\. When the cache refresh completes, it doesn't necessarily mean that the file refresh is complete\. To determine that the file refresh operation is complete before you check for new files on the gateway file share, use the `refresh-complete` notification\. To do this, you can subscribe to be notified through an Amazon CloudWatch event\. For more information, see [Getting notified about file operations](monitoring-file-gateway.md#get-notification)\.