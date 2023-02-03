--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# NFS CSI drivers<a name="use-nfs-csi"></a>

Follow the procedures in this section to install and configure the CSI drivers that are required to use an NFS file share on an Amazon S3 File Gateway for storage in your Kubernetes cluster\. For more information, see the open\-source NFS CSI driver documentation on GitHub at [https://github.com/kubernetes-csi/csi-driver-nfs/blob/master/docs/install-csi-driver-master.md](https://github.com/kubernetes-csi/csi-driver-nfs/blob/master/docs/install-csi-driver-master.md)\.

## Install drivers<a name="install-nfs-csi"></a>

**To install Kubernetes NFS CSI drivers:**

1. From a command\-line terminal with access to `kubectl` for your Kubernetes cluster, run the following command:

   curl \-skSL https://raw\.githubusercontent\.com/kubernetes\-csi/csi\-driver\-nfs/master/deploy/install\-driver\.sh \| bash \-s master \-\-

1. Wait for the previous command to finish, then use the following commands to ensure that the CSI driver pods are running:

   kubectl \-n kube\-system get pod \-o wide \-l app=csi\-nfs\-controller

   kubectl \-n kube\-system get pod \-o wide \-l app=csi\-nfs\-node

   The output should look similar to the following:

   ```
   NAME                                       READY   STATUS    RESTARTS   AGE     IP             NODE
   csi-nfs-controller-56bfddd689-dh5tk       4/4     Running   0          35s     10.240.0.19    k8s-agentpool-22533604-0
   csi-nfs-controller-56bfddd689-8pgr4       4/4     Running   0          35s     10.240.0.35    k8s-agentpool-22533604-1
   csi-nfs-node-cvgbs                        3/3     Running   0          35s     10.240.0.35    k8s-agentpool-22533604-1
   csi-nfs-node-dr4s4                        3/3     Running   0          35s     10.240.0.4     k8s-agentpool-22533604-0
   ```

## Create an NFS StorageClass object<a name="create-storageclass-nfs-csi"></a>

**To create an NFS StorageClass object for your Kubernetes cluster:**

1. Create a configuration file named `storageclass.yaml` with contents that are similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   ---
   apiVersion: storage.k8s.io/v1 
   kind: StorageClass 
   metadata:
       name: example-nfs-classname 
       namespace: example-namespace 
   provisioner: nfs.csi.k8s.io 
   parameters: 
       server: gateway-dns-name-or-ip-address 
       share: /example-share-name 
   reclaimPolicy: Retain 
   volumeBindingMode: Immediate
   mountOptions: 
       - hard 
       - nfsvers=4.1
   ```

1. From a command\-line terminal with access to `kubectl` and `storageclass.yaml`, run the following command:

   kubectl apply \-f storageclass\.yaml
**Note**  
You can also create the StorageClass by providing the `.yaml` configuration text from the previous step to most third\-party Kubernetes management and containerization platforms\.

1. Configure the pods in your Kubernetes cluster to use the new StorageClass object that you created\. For more information, refer to the Kubernetes online documentation at [https://kubernetes.io/docs/concepts/storage/](https://kubernetes.io/docs/concepts/storage/)\.

## Create NFS PersistentVolume and PersistentVolumeClaim objects<a name="create-persistentvolume-volumeclaim-nfs-csi"></a>

**To create new NFS PersistentVolume and PersistentVolumeClaim objects:**

1. Create two configuration files named `persistentvolume.yaml` and `persistentvolumeclaim.yaml`\.

1. For `persistentvolume.yaml`, add contents that are similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   --- 
   apiVersion: v1 
   kind: PersistentVolume 
   metadata: 
       name: pv-nfs-examplename 
   spec: 
       capacity:
           storage: 10Gi 
       accessModes: 
           - ReadWriteMany
       persistentVolumeReclaimPolicy: Retain
       mountOptions: 
           - hard 
           - nolock 
           - nfsvers=4.1 
       csi: 
           driver: nfs.csi.k8s.io
           readOnly: false 
           volumeHandle: unique-volumeid-example # make sure it's a unique id in the cluster 
           volumeAttributes: 
               server: gateway-dns-name-or-ip-address 
               share: /example-share-name
   ```

1. For `persistentvolumeclaim.yaml`, add contents that are similar to the following example\. Substitute your own deployment\-specific information for the *ExampleValues* shown\.

   ```
   ---
   kind: PersistentVolumeClaim 
   apiVersion: v1 
   metadata: 
       name: examplename-pvc-nfs-static 
   spec:
       accessModes: 
           - ReadWriteMany 
       resources: 
           requests: 
               storage: 10Gi
       volumeName: pv-nfs-examplename # make sure specfied volumeName matches the name of the PersistentVolume you created
       storageClassName: ""
   ```

1. From a command\-line terminal with access to `kubectl` and both `.yaml` files, run the following commands:

   kubectl apply \-f persistentvolume\.yaml

   kubectl apply \-f persistentvolumeclaim\.yaml
**Note**  
You can also create the PersistentVolume and PersistentVolumeClaim objects by providing the `.yaml` configuration text from the previous step to most third\-party Kubernetes management and containerization platforms\.

1. Configure the pods in your Kubernetes cluster to use the new PersistentVolumeClaim object that you created\. For more information, refer to the Kubernetes online documentation at [https://kubernetes.io/docs/concepts/storage/](https://kubernetes.io/docs/concepts/storage/)\.

## Uninstall drivers<a name="uninstall-nfs-csi"></a>

**To uninstall Kubernetes NFS CSI drivers:**
+ From a command\-line terminal with access to `kubectl` for your Kubernetes cluster, run the following command:

  curl \-skSL https://raw\.githubusercontent\.com/kubernetes\-csi/csi\-driver\-nfs/master/deploy/uninstall\-driver\.sh \| bash \-s master \-\-