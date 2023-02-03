--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Removing gateway software from a hardware appliance<a name="appliance-remove-gateway"></a>

To remove gateway software from your hardware appliance, use the following procedure\.

**To remove a gateway from a hardware appliance**

1. On the **Hardware** page of the Storage Gateway console, choose the hardware appliance you want to delete\.

1. For **Actions**, choose **Remove Gateway**\. The confirmation dialog box appears\.

1. Verify that you want to remove the gateway software from specified hardware appliance, then type the word *remove* in the confirmation box and choose **Remove**\. 
**Note**  
After you remove the gateway software, you can't undo the action\. For certain gateway types, you can lose data on deletion, particularly cached data\. For more information on deleting a gateway, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

Removing the gateway doesn't delete the hardware appliance from the console\. The hardware appliance remains for future gateway deployments\.