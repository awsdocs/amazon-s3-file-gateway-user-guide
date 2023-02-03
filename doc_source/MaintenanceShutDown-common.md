--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Shutting down your gateway VM<a name="MaintenanceShutDown-common"></a>

You might need to shutdown or reboot your VM for maintenance, such as when applying a patch to your hypervisor\. You shut down on\-premises gateway VMs using your hypervisor interface, and Amazon EC2 instances using the Amazon EC2 console\. When you power on your VM, remember to restart the gateway that runs on the VM\.

**Important**  
If you stop and start an Amazon EC2 gateway that uses ephemeral storage, the gateway will be permanently offline\. This happens because the physical storage disk is replaced\. There is no work\-around for this issue\. The only resolution is to delete the gateway and activate a new one on a new EC2 instance\.