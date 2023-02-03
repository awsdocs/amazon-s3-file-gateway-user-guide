--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Best practices: preserve security information while copying files to your gateway on Microsoft Windows Server<a name="best-practices-copying-files-on-windows"></a>

It is possible to copy files to your File Gateway using the basic `copy` command on Microsoft Windows, but this command copies only the file data by default \- omitting certain file attributes such as security descriptors\. If the files are copied to the gateway without the corresponding security restrictions and Discretionary Access Control List \(DACL\) information, it is possible that they could be accessed by unauthorized users\.

As a best practice for preserving all file attributes and security information when copying files to your gateway on Microsoft Windows Server, we recommend using the `robocopy` or `xcopy` commands, with the `/copy:DS` or `/o` flags, respectively\. For more information, see [robocopy](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy) and [xcopy](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/xcopy) in the Microsoft Windows Server command reference documentation\.