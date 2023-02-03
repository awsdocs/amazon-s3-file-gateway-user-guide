--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Troubleshooting: File Gateway issues<a name="troubleshooting-file-gateway-issues"></a>

You can configure your File Gateway with an Amazon CloudWatch log group when you run VMware vSphere High Availability \(HA\)\. If you do, you receive notifications about your File Gateway's health status and about errors that the File Gateway encounters\. You can find information about these error and health notifications in CloudWatch Logs\.

In the following sections, you can find information that can help you understand the cause of each error and health notification and how to fix issues\.

**Topics**
+ [Error: 1344 \(0x00000540\)](#troubleshoot-copying-files-to-s3)
+ [Error: GatewayClockOutOfSync](#troubleshoot-logging-errors-gatewayclockoutofsync)
+ [Error: InaccessibleStorageClass](#troubleshoot-logging-errors-inaccessiblestorageclass)
+ [Error: InvalidObjectState](#troubleshoot-logging-errors-invalidobjectstate)
+ [Error: ObjectMissing](#troubleshoot-logging-errors-objectmissing)
+ [Error: RoleTrustRelationshipInvalid](#misconfig-trust)
+ [Error: S3AccessDenied](#troubleshoot-logging-errors-s3accessdenied)
+ [Notification: AvailabilityMonitorTest](#troubleshoot-availabilitymonitortest-notification)
+ [Notification: HardReboot](#troubleshoot-hardreboot-notification)
+ [Notification: HealthCheckFailure](#troubleshoot-healthcheckfailure-notification)
+ [Notification: Reboot](#troubleshoot-reboot-notification)
+ [Troubleshooting: Using CloudWatch metrics](#troubleshooting-with-cw-metrics)

## Error: 1344 \(0x00000540\)<a name="troubleshoot-copying-files-to-s3"></a>

While migrating files to Amazon S3 you may encounter an `ERROR 1344 (0x00000540)` if you are trying to copy files with more than 10 Access Control Entries \(ACEs\) into Amazon S3\. Access Control Entries are listed in the Access Control List \(ACL\)\.

 The Amazon S3 File Gateway can only preserve 10 ACE entries per given file or folder\.

**To resolve an Error 1344: Copying NTFS Security to Destination Directory\.** 

Reduce the number of entries in Windows Permissions for files or folders that contain more than 10 entries\. A common approach is to create a group containing the full list of entries, then replacing the list of entries with that single group\. Once the number of entries is less the 10, you can retry copying the files or folders to the gateway\.

## Error: GatewayClockOutOfSync<a name="troubleshoot-logging-errors-gatewayclockoutofsync"></a>

You can get a `GatewayClockOutOfSync` error when the gateway detects a difference of 5 minutes or more between the local system time and the time reported by the AWS Storage Gateway servers\. Clock synchronization issues can negatively impact SMB connections\.

**To resolve a GatewayClockOutOfSync error**
+ Check the network configuration between the gateway and the NTP server\. For more information about synchronizing the gateway VM time and updating the NTP server configuration, see [Configuring a Network Time Protocol \(NTP\) server for your gateway](https://docs.aws.amazon.com/filegateway/latest/files3/manage-on-premises-fgw.html#MaintenanceTimeSync-fgw)\.

## Error: InaccessibleStorageClass<a name="troubleshoot-logging-errors-inaccessiblestorageclass"></a>

You can get an `InaccessibleStorageClass` error when an object has moved out of the Amazon S3 Standard storage class\.

Here, usually your File Gateway encounters the error when it tries to either upload the specified object to S3 bucket or read the object from S3 bucket\. With this error, generally the object has moved to Amazon S3 Glacier and is in either the S3 Glacier or S3 Glacier Deep Archive storage class\.

**To resolve an InaccessibleStorageClass error**
+ Move the object from the S3 Glacier or S3 Glacier Deep Archive storage class back to S3\.

  If you move the object to the S3 bucket to fix an upload error, the file is eventually uploaded\. If you move the object to the S3 bucket to fix a read error, the File Gateway's SMB or NFS client can then read the file\.

## Error: InvalidObjectState<a name="troubleshoot-logging-errors-invalidobjectstate"></a>

You can get an `InvalidObjectState` error when a writer other than the specified File Gateway modifies the specified file in the specified S3 bucket\. As a result, the state of the file for the File Gateway doesn't match its state in Amazon S3\. Any subsequent uploads of the file to Amazon S3 or retrievals of the file from Amazon S3 fail\.

**To resolve an InvalidObjectState error**

If the operation that modifies the file is `S3Upload` or `S3GetObject`, do the following:

1. Save the latest copy of the file to the local file system of your SMB or NFS client \(you need this file copy in step 4\)\. If the version of the file in Amazon S3 is the latest, download that version\. You can do this using the AWS Management Console or AWS CLI\.

1. Delete the file in Amazon S3 using the AWS Management Console or AWS CLI\.

1. Delete the file from the File Gateway using your SMB or NFS client\.

1. Copy the latest version of the file that you saved in step 1 to Amazon S3 using your SMB or NFS client\. Do this through your File Gateway\.

## Error: ObjectMissing<a name="troubleshoot-logging-errors-objectmissing"></a>

You can get an `ObjectMissing` error when a writer other than the specified File Gateway deletes the specified file from the S3 bucket\. Any subsequent uploads to Amazon S3 or retrievals from Amazon S3 for the object fail\.

**To resolve an ObjectMissing error**

If the operation that modifies the file is `S3Upload` or `S3GetObject`, do the following:

1. Save the latest copy of the file to the local file system of your SMB or NFS client \(you need this file copy in step 3\)\.

1. Delete the file from the File Gateway using your SMB or NFS client\.

1. Copy the latest version of the file that you saved in step 1 using your SMB or NFS client\. Do this through your File Gateway\.

## Error: RoleTrustRelationshipInvalid<a name="misconfig-trust"></a>

You get this error when the IAM role for a file share has a misconfigured IAM trust relationship \(that is, the IAM role does not trust the Storage Gateway principal named `storagegateway.amazonaws.com`\)\. As a result, the File Gateway would not be able to get the credentials to run any operations on the S3 bucket that backs the file share\.

**To resolve an RoleTrustRelationshipInvalid error**
+ Use the IAM console or IAM API to include `storagegateway.amazonaws.com` as a principal that is trusted by your file share's IAMrole\. For information about IAM role, see [Tutorial: delegate access across AWS accounts using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)\.

## Error: S3AccessDenied<a name="troubleshoot-logging-errors-s3accessdenied"></a>

You can get an `S3AccessDenied` error for a file share's Amazon S3 bucket access AWS Identity and Access Management \(IAM\) role\. In this case, the S3 bucket access IAM role that is specified by `roleArn` in the error doesn't allow the operation involved\. The operation isn't allowed because of the permissions for the objects in the directory specified by the Amazon S3 prefix\.

**To resolve an S3AccessDenied error**
+ Modify the Amazon S3 access policy that is attached to `roleArn` in the File Gateway health log to allow permissions for the Amazon S3 operation\. Make sure that the access policy allows permission for the operation that caused the error\. Also, allow permission for the directory specified in the log for `prefix`\. For information about Amazon S3 permissions, see [Specifying permissions in a policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in *Amazon Simple Storage Service User Guide\.*

  These operations can cause an `S3AccessDenied` error to occur:
  + `S3HeadObject`
  + `S3GetObject`
  + `S3ListObjects`
  + `S3DeleteObject`
  + `S3PutObject`

## Notification: AvailabilityMonitorTest<a name="troubleshoot-availabilitymonitortest-notification"></a>

You get an `AvailabilityMonitorTest` notification when you [run a test](vmware-ha.md#vmware-ha-test-failover) of the [Availability and application monitoring](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html) system on gateways running on a VMware vSphere HA platform\.

## Notification: HardReboot<a name="troubleshoot-hardreboot-notification"></a>

You can get a `HardReboot` notification when the gateway VM is restarted unexpectedly\. Such a restart can be due to loss of power, a hardware failure, or another event\. For VMware gateways, a reset by vSphere High Availability Application Monitoring can trigger this event\.

When your gateway runs in such an environment, check for the presence of the `HealthCheckFailure` notification and consult the VMware events log for the VM\.

## Notification: HealthCheckFailure<a name="troubleshoot-healthcheckfailure-notification"></a>

For a gateway on VMware vSphere HA, you can get a `HealthCheckFailure` notification when a health check fails and a VM restart is requested\. This event also occurs during a test to monitor availability, indicated by an `AvailabilityMonitorTest` notification\. In this case, the `HealthCheckFailure` notification is expected\.

**Note**  
This notification is for VMware gateways only\.

If this event repeatedly occurs without an `AvailabilityMonitorTest` notification, check your VM infrastructure for issues \(storage, memory, and so on\)\. If you need additional assistance, contact AWS Support\.

## Notification: Reboot<a name="troubleshoot-reboot-notification"></a>

You can get a reboot notification when the gateway VM is restarted\. You can restart a gateway VM by using the VM Hypervisor Management console or the Storage Gateway console\. You can also restart by using the gateway software during the gateway's maintenance cycle\.

If the time of the reboot is within 10 minutes of the gateway's configured [maintenance start time](MaintenanceManagingUpdate-common.md), this reboot is probably a normal occurrence and not a sign of any problem\. If the reboot occurred significantly outside the maintenance window, check whether the gateway was restarted manually\.

## Troubleshooting: Using CloudWatch metrics<a name="troubleshooting-with-cw-metrics"></a>

You can find information following about actions to address issues using Amazon CloudWatch metrics with Storage Gateway\.

**Topics**
+ [Your gateway reacts slowly when browsing directories](#slow-gateway)
+ [Your gateway isn't responding](#gateway-not-responding)
+ [Your gateway is slow transferring data to Amazon S3](#slow-data-transfer-to-S3)
+ [Your gateway is performing more Amazon S3 operations than expected](#gateway-performing-more-s3-operations)
+ [You do not see files in your Amazon S3 bucket](#files-missing-s3-bucket)
+ [Your gateway backup job fails or there are errors when writing to your gateway](#backup-job-fails)

### Your gateway reacts slowly when browsing directories<a name="slow-gateway"></a>

If your File Gateway reacts slowly when you run the ls command or browse directories, check the `IndexFetch` and `IndexEviction` CloudWatch metrics:
+ If the `IndexFetch` metric is greater than 0 when you run an `ls` command or browse directories, your File Gateway started without information on the contents of the directory affected and had to access Amazon S3\. Subsequent efforts to list the contents of that directory should go faster\.
+ If the `IndexEviction` metric is greater than 0, it means that your File Gateway has reached the limit of what it can manage in its cache at that time\. In this case, your File Gateway has to free some storage space from the least recently accessed directory to list a new directory\. If this occurs frequently and there is a performance impact, contact AWS Support\. 

  Discuss with AWS Support the contents of the related S3 bucket and recommendations to improve performance based on your use case\.

### Your gateway isn't responding<a name="gateway-not-responding"></a>

If your File Gateway isn't responding, do the following:
+  If there was a recent reboot or software update, then check the `IOWaitPercent` metric\. This metric shows the percentage of time that the CPU is idle when there is an outstanding disk I/O request\. In some cases, this might be high \(10 or greater\) and might have risen after the server was rebooted or updated\. In these cases, then your File Gateway might be bottlenecked by a slow root disk as it rebuilds the index cache to RAM\. You can address this issue by using a faster physical disk for the root disk\.
+ If the `MemUsedBytes` metric is at or nearly the same as the `MemTotalBytes` metric, then your File Gateway is running out of available RAM\. Make sure that your File Gateway has at least the minimum required RAM\. If it already does, consider adding more RAM to your File Gateway based on your workload and use case\. 

  If the file share is SMB, the issue might also be due to the number of SMB clients connected to the file share\. To see the number of clients connected at any given time, check the `SMBV(1/2/3)Sessions` metric\. If there are many clients connected, you might need to add more RAM to your File Gateway\.

### Your gateway is slow transferring data to Amazon S3<a name="slow-data-transfer-to-S3"></a>

If your File Gateway is slow transferring data to Amazon S3, do the following:
+ If the `CachePercentDirty` metric is 80 or greater, your File Gateway is writing data faster to disk than it can upload the data to Amazon S3\. Consider increasing the bandwidth for upload from your File Gateway, adding one or more cache disks, or slowing down client writes\.
+ If the `CachePercentDirty` metric is low, check the `IoWaitPercent` metric\. If `IoWaitPercent` is greater than 10, your File Gateway might be bottlenecked by the speed of the local cache disk\. We recommend local solid state drive \(SSD\) disks for your cache, preferably NVM Express \(NVMe\)\. If such disks aren't available, try using multiple cache disks from separate physical disks for a performance improvement\.
+ If `S3PutObjectRequestTime`, `S3UploadPartRequestTime`, or `S3GetObjectRequestTime` are high, there might be a network bottleneck\. Try analyzing your network to verify that the gateway has the expected bandwidth\.

### Your gateway is performing more Amazon S3 operations than expected<a name="gateway-performing-more-s3-operations"></a>

If your File Gateway is performing more Amazon S3 operations than expected, check the `FilesRenamed` metric\. Rename operations are expensive to execute in Amazon S3\. Optimize your workflow to minimize the number of rename operations\.

### You do not see files in your Amazon S3 bucket<a name="files-missing-s3-bucket"></a>

If you notice that files on the gateway are not reflected in the Amazon S3 bucket, check the `FilesFailingUpload` metric\. If the metric reports that some files are failing upload, check your health notifications\. When files fail to upload, the gateway generates a health notification containing more details on the issue\.

### Your gateway backup job fails or there are errors when writing to your gateway<a name="backup-job-fails"></a>

If your File Gateway backup job fails or there are errors when writing to your File Gateway, do the following:
+ If the `CachePercentDirty` metric is 90 percent or greater, your File Gateway can't accept new writes to disk because there is not enough available space on the cache disk\. To see how fast your File Gateway is uploading to Amazon FSx or Amazon S3, view the `CloudBytesUploaded` metric\. Compare that metric with the `WriteBytes` metric, which shows how fast the client is writing files to your File Gateway\. If your File Gateway is writing faster than it can upload to Amazon FSx or Amazon S3, add more cache disks to cover the size of the backup job at a minimum\. Or, increase the upload bandwidth\.
+ If a backup job fails but the `CachePercentDirty` metric is less than 80 percent, your File Gateway might be hitting a client\-side session timeout\. For SMB, you can increase this timeout using the PowerShell command `Set-SmbClientConfiguration -SessionTimeout 300`\. Running this command sets the timeout to 300 seconds\.

  For NFS, make sure that the client is mounted using a hard mount instead of a soft mount\.