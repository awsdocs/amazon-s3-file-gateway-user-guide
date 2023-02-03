--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Using storage classes<a name="storage-classes"></a>

 Amazon S3 File Gateway supports the Amazon S3 Standard, Amazon S3 Standard\-Infrequent Access, Amazon S3 One Zone\-Infrequent Access, Amazon S3 Intelligent\-Tiering, and S3 Glacier storage classes\. For more information about storage classes, see [Amazon S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service User Guide*\.

**Note**  
S3 File Gateway does not currently support the Amazon S3 Glacier Instant Retrieval storage class\.

**Topics**
+ [Using storage classes with a File Gateway](#ia-file-gateway)
+ [Using the GLACIER storage class with File Gateway](#using-glacier-strage-class)

## Using storage classes with a File Gateway<a name="ia-file-gateway"></a>

When you create or update a file share, you have the option to select a storage class for your objects\. You can choose the Amazon S3 Standard storage class, or any of the S3 Standard\-IA, S3 One Zone\-IA, or S3 Intelligent\-Tiering storage classes\. Objects stored in any of these storage classes can be transitioned to GLACIER using a lifecycle policy


| Amazon S3 storage class | Considerations | 
| --- | --- | 
| Standard | Choose Standard to store your frequently accessed files redundantly in multiple Availability Zones that are geographically separated\. This is the default storage class\. See Amazon S3 pricing for more details\.  | 
| S3 Intelligent\-Tiering | Choose Intelligent\-Tiering to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. Objects smaller than 128 KB are not eligible for auto tiering in the Intelligent\-Tiering storage class\. These objects are charged at the frequent access tier rates, and don't incur the monitoring fee charged for auto\-tiered objects\. S3 Intelligent\-Tiering now supports an Archive Access tier and a Deep Archive Access tier\. S3 Intelligent\-Tiering automatically moves objects that haven’t been accessed for 90 days to the Archive Access tier, and after 180 days without being accessed, to the Deep Archive Access tier\. Whenever an object in one of the archive access tiers is restored, the object moves to the Frequent Access tier within a few hours and is ready to be retrieved\. This creates timeout errors for users or applications trying to access files through a file share if the object only exists in one of the two archive tiers\. Don't use the archive tiers with S3 Intelligent\-Tiering if your applications are accessing files through the file shares that are presented by the File Gateway\. When file operations that update metadata \(such as owner, timestamp, permissions, and ACLs\) are performed against files managed by the File Gateway, the existing object is deleted and a new version of the object is created in this Amazon S3 storage class\. Validate how file operations impact object creation before using this storage class in production\. See Amazon S3 pricing for more details\.  | 
| S3 Standard\-IA | Choose Standard\-IA to store your infrequently accessed files redundantly in multiple Availability Zones that are geographically separated\. Objects stored in the Standard\-IA storage class can incur additional charges for overwriting, deleting, requesting, retrieving, or transitioning objects between storage classes within 30 days\. There is a minimum storage duration of 30 days\. Objects deleted before 30 days incur a pro\-rated charge equal to the storage charge for the remaining days\. Consider how often these objects change, how long you plan to keep these objects, and how often you need to access them\. Objects smaller than 128 KB are charged for 128 KB and early deletion fees apply\. When file operations that update metadata \(such as owner, timestamp, permissions, and ACLs\) are performed against files managed by the File Gateway, the existing object is deleted and a new version of the object is created in this Amazon S3 storage class\. You should validate how file operations impact object creation before using this storage class in production because early deletion fees apply\. See Amazon S3 pricing for more details\.  | 
| S3 One Zone\-IA | Choose One Zone\-IA to store your infrequently accessed files in a single Availability Zone\. Objects stored in the One Zone\-IA storage class can incur additional charges for overwriting, deleting, requesting, retrieving, or transitioning objects between storage classes within 30 days\. There is a minimum storage duration of 30 days, and objects deleted before 30 days incur a pro\-rated charge equal to the storage charge for the remaining days\. Consider how often these objects change, how long you plan to keep these objects, and how often you need to access them\. Objects smaller than 128 KB are charged for 128 KB and early deletion fees apply\. When file operations that update metadata \(such as owner, timestamp, permissions, and ACLs\) are performed against files managed by the File Gateway, the existing object is deleted and a new version of the object is created in this Amazon S3 storage class\. You should validate how file operations impact object creation before using this storage class in production because early deletion fees apply\. See Amazon S3 pricing for more details\.  | 

Although you can write objects directly from a file share to the S3\-Standard\-IA, S3\-One Zone\-IA, or S3 Intelligent\-Tiering storage class, we recommend that you use a lifecycle policy to transition your objects rather than write directly from the file share, especially if you're expecting to update or delete the object within 30 days of archiving it\. For information about lifecycle policy, see [Object lifecycle management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)\.

## Using the GLACIER storage class with File Gateway<a name="using-glacier-strage-class"></a>

If you transition a file to S3 Glacier through Amazon S3 lifecycle policies, and the file is visible to your file share clients through the cache, you will encounter I/O errors when you update the file\. We recommend that you set up CloudWatch Events to receive notification when these I/O errors occur, and use the notification to take action\. For example, you can take action to restore the archived object to Amazon S3\. After the object is restored to S3, your file share clients can access and update them successfully through the file share\. 

For information about how to restore archived objects, see [Restoring archived objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/restoring-objects.html) in the *Amazon Simple Storage Service User Guide*\.

**Important**  
S3 File Gateway does not officially support the S3 Glacier Instant Retrieval storage class\. Although you can designate objects in a file share bucket for S3 Glacier Instant Retrieval by using lifecycle policies or direct `PUT` requests, S3 File Gateway cannot recognize which files are in that storage class, and will perform file operations on them like any other object\. Because S3 Glacier Instant Retrieval uses a different cost structure than basic Amazon S3 storage, bulk file operations such as virus scans, `rsync`, and renames, can result in large Amazon S3 bills if not managed carefully\. For this reason, we do not recommend using S3 Glacier Instant Retrieval with S3 File Gateway\.