--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Deploy an Amazon EC2 with Default Settings<a name="ec2-quicklaunch-settings"></a>

This topic lists the steps to deploy an Amazon EC2 host using the default specifications\.

You can deploy and activate an Amazon S3 File Gateway on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. The AWS Storage Gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**Note**  
Storage Gateway community AMIs are published and fully supported by AWS\. You can see that the publisher is AWS, a verified provider\.

1. To set up the Amazon EC2 instance, choose **Amazon EC2** as the **Host platform** in the **Platform options** section of the workflow\. For instructions on configuring the Amazon EC2 instance, see [Deploying an Amazon EC2 instance to host your Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/ec2-gateway-file.html)\.

1. Select **Launch instance** to open the AWS Storage Gateway AMI template in the Amazon EC2 console and customize additional settings such as **Instance types**, **Network settings** and **Configure storage**\.

1. Optionally, you can select **Use default settings** in the Storage Gateway console to deploy an Amazon EC2 instance with the default configuration\.

   The Amazon EC2 instance that **Use default settings** creates has the following default specifications:
   + **Instance type** — *m5\.xlarge*
   + **Network Settings**
     + For **VPC**, select the VPC that you want your EC2 instance to run in\.
     + For **Subnet**, specify the subnet that your EC2 instance should be launched in\.
**Note**  
Only subnets that have public\-IP enabled are listed\.
     + **Auto\-assign Public IP** — *Enabled*
     + An EC2 security group is created and associated with the EC2 Instance\. The security group has the following inbound port rules:
**Note**  
You will need Port 80 during gateway activation and the port is closed immediately thereafter\. Your EC2 instance can only be accessed over the other ports from the selected VPC\.   
You can edit security groups anytime by navigating to the EC2 instance details page, selecting **Security**, and navigating to **Security group details** by choosing the security group ID\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-quicklaunch-settings.html)
   + **Configure storage**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-quicklaunch-settings.html)