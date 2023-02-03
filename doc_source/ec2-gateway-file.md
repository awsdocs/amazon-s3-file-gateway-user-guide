--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Deploying an Amazon EC2 instance to host your Amazon S3 File Gateway<a name="ec2-gateway-file"></a>

You can deploy and activate an Amazon S3 File Gateway on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. The AWS Storage Gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**Note**  
Storage Gateway community AMIs are published and fully supported by AWS\. You can see that the publisher is AWS, a verified provider\.

**To deploy an Amazon EC2 instance to host your Amazon S3 File Gateway**

1. Start setting up a new gateway using the Storage Gateway console\. For instructions, see [Set up an Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/create-gateway-file.html#set-up-gateway-s3-file)\. When you reach the **Platform options** section, choose **Amazon EC2** as the **Host platform**, then use the following steps to launch the Amazon EC2 instance that will host your File Gateway\.

1. Choose **Launch instance** to open the AWS Storage Gateway AMI template in the Amazon EC2 console, where you can configure additional settings\.

   Use **Quicklaunch** to launch the Amazon EC2 instance with default settings\. For more information on Amazon EC2 Quicklaunch default specifications, see [Quicklaunch Configuration Specifications for Amazon EC2](https://docs.aws.amazon.com/filegateway/latest/files3/ec2-quicklaunch-settings.html)\. 

1. For **Name**, enter a name for the Amazon EC2 instance\. After the instance is deployed, you can search for this name to find your instance on list pages in the Amazon EC2 console\.

1. In the **Instance type** section, for **Instance type**, choose the hardware configuration for your instance\. The hardware configuration must meet certain minimum requirements to support your gateway\. We recommend starting with the **m5\.xlarge** instance type, which meets the minimum hardware requirements for your gateway to function properly\. For more information, see [Requirements for Amazon EC2 instance types](Requirements.md#requirements-hardware)\.

   You can resize your instance after you launch, if necessary\. For more information, see [Resizing your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
Certain instance types, particularly i3 EC2, use NVMe SSD disks\. These can cause problems when you start or stop File Gateway; for example, you can lose data from the cache\. Monitor the `CachePercentDirty` Amazon CloudWatch metric, and only start or stop your system when that parameter is `0`\. To learn more about monitoring metrics for your gateway, see [Storage Gateway metrics and dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/awssg-metricscollected.html) in the CloudWatch documentation\.

1. In the **Key pair \(login\)** section, for **Key pair name \- *required***, select the key pair you want to use to securely connect to your instance\. You can create a new key pair if necessary\. For more information, see [Create a key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html#create-a-key-pair) in the *Amazon Elastic Compute Cloud User Guide for Linux Instances*\.

1. In the **Network settings** section, review the preconfigured settings and choose **Edit** to make changes to the following fields:

   1. For **VPC \- *required***, choose the VPC where you want to launch your Amazon EC2 instance\. For more information, see [How Amazon VPC works](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html) in the *Amazon Virtual Private Cloud User Guide*\.

   1. \(Optional\) For **Subnet**, choose the subnet where you want to launch your Amazon EC2 instance\.

   1. For **Auto\-assign Public IP**, choose **Enable**\.

1. In the **Firewall \(security groups\)** subsection, review the preconfigured settings\. You can change the default name and description of the new security group to be created for your Amazon EC2 instance if you want, or choose to apply firewall rules from an existing security group instead\.

1. In the **Inbound security groups rules** subsection, add firewall rules to open the ports that clients will use to connect to your instance\. For more information on the ports required for Amazon S3 File Gateway, see [Port requirements](https://docs.aws.amazon.com/filegateway/latest/files3/Requirements.html#requirements-network)\. For more information on adding firewall rules, see [Security group rules](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules.html) in the *Amazon Elastic Compute Cloud User Guide for Linux Instances*\.
**Note**  
Amazon S3 File Gateway requires TCP port 80 to be open for inbound traffic and one\-time HTTP access during gateway activation\. After activation, you can close this port\.  
If you plan to create NFS file shares, you must open TCP/UDP port 2049 for NFS access, TCP/UDP port 111 for NFSv3 access, and TCP/UDP port 20048 for NFSv3 access\.  
If you plan to create SMB file shares, you must open TCP port 445 for SMB access\.

1. In the **Advanced network configuration** subsection, review the preconfigured settings and make changes if necessary\.

1. In the **Configure storage** section, choose **Add new volume** to add storage to your gateway instance\.
**Important**  
You must add at least one Amazon EBS volume with at least **150 GiB** capacity for cache storage in addition to the preconfigured **Root volume**\. For increased performance, we recommend allocating multiple EBS volumes for cache storage with at least 150 GiB each\.

1. In the **Advanced details** section, review the preconfigured settings and make changes if necessary\.

1. Choose **Launch instance** to launch your new Amazon EC2 gateway instance with the configured settings\.

1. To verify that your new instance launched successfully, navigate to the **Instances** page in the Amazon EC2 console and search for your new instance by name\. Ensure that that **Instance state** displays **Running** *with a green check mark*, and that the **Status check** is complete, and *shows a green check mark*\.

1. Select your instance from the details page\. Copy the **Public IPv4 address** from the **Instance summary** section, then return to the **Set up gateway** page in the Storage Gateway console to resume setting up your Amazon S3 File Gateway\.

You can determine the AMI ID to use for launching a File Gateway by using the Storage Gateway console or by querying the AWS Systems Manager parameter store\.

**To determine the AMI ID**
+ Start setting up a new gateway using the Storage Gateway console\. For instructions, see [Set up an Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/create-gateway-file.html#set-up-gateway-s3-file)\. When you reach the **Platform options** section, choose **Amazon EC2** as the **Host platform**, then choose **Launch instance** to open the AWS Storage Gateway AMI template in the Amazon EC2 console\.

  You are redirected to the EC2 community AMI page, where you can see the AMI ID for your AWS Region in the URL\.

  You can also query the Systems Manager parameter store\. You can use the AWS CLI or Storage Gateway API to query the Systems Manager public parameter under the namespace `/aws/service/storagegateway/ami/FILE_S3/latest`\. For example, using the following CLI command returns the ID of the current AMI in the AWS Region you specify\.

  ```
  aws --region us-east-2 ssm get-parameter --name /aws/service/storagegateway/ami/FILE_S3/latest
  ```

  The CLI command returns output similar to the following\.

  ```
  {
      "Parameter": {
          "Type": "String",
          "LastModifiedDate": 1561054105.083,
          "Version": 4,
          "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/storagegateway/ami/FILE_S3/latest",
          "Name": "/aws/service/storagegateway/ami/FILE_S3/latest",
          "Value": "ami-123c45dd67d891000"
      }
  }
  ```