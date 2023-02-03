--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# How AWS Storage Gateway works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to AWS SGW, you should understand what IAM features are available to use with AWS SGW\. To get a high\-level view of how AWS SGW and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [AWS SGW Identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [AWS SGW resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Access control lists \(ACLs\)](#security_iam_service-with-iam-acls)
+ [Authorization based on AWS SGW tags](#security_iam_service-with-iam-tags)
+ [AWS SGW IAM roles](#security_iam_service-with-iam-roles)

## AWS SGW Identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. AWS SGW supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in AWS SGW use the following prefix before the action: `YOUR-SERVICE-PREFIX:`\. For example, to grant someone permission to run an Amazon EC2 instance with the Amazon EC2 `RunInstances` API operation, you include the `ec2:RunInstances` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS SGW defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ec2:action1",
      "ec2:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ec2:Describe*"
```



To see a list of AWS SGW actions, see [Actions Defined by AWS Storage Gateway ](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_storagegateway.html#storagegateway-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



The Amazon EC2 instance resource has the following ARN:

```
arn:${Partition}:ec2:${Region}:${Account}:instance/${InstanceId}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `i-1234567890abcdef0` instance in your statement, use the following ARN:

```
"Resource": "arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0"
```

To specify all instances that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:ec2:us-east-1:123456789012:instance/*"
```

Some AWS SGW actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Many Amazon EC2 API actions involve multiple resources\. For example, `AttachVolume` attaches an Amazon EBS volume to an instance, so an IAM user must have permissions to use the volume and the instance\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of AWS SGW resource types and their ARNs, see [Resources Defined by AWS Storage Gateway ](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_storagegateway.html#storagegateway-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS Storage Gateway ](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_storagegateway.html#storagegateway-actions-as-permissions)\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

AWS SGW defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



 All Amazon EC2 actions support the `aws:RequestedRegion` and `ec2:Region` condition keys\. For more information, see [Example: Restricting Access to a Specific Region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ExamplePolicies_EC2.html#iam-example-region)\. 

To see a list of AWS SGW condition keys, see [Condition Keys for AWS Storage Gateway ](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_storagegateway.html#storagegateway-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS Storage Gateway ](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_storagegateway.html#storagegateway-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of AWS SGW identity\-based policies, see [AWS Storage Gateway identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## AWS SGW resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on the AWS SGW resource and under what conditions\. Amazon S3 supports resource\-based permissions policies for Amazon S3 *buckets*\. Resource\-based policies let you grant usage permission to other accounts on a per\-resource basis\. You can also use a resource\-based policy to allow an AWS service to access your Amazon S3 *buckets*\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the [principal in a resource\-based policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, you must also grant the principal entity permission to access the resource\. Grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM Roles Differ from Resource\-based Policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

The Amazon S3 service supports only one type of resource\-based policy called a **bucket* policy*, which is attached to a *bucket*\. This policy defines which principal entities \(accounts, users, roles, and federated users\) can perform actions on the *widget*\.

To learn how to attach a resource\-based policy to a *bucket*, see ***ADD XREF TO YOUR DOCS HERE***\.

### Examples<a name="security_iam_service-with-iam-resource-based-policies-examples"></a>



To view examples of AWS SGW resource\-based policies, see [AWS Storage Gateway resource\-based policy examples](security_iam_resource-based-policy-examples.md),

## Access control lists \(ACLs\)<a name="security_iam_service-with-iam-acls"></a>

Access control lists \(ACLs\) are lists of grantees that you can attach to resources\. They grant accounts permissions to access the resource to which they are attached\. You can attach ACLs to an Amazon S3 *bucket* resource\. For more information about attaching ACLs to *buckets*, see ***ADD XREF TO YOUR DOCS HERE***\.

With Amazon S3 access control lists \(ACLs\), you can manage access to *bucket* resources\. Each *bucket* has an ACL attached to it as a subresource\. It defines which AWS accounts, IAM users or groups of users, or IAM roles are granted access and the type of access\. When a request is received for a resource, AWS checks the corresponding ACL to verify that the requester has the necessary access permissions\.

When you create a *bucket* resource, Amazon S3 creates a default ACL that grants the resource owner full control over the resource\. In the following example *bucket* ACL, John Doe is listed as the owner of the *bucket* and is granted full control over that *bucket*\. An ACL can have up to 100 grantees\.

```
<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy xmlns="http://YOUR-SERVICE-PREFIX.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>c1daexampleaaf850ea79cf0430f33d72579fd1611c97f7ded193374c0b163b6</ID>
    <DisplayName>john-doe</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
               xsi:type="Canonical User">
        <ID>c1daexampleaaf850ea79cf0430f33d72579fd1611c97f7ded193374c0b163b6</ID>
        <DisplayName>john-doe</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

The ID field in the ACL is the AWS account canonical user ID\. To learn how to view this ID in an account that you own, see [Finding an AWS Account Canonical User ID](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html#FindingCanonicalId)\. 

## Authorization based on AWS SGW tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to AWS SGW resources or pass tags in a request to AWS SGW\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `YOUR-SERVICE-PREFIX:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging AWS SGW resources, see ***ADD XREF TO YOUR DOCS HERE***\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Viewing AWS SGW *widgets* based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-widget-tags)\.

## AWS SGW IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with AWS SGW<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

AWS SGW supports using temporary credentials\. 

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

AWS SGW supports service\-linked roles\. For details about creating or managing AWS SGW service\-linked roles, see ***ADD XREF TO YOUR DOCS HERE***\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS SGW supports service roles\. 

### Choosing an IAM role in AWS SGW<a name="security_iam_service-with-iam-roles-choose"></a>

When you create a *widget* resource in AWS SGW, you must choose a role to allow AWS SGW to access Amazon EC2 on your behalf\. If you have previously created a service role or service\-linked role, then AWS SGW provides you with a list of roles to choose from\. It's important to choose a role that allows access to start and stop Amazon EC2 instances\. For more information, see ***ADD XREF TO YOUR DOCS HERE***\.