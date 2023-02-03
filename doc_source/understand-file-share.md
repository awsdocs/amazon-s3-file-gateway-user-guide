--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Understanding file share status<a name="understand-file-share"></a>

Each file share has an associated status that tells you at a glance what the health of the file share is\. Most of the time, the status indicates that the file share is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem that might or might not require action on your part\.

You can see file share status on the Storage Gateway console\. File share status appears in the **Status** column for each file share in your gateway\. A file share that is functioning normally has the status of AVAILABLE\.

In the following table, you can find a description of each file share status, and if and when you should act based on the status\. A file share should have AVAILABLE status all or most of the time it's in use\.


| Status | Meaning | 
| --- | --- | 
| AVAILABLE |  The file share is configured properly and is available to use\. The AVAILABLE status is the normal running status for a file share\.  | 
| CREATING |  The file share is being created and is not ready for use\. The CREATING status is transitional\. No action is required\. If file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| UPDATING |  The file share configuration is being updated\. If a file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| DELETING |  The file share is being deleted\. The file share is not deleted until all data is uploaded to AWS\. The DELETING status is transitional, and no action is required\.  | 
| FORCE\_DELETING |  The file share is being deleted forcibly\. The file share is deleted immediately and uploading to AWS is aborted\. The FORCE\_DELETING status is transitional, and no action is required\.  | 
| UNAVAILABLE |  The file share is in an unhealthy state\. Certain issues can cause the file share to go into an unhealthy state\. For example, role policy errors can cause this, or if the file share maps to an Amazon S3 bucket that doesn't exist\. When the issue that caused the unhealthy state is resolved, the file returns to AVAILABLE state\.  | 