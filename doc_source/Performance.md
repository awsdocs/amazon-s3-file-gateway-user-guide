--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Performance<a name="Performance"></a>

This section describes Storage Gateway performance\.

**Topics**
+ [Basic performance guidance for S3 File Gateway](performance-fgw.md)
+ [Performance guidance for gateways with multiple file shares](#performance-multiple-file-shares)
+ [Optimizing Gateway Performance](Optimizing-common.md)
+ [Using VMware vSphere High Availability with Storage Gateway](vmware-ha.md)

## Performance guidance for gateways with multiple file shares<a name="performance-multiple-file-shares"></a>

Amazon S3 File Gateway supports attaching up to 50 file shares to a single Storage Gateway appliance\. By adding multiple file shares per gateway, you can support more users and workloads while managing fewer gateways and virtual hardware resources\. In addition to other factors, the number of file shares managed by a gateway can affect that gateway's performance\. This section describes how gateway performance is expected to change depending on the number of attached file shares and recommends virtual hardware configurations to optimize performance for gateways that manage multiple shares\.

In general, increasing the number of file shares managed by a single Storage Gateway can have the following consequences:
+ Increased time required to restart the gateway\.
+ Increased utilization of virtual hardware resources such as vCPU and RAM\.
+ Decreased performance for data and metadata operations if virtual hardware resources become saturated\.

The following table lists recommended virtual hardware configurations for gateways that manage multiple file shares:


| File Shares Per Gateway | Recommended Gateway Capacity Setting | Recommended vCPU Cores | Recommended RAM | Recommended Disk Size | 
| --- | --- | --- | --- | --- | 
|  1\-10  | Small |  4 \(EC2 instance type **m4\.xlarge** or greater\)  |  16 GiB  |  80 GiB  | 
|  10\-20  | Medium |  8 \(EC2 instance type **m4\.2xlarge** or greater\)  |  32 GiB  |  160 GiB  | 
|  20\+  | Large |  16 \(EC2 instance type **m4\.xlarge** or greater\)  |  64 GiB  |  240 GiB  | 

**Note**  
**Gateway Capacity** is a configuration parameter that specifies the size of your gateway’s metadata cache\. By default, **Gateway Capacity** is set to *Small* for all new gateways\. To enable your gateway to support more than 10 file shares, you must specify the recommended **Gateway Capacity** using the `update-gateway-information` command from the AWS CLI\. For more information, see [UpdateGatewayInformation](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewayInformation.html) in the *Storage Gateway API Reference*\.

In addition to the virtual hardware configurations recommended above, we recommend the following best practices for configuring and maintaining Storage Gateway appliances that manage multiple file shares:
+ Consider that the relationship between the number of file shares and the demand placed on the gateway's virtual hardware is not necessarily linear\. Some file shares might generate more throughput, and therefore more hardware demand than others\. The recommendations in the preceding table are based on maximum hardware capacities and various file share throughput levels\.
+ If you find that adding multiple file shares to a single gateway reduces performance, consider moving the most active file shares to other gateways\. In particular, if a file share is used for a very\-high\-throughput application, consider creating a separate gateway for that file share\.
+ We do not recommend configuring one gateway for multiple high\-throughput applications and another for multiple low\-throughput applications\. Instead, try to spread high and low throughput file shares evenly across gateways to balance hardware saturation\. To measure your file share throughput, use the `ReadBytes` and `WriteBytes` metrics\. For more information, see [Understanding file share metrics](https://docs.aws.amazon.com/filegateway/latest/files3/monitoring-file-gateway.html#monitoring-file-gateway-resources)\.