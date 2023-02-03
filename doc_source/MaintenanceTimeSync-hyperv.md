--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Synchronizing Your Gateway VM Time<a name="MaintenanceTimeSync-hyperv"></a>

For a gateway deployed on VMware ESXi, setting the hypervisor host time and synchronizing the VM time to the host is sufficient to avoid time drift\. For more information, see [Synchronizing VM Time with Host Time](configure-vmware.md#GettingStartedSyncVMTime-common)\. For a gateway deployed on Microsoft Hyper\-V, you should periodically check your VM's time using the procedure described following\. 

**To view and synchronize the time of a hypervisor gateway VM to a Network Time Protocol \(NTP\) server**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the local console for Linux Kernel\-based Virtuam Machine \(KVM\), see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. On the **Storage Gateway Configuration** main menu, enter **4** for **System Time Management**\.

      
![\[gateway local console main menu screen with configuration options listed.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/LocalConsoleLogin.png)

1. On the **System Time Management** menu, enter **1** for **View and Synchronize System Time**\.

      
![\[gateway local console system time management screen.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-timesync01.png)

1. If the result indicates that you should synchronize your VM's time to the NTP time, enter **y**\. Otherwise, enter **n**\.

   If you enter **y** to synchronize, the synchronization might take a few moments\.

   The following screen shot shows a VM that doesn't require time synchronization\.

      
![\[time synchronization test results showing acceptable disparity of less than one second.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-timesync02.png)

   The following screen shot shows a VM that does require time synchronization\.

      
![\[time synchronization test results showing unacceptable disparity of more than sixty seconds.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/hyperv-timesync03.png)