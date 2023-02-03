--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Creating recommended CloudWatch alarms for your gateway<a name="cloudwatch-alarms-create-recommended"></a>

When you create a new gateway using the Storage Gateway console, you can choose to create all recommended CloudWatch alarms automatically as part of the initial setup process\. For more information, see [Configure your Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/create-gateway-file.html#configure-gateway-s3-file)\. If you want to add or update recommended CloudWatch alarms for an existing gateway, use the following procedure\.

**To add or update recommended CloudWatch alarms for an existing gateway**
**Note**  
This feature requires CloudWatch policy permissions, which are *not* automatically granted as part of the preconfigured Storage Gateway full access policy\. Make sure your security policy grants the following permissions before you attempt to create recommended CloudWatch alarms:  
`cloudwatch:PutMetricAlarm` \- create alarms
`cloudwatch:DisableAlarmActions` \- turn alarm actions off
`cloudwatch:EnableAlarmActions` \- turn alarm actions on
`cloudwatch:DeleteAlarms` \- delete alarms

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. In the navigation pane, choose **Gateways**, then choose the gateway for which you want to create recommended CloudWatch alarms\.

1. On the gateway details page, choose the **Monitoring** tab\.

1. Under **Alarms**, choose **Create recommended alarms**\. The recommended alarms are created automatically\.

   The **Alarms** section lists all CloudWatch alarms for a specific gateway\. From here, you can select and delete one or more alarms, turn alarm actions on or off, and create new alarms\.