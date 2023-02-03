--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Mount your NFS file share on your client<a name="GettingStartedAccessFileShare"></a>

Now you mount your NFS file share on a drive on your client and map it to your Amazon S3 bucket\.

**To mount a file share and map it to an Amazon S3 bucket**

1. If you are using a Microsoft Windows client, we recommend that you [create an SMB file share](https://docs.aws.amazon.com/storagegateway/latest/userguide/CreatingAnSMBFileShare.html) and access it using an SMB client that is already installed on Windows client\. If you use NFS, turn on Services for NFS in Windows\.

1. Mount your NFS file share:
   + For Linux clients, type the following command at the command prompt\.

     **sudo mount \-t nfs \-o nolock,hard *\[GatewayVMIPAddress\]*:/*\[FileShareName\]* *\[ClientMountPath\]***
   + For MacOS clients, type the following command at the command prompt\.

     **sudo mount\_nfs \-o vers=4,nolock,rwsize=1048576,hard \-v *\[GatewayVMIPAddress\]*:/*\[FileShareName\]* *\[ClientMountPath\]***
   + For Windows clients, type the following command at the command prompt \(**cmd\.exe**\)\.

     **mount –o nolock \-o mtype=hard *\[GatewayVMIPAddress\]*:/*\[FileShareName\]* *\[WindowsDriveLetter\]***

   For example, suppose that on a Windows client your VM's IP address is 123\.123\.1\.2 and your file share name name is `test-fileshare`\. Suppose also that you want to map to drive T\. In this case, your command looks like the following\.

   **mount \-o nolock \-o mtype=hard 123\.123\.1\.2:/test\-fileshare T:**
**Note**  
When mounting file shares, be aware of the following:  
S3 File Gateway does not support NFS file locking\. Always use the `-o nolock` option to disable file locking when mounting NFS file shares\.
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a File Gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a File Gateway\.
You might need to remount your file share after a reboot of your client\.
By default Windows uses a soft mount for mounting your NFS share\. Soft mounts time out more easily when there are connection issues\. We recommend using a hard mount because a hard mount is safer and better preserves your data\. The soft mount command omits the **\-o mtype=hard** switch\. The Windows hard mount command uses the `-o mtype=hard` switch\.
If you are using Windows clients, check your `mount` options after mounting by running the `mount` command with no options\. The response should that confirm the file share was mounted using the latest options you provided\. It also should confirm that you are not using cached old entries, which take at least 60 seconds to clear\.

**Next Step**

[Test your S3 File Gateway](GettingStartedTestFileShare.md)