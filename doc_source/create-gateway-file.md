--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Create and activate an Amazon S3 File Gateway<a name="create-gateway-file"></a>

In this section, you can find instructions on how to create, deploy, and activate a File Gateway in AWS Storage Gateway\.

**Topics**
+ [Set up an Amazon S3 File Gateway](#set-up-gateway-s3-file)
+ [Connect your Amazon S3 File Gateway to AWS](#connect-to-amazon-s3-file)
+ [Review settings and activate your Amazon S3 File Gateway](#review-and-activate-s3-file)
+ [Configure your Amazon S3 File Gateway](#configure-gateway-s3-file)

## Set up an Amazon S3 File Gateway<a name="set-up-gateway-s3-file"></a>

**To set up a new S3 File Gateway**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/), and choose the AWS Region where you want to create your gateway\.

1. Choose **Create gateway** to open the **Set up gateway** page\.

1. In the **Gateway settings** section, do the following:

   1. For **Gateway name**, enter a name for your gateway\. After your gateway is created, you can search for this name to find your gateway on the list pages in the AWS Storage Gateway console\.

   1. For **Gateway time zone**, choose the local time zone for the part of the world where you want to deploy your gateway\.

1. In the **Gateway options** section, for **Gateway type**, choose **Amazon S3 File Gateway**\.

1. In the **Platform options** section, do the following:

   1. For **Host platform**, choose the platform on which you want to deploy your gateway\. Then follow the platform\-specific instructions displayed on the Storage Gateway console page to set up your host platform\. You can choose from the following options:
      + **VMware ESXi** – Download, deploy, and configure the gateway virtual machine using VMware ESXi\.
      + **Microsoft Hyper\-V** – Download, deploy, and configure the gateway virtual machine using Microsoft Hyper\-V\.
      + **Linux KVM** – Download, deploy, and configure the gateway virtual machine using Linux Kernel\-based Virtual Machine \(KVM\)\.
      + **Amazon EC2** – Configure and launch an Amazon EC2 instance to host your gateway\.
      + **Hardware appliance** – Order a dedicated physical hardware appliance from AWS to host your gateway\.

   1. For **Confirm set up gateway**, select the check box to confirm that you performed the deployment steps for the host platform that you chose\. This step is not applicable for the **Hardware appliance** host platform\.

1. Now that your gateway is set up, you must choose how you want it to connect and communicate with AWS\. Choose **Next** to proceed\.

## Connect your Amazon S3 File Gateway to AWS<a name="connect-to-amazon-s3-file"></a>

**To connect a new S3 File Gateway to AWS**

1. If you have not done so already, complete the procedure described in [Set up an Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/set-up-gateway-s3-file.html)\. When finished, choose **Next** to open the **Connect to AWS** page in the AWS Storage Gateway console\.

1. In the **Endpoint options** section, for **Service endpoint**, choose the type of endpoint that your gateway will use to communicate with AWS\. You can choose from the following options:
   + **Publicly accessible** – Your gateway communicates with AWS over the public internet\. If you select this option, use the **FIPS enabled endpoint** check box to specify whether the connection must comply with Federal Information Processing Standards \(FIPS\)\.
**Note**  
If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS\-compliant endpoint\. For more information, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.  
The FIPS service endpoint is available only in some AWS Regions\. For more information, see [AWS Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.
   + **VPC hosted** – Your gateway communicates with AWS through a private connection with your virtual private cloud \(VPC\), allowing you to control your network settings\. If you select this option, you must specify an existing VPC endpoint by choosing its VPC endpoint ID from the dropdown list\. You can also provide its VPC endpoint Domain Name System \(DNS\) name or IP address\.

1. In the **Gateway connection options** section, for **Connection options**, choose how to identify your gateway to AWS\. You can choose from the following options:
   + **IP address** – Provide the IP address of your gateway in the corresponding field\. This IP address must be public or accessible from within your current network, and you must be able to connect to it from your web browser\.

     You can obtain the gateway IP address by logging into the gateway's local console from your hypervisor client, or by copying it from your Amazon EC2 instance details page\.
   + **Activation key** – Provide the activation key for your gateway in the corresponding field\. You can generate an activation key using the gateway's local console\. If your gateway's IP address is unavailable, choose this option\.

1. Now that you have chosen how you want your gateway to connect to AWS, you must activate the gateway\. Choose **Next** to proceed\.

## Review settings and activate your Amazon S3 File Gateway<a name="review-and-activate-s3-file"></a>

**To activate a new S3 File Gateway**

1. If you have not done so already, complete the procedures described in the following topics:
   + [Set up an Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/set-up-gateway-s3-file.html)
   + [Connect your Amazon S3 File Gateway to AWS](https://docs.aws.amazon.com/filegateway/latest/files3/connect-to-amazon-s3-file.html)

   When finished, choose **Next** to open the **Review and activate** page in the AWS Storage Gateway console\.

1. Review the initial gateway details for each section on the page\.

1. If a section contains errors, choose **Edit** to return to the corresponding settings page and make changes\.
**Important**  
You cannot modify the gateway options or connection settings after your gateway is activated\.

1. Now that you have activated your gateway, you must perform the first\-time configuration to allocate local storage disks and configure logging\. Choose **Next** to proceed\.

## Configure your Amazon S3 File Gateway<a name="configure-gateway-s3-file"></a>

**To perform the first\-time configuration on a new S3 File Gateway**

1. If you have not done so already, complete the procedures described in the following topics:
   + [Set up an Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/set-up-gateway-s3-file.html)
   + [Connect your Amazon S3 File Gateway to AWS](https://docs.aws.amazon.com/filegateway/latest/files3/connect-to-amazon-s3-file.html)
   + [Review settings and activate your Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/review-and-activate-s3-file.html)

   When finished, choose **Next** to open the **Configure gateway** page in the AWS Storage Gateway console\.

1. In the **Configure storage** section, use the dropdown lists to allocate at least one local disk with at least 150 gibibytes \(GiB\) capacity to **Cache**\. The local disks listed in this section correspond to the physical storage that you provisioned on your host platform\.

1. In the **CloudWatch log group** section, choose how to set up Amazon CloudWatch Logs to monitor the health of your gateway\. You can choose from the following options:
   + **Create a new log group** – Set up a new log group to monitor your gateway\.
   + **Use an existing log group** – Choose an existing log group from the corresponding dropdown list\.
   + **Deactivate logging** – Do not use Amazon CloudWatch Logs to monitor your gateway\.

1. In the **CloudWatch alarms** section, choose how to set up Amazon CloudWatch alarms to notify you when your gateway's metrics deviate from defined limits\. You can choose from the following options:
   + **Create Storage Gateway's recommended alarms** – Create all recommended CloudWatch alarms automatically when the gateway is created\. For more information about recommended alarms, see [Understanding CloudWatch alarms](https://docs.aws.amazon.com/filegateway/latest/files3/Main_monitoring-gateways-common.html#cloudwatch-alarms)\.
**Note**  
This feature requires CloudWatch policy permissions, which are *not* automatically granted as part of the preconfigured Storage Gateway full access policy\. Make sure your security policy grants the following permissions before you attempt to create recommended CloudWatch alarms:  
`cloudwatch:PutMetricAlarm` \- create alarms
`cloudwatch:DisableAlarmActions` \- turn alarm actions off
`cloudwatch:EnableAlarmActions` \- turn alarm actions on
`cloudwatch:DeleteAlarms` \- delete alarms
   + **Create a custom alarm** – Configure a new CloudWatch alarm to notify you about your gateway's metrics\. Choose **Create alarm** to define metrics and specify alarm actions in the Amazon CloudWatch console\. For instructions, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.
   + **No alarm** – Don't receive CloudWatch notifications about your gateway's metrics\.

1. \(Optional\) In the **Tags** section, choose **Add new tag**, then enter a case\-sensitive key\-value pair to help you search and filter for your gateway on the list pages in the AWS Storage Gateway console\. Repeat this step to add as many tags as you need\.

1. \(Optional\) In the **Verify VMware High Availability configuration** section, if your gateway is deployed on a VMware host as part of a cluster that is enabled for VMware High Availability \(HA\), choose **Verify VMware HA** to test whether the HA configuration is working properly\.
**Note**  
This section appears only for gateways that are running on the VMware host platform\.  
This step is not required to complete the gateway configuration process\. You can test your gateway's HA configuration at any time\. Verification takes a few minutes, and reboots the Storage Gateway virtual machine \(VM\)\.

1. Choose **Configure** to finish creating your gateway\.

   To check the status of your new gateway, search for it on the **Gateway overview** page of the AWS Storage Gateway console\.

Now that you have created your gateway, you must create a file share for it to use\. For instructions, see [Create a file share](https://docs.aws.amazon.com/filegateway/latest/files3/GettingStartedCreateFileShare.html)\.