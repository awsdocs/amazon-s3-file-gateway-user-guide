--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Monitoring Storage Gateway<a name="Main_monitoring-gateways-common"></a>

This section describes how to monitor a gateway using Amazon CloudWatch, including monitoring cache storage and other resources associated with the gateway\. You use the Storage Gateway console to view metrics and alarms for your gateway\. For example, you can view the number of bytes used in read and write operations, the time spent in read and write operations, and the time taken to retrieve data from the AWS Cloud\. With metrics, you can track the health of your gateway and set up alarms to notify you when one or more metrics fall outside a defined threshold\.

Storage Gateway provides CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective on how your gateways are performing\. Storage Gateway also provides CloudWatch alarms, except high\-resolution alarms, at no additional charge\. For more information about CloudWatch pricing, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing/)\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\.

**Topics**
+ [Understanding CloudWatch alarms](cloudwatch-alarms.md)
+ [Creating recommended CloudWatch alarms for your gateway](cloudwatch-alarms-create-recommended.md)
+ [Creating a custom CloudWatch alarm for your gateway](cloudwatch-alarms-create-alarm.md)
+ [Monitoring your S3 File Gateway](monitoring-file-gateway.md)