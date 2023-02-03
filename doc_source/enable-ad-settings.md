--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Using Active Directory to authenticate users<a name="enable-ad-settings"></a>

To use your corporate Active Directory for user authenticated access to your SMB file share, edit the SMB settings for your gateway with your Microsoft AD domain credentials\. Doing this allows your gateway to join your Active Directory domain and allows members of the domain to access the SMB file share\.

**Note**  
Using AWS Directory Service, you can create a hosted Active Directory domain service in the AWS Cloud\.

Anyone who can provide the correct password gets guest access to the SMB file share\.

You can also enable access control lists \(ACLs\) on your SMB file share\. For information about how to enable ACLs, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.

**To enable Active Directory authentication**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, then choose the gateway for which you want to edit SMB settings\.

1. From the **Actions** drop\-down menu, choose **Edit SMB settings**, then choose **Active Directory settings**\.

1. For **Domain name**, provide the domain that you want the gateway to join\. You can join a domain by using its IP address or its organizational unit\. An *organizational unit* is an Active Directory subdivision that can hold users, groups, computers, and other organizational units\.
**Note**  
**Active Directory status** shows **Detached** when a gateway has never joined a domain\.  
Joining a domain creates an Active Directory computer account in the default computers container \(which is not an OU\), using the gateway's **Gateway ID** as the account name \(for example, SGW\-1234ADE\)\.  
If your Active Directory environment requires that you pre\-stage accounts to facilitate the join domain process, you will need to create this account ahead of time\.  
If your Active Directory environment has a designated OU for new computer objects, you must specify that OU when joining the domain\.  
If your gateway can't join an Active Directory directory, try joining with the directory's IP address by using the [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html) API operation\.

1. Provide the domain user and the domain password, and then choose **Save**\.

   A message at the bottom of the **Gateways** section of your console indicates that your gateway successfully joined your AD domain\.

**To limit file share access to specific AD users and groups**

1. In the Storage Gateway console, choose the file share that you want to limit access to\.

1. From the **Actions** drop\-down menu, choose **Edit file share access settings**\.

1. In the **User and group file share access** section, choose your settings\.

   For **Allowed users and groups**, choose **Add allowed user** or **Add allowed group** and enter an AD user or group that you want to allow file share access\. Repeat this process to allow as many users and groups as necessary\.

   For **Denied users and groups**, choose **Add denied user** or **Add denied group** and enter an AD user or group that you want to deny file share access\. Repeat this process to deny as many users and groups as necessary\.
**Note**  
The **User and group file share access** section appears only if **Active Directory** is selected\.  
Groups must be prefixed with the `@` character\. Acceptable formats include: `DOMAIN\User1`, `user1`, `@group1`, and `@DOMAIN\group1`\.  
If you configure **Allowed and Denied Users and Groups** lists, then Windows ACLs will not grant any access that overrides those lists\.  
The **Allowed and Denied Users and Groups** lists are evaluated before ACLs, and control which users can mount or access the file share\. If any users or groups are placed on the **Allowed** list, the list is considered enabled, and only those users can mount the file share\.  
After a user has mounted a file share, ACLs then provide more granular protection that controls which specific files or folders the user can access\. For more information, see [Enabling Windows ACLs on a new SMB file share](https://docs.aws.amazon.com/filegateway/latest/files3/smb-acl.html#enable-acl-new-fileshare)\.

1. When you finish adding your entries, choose **Save**\.

