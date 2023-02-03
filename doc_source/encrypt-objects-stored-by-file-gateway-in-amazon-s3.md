--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Encrypt objects stored by File Gateway in Amazon S3<a name="encrypt-objects-stored-by-file-gateway-in-amazon-s3"></a>

You can use AWS Key Management Service \(AWS KMS\) to encrypt objects that your File Gateway stores in Amazon S3\. To do this using the Storage Gateway console, see [Create an NFS file share with custom configuration](CreatingAnNFSFileShare.md) or [Create an SMB file share with custom configuration](CreatingAnSMBFileShare.md)\. You can also do this by using the Storage Gateway API\. For instructions, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) or [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) in the *AWS Storage Gateway API Reference*\.

By default, a File Gateway uses server\-side encryption managed with Amazon S3 \(SSE\-S3\) when it writes data to an S3 bucket\. If you make SSE\-KMS \(server\-side encryption with AWS KMS–managed keys\) the default encryption for your S3 bucket, objects that a File Gateway stores there are encrypted using SSE\-KMS\.

To encrypt using SSE\-KMS with your own AWS KMS key, you must enable SSE\-KMS encryption\. When you do so, provide the Amazon Resource Name \(ARN\) of the KMS key when you create your file share\. You can also update KMS settings for your file share by using the [UpdateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html) or [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html) API operation\. This update applies to objects stored in the Amazon S3 buckets after the update\.

If you configure your File Gateway to use SSE\-KMS for encryption, you must manually add `kms:Encrypt`, `kms:Decrypt`, `kms:ReEncrypt`, `kms:GenerateDataKey`, and `kms:DescribeKey` permissions to the IAM role associated with the file share\. For more information, see [Using Identity\-Based Policies \(IAM Policies\) for Storage Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/using-identity-based-policies.html)\.