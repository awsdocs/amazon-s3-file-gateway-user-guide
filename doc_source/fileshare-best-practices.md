--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# File share best practices<a name="fileshare-best-practices"></a>

This section describes best practices for creating file shares\.

**Topics**
+ [Working with multiple file shares and Amazon S3 buckets](#prevent-multiple-writes)
+ [Allowing specific NFS clients to mount your file share](#nfs-client-mount)

## Working with multiple file shares and Amazon S3 buckets<a name="prevent-multiple-writes"></a>

Unpredictable results can sometimes occur if you configure one Amazon S3 bucket to be written to by multiple gateways or file shares, but there are two different configuration methods you can use to prevent this\. Choose the method that best fits your use case from the following options:
+ Configure your S3 buckets so that only one file share can write to each bucket\. Use a different file share to write to each bucket\.

  To do this, you create an S3 bucket policy that denies all roles except the role used for a specific file share to put or delete objects in the bucket\. Attach a similar policy to each bucket, specifying a different file share to write to each bucket\.

  The following example policy denies S3 bucket write permissions to all roles except the role that created the bucket\. The `s3:DeleteObject` and `s3:PutObject` actions are denied for all roles except `"TestUser"`\. The policy applies to all objects in the `"arn:aws:s3:::TestBucket/*"` bucket\.

  ```
  {
     "Version":"2012-10-17",
     "Statement":[
        {
           "Sid":"DenyMultiWrite",
           "Effect":"Deny",
           "Principal":"*",
           "Action":[
              "s3:DeleteObject",
              "s3:PutObject"
           ],
           "Resource":"arn:aws:s3:::TestBucket/*",
           "Condition":{
              "StringNotLike":{
                 "aws:userid":"TestUser:*"
              }
           }
        }
     ]
  }
  ```
+ If you want to write to the same Amazon S3 bucket from multiple file shares, you must prevent the file shares from trying to write to the same objects simultaneously\.

  To do this, you configure a separate, unique object prefix for each file share, which means that each file share will only write to objects with its corresponding prefix, and will not write to objects associated with the other file shares in your deployment\. You configure the object prefix in the **S3 prefix name** field when you create a new file share\.

## Allowing specific NFS clients to mount your file share<a name="nfs-client-mount"></a>

We recommend that you change the allowed NFS client settings for your file share\. If you don't, any client on your network can mount your file share\. For information about how to edit your NFS client settings, see [Editing access settings for your NFS file share](edit-nfs-client.md)\.