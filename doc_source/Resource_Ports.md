--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Port Requirements<a name="Resource_Ports"></a>

Storage Gateway requires the following ports for its operation\. Some ports are common to all gateway types and are required by all gateway types\. Other ports are required by specific gateway types\. In this section, you can find an illustration of the required ports and a list of the ports required by each gateway type\.

**File Gateways**

The following illustration shows the ports to open for File Gateways' operation\.

![\[network resources connected to Storage Gateway using various ports.\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/File-Gateway-Port-Diagram.png)



The following ports are common to all gateway types and are required by all gateway types\.


|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  Storage Gateway VM  |  Amazon Web Services  |  Transmission Control Protocol \(TCP\)  |  443 \(HTTPS\)  |  For communication from an Storage Gateway VM to an AWS service endpoint\. For information about service endpoints, see [Allowing AWS Storage Gateway access through firewalls and routers](Requirements.md#allow-firewall-gateway-access)\.  | 
|  Your web browser  |  Storage Gateway VM  |  TCP  |  80 \(HTTP\)  |  By local systems to obtain the Storage Gateway activation key\. Port 80 is used only during activation of a Storage Gateway appliance\.  A Storage Gateway VM doesn't require port 80 to be publicly accessible\. The required level of access to port 80 depends on your network configuration\. If you activate your gateway from the Storage Gateway Management Console, the host from which you connect to the console must have access to your gateway’s port 80\.  | 
|  Storage Gateway VM  |  Domain Name Service \(DNS\) server  |  User Datagram Protocol \(UDP\)/UDP   |  53 \(DNS\)  |  For communication between a Storage Gateway VM and the DNS server\.  | 
|  Storage Gateway VM  |  Amazon Web Services  |  TCP  |  22 \(Support channel\)  |  Allows Amazon Web Services Support to access your gateway to help you with troubleshooting gateway issues\. You don't need this port open for the normal operation of your gateway, but it is required for troubleshooting\.  | 
|  Storage Gateway VM  |  Network Time Protocol \(NTP\) server  |  UDP  |  123 \(NTP\)  |  Used by local systems to synchronize VM time to the host time\. A Storage Gateway VM is configured to use the following NTP servers: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/Resource_Ports.html)  | 
|  AWS Storage Gateway Hardware Appliance  |  Hypertext Transfer Protocol \(HTTP\) proxy  |  TCP  |  8080 \(HTTP\)  | Required briefly for activation\. | 

The following table lists the required ports that must be opened for a File Gateway using either the Network File System \(NFS\) or Server Message Block \(SMB\) protocol\. These port rules are part of your security group definition\.


| Rule | Network Element | File Share Type | Protocol | Port | Inbound | Outbound | Required? | Notes | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | File share client | NFS | TCP/UDP Data | 111 | ✓ | ✓ | ✓ | File sharing data transfer \(for NFS only\) | 
|  |  |  | TCP/UDP NFS | 2049 | ✓ | ✓ | ✓ | File sharing data transfer \(for NFS only\) | 
|  |  |  | TCP/UDP NFSv3 | 20048 | ✓ | ✓ | ✓ | File sharing data transfer \(for NFS only\) | 
|  |  | SMB | TCP/UDP SMBv2 | 139 | ✓ | ✓ | ✓ | File sharing data transfer session service \(for SMB only\); replaces ports 137–139 for Microsoft Windows NT and later | 
|  |  |  | TCP/UDP SMBv3 | 445 | ✓ | ✓ | ✓ | File sharing data transfer session service \(for SMB only\); replaces ports 137–139 for Microsoft Windows NT and later | 
| 2 | Web browser | NFS and SMB | TCP HTTP | 80 | ✓ | ✓ | ✓ | Amazon Web Services Management Console \(activation only\) | 
|  |  |  | TCP HTTPS | 443 | ✓ | ✓ | ✓ | Amazon Web Services Management Console \(all other operations\) | 
| 3 | DNS | NFS and SMB | TCP/UDP DNS | 53 | ✓ | ✓ | ✓ | IP name resolution | 
| 4 | NTP | NFS and SMB | UDP NTP | 123 | ✓ | ✓ | ✓ | Time synchronization service | 
| 5 | Microsoft Active Directory | SMB | UDP NetBIOS | 137 | ✓ | ✓ | ✓ | Name service \(not used for NFS\) | 
|  |  |  | UDP NetBIOS | 138 | ✓ | ✓ | ✓ | Datagram service | 
|  |  |  | TCP/UDP LDAP | 389 | ✓ | ✓ |  | Microsoft Active Directory \- Directory System Agent \(DSA\); client connection | 
|  |  |  | TCP/UDP Kerberos | 88 | ✓ | ✓ |  |  | 
|  |  |  | TCP Distributed Computing Environment/End Point Mapper \(DCE/EMAP\) | 135 | ✓ | ✓ |  |  | 
| 6 | Amazon S3 | NFS and SMB | HTTPS data | 443 | ✓ | ✓ | ✓ | Storage data transfer | 
| 7 | Storage Gateway | NFS and SMB | TCP SSH | 22 | ✓ | ✓ | ✓ | Support channel | 
|  |  |  | TCP HTTPS | 443 | ✓ | ✓ | ✓ | Management control | 
| 8 | Amazon CloudFront | NFS and SMB | TCP HTTPS | 443 | ✓ | ✓ | ✓ | For activation | 