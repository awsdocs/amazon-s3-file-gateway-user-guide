--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Quotas<a name="fgw-quotas"></a>

## Quotas for file shares<a name="resource-file-limits"></a>

The following table lists quotas for file shares\.


| Description | Limit | 
| --- | --- | 
| Maximum number of file shares per gateway Each file share can only connect to one S3 bucket, but multiple file shares can connect to the same bucket\. If you connect more than one file share to the same bucket, you must configure each file share to use a unique, non\-overlapping prefix name to prevent read/write conflicts\. The number of file shares managed by a gateway can impact the gateway's performance\. For more information, see [Performance guidance for gateways with multiple file shares](https://docs.aws.amazon.com/filegateway/latest/files3/Performance.html#performance-multiple-file-shares)\.   | 50 | 
| The maximum size of an individual file, which is the maximum size of an individual object in Amazon S3  If you write a file larger than 5 TB, you get a "file too large" error message and only the first 5 TB of the file is uploaded\.   | 5 TB | 
| Maximum path length  Clients are not allowed to create a path exceeding this length, and doing so results in an error\. This limit applies to both protocols supported by File Gateways, NFS and SMB\.  | 1024 bytes | 

## Recommended local disk sizes for your gateway<a name="disk-sizes"></a>

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | 
| S3 File Gateway | 150 GiB | 64 TiB | — | 

**Note**  
You can configure one or more local drives for your cache up to the maximum capacity\.  
When adding cache to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as a cache\.