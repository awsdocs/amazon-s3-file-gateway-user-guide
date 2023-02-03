--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# DOS or Windows file attribute support in Amazon S3 File Gateway<a name="s3-dos-attribute-support"></a>

Amazon S3 File Gateway supports DOS or Windows file attributes by default\. Using S3 File Gateway, you can preserve file data and metadata and update settings — such as marking items as archived when they are placed in Amazon S3\. For more information about DOS and Windows file attributes, see the [File Attribute Constants](https://learn.microsoft.com/en-us/windows/win32/fileio/file-attribute-constants) article on the Windows app development documentation website\.

S3 File Gateway supports the following attributes:
+ *ReadOnly* – The S3 File Gateway prevents changes to files that have the ReadOnly attribute set\.
+ *Archive* – The S3 File Gateway sets this attribute when files are first added to the gateway\.
**Note**  
Backup applications commonly backup files that have the Archive bit set and then clear the bit after successful backup\.
+ *Hidden* – Server Message Block \(SMB\) clients hide files that use this bit set\.
+ *System* – This attribute persists once you have set it\.

When you copy a file to the S3 File Gateway with the attributes set, the file's DOS or Windows attributes are preserved on the S3 File Gateway and in Amazon S3\. You can update these attributes for files on the gateway, and those updates also apply to the object in Amazon S3\. If a file is evicted from the gateway the gateway pull the file, its metadata, and its persistent attributes from Amazon S3 when you request\.

**Note**  
DOS attributes are only supported on SMB shares and if access is controlled by Windows Access Control Lists\.