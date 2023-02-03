--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Create an NFS file share with default settings<a name="nfs-fileshare-quickstart-settings"></a>

This section explains how to create an NFS file share using the default settings\.

**Topics**
+ [Create an NFS file share](#nfs-fileshare-quickstart)
+ [NFS file share default settings](#quickstart-default-settings)

## Create an NFS file share<a name="nfs-fileshare-quickstart"></a>

Use the following procedure to quickly create a Network File System \(NFS\) file share with default settings\.

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/) and choose **File shares** from the left navigation pane\.

1. Choose **Create file share** to open the **File share settings** page\.

1. For **Gateway**, choose your Amazon S3 File Gateway from the list\.

1. For **File share type**, choose **Network File System \(NFS\)**\.

1. For **S3 bucket**, you can pick an existing Amazon S3 bucket in your account or create a new one using **Create new S3 bucket**\. Choose the **Region** where the Amazon S3 endpoint is located and set a unique bucket name in **S3 bucket name**\.

   For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon S3 User Guide*\.

   If you want to connect to a bucket in another account, choose **A bucket in another account** from the drop downlist\. Enter the name of the crossaccount bucket in **Cross\-account bucket name**\.

1. Choose **Create file share** to create an NFS file share with default settings described in the next section\.

**Note**  
Using S3 Versioning, Cross\-Region Replication, or the Rsync utility when uploading data from a File Gateway can have significant cost implications\. For more information, see [Avoiding unanticipated costs when uploading data from File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/avoid-unanticipated-costs.html)\.

## NFS file share default settings<a name="quickstart-default-settings"></a>

The NFS file share that you created in the previous section uses the default settings listed in the table below\.

**Note**  
By default, the workflow described in the previous section provides full control to the owner of the S3 bucket that maps to the NFS file share\. For more information about using your file share to access objects in a bucket that's owned by another account, see [Using a file share for cross\-account access](add-file-share.md#cross-account-access)\.


| Setting | Default value | Notes | 
| --- | --- | --- | 
| **Amazon S3 location** | The file share is connected directly to the Amazon S3 bucket and has the same name as the bucket\. Your gateway uses this bucket to store and retrieve files\. | The name does not include a prefix\. | 
| **AWS PrivateLink for S3** | The file share is not connected to Amazon S3 through an interface endpoint in your virtual private cloud \(VPC\)\. |  | 
|   **File upload notification**   |  Off  |   | 
|  **Storage class for new objects**   |  S3 Standard   |  This lets you store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service User Guide*\.   | 
|   **Object metadata**  | Guess MIME type | This enables guessing the MIME type for uploaded objects based on file extensions\. This option requires that Access Control Lists \(ACLs\) are enabled  on the S3 bucket associated with your file share\. If ACLs are  disabled, the file share will not be able to access the Amazon S3 bucket, and will remain in the **Unavailable** state  indefinitely\.  | 
| **Enable requester pays** | Off | For more information, see [Requester Pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\. | 
|  **Audit logs** |  Off  | Logging in to an Amazon CloudWatch group is disabled by default\. | 
|   **Access to your S3 bucket**   |  Create a new IAM role   |  The default option enables the File Gateway to create a new IAM role and access  policy on your behalf\. All NFS clients are allowed access\. For information about supported  NFS clients, see [Supported NFS clients for a File Gateway](Requirements.md#requirements-nfs-clients)\.    | 
|  **Mount options**  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/nfs-fileshare-quickstart-settings.html)  | The default value of **Squash level** means that  access for the remote  superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\. | 
|  **File metadata defaults**  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/nfs-fileshare-quickstart-settings.html)  | 

After your NFS file share is created, you can see your file share settings on the **Details** tab of the new file share\.

[Mount your NFS file share on your client](GettingStartedAccessFileShare.md)