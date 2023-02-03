--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Editing Basic Gateway Information<a name="edit-gateway-information"></a>

You can use the Storage Gateway console to edit basic information for an existing gateway, including the gateway name, time zone, and CloudWatch log group\.

**To edit basic information for an existing gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, then choose the gateway for which you want to edit basic information\.

1. From the **Actions** dropdown menu, choose **Edit gateway information**\.

1. Modify the settings you want to change, then choose **Save changes**\.
**Note**  
Changing a gateway's name will disconnect any CloudWatch alarms set up to monitor the gateway\. To reconnect the alarms, update the **GatewayName** for each alarm in the CloudWatch console\.