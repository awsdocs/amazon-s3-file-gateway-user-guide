--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Mount your SMB file share on your client<a name="using-smb-fileshare"></a>

Now you mount your SMB file share and map to a drive accessible to your client\. The console's File Gateway section shows the supported mount commands that you can use for SMB clients\. Following, you can find some additional options to try\.

You can use several different methods for mounting SMB file shares, including the following:
+ Command Prompt \(`cmdkey` and `net use`\) – Use the command prompt to mount your file share\. Store your credentials with `cmdkey`, then mount the drive with `net use` and include the `/persistent:yes` and `/savecred` switches if you want the connection to persist across system reboots\. The specific commands you use will be different depending on whether you want to mount the drive for Microsoft Active Directory \(AD\) access or guest user access\. Examples are provided below\.
+ File Explorer \(Map Network Drive\) – Use Windows File Explorer to mount your file share\. Configure settings to specify whether you want the connection to persist across system reboots and prompt for network credentials\.
+ PowerShell script – Create a custom PowerShell script to mount your file share\. Depending on the parameters you specify in the script, the connection can be persistent across system reboots, and the share can be either visible or invisible to the operating system while mounted\.

**Note**  
If you are a Microsoft AD user, check with your administrator to ensure that you have access to the SMB file share before mounting the file share to your local system\.  
If you are a guest user, make sure that you have the guest user password before attempting to mount the file share\.

**To mount your SMB file share for authorized Microsoft AD users using the command prompt:**

1. Make sure the Microsoft AD user has the necessary permissions to the SMB file share before mounting the file share to the user's system\.

1. Enter the following at the command prompt to mount the file share:

   **net use *WindowsDriveLetter*: \\\\*GatewayIPAddress*\\*FileShareName* /persistent:*yes***

**To mount your SMB file share with a specific sign\-in credentials combination using the command prompt:**

1. Make sure that the user has access to the SMB file share before mounting the file share to the system\.

1. Enter the following at the command prompt to save the user credentials in Windows Credential Manager:

   **cmdkey /add:*GatewayIPAddress* /user:*DomainName*\\*UserName* /pass:*Password***

1. Enter the following at the command prompt to mount the file share:

   **net use *WindowsDriveLetter*: \\\\*GatewayIPAddress*\\*FileShareName* /persistent:*yes* /savecred**

**To mount your SMB file share for guest users using the command prompt:**

1. Make sure that you have the guest user password before mounting the file share\.

1. Type the following at the command prompt to save the guest credentials in Windows Credential Manager:

   **cmdkey /add:*GatewayIPAddress* /user:*DomainName*\\smbguest /pass:*Password***

1. Type the following at the command prompt\.

   **net use *WindowsDriveLetter*: \\\\$*GatewayIPAddress*\\$*Path* /user:$*Gateway ID*\\smbguest /persistent:yes /savecred**

**Note**  
When mounting file shares, be aware of the following:  
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a File Gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a File Gateway\.
Unless you configure your file share connection to save your user credentials and persist across system restarts, you might need to remount your file share each time you restart your client system\.

**To mount an SMB file share using Windows File Explorer**

1. Press the Windows key and type **File Explorer** in the **Search Windows** box, or press **Win\+E**\.

1. In the navigation pane, choose **This PC**, then choose **Map Network Drive** for **Map Network Drive** in the **Computer** tab, as shown in the following screen shot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/map-on-windows-explorer.png)  
  


1. In the **Map Network Drive** dialog box, choose a drive letter for **Drive**\.

1. For **Folder**, type **\\\\*\[File Gateway IP\]*\\*\[SMB File Share Name\]***, or choose **Browse** to select your SMB file share from the dialog box\.

1. \(Optional\) Select **Reconnect at sign\-up** if you want your mount point to persist after reboots\.

1. \(Optional\) Select **Connect using different credentials** if you want a user to enter the Microsoft AD logon or guest account user password\.

1. Choose **Finish** to complete your mount point\.

You can edit file share settings, edit allowed and denied users and groups, and change the guest access password from the Storage Gateway Management Console\. You can also refresh the data in the file share's cache and delete a file share from the console\.

**To modify your SMB file share's properties**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **File Shares**\.

1. On the **File Share** page, select the check box by the SMB file share that you want to modify\.

1. For Actions, choose the action that you want:
   + Choose **Edit file share settings** to modify share access\.
   + Choose **Edit allowed/denied users** to add or delete users and groups, and then type the allowed and denied users and groups into the **Allowed Users**, **Denied Users**, **Allowed Groups**, and **Denied Groups** boxes\. Use the **Add Entry** buttons to create new access rights, and the **\(X\)** button to remove access\.
**Note**  
Groups must be prefixed with the `@` character\. Acceptable formats include: `DOMAIN\User1`, `user1`, `@group1`, and `@DOMAIN\group1`\.

1. When you're finished, choose **Save**\.

   When you enter allowed users and groups, you are creating an allow list\. Without an allow list, all authenticated Microsoft AD users can access the SMB file share\. Any users and groups that are marked as denied are added to a deny list and can't access the SMB file share\. In instances where a user or group is on both the deny list and allow list, the deny list takes precedence\.

   You can enable Access Control Lists\(ACLs\) on your SMB file share\. For information about how to enable ACLs, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.

**Next Step**

[Test your S3 File Gateway](GettingStartedTestFileShare.md)