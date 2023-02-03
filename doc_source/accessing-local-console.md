--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Accessing the Gateway Local Console<a name="accessing-local-console"></a>

How you access your VM's local console depends on the type of the Hypervisor you deployed your gateway VM on\. In this section, you can find information on how to access the VM local console using Linux Kernel\-based Virtual Machine \(KVM\), VMware ESXi, and Microsoft Hyper\-V Manager\.

**Topics**
+ [Accessing the Gateway Local Console with Linux KVM](#MaintenanceConsoleWindowKVM-common)
+ [Accessing the Gateway Local Console with VMware ESXi](#MaintenanceConsoleWindowVMware-common)
+ [Access the Gateway Local Console with Microsoft Hyper\-V](#MaintenanceConsoleWindowHyperV-common)

## Accessing the Gateway Local Console with Linux KVM<a name="MaintenanceConsoleWindowKVM-common"></a>

There are different ways to configure virtual machines running on KVM, depending on the Linux distribution being used\. Instructions for accessing KVM configuration options from the command line follow\. Instructions might differ depending on your KVM implementation\.

**To access your gateway's local console with KVM**

1. Use the following command to list the VMs that are currently available in KVM\. 

   ```
   # virsh list
   ```

   You can choose available VMs by `Id`\.  
![\[Linux terminal showing virsh list results with VM ID, name, and state info.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_52.png)

1. Use the following command to access the local console\.

   ```
   # virsh console VM_Id
   ```  
![\[Linux terminal showing virsh console command and AWS appliance login prompt.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_51.png)

1. To get default credentials to log in to the local console, see [Logging in to the File Gateway local console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.

1. After you have logged in, you can activate and configure your gateway\.  
![\[gateway local console main menu displayed in a Linux terminal.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_50.png)

## Accessing the Gateway Local Console with VMware ESXi<a name="MaintenanceConsoleWindowVMware-common"></a>



**To access your gateway's local console with VMware ESXi**

1. In the VMware vSphere client, select your gateway VM\.

1. Make sure that the gateway is turned on\.
**Note**  
If your gateway VM is turned on, a green arrow icon appears with the VM icon, as shown in the following screen shot\. If your gateway VM is not turned on, you can turn it on by choosing the green **Power On** icon on the **Toolbar** menu\.

      
![\[VMware vSphere Inventory screen showing Storage Gateway VM with green power on icon.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_65.png)

1. Choose the **Console** tab\.

      
![\[VMware vSphere Inventory screen showing Storage Gateway VM selected and console tab highlighted.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_70.png)

   After a few moments, the VM is ready for you to log in\.
**Note**  
To release the cursor from the console window, press **Ctrl\+Alt**\.

      
![\[Storage Gateway local console login prompt displayed in a terminal screen.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_75.png)

1. To log in using the default credentials, continue to the procedure [Logging in to the File Gateway local console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.

## Access the Gateway Local Console with Microsoft Hyper\-V<a name="MaintenanceConsoleWindowHyperV-common"></a>



**To access your gateway's local console \(Microsoft Hyper\-V\)**

1. In the **Virtual Machines** list of the Microsoft Hyper\-V Manager, select your gateway VM\.

1. Make sure that the gateway is turned on\.

    
**Note**  
If your gateway VM is turned on, `Running` is displayed as the **State** of the VM, as shown in the following screen shot\. If your gateway VM is not turned on, you can turn it on by choosing **Start** in the **Actions** pane\.  
![\[Microsoft Hyper-V virtual machines screen showing Storage Gateway VM in running state.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-manager09.png)

1. In the **Actions** pane, choose **Connect**\.

   The **Virtual Machine Connection** window appears\. If an authentication window appears, type the sign\-in credentials provided to you by the hypervisor administrator\.

      
![\[Microsoft Hyper-V virtual machine connection screen connecting to Storage Gateway VM.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-vm-connect01.png)

    After a few moments, the VM is ready for you to log in\.

      
![\[Storage Gateway local console login prompt displayed in a terminal screen.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GatewayMaintenance_75.png)

1. To log in using the default credentials, continue to the procedure [Logging in to the File Gateway local console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.