--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Providing guest access to your file share<a name="guest-access"></a>

If you want to provide only guest access, your S3 File Gateway doesn't have to be part of a Microsoft AD domain\. You can also use a S3 File Gateway that is a member of an AD domain to create file shares with guest access\. Before you create a file share using guest access, you need to change the default password\.

These steps set the password for the guest user `smbguest`\. The `smbguest` user is the user when the authentication method for the file share is set to `GuestAccess`\.

**To change the guest access password**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, then choose the gateway for which you want to edit SMB settings\.

1. From the **Actions** drop\-down menu, choose **Edit SMB settings**, then choose **Guest access settings**\.

1. For **Guest password**, provide a password, and then choose **Save**\.