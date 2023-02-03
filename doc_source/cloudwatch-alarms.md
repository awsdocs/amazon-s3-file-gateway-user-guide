--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Understanding CloudWatch alarms<a name="cloudwatch-alarms"></a>

CloudWatch alarms monitor information about your gateway based on metrics and expressions\. You can add CloudWatch alarms for your gateway and view their statuses in the Storage Gateway console\. For more information about the metrics that are used to monitor S3 File Gateway, see [Understanding gateway metrics](https://docs.aws.amazon.com/filegateway/latest/files3/monitoring-file-gateway.html#understanding-file-gateway-metrics) and [Understanding file share metrics](https://docs.aws.amazon.com/filegateway/latest/files3/monitoring-file-gateway.html#monitoring-file-gateway-resources)\. For each alarm, you specify conditions that will trigger its ALARM state\. Alarm status indicators in the Storage Gateway console turn red when in the ALARM state, making it easier for you to monitor status proactively\. You can configure alarms to invoke actions automatically based on sustained changes in state\. For more information about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

**Note**  
If you don't have permission to view CloudWatch, you can't view the alarms\.

For each activated gateway, we recommend that you create the following CloudWatch alarms:
+ High IO wait: `IoWaitpercent` >= 20 for 3 datapoints in 15 minutes
+ Cache percent dirty: `CachePercentDirty` > 80 for 4 datapoints within 20 minutes
+ Files failing upload: `FilesFailingUpload` >= 1 for 1 datapoint within 5 minutes
+ File shares unavailable: `FileSharesUnavailable` >= 1 for 1 datapoint within 5 minutes

For gateways on VMware host platforms with HA mode enabled, we also recommend these additional CloudWatch alarms:
+ Availability notifications: `AvailabilityNotifications` >= 1 for 1 datapoints within 5 minutes
+ Health notifications: `HealthNotifications` >= 1 for 1 datapoints within 5 minutes
**Note**  
You can set a health notification alarm only if the gateway had a previous health notification in CloudWatch\.

The following table describes CloudWatch alarm states\.


| State | Description | 
| --- | --- | 
|  **OK**  |  The metric or expression is within the defined threshold\.  | 
|  **Alarm**  |  The metric or expression is outside of the defined threshold\.  | 
|  **Insufficient data**  |  The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\.  | 
|  **None**  |  No alarms are created for the gateway\. To create a new alarm, see [Creating a custom CloudWatch alarm for your gateway](cloudwatch-alarms-create-alarm.md)\.  | 
|  **Unavailable**  |  The state of the alarm is unknown\. Choose **Unavailable** to view error information in the **Monitoring** tab\.  | 