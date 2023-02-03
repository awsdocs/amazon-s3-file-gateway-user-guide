--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Deleting a file share<a name="remove-file-share"></a>

If you no longer need a file share, you can delete it from the Storage Gateway console\. When you delete a file share, the gateway is detached from the Amazon S3 bucket that the file share maps to\. However, the S3 bucket and its contents aren't deleted\.

If your gateway is uploading data to a S3 bucket when you delete a file share, the delete process doesn't complete until all the data is uploaded\. The file share has the DELETING status until the data is completely uploaded\.

If you don't want to wait for your data to be completely uploaded, see the **To forcibly delete a file share** procedure later in this topic\.

**To delete a file share**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, then select one or more file shares to delete\.

1. For **Actions**, choose **Delete file share**\. The confirmation dialog box appears\.

1. Verify that you want to delete the specified file shares, then type the word *delete* in the confirmation box and choose **Delete**\. 

In certain cases, you might not want to wait until all the data written to files on the Network File System \(NFS\) file share is uploaded before deleting the file share\. For example, you might want to intentionally discard data that was written but has not yet been uploaded, or the Amazon S3 bucket that backs the file share might have already been deleted, meaning that uploading the specified data is no longer possible\.

In these cases, you can forcibly delete the file share by using the AWS Management Console or the `DeleteFileShare` API operation\. This operation aborts the data upload process\. When it does, the file share enters the FORCE\_DELETING status\. To forcibly delete a file share using the Storage Gateway console, see the procedure following\.<a name="force-delete"></a>

**To forcibly delete a file share**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. From the **File shares** list page, choose file share that you flagged for deletion in the procedure above to view its details\. After a few seconds, a deletion notification message appears on the **Details** tab\.

1. In the message that appears on the **Details** tab, verify the ID of the file share that you want to forcibly delete, select the confirmation box, and choose **Force delete now**\.
**Note**  
You cannot undo the force delete operation\.

You can also use the [DeleteFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html) API operation to forcibly delete the file share\.