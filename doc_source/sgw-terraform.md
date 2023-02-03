--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# AWS Storage Gateway Terraform module<a name="sgw-terraform"></a>

[HashiCorp](https://www.hashicorp.com/)[Terraform](https://www.terraform.io/) is a widely used open\-source Infrastructure as Code \(IaC\) engine developed using the HashiCorp Configuration Language \(HCL\)\. Terraform provides a consistent command line interface \(CLI\) workflow that, in conjunction with Amazon S3 File Gateway for the back\-end infrastructure, can manage hundreds of cloud services and codify cloud APIs into declarative configuration files\.

You can use Terraform to safely deploy an Amazon S3 File Gateway as a virtual machine \(VM\) in your on\-premises virtual infrastructure\. Terraform provides automation for on\-premises virtual infrastructure\. See [ Automate Amazon S3 File Gateway deployments in VMware with Terraform by HashiCorp](http://aws.amazon.com/https://aws.amazon.com/blogs/storage/automate-amazon-s3-file-gateway-deployments-in-vmware-with-terraform-by-hashicorp/) for information about quickly deploying an Amazon S3 File Gateway using Terraform within an on\-premises VMware virtual environment\. 

This automation provides you with a customizable Terraform module that you can use to provision an Amazon S3 File Gateway with all of the resources and dependencies needed to fully deploy the gateway and file shares in your VM environment\. The Terraform module provisions the gateway VM, activates the gateway, configures the cache disk, joins the gateway to a domain, creates the Amazon S3 buckets, creates the file shares, and maps them to buckets\. For a complete example of a repository that contains Terraform code to create the resources required to run Amazon S3 File Gateway on premises, see the [Terraform Storage Gateway module](https://github.com/aws-ia/terraform-aws-storagegateway) source code on GitHub\. 

**Note**  
The Amazon S3 File Gateway module for Terraform is a community supported effort\. It is not part of an AWS service\. Best\-effort support is provided by the AWS Storage community\.