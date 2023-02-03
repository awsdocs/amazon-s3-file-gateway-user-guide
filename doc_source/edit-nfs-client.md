--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Editing access settings for your NFS file share<a name="edit-nfs-client"></a>

We recommend changing the allowed NFS client settings for your NFS file share\. If you don't, any client on your network can mount to your file share\.

**To edit NFS access settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the NFS file share that you want to edit\.

1. For **Actions**, choose **Edit share access settings**\.

1. In the **Edit allowed clients** dialog box, choose **Add entry**, provide the IP address or CIDR notation for the clients that you want to allow, and then choose **Save**\.