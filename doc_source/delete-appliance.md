--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Deleting your hardware appliance<a name="delete-appliance"></a>

After you activate your hardware appliance in your AWS account, you might have a need to move and activate it in a different AWS account\. In this case, you first delete the appliance from the AWS account and activate it in another AWS account\. You might also want to delete the appliance completely from your AWS account because you no longer need it\. Follow these instructions to delete your hardware appliance\.

**To delete your hardware appliance**

1. If you have installed a gateway on the hardware appliance, you must first remove the gateway before you can delete the appliance\. For instructions on how to remove a gateway from your hardware appliance, see [Removing gateway software from a hardware appliance](appliance-remove-gateway.md)\.

1. On the Hardware page of the Storage Gateway console, choose the hardware appliance you want to delete\.

1. For **Actions**, choose **Delete Appliance**\. The confirmation dialog box appears\.

1. Verify that you want to delete the specified hardware appliance, then type the word *delete* in the confirmation box and choose **Delete**\. 

   When you delete the hardware appliance, all resources associated with the gateway that is installed on the appliance are deleted, but the data on the hardware appliance itself is not deleted\.