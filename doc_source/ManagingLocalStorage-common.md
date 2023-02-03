--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Managing local disks for your Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. Gateways created on Amazon EC2 instances use Amazon EBS volumes as local disks\. 

**Topics**
+ [Deciding the amount of local disk storage](#decide-local-disks-and-sizes)
+ [Determining the size of cache storage to allocate](#CachedLocalDiskCacheSizing-common)
+ [Configuring additional cache storage](#ConfiguringLocalDiskStorage)
+ [Using ephemeral storage with EC2 gateways](#ephemeral-disk-cache)

## Deciding the amount of local disk storage<a name="decide-local-disks-and-sizes"></a>

The number and size of disks that you want to allocate for your gateway is up to you\. File Gateways require at least one 150 GiB disk to use as a cache\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 or file system\. After the initial configuration and deployment of your gateway, you can add more disks for cache storage as your workload demands increase\.

**Note**  
Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.  
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage\. A data store that is backed by only one underlying physical disk can lead to poor performance in some situations when it is used to back both the cache storage\. This is also true if the backup is a less\-performant RAID configuration such as RAID1\.

## Determining the size of cache storage to allocate<a name="CachedLocalDiskCacheSizing-common"></a>

Your gateway uses its cache storage to provide low\-latency access to your recently accessed data\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3\.

You can use the initial approximation of 150 GiB to provision disks for the cache storage during gateway setup\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Performance](Performance.md)\.

## Configuring additional cache storage<a name="ConfiguringLocalDiskStorage"></a>

As your application needs change, you can increase the gateway's cache storage capacity\. You can add storage capacity to your gateway without interrupting functionality or causing downtime\. When you add more storage, you do so with the gateway VM turned on\.

**Important**  
When adding cache to an existing gateway, you must create new disks on the gateway host hypervisor or Amazon EC2 instance\. Do not remove or change the size of existing disks that have already been allocated as cache\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To configure additional cache storage for your gateway**

1. Provision one or more new disks on your gateway host hypervisor or Amazon EC2 instance\. For information about how to provision a disk on a hypervisor, see your hypervisor's documentation\. For information about provisioning Amazon EBS volumes for an Amazon EC2 instance, see [Amazon EBS volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html) in the *Amazon Elastic Compute Cloud User Guide for Linux Instances*\. In the following steps, you will configure this disk as cache storage\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. Search for your gateway and select it from the list\.

1. From the **Actions** menu, choose **Configure storage**\.

1. In the **Configure storage** section, identify the disks you provisioned\. If you don't see your disks, choose the refresh icon to refresh the list\. For each disk, choose **CACHE STORAGE** from the **Allocated to** drop\-down menu\.
**Note**  
**CACHE STORAGE** is the only available option for allocating disks on File Gateways\.

1. Choose **Save changes** to save your configuration settings\.

## Using ephemeral storage with EC2 gateways<a name="ephemeral-disk-cache"></a>

This section describes steps you need to take to prevent data loss when you select an ephemeral disk as storage for your gateway's cache\.

Ephemeral disks provide temporary block\-level storage for your Amazon EC2 instance\. Ephemeral disks are ideal for temporary storage of data that changes frequently, such as data in a gateway's cache storage\. When you launch your gateway with an Amazon EC2 Amazon Machine Image and the instance type you select supports ephemeral storage, the ephemeral disks are listed automatically\. You can select one of the disks to store your gateway's cache data\. For more information, see [Amazon EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Data that applications write to the gateway is stored synchronously in cache on the ephemeral disks, and then asynchronously uploaded to durable storage in Amazon S3\. If the Amazon EC2 instance is stopped after data is written to ephemeral storage, but before an asynchronous upload occurs, any data that has not yet been uploaded to Amazon S3 can be lost\. You can prevent such data loss by following the steps before you restart or stop the EC2 instance that hosts your gateway\.

**Important**  
If you stop and start an Amazon EC2 gateway that uses ephemeral storage, the gateway will be permanently offline\. This happens because the physical storage disk is replaced\. There is no work\-around for this issue\. The only resolution is to delete the gateway and activate a new one on a new EC2 instance\.

These steps in this following procedure are specific for File Gateways\.

**To prevent data loss in File Gateways that use ephemeral disks**

1. Stop all the processes that are writing to Amazon S3\.

1. Subscribe to receive notification from CloudWatch Events\. For information, see [Getting notified about file operations](monitoring-file-gateway.md#get-notification)\.

1. Call the [NotifyWhenUploaded API](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html) to get notified when data that is written, up until the ephemeral storage was lost, has been durably stored in Amazon S3\.

1. Wait for the API to complete and you receive a notification id\.

   You receive a CloudWatch event with the same notification id\.

1. Verify that the `CachePercentDirty` metric for your file share is 0\. This confirms that all your data has been written to Amazon S3\. For information about file share metrics, see [Understanding file share metrics](monitoring-file-gateway.md#monitoring-file-gateway-resources)\.

1. You can now restart or stop the File Gateway without risk of losing any data\.