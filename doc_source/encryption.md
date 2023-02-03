--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Data encryption using AWS KMS<a name="encryption"></a>

Storage Gateway uses SSL/TLS \(Secure Socket Layers/Transport Layer Security\) to encrypt data that is transferred between your gateway appliance and AWS storage\. By default, Storage Gateway uses Amazon S3\-Managed encryption keys \(SSE\-S3\) to server\-side encrypt all data it stores in Amazon S3\. You have an option to use the Storage Gateway API to configure your gateway to encrypt data stored in the cloud using server\-side encryption with AWS Key Management Service \(SSE\-KMS\) keys\.

**Important**  
When you use an AWS KMS key for server\-side encryption, you must choose a symmetric key\. Storage Gateway does not support asymmetric keys\. For more information, see [Using symmetric and asymmetric keys](https://docs.aws.amazon.com/kms/latest/developerguide/symmetric-asymmetric.html) in the *AWS Key Management Service Developer Guide*\.

**Encrypting a file share**  
For a file share, you can configure your gateway to encrypt your objects with AWS KMS–managed keys by using SSE\-KMS\. For information on using the Storage Gateway API to encrypt data written to a file share, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.

**Encrypting a file system**  
For information see, [Data Encryption in Amazon FSx](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/encryption.html)in the *Amazon FSx for Windows File Server User Guide*\.

When using AWS KMS to encrypt your data*,* keep the following in mind:
+ Your data is encrypted at rest in the cloud\. That is, the data is encrypted in Amazon S3\.
+ IAM users must have the required permissions to call the AWS KMS API operations\. For more information, see [Using IAM policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) in the *AWS Key Management Service Developer Guide*\.
+ If you delete or disable your KMS key or revoke the grant token, you can't access the data on the volume or tape\. For more information, see [Deleting KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html) in the *AWS Key Management Service Developer Guide*\.
+ If you create a snapshot from a volume that is KMS\-encrypted, the snapshot is encrypted\. The snapshot inherits the volume's KMS key\.
+ If you create a new volume from a snapshot that is KMS\-encrypted, the volume is encrypted\. You can specify a different KMS key for the new volume\.
**Note**  
Storage Gateway doesn't support creating an unencrypted volume from a recovery point of a KMS\-encrypted volume or a KMS\-encrypted snapshot\.

For more information about AWS KMS, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)