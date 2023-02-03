--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Using Kubernetes Container Storage Interface \(CSI\) drivers<a name="using-csi-drivers"></a>

Kubernetes is an open\-source system for automating deployment, scaling, and management of containerized applications\. If you have a Kubernetes cluster, you can install and configure Kubernetes Container Storage Interface \(CSI\) drivers across the instances in your cluster to allow them to use an existing Amazon S3 File Gateway for storage\.

After you install the CSI drivers for the type of file share that you want to use, you must create an object or objects\. Depending on the type of provisioning that you want Kubernetes to use when your pods request storage, create either a single Kubernetes `StorageClass` object, or both a `PersistentVolume` object *and* a `PersistentVolumeClaim` object to connect your Kubernetes compute pods to your file share\.\. For more information, refer to the Kubernetes online documentation at [https://kubernetes.io/docs/concepts/storage/](https://kubernetes.io/docs/concepts/storage/)\.

**Topics**
+ [SMB CSI drivers](use-smb-csi.md)
+ [NFS CSI drivers](use-nfs-csi.md)