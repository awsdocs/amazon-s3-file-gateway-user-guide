--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# SMB CSI drivers<a name="use-smb-csi"></a>

Follow the procedures in this section to install and configure the CSI drivers that are required to use an SMB file share on an Amazon S3 File Gateway for storage in your Kubernetes cluster\. For more information, see the open\-source SMB CSI driver documentation on GitHub at [https://github.com/kubernetes-csi/csi-driver-smb/blob/master/docs/install-csi-driver-master.md](https://github.com/kubernetes-csi/csi-driver-smb/blob/master/docs/install-csi-driver-master.md)\.

**Note**  
When you create a `PersistentVolume` object or a `StorageClass` object, you can specify a `ReclaimPolicy` parameter to determine what happens to the external storage when objects are deleted\. The SMB CSI driver supports the `Retain` and `Recycle` options, but does not currently support a `Delete` option\.

## Install drivers<a name="install-smb-csi"></a>

**To install Kubernetes SMB CSI drivers:**

1. From a command\-line terminal with access to `kubectl` for your Kubernetes cluster, run the following command:

   curl \-skSL https://raw\.githubusercontent\.com/kubernetes\-csi/csi\-driver\-smb/master/deploy/install\-driver\.sh \| bash \-s master \-\-

1. Wait for the previous command to finish, then use the following commands to ensure that the CSI driver pods are running:

   kubectl \-n kube\-system get pod \-o wide \-\-watch \-l app=csi\-smb\-controller

   kubectl \-n kube\-system get pod \-o wide \-\-watch \-l app=csi\-smb\-node

   The output should look similar to the following:

   ```
   NAME                                       READY   STATUS    RESTARTS   AGE     IP             NODE
   csi-smb-controller-56bfddd689-dh5tk       4/4     Running   0          35s     10.240.0.19    k8s-agentpool-22533604-0
   csi-smb-controller-56bfddd689-8pgr4       4/4     Running   0          35s     10.240.0.35    k8s-agentpool-22533604-1
   csi-smb-node-cvgbs                        3/3     Running   0          35s     10.240.0.35    k8s-agentpool-22533604-1
   csi-smb-node-dr4s4                        3/3     Running   0          35s     10.240.0.4     k8s-agentpool-22533604-0
   ```

## Create an SMB StorageClass object<a name="create-storageclass-smb-csi"></a>

**To create a new SMB StorageClass object for your Kubernetes cluster:**

1. Create a configuration file named `storageclass.yaml` with contents similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   ---
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
       name: ExampleStorageClassName
   provisioner: smb.csi.k8s.io
   parameters:
       source: "//gateway-dns-name-or-ip-address/example-share-name"
       # if csi.storage.k8s.io/provisioner-secret is provided, will create a sub directory
       # with PV name under source
       csi.storage.k8s.io/provisioner-secret-name: "examplesmbcreds"
       csi.storage.k8s.io/provisioner-secret-namespace: "examplenamespace"
       csi.storage.k8s.io/node-stage-secret-name: "examplesmbcreds"
       csi.storage.k8s.io/node-stage-secret-namespace: "examplenamespace"
   volumeBindingMode: Immediate
   reclaimPolicy: Retain
   mountOptions:
       - dir_mode=0777
       - file_mode=0777
       - uid=1001
       - gid=1001
   ```

1. From a command\-line terminal with access to `kubectl` and `storageclass.yaml`, run the following command:

   kubectl apply \-f storageclass\.yaml
**Note**  
You can also create the StorageClass by providing the `.yaml` configuration text from the previous step to most third\-party Kubernetes management and containerization platforms\.

1. Configure the pods in your Kubernetes cluster to use the new StorageClass that you created\. For more information, refer to the Kubernetes online documentation at [https://kubernetes.io/docs/concepts/storage/](https://kubernetes.io/docs/concepts/storage/)\.

## Create SMB PersistentVolume and PersistentVolumeClaim objects<a name="create-persistentvolume-volumeclaim-smb-csi"></a>

**To create new SMB PersistentVolume and PersistentVolumeClaim objects:**

1. Create two configuration files\. One named `persistentvolume.yaml`, and one named `persistentvolumeclaim.yaml`\.

1. For `persistentvolume.yaml`, add contents that are similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   ---
   apiVersion: v1
   kind: PersistentVolume
   metadata:
       name: pv-smb-example-name
       namespace: smb-example-namespace # PersistentVolume and PersistentVolumeClaim must use the same namespace parameter
   spec:
       capacity:
           storage: 100Gi
       accessModes:
           - ReadWriteMany
       persistentVolumeReclaimPolicy: Retain
       mountOptions:
           - dir_mode=0777
           - file_mode=0777
           - vers=3.0
       csi:
           driver: smb.csi.k8s.io
           readOnly: false
           volumeHandle: examplehandle  # make sure it's a unique id in the cluster
           volumeAttributes:
               source: "//gateway-dns-name-or-ip-address/example-share-name"
           nodeStageSecretRef:
               name: example-smbcreds
               namespace: smb-example-namespace
   ```

1. For `persistentvolumeclaim.yaml`, add contents that are similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1 
   metadata: 
       name: examplename-pvc-smb-static
       namespace: smb-example-namespace # PersistentVolume and PersistentVolumeClaim must use the same namespace parameter
   spec: 
       accessModes: 
           - ReadWriteMany 
       resources: 
           requests: 
               storage: 10Gi 
           volumeName: pv-smb-example-name # make sure specfied volumeName matches the name of the PersistentVolume you created
           storageClassName: ""
   ```

1. From a command\-line terminal with access to `kubectl` and both of the `.yaml` files that you created, run the following commands:

   kubectl apply \-f persistentvolume\.yaml

   kubectl apply \-f persistentvolumeclaim\.yaml
**Note**  
You can also create the PersistentVolume and PersistentVolumeClaim objects by providing the `.yaml` configuration text from the previous step to most third\-party Kubernetes management and containerization platforms\.

1. Configure the pods in your Kubernetes cluster to use the new PersistentVolumeClaim that you created\. For more information, refer to the Kubernetes online documentation at [https://kubernetes.io/docs/concepts/storage/](https://kubernetes.io/docs/concepts/storage/)\.

## Uninstall drivers<a name="uninstall-smb-csi"></a>

**To uninstall the Kubernetes SMB CSI drivers:**
+ From a command\-line terminal with access to `kubectl` for your Kubernetes cluster, run the following command:

  curl \-skSL https://raw\.githubusercontent\.com/kubernetes\-csi/csi\-driver\-smb/master/deploy/uninstall\-driver\.sh \| bash \-s \-\-