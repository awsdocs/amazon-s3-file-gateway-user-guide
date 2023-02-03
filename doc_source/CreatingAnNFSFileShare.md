--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Create an NFS file share with custom configuration<a name="CreatingAnNFSFileShare"></a>

Use the following procedure to create a Network File System \(NFS\) file share with custom configuration\. To launch an NFS file share with default settings, see [Create an NFS file share with default settings](https://docs.aws.amazon.com/filegateway/latest/files3/nfs-fileshare-quickstart-settings.html)\.

**Note**  
Using S3 Versioning, Cross\-Region Replication, or the Rsync utility when uploading data from a File Gateway can have significant cost implications\. For more information, see [Avoiding unanticipated costs when uploading data from File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/avoid-unanticipated-costs.html)\.

**To create an NFS file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share** to open the **File share settings** page\.

1. For **Gateway**, choose your Amazon S3 File Gateway from the list\.

1. For **Amazon S3 location**, do one of the following:
   + To connect the file share directly to an S3 bucket, choose **S3 bucket name**, then enter the S3 bucket name and, optionally, a prefix name for objects created by the file share\. Your gateway uses this bucket to store and retrieve files\. For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon S3 User Guide*\. 
   + To connect the file share to an S3 bucket through an access point, choose **S3 access point**, then enter the S3 access point name and, optionally, a prefix name for objects created by the file share\. Your bucket policy must be configured to delegate access control to the access point\. For information about access points, see [Managing data access with Amazon S3 access points](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points.html) and [Delegating access control to access points](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points-policies.html#access-points-delegating-control) in the *Amazon S3 User Guide*\.
   + To connect the file share to an S3 bucket through an access point alias, choose **S3 access point alias**, then enter the S3 access point alias name and, optionally, a prefix name for objects created by the file share\. If you choose this option, the File Gateway cannot create a new AWS Identity and Access Management \(IAM\) role and access policy on your behalf\. You must select an existing IAM role and configure an access policy in the **Access to your S3 bucket** section that follows\. For more information about access point aliases, see [Using a bucket\-style alias for your access point](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points-alias.html) in the *Amazon S3 User Guide*\.
**Note**  
Each file share can only connect to one S3 bucket, but multiple file shares can connect to the same bucket\. If you connect more than one file share to the same bucket, you must configure each file share to use a unique, non\-overlapping prefix name to prevent read/write conflicts\.
If you enter a prefix name, or choose to connect through an access point or access point alias, you must enter a file share name\.
The prefix name must end with a forward slash \(`/`\)\.
After the file share is created, the prefix name can't be modified or deleted\.
For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon S3 User Guide*\.

1. For **AWS Region**, choose the AWS Region of the S3 bucket\.

1. For **File share name**, enter a name for the file share\. The default name is the S3 bucket name or access point name\.
**Note**  
If you entered a prefix name, or chose to connect through an access point or access point alias, you must enter a file share name\.
After the file share is created, the file share name can't be deleted\.

1. \(Optional\) For **AWS PrivateLink for S3**, do the following:

   1. To configure the file share to connect to S3 through an interface endpoint in your virtual private cloud \(VPC\) powered by AWS PrivateLink, choose **Use VPC endpoint**\.

   1. To identify the VPC interface endpoint that you want the file share to connect through, choose either **VPC endpoint ID** or **VPC endpoint DNS name**, and then provide the required information in the corresponding field\.
**Note**  
This step is required if the file share connects to S3 through a VPC access point or through an alias associated with a VPC access point\.
File share connections using AWS PrivateLink are not supported on FIPS gateways\.
For information about AWS PrivateLink, see [AWS PrivateLink for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) in the *Amazon S3 User Guide*\.

1. For **Access objects using**, choose **Network File System \(NFS\)**\.

1. For **Audit logs**, choose one of the following:
   + To turn off logging, choose **Disable logging**\.
   + To create a new audit log, choose **Create a new log group**\.
   + To use an existing audit log, choose **Use an existing log group**, and then choose an audit log from the list\.

   For more information about audit logs, see [Understanding S3 File Gateway audit logs](monitoring-file-gateway.md#audit-logs)\.

1. For **Automated cache refresh from S3**, choose **Set refresh interval**, and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the File Gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. For **File upload notification**, choose **Settling time \(seconds\)** to be notified when a file has been fully uploaded to S3 by the File Gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + To store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated, choose **S3 Standard**\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service User Guide*\.
   + To optimize storage costs by automatically moving data to the most cost\-effective storage access tier, choose **S3 Intelligent\-Tiering**\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service User Guide*\.
   + To store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated, choose **S3 Standard\-IA**\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service User Guide*\.
   + To store your infrequently accessed object data in a single Availability Zone, choose **S3 One Zone\-IA**\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service User Guide*\.

   To help monitor your S3 billing, use AWS Trusted Advisor\. For more information, see [Monitoring tools](https://docs.aws.amazon.com/AmazonS3/latest/userguide/monitoring-automated-manual.html) in the *Amazon Simple Storage Service User Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + To enable guessing of the MIME type for uploaded objects based on file extensions, choose **Guess MIME type**\.
   + To give full control to the owner of the S3 bucket that maps to the NFS file share, choose **Give bucket owner full control**\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](add-file-share.md#cross-account-access)\.
**Note**  
This option requires that Access Control Lists \(ACLs\) are enabled on the S3 bucket associated with your file share\. If ACLs are disabled, the file share will not be able to access the S3 bucket, and will remain in the **Unavailable** state indefinitely\.
   + If you are using this file share on a bucket that requires the requester or reader instead of the bucket owner to pay for access charges, choose **Enable requester pays**\. For more information, see [Requester Pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your File Gateway to use to access your Amazon S3 bucket:
   + To enable the File Gateway to create a new IAM role and access policy on your behalf, choose **Create a new IAM role**\. This option is not available if the file share connects to Amazon S3 using an access point alias\.
   + To select an existing IAM role and to set up the access policy manually, choose **Use an existing IAM role**\. You must use this option if your file share connects to Amazon S3 using an access point alias\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](add-file-share.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your File Gateway stores in Amazon S3:
   + To use server\-side encryption managed with Amazon S3 \(SSE\-S3\), choose **S3\-Managed Keys \(SSE\-S3\)**\.
   + To use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\), choose **KMS\-Managed Keys \(SSE\-KMS\)**\. In the **Primary key** box, choose an existing AWS KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, see [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias that is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric KMS keys are not supported\.

1. Choose **Next** to configure file access settings\.

**To configure file access settings**

1. For **Allowed clients**, specify whether to allow or restrict each client's access to your file share\. Provide the IP address or CIDR notation for the clients that you want to allow\. For information about supported NFS clients, see [Supported NFS clients for a File Gateway](Requirements.md#requirements-nfs-clients)\.

1. For **Mount options**, specify the options that you want for **Squash level** and **Export as**\.

   For **Squash level**, choose one of the following:
   + **All squash**: All user access is mapped to User ID \(UID\) \(65534\) and Group ID \(GID\) \(65534\)\.
   + **No root squash**: The remote superuser \(root\) receives access as root\.
   + **Root squash \(default\)**: Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.

   For **Export as**, choose one of the following:
   + **Read\-write**
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

1. For **File metadata defaults**, you can edit the **Directory permissions**, **File permissions**, **User ID**, and **Group ID**\. For more information, see [Editing metadata defaults for your NFS file share](edit-metadata-defaults.md)\.

1. Choose **Next**\.

1. Review your file share configuration settings, and then choose **Finish**\.

   After your NFS file share is created, you can see your file share settings in the file share's **Details** tab\.

**Next Step**

[Mount your NFS file share on your client](GettingStartedAccessFileShare.md)