--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Editing settings for your SMB file share<a name="edit-smbfileshare-settings"></a>

After you have created an SMB file share, you can edit the storage class for your Amazon S3 bucket, object metadata, case sensitivity, access based enumeration, audit logs, automated cache refresh, and the export as settings for your file share\.

**Note**  
You cannot edit an existing file share to point to a new bucket or access point, or to modify the VPC endpoint settings\. You can configure those settings only when creating a new file share\.

**To edit SMB file share settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share that you want to update\.

1. For **Actions**, choose **Edit share settings**\.

1. Do one or more of the following:
   + \(Optional\) For **File share name**, enter a new name for the file share\.
   + For **Audit logs**, choose one of the following:
     + Choose **Disable logging** to turn off logging\.
     + Choose **Create a new log group** to create a new audit log\.
     + Choose **Use an existing log group**, and then choose an existing audit log from the list\.

     For more information about audit logs, see [Understanding S3 File Gateway audit logs](monitoring-file-gateway.md#audit-logs)\.
   + \(Optional\) For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the File Gateway to first refresh that directory's contents from the Amazon S3 bucket\.
   + \(Optional\) For **File upload notification**, choose the check box to be notified when a file has been fully uploaded to S3 by the S3 File Gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.
   + For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
     + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service User Guide*\.
     + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service User Guide*\.
     + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service User Guide*\.
     + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service User Guide*\.
   + For **Object metadata**, choose the metadata that you want to use:
     + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
     + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file's Network File System \(NFS\) or Server Message Block \(SMB\) file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](add-file-share.md#cross-account-access)\.
     + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.
   + For **Export as**, choose an option for your file share\. The default value is **Read\-write**\.
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you select **Read\-only** for **Export as**, you might see an error message about an unexpected error keeping you from creating the folder\. This error message is a known issue with NFS version 3\. You can ignore the message\.
   + For **File/directory access controlled by**, choose one of the following:
     + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.
     + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

     If your authentication method is **Active Directory**, for **Admin users/groups**, enter a comma\-separated list of AD users and groups\. Do this if you want the admin user to have privileges to update ACLs on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the `@` character, for example, `@group1`\.
   + For **Case sensitivity**, select the check box to allow the gateway to control the case sensitivity, or clear the check box to allow the client to control the case sensitivity\.
**Note**  
If you are selecting this check box, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.
If you are clearing this check box, this setting might cause you to loss access to files with names that differ only in their case\.
   + For **Access based enumeration**, select the check box to make the files and folders on the share visible only to users who have read access\. Keep the check box cleared to make the files and folders on the share visible to all users during directory enumeration\.
**Note**  
Access\-based enumeration is a system that filters the enumeration of files and folders on an SMB file share based on the share's access control lists \(ACLs\)\.
   + For **Opportunistic lock \(oplock\)**, choose one of the following:
     + Choose **Enabled** to allow the file share to use opportunistic locking to optimize the file buffering strategy, which improves performance in most cases, particularly with regard to Windows context menus\.
     + Choose **Disabled** to prevent the use of opportunistic locking\. If multiple Windows clients in your environment frequently edit the same files simultaneously, disabling opportunistic locking can sometimes improve performance\.
**Note**  
Enabling opportunistic locking on case\-sensitive shares is not recommended for workloads that involve access to files with the same name in different case\.

1. Choose **Save changes**\.