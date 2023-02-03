--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Creating a gateway<a name="appliance-launch-gateway"></a>

You can create an S3 File Gateway, FSx File Gateway, Tape Gateway, or Volume Gateway on the hardware appliance\.

**To create a gateway on your hardware appliance**

1. Sign in to the AWS Management Console and open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Hardware**\.

1. Select the activated hardware appliance on which you want to create your gateway, then choose **Create Gateway**\.

1. Follow the procedures described in [Creating Your Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/create-file-gateway.html) to set up, connect, and configure your chosen gateway type\.

When you finish creating your gateway in the Storage Gateway console, the Storage Gateway software automatically starts installing on the hardware appliance\. It can take 5–10 minutes for a gateway to display as **online** in the console\.

To assign a static IP address to your installed gateway, you next configure the gateway's network interfaces so your applications can use it\.

**Next step**

[Configuring an IP address for the gateway](appliance-configure-ip.md)