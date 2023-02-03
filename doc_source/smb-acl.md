--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Using Microsoft Windows ACLs to Control Access to an SMB File Share<a name="smb-acl"></a>

Amazon S3 file gateway supports two different methods for controlling access to files and directories that are stored through an SMB file share: POSIX permissions, or Windows ACLs\.

This section describes how to use Microsoft Windows access control lists \(ACLs\) on SMB file shares enabled with Microsoft Active Directory \(AD\)\. By using Windows ACLs, you can set fine\-grained permissions on files and folders in your SMB file share\.

Following are some important characteristics of Windows ACLs on SMB file shares:
+ Windows ACLs are selected by default for SMB file shares when your file gateway is joined to an Active Directory domain\.
+ When ACLs are enabled, the ACL information is persisted in Amazon S3 object metadata\.
+ The gateway preserves up to 10 ACLs per file or folder\.
+ When you use an SMB file share enabled with ACLs to access S3 objects created outside your gateway, the objects inherit ACLs' information from the parent folder\.
+ The default root ACL for an SMB file share gives full access to everyone, but you can change the permissions of the root ACL\. You can use root ACLs to control access to the file share\. You can set who can mount the file share \(map the drive\) and what permissions the user gets to the files and folders recursively in the file share\. However, we recommend that you set this permission on the top\-level folder in the S3 bucket so that your ACL is persisted\. 

You can enable Windows ACLs when you create a new SMB file share by using the [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) API operation\. Or you can enable Windows ACLs on an existing SMB file share by using the [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html) API operation\.

## Enabling Windows ACLs on a New SMB File Share<a name="enable-acl-new-fileshare"></a>

Take the following steps to enable Windows ACLs on a new SMB file share\.

**To enable Windows ACLs when creating a new SMB file share**

1. Create a file gateway if you don't already have one\. For more information, see [Creating Your Gateway](create-file-gateway.md)\. 

1. If the gateway is not joined to a domain, add it to a domain\. For more information, see [Using Active Directory to authenticate users](filegateway/latest/files3/enable-ad-settings.html)\. 

1. Create an SMB file share\. For more information, see 

1. Enable Windows ACL on the file share from the console\.

   To use the Console, do the following:

   1. Choose the file share and choose **Edit file share**\.

   1. For the **File/directory access controlled by** option, choose **Windows Access Control List**\.

1. \(Optional\) Add an admin user to the [AdminUsersList](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html#StorageGateway-CreateSMBFileShare-request-AdminUserList), if you want the admin user to have privileges to update ACLs on all files and folders in the file share\.
**Note**  
If you have configured the **Allowed and Denied Users and Groups** lists in the SMB file share's settings, then ACLs will not grant any access that overrides those lists\.  
The **Allowed and Denied Users and Groups** lists are evaluated before ACLs, and control which users can mount or access the file share\. If any users or groups are placed on the **Allowed** list, the list is considered enabled, and only those users can mount the file share\.  
After a user has mounted a file share, ACLs then provide more granular protection that controls which specific files or folders the user can access\.

1. Update the ACLs for the parent folders under the root folder\. To do this, use Windows File Explorer to configure the ACLs on the folders in the SMB file share\.
**Note**  
If you configure the ACLs on the root instead of the parent folder under root, the ACL permissions aren't persisted in Amazon S3\.

   We recommend setting ACLs at the top\-level folder under the root of your file share, instead of setting ACLs directly at the root of the file share\. This approach persists the information as object metadata in Amazon S3\.

1. Enable inheritance as appropriate\.
**Note**  
You can enable inheritance for file shares created after May 8, 2019\. 

If you enable inheritance and update the permissions recursively, updates all the objects in the S3 bucket\. Depending on the number of objects in the bucket, the update can take a while to complete\. 

## Enabling Windows ACLs on an Existing SMB File Share<a name="enable-acl-existing-fileshare"></a>

Take the following steps to enable Windows ACLs on an existing SMB file share that has POSIX permissions\.

**To enable Windows ACLs on an existing SMB file share using the Console**

1. Choose the file share and choose **Edit file share**\.

1. For the **File/directory access controlled by** option, choose **Windows Access Control List**\.

1. Enable inheritance as appropriate\.
**Note**  
We don't recommend setting the ACLs at the root level, because if you do this and delete your gateway, you need to reset the ACLs again\.

If you enable inheritance and update the permissions recursively, updates all the objects in the S3 bucket\. Depending on the number of objects in the bucket, the update can take a while to complete\. 

## Limitations When Using Windows ACLs<a name="acl-limits"></a>

Keep the following limitations in mind when using Windows ACLs to control access to SMB file shares:
+ Windows ACLs are only supported on file shares that are enabled for Active Directory when you use Windows SMB clients to access the file shares\.
+ file gateways support a maximum of 10 ACL entries for each file and directory\.
+ file gateways don't support `Audit` and `Alarm` entries, which are system access\-control list \(SACL\) entries\. file gateways support `Allow` and `Deny` entries, which are discretionary access control list \(DACL\) entries\. 
+ The root ACL settings of SMB file shares are only on the gateway, and the settings are persisted across gateway updates and restarts\.
**Note**  
If you configure the ACLs on the root instead of the parent folder under the root, the ACL permissions aren't persisted in Amazon S3\.

  Given these conditions, make sure to do the following:
  + If you configure multiple gateways to access the same Amazon S3 bucket, configure the root ACL on each of the gateways to keep the permissions consistent\.
  + If you delete a file share and recreate it on the same Amazon S3 bucket, make sure that you use the same set of root ACLs\.