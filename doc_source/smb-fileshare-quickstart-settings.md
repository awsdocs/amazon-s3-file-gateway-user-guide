--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Create an SMB file share with default settings<a name="smb-fileshare-quickstart-settings"></a>

This section explains how to create an SMB file share using the default settings\.

**Topics**
+ [Create an SMB file share](#SMB-fileshare-quickstart)
+ [SMB file share default settings](#quickstart-default-settings)

## Create an SMB file share<a name="SMB-fileshare-quickstart"></a>

### SMB File share prerequisite<a name="SMB-fileshare-prerequisite"></a>

Before you create a Server Message Block \(SMB\) file share, make sure that you configure SMB security settings for your File Gateway\. You must also configure either Microsoft Active Directory \(AD\) or guest access for authentication\. A file share provides one type of SMB access only\. For instructions, see [Editing SMB settings for a gateway](https://docs.aws.amazon.com/filegateway/latest/files3/edit-smb-access-settings.html)\.

### SMB file share creation with default settings<a name="SMB-fileshare-default-steps"></a>

Use the following procedure to create an SMB file share using the default settings\.

**Note**  
An SMB file share doesn't work properly unless the required ports are open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/) and choose **File shares** from the left navigation pane\.

1. Choose **Create file share** to open the **File share settings** page\.

1. For **Gateway**, select your Amazon S3 File Gateway from the list\.

1. For **File share type**, choose **Server Message Block \(SMB\)**\.

1. For **S3 bucket**, you can select an existing Amazon S3 bucket in your account or create a new one using **Create new S3 bucket**\. Choose the **Region** where the Amazon S3 endpoint is located and set a unique bucket name in **S3 bucket name**\.

   For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon S3 User Guide*\.

   If you want to connect to a bucket in another account, choose **A bucket in another account** from the dropdown list\. Enter the name of the account bucket in **Cross\-account bucket name**\.

1. Choose **Configure** to authenticate the configuration\.

   1. For **Authentication method**, choose the authentication method that you want to use\.
     + To use your corporate Microsoft AD for user authenticated access to your SMB file share, choose **Active Directory**\.
**Note**  
Your File Gateway must be joined to a domain\. For more information, see [Using Active Directory to authenticate users](https://docs.aws.amazon.com/filegateway/latest/files3/enable-ad-settings.html)\.  
Joining a domain creates an AD account in the default container \(which isn't an organizational unit\), using the gateway's **Gateway ID** as the account name \(for example, SGW\-1234ADE\)\.  
If your AD environment requires that you pre\-stage accounts to facilitate the join domain process, you will need to create this account ahead of time\.  
If your AD environment has a designated organizational unit \(OU\) for new computer objects, you must specify that OU when joining the domain\.
     + To provide only guest access, choose **Guest access**\. If you choose this authentication method, your File Gateway doesn't have to be part of a Microsoft AD domain\. You can also use a File Gateway that's a member of an AD domain to create file shares with guest access\. You must set a guest password for your SMB server in the corresponding field\.

1. Choose **Create file share** to create an SMB file share using the default settings described in the next section\.

**Note**  
Using S3 Versioning, Cross\-Region Replication, or the Rsync utility when uploading data from a File Gateway can have significant cost implications\. For more information, see [Avoiding unanticipated costs when uploading data from File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/avoid-unanticipated-costs.html)\.

## SMB file share default settings<a name="quickstart-default-settings"></a>

The SMB file share that you created in the previous section uses the default settings listed in the table below\.

**Note**  
By default, the work flow described in the previous section provides full control to the owner of the Amazon S3 bucket that maps to the SMB file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](add-file-share.md#cross-account-access)\.


| Setting | Default value | Notes | 
| --- | --- | --- | 
| **Amazon S3 location** | The file share is connected directly to the Amazon S3 bucket and has the same name as the bucket\. Your gateway uses this bucket to store and retrieve files\. | The name doesn't include a prefix\. | 
| **AWS PrivateLink for S3** | The file share isn't connected to Amazon S3 through an interface endpoint in your virtual private cloud \(VPC\)\. |  | 
|   **File upload notification**   |  Off  |   | 
|  **Storage class for new objects**   |  S3 Standard   |  This lets you store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service User Guide*\.   | 
|   **Object metadata**  | Guess MIME type | This enables guessing the MIME type for uploaded objects based on file extensions\. This option requires that Access Control Lists \(ACLs\) are enabled  on the S3 bucket that's associated with your file share\. If ACLs are  disabled, the file share won't be able to access the Amazon S3 bucket, and will remain in the **Unavailable** state  indefinitely\.  | 
|  **Access based enumeration**  |  Not activated  | The files and folders on the file share are visible to all users  during directory enumeration\. Access\-based enumeration is a system that filters the enumeration of  files and folders on an SMB file share based on the share's access  control lists \(ACLs\)\.  | 
| **Enable requester pays** | Off | For more information, see [Requester Pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\. | 
|  **Opportunistic lock \(oplock\)**  |  On  |  This allows the file share to use opportunistic locking to optimize  the file buffering strategy\.  In most cases, activating opportunistic locking improves  performance, particularly with regard to Windows context  menus\.  | 
|  **Audit logs** |  Off  | Logging in to an Amazon CloudWatch group is disabled by default\. | 
|  **Force case sensitivity**  |  On  |  This allows the client to control the case sensitivity\.  | 
|   **Access to your S3 bucket**   |  Create a new IAM role   |   The default option enables the File Gateway to create a new IAM role and access  policy on your behalf\.   | 

After your SMB file share is created, you can see your file share settings on the **Details** tab of the new file share\.

[Mount your SMB file share on your client](using-smb-fileshare.md)