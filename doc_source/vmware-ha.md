--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Using VMware vSphere High Availability with Storage Gateway<a name="vmware-ha"></a>

Storage Gateway provides high availability on VMware through a set of application\-level health checks integrated with VMware vSphere High Availability \(VMware HA\)\. This approach helps protect storage workloads against hardware, hypervisor, or network failures\. It also helps protect against software errors, such as connection timeouts and file share or volume unavailability\.

With this integration, a gateway deployed in a VMware environment on\-premises or in a VMware Cloud on AWS automatically recovers from most service interruptions\. It generally does this in under 60 seconds with no data loss\. 

To use VMware HA with Storage Gateway, take the steps listed following\.

**Topics**
+ [Configure Your vSphere VMware HA Cluster](#vmware-ha-configure-cluster)
+ [Set Up Your Gateway Type](#vmware-ha-download-image)
+ [Deploy the Gateway](#vmware-ha-deploy-gateway)
+ [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)
+ [Activate Your Gateway](#vmware-ha-activate-gateway)
+ [Test Your VMware High Availability Configuration](#vmware-ha-test-failover)

## Configure Your vSphere VMware HA Cluster<a name="vmware-ha-configure-cluster"></a>

First, if you haven’t already created a VMware cluster, create one\. For information about how to create a VMware cluster, see [Create a vSphere HA Cluster](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.avail.doc/GUID-4BC60283-B638-472F-B1D2-1E4E57EAD213.html) in the VMware documentation\.

Next, configure your VMware cluster to work with Storage Gateway\.

**To configure your VMware cluster**

1. On the **Edit Cluster Settings** page in VMware vSphere, make sure that VM monitoring is configured for VM and application monitoring\. To do so, set the following options as listed:
   + **Host Failure Response**: **Restart VMs**
   + **Response for Host Isolation**: **Shut down and restart VMs**
   + **Datastore with PDL**: **Disabled**
   + **Datastore with APD**: **Disabled**
   + **VM Monitoring**: **VM and Application Monitoring**

   For an example, see the following screen shot\.   
![\[VMware vSphere Edit Cluster Settings screen with VM and application monitoring options configured.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/edit-cluster-settings.png)

1. Fine\-tune the sensitivity of the cluster by adjusting the following values: 
   + **Failure interval** – After this interval, the VM is restarted if a VM heartbeat isn't received\. 
   + **Minimum uptime** – The cluster waits this long after a VM starts to begin monitoring for VM tools' heartbeats\.
   + **Maximum per\-VM resets** – The cluster restarts the VM a maximum of this many times within the maximum resets time window\.
   + **Maximum resets time window** – The window of time in which to count the maximum resets per\-VM resets\. 

   If you aren't sure what values to set, use these example settings: 
   + **Failure interval**: **30** seconds 
   + **Minimum uptime**: **120** seconds 
   + **Maximum per\-VM resets**: **3**
   + **Maximum resets time window**: **1** hour 

If you have other VMs running on the cluster, you might want to set these values specifically for your VM\. You can't do this until you deploy the VM from the \.ova\. For more information on setting these values, see [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)\.

## Set Up Your Gateway Type<a name="vmware-ha-download-image"></a>

Use the following procedure to set up the gateway

**To download the \.ova image for your gateway type**
+ Download the \.ova image for your gateway type from one of the following:
  + File Gateway – [Create and activate an Amazon S3 File Gateway](create-gateway-file.md)

## Deploy the Gateway<a name="vmware-ha-deploy-gateway"></a>

In your configured cluster, deploy the \.ova image to one of the cluster's hosts\.

**To deploy the gateway \.ova image**

1.  Deploy the \.ova image to one of the hosts in the cluster\.

1. Make sure the data stores that you choose for the root disk and the cache are available to all hosts in the cluster\.

## \(Optional\) Add Override Options for Other VMs on Your Cluster<a name="vmware-ha-overrides"></a>

If you have other VMs running on your cluster, you might want to set the cluster values specifically for each VM\. 

**To add override options for other VMs on your cluster**

1. On the **Summary** page in VMware vSphere, choose your cluster to open the cluster page, and then choose **Configure**\.

1. Choose the **Configuration** tab, and then choose **VM Overrides**\.

1. Add a new VM override option to change each value\. 

   For override options, see the following screen shot\.  
![\[VMware vSphere Add VM Override Cluster screen with override options configured.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/override-cluster-settings.png)

## Activate Your Gateway<a name="vmware-ha-activate-gateway"></a>

After the \.ova for your gateway is deployed, activate your gateway\. The instructions about how are different for each gateway type\.

**To activate your gateway**
+  Choose activation instructions based on your gateway type:
  + File Gateway – 

## Test Your VMware High Availability Configuration<a name="vmware-ha-test-failover"></a>

After you activate your gateway, test your configuration\.

**To test your VMware HA configuration**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and then choose the gateway that you want to test for VMware HA\.

1. For **Actions**, choose **Verify VMware HA**\.

1. In the **Verify VMware High Availability Configuration** box that appears, choose **OK**\.
**Note**  
Testing your VMware HA configuration reboots your gateway VM and interrupts connectivity to your gateway\. The test might take a few minutes to complete\. 

   If the test is successful, the status of **Verified** appears in the details tab of the gateway in the console\.

1. Choose **Exit**\.

You can find information about VMware HA events in the Amazon CloudWatch log groups\. For more information, see [Getting S3 File Gateway health logs with CloudWatch log groups](monitoring-file-gateway.md#cw-log-groups)\.