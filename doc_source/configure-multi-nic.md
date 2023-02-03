--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Configuring Network Adapters for Your Gateway<a name="configure-multi-nic"></a>

In this section you can find information about how configure multiple network adapters for your gateway\.

**Topics**
+ [Configuring Your Gateway for Multiple NICs in a VMware ESXi Host](#MaintenanceMultiNIC-vmaware)
+ [Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host](#MaintenanceMultiNIC-hyperv)

## Configuring Your Gateway for Multiple NICs in a VMware ESXi Host<a name="MaintenanceMultiNIC-vmaware"></a>

The following procedure assumes that your gateway VM already has one network adapter defined, and describes how to add an adapter on VMware ESXi\.

**To configure your gateway to use an additional network adapter in VMware ESXi host**

1. Shut down the gateway\.

1. In the VMware vSphere client, select your gateway VM\. 

   The VM can remain turned on for this procedure\.

1. In the client, open the context \(right\-click\) menu for your gateway VM, and choose **Edit Settings**\.

1. On the **Hardware** tab of the **Virtual Machine Properties** dialog box, choose **Add** to add a device\.

1. Follow the Add Hardware wizard to add a network adapter\.

   1. In the **Device Type** pane, choose **Ethernet Adapter** to add an adapter, and then choose **Next**\.

   1. In the **Network Type** pane, ensure that **Connect at power on** is selected for **Type**, and then choose **Next**\.

      We recommend that you use the VMXNET3 network adapter with Storage Gateway\. For more information on the adapter types that might appear in the adapter list, see Network Adapter Types in the [ESXi and vCenter Server Documentation](http://pubs.vmware.com/vsphere-50/index.jsp?topic=/com.vmware.vsphere.vm_admin.doc_50/GUID-AF9E24A8-2CFA-447B-AC83-35D563119667.html&resultof=%22VMXNET%22%20%22vmxnet%22)\.

   1. In the **Ready to Complete** pane, review the information, and then choose **Finish**\.

1. Choose the **Summary** tab for the VM, and choose **View All** next to the **IP Address** box\. The **Virtual Machine IP Addresses** window displays all the IP addresses you can use to access the gateway\. Confirm that a second IP address is listed for the gateway\.
**Note**  
It might take several moments for the adapter changes to take effect and the VM summary information to refresh\.

1. In the Storage Gateway console, turn on the gateway\.

1. In the **Navigation** pane of the Storage Gateway console, choose **Gateways** and choose the gateway to which you added the adapter\. Confirm that the second IP address is listed in the **Details** tab\.
**Note**  
The example mounting commands provided on the info page for a file share in the Storage Gateway console will always include the IP address of the network adapter that was most recently added to the file share's associated gateway\.

For information about local console tasks common to VMware, Hyper\-V, and KVM hosts, see [Performing tasks on the VM local console \(File Gateway\)](manage-on-premises-fgw.md)

## Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host<a name="MaintenanceMultiNIC-hyperv"></a>

The following procedure assumes that your gateway VM already has one network adapter defined and that you are adding a second adapter\. This procedure shows how to add an adapter for a Microsoft Hyper\-V host\.

**To configure your gateway to use an additional network adapter in a Microsoft Hyper\-V Host**

1. On the Storage Gateway console, turn off the gateway\.

1. In the Microsoft Hyper\-V Manager, select your gateway VM\.

1. If the VM isn't turned off already, open the context \(right\-click\) menu for your gateway and choose **Turn Off**\.

1. In the client, open the context menu for your gateway VM and choose **Settings**\.

      
![\[Microsoft Hyper-V virtual machines screen showing context menu settings for Storage Gateway VM.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-manager10.png)

1. In the **Settings** dialog box for the VM, for **Hardware**, choose **Add Hardware**\.

1. In the **Add Hardware** pane, choose **Network Adapter**, and then choose **Add** to add a device\. 

      
![\[Microsoft Hyper-V VM settings screen with Add Hardware and Network Adapter selected.\]](http://docs.aws.amazon.com/filegateway/latest/files3/../shared/images/hyperv-vm-settings15.png)

1. Configure the network adapter, and then choose **Apply** to apply settings\.

   In the following example, **Virtual Network 2** is selected for the new adapter\.

      
![\[Microsoft Hyper-V VM settings screen with new virtual NIC selected.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-vm-settings16.png)

1. In the **Settings** dialog box, for **Hardware**, confirm that the second adapter was added, and then choose **OK**\.

1. On the Storage Gateway console, turn on the gateway\.

1. In the **Navigation** pane choose **Gateways**, then select the gateway to which you added the adapter\. Confirm that the second IP address is listed in the **Details** tab\.
**Note**  
The example mounting commands provided on the info page for a file share in the Storage Gateway console will always include the IP address of the network adapter that was most recently added to the file share's associated gateway\.

For information about local console tasks common to VMware, Hyper\-V, and KVM hosts, see [Performing tasks on the VM local console \(File Gateway\)](manage-on-premises-fgw.md)