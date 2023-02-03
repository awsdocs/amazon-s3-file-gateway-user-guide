--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Adding a file share<a name="add-file-share"></a>

After your S3 File Gateway is activated and running, you can add additional file shares and grant access to Amazon S3 buckets\. Buckets that you can grant access to include buckets in a different AWS account than your file share\. For information about how to add a file share, see [Create a file share](GettingStartedCreateFileShare.md)\.

**Topics**
+ [Granting access to an Amazon S3 bucket](#grant-access-s3)
+ [Cross\-service confused deputy prevention](#cross-service-confused-deputy-prevention)
+ [Using a file share for cross\-account access](#cross-account-access)

## Granting access to an Amazon S3 bucket<a name="grant-access-s3"></a>

When you create a file share, your File Gateway requires access to upload files into your Amazon S3 bucket, and to perform actions on any access points or virtual private cloud \(VPC\) endpoints that it uses to connect to the bucket\. To grant this access, your File Gateway assumes an AWS Identity and Access Management \(IAM\) role that is associated with an IAM policy that grants this access\.

The role requires this IAM policy and a security token service trust \(STS\) relationship for it\. The policy determines which actions the role can perform\. In addition, your S3 bucket and any associated access points or VPC endpoints must have an access policy that allows the IAM role to access them\.

You can create the role and access policy yourself, or your File Gateway can create them for you\. If your File Gateway creates the policy for you, the policy contains a list of S3 actions\. For information about roles and permissions, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

The following example is a trust policy that allows your File Gateway to assume an IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "storagegateway.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

If you don't want your File Gateway to create a policy on your behalf, you can create your own policy and attach it to your file share\. For more information about how to do this, see [Create a file share](GettingStartedCreateFileShare.md)\.

The following example policy allows your File Gateway to perform all the Amazon S3 actions listed in the policy\. The first part of the statement allows all the actions listed to be performed on the S3 bucket named `TestBucket`\. The second part allows the listed actions on all objects in `TestBucket`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:GetAccelerateConfiguration",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:ListBucket",
                "s3:ListBucketVersions",
                "s3:ListBucketMultipartUploads"
            ],
            "Resource": "arn:aws:s3:::TestBucket",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:ListMultipartUploadParts",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::TestBucket/*",
            "Effect": "Allow"
        }
    ]
}
```

The following example policy is similar to the preceding one, but allows your File Gateway to perform actions required to access a bucket through an access point\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:ListMultipartUploadParts",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:us-east-1:123456789:accesspoint/TestAccessPointName/*",
            "Effect": "Allow"
        }
    ]
}
```

**Note**  
If you need to connect your file share to an S3 bucket through a VPC endpoint, see [Endpoint policies for Amazon S3](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html#vpc-endpoints-policies-s3) in the *AWS PrivateLink User Guide*\.

**Note**  
For encrypted buckets, the fileshare must use the key in the destination S3 bucket account\.

**Note**  
If your File Gateway uses SSE\-KMS for encryption, make sure the IAM role associated with the file share includes *kms:Encrypt*, *kms:Decrypt*, *kms:ReEncrypt*, *kms:GenerateDataKey*, and *kms:DescribeKey* permissions\. For more information, see [Using Identity\-Based Policies \(IAM Policies\) for Storage Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/using-identity-based-policies.html)\.

## Cross\-service confused deputy prevention<a name="cross-service-confused-deputy-prevention"></a>

The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in resource policies to limit the permissions that AWS Storage Gateway gives another service to the resource\. If you use both global condition context keys, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\.

The value of `aws:SourceArn` must be the ARN of the Storage Gateway with which your file share is associated\.

The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full ARN of the resource\. If you don't know the full ARN of the resource or if you are specifying multiple resources, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:servicename::123456789012:*`\. 

The following example shows how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys in Storage Gateway to prevent the confused deputy problem\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ConfusedDeputyPreventionExamplePolicy",
      "Effect": "Allow",
      "Principal": {
        "Service": "storagegateway.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "123456789012"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-712345DA"
        }
      }
    }
  ]
}
```

## Using a file share for cross\-account access<a name="cross-account-access"></a>

*Cross\-account* access is when an Amazon Web Services account and users for that account are granted access to resources that belong to another Amazon Web Services account\. With File Gateways, you can use a file share in one Amazon Web Services account to access objects in an Amazon S3 bucket that belongs to a different Amazon Web Services account\.

**To use a file share owned by one Amazon Web Services account to access an S3 bucket in a different Amazon Web Services account**

1. Make sure that the S3 bucket owner has granted your Amazon Web Services account access to the S3 bucket that you need to access and the objects in that bucket\. For information about how to grant this access, see [Example 2: Bucket owner granting cross\-account bucket permissions](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.html) in the *Amazon Simple Storage Service User Guide*\. For a list of the required permissions, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

1. Make sure that the IAM role that your file share uses to access the S3 bucket includes permissions for operations such as `s3:GetObjectAcl` and `s3:PutObjectAcl`\. In addition, make sure that the IAM role includes a trust policy that allows your account to assume that IAM role\. For an example of such a trust policy, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

   If your file share uses an existing role to access the S3 bucket, you should include permissions for `s3:GetObjectAc`l and `s3:PutObjectAcl` operations\. The role also needs a trust policy that allows your account to assume this role\. For an example of such a trust policy, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Give bucket owner full control** in the **Object metadata** settings in the **Configure file share setting** dialog box\.

When you have created or updated your file share for cross\-account access and mounted the file share on\-premises, we highly recommend that you test your setup\. You can do this by listing directory contents or writing test files and making sure the files show up as objects in the S3 bucket\.

**Important**  
Make sure to set up the policies correctly to grant cross\-account access to the account used by your file share\. If you don't, updates to files through your on\-premises applications don't propagate to the Amazon S3 bucket that you're working with\.

### Resources<a name="related-topics-fileshare"></a>

For additional information about access policies and access control lists, see the following:

[Guidelines for using the available access policy options](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-policy-alternatives-guidelines.html) in the *Amazon Simple Storage Service User Guide*

[Access Control List \(ACL\) overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html) in the *Amazon Simple Storage Service User Guide*