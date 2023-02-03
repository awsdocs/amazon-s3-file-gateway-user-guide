--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Best practices: managing multipart uploads<a name="best-practices-managing-multi-part-uploads"></a>

When transferring large files, S3 File Gateway makes use of the Amazon S3 multipart upload feature to split the files into smaller parts and transfer them in parallel for improved efficiency\. For more information about multipart upload, see [Uploading and copying objects using multipart upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html) in the *Amazon Simple Storage Service User Guide*\.

If a multipart upload doesn't complete successfully for any reason, the gateway typically stops the transfer, deletes any partially\-transferred pieces of the file from Amazon S3, and attempts the transfer again\. In rare cases, such as when hardware or network failure prevent the gateway from cleaning up after an unsuccessful multipart upload, pieces of the partially\-transferred file might remain on Amazon S3 where they can incur storage charges\.

As a best practice for minimizing Amazon S3 storage costs from incomplete multipart uploads, we recommend configuring an Amazon S3 bucket lifecycle rule that uses the `AbortIncompleteMultipartUpload` API action to automatically stop unsuccessful transfers and delete associated file parts after a designated number of days\. For instructions, see [Configuring a bucket lifecycle policy to abort incomplete multipart uploads](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpu-abort-incomplete-mpu-lifecycle-config.html) in the *Amazon Simple Storage Service User Guide*\.