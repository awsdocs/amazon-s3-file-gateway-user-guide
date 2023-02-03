--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Basic performance guidance for S3 File Gateway<a name="performance-fgw"></a>

In this section, you can find guidance for provisioning hardware for your S3 File Gateway VM\. The instance configurations that are listed in the table are examples, and are provided for reference\.

For best performance, the cache disk size must be tuned to the size of the active working set\. Using multiple local disks for the cache increases write performance by parallelizing access to data and leads to higher IOPS\.

**Note**  
We don't recommend using ephemeral storage\. For information about using ephemeral storage, see [Using ephemeral storage with EC2 gateways](ManagingLocalStorage-common.md#ephemeral-disk-cache)\.  
For Amazon EC2 instances, if you have more than 5 million objects in your S3 bucket and you are using a General Purposes SSD volume, a minimum root EBS volume of 350 GiB is needed for acceptable performance of your gateway during start up\. For information about how to increase your volume size, see [Modifying an EBS volume using elastic volumes \(console\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/requesting-ebs-volume-modifications.html#modify-ebs-volume)\.

In the following tables, *cache hit* read operations are reads from the file shares that are served from cache\. *Cache miss* read operations are reads from the file shares that are served from Amazon S3\.

The following tables show example S3 File Gateway configurations\.

## S3 File Gateway performance on Linux clients<a name="performance-fgw-linux-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/performance-fgw.html)

## File Gateway performance on Windows clients<a name="performance-fgw-windows-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/performance-fgw.html)

**Note**  
Your performance might vary based on your host platform configuration and network bandwidth\. Write throughput performance decreases with file size, with the highest achievable throughput for small files being 16 files per second\.