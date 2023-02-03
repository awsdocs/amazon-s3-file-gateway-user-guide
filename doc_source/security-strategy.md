--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Setting a security level for your gateway<a name="security-strategy"></a>

By using a S3 File Gateway, you can specify a security level for your gateway\. By specifying this security level, you can set whether your gateway should require Server Message Block \(SMB\) signing or SMB encryption, or whether you want to enable SMB version 1\.

**To configure security level**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, then choose the gateway for which you want to edit SMB settings\.

1. From the **Actions** dropdown menu, choose **Edit SMB settings**, then choose **SMB security settings**\.

1. For **Security level**, choose one of the following:
**Note**  
This setting is called `SMBSecurityStrategy` in the API Reference\.  
A higher security level can affect performance\.
   + **Enforce encryption** – If you choose this option, S3 File Gateway only allows connections from SMBv3 clients that have encryption enabled\. This option is highly recommended for environments that handle sensitive data\. This option works with SMB clients on Microsoft Windows 8, Windows Server 2012, or later\. 
   + **Enforce signing** – If you choose this option, S3 File Gateway only allows connections from SMBv2 or SMBv3 clients that have signing enabled\. This option works with SMB clients on Microsoft Windows Vista, Windows Server 2008, or later\. 
   + **Client negotiated** – If you choose this option, requests are established based on what is negotiated by the client\. This option is recommended when you want to maximize compatibility across different clients in your environment\.
**Note**  
For gateways activated before June 20, 2019, the default security level is **Client negotiated**\.  
For gateways activated on June 20, 2019 and later, the default security level is **Enforce encryption**\.

1. Choose **Save**\.