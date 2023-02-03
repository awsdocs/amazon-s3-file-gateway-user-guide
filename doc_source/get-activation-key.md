--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Getting an Activation Key for Your Gateway<a name="get-activation-key"></a>

To get an activation key for your gateway, you make a web request to the gateway VM and it returns a redirect that contains the activation key\. This activation key is passed as one of the parameters to the `ActivateGateway` API action to specify the configuration of your gateway\. For more information, see [ActivateGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ActivateGateway.html) in the *Storage Gateway API Reference*\.

The request you make to the gateway VM contains the AWS Region in which activation occurs\. The URL returned by the redirect in the response contains a query string parameter called `activationkey`\. This query string parameter is your activation key\. The format of the query string looks like the following: ` http://gateway_ip_address/?activationRegion=activation_region`\.

The URL returned by the redirect also includes the following query string parameters:
+ `gatewayType` \- The type of gateway that received the request
+ `endpointType` \- The type of endpoint the gateway uses to connect to AWS
+ `vpcEndpoint` \- The VPC Endpoint ID for gateways that connect using the VPC endpoint type

**Note**  
The AWS Storage Gateway Hardware Appliance, VM image templates, and EC2 AMI come preconfigured with the HTTP services necessary to receive and respond to the web requests described on this page\. It is not required or recommended to install any additional services on your gateway\.

**Topics**
+ [AWS CLI](#get-activation-key-cli)
+ [Linux \(curl\)](#get-activation-key-linux-curl)
+ [Linux \(bash/zsh\)](#get-activation-key-linux)
+ [Microsoft Windows PowerShell](#get-activation-key-powershell)

## AWS CLI<a name="get-activation-key-cli"></a>

If you haven't already done so, you must install and configure the AWS CLI\. To do this, follow these instructions in the *AWS Command Line Interface User Guide:*
+ [ Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)
+ [ Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

The following example shows you how to use the AWS CLI to fetch the HTTP response, parse HTTP headers and get the activation key\.

**To get the activation key for a public endpoint:**

```
wget 'gateway_ip_address/?activationRegion = region_code
      &gatewaytype = gateway_type'2>&1 | \
grep -i location | \
grep -oE 'activationKey = [A-Z0-9-]+' | \
cut -f2 -d =
```

**To get the activation key for a VPC endpoint:**

```
wget 'gateway_ip_address/?activationRegion = region_code
       &vpcEndpoint = vpc_endpoint
      &gatewaytype =  gateway_type'2>&1 | \
grep -i location | \
grep -oE 'activationKey = [A-Z0-9-]+' | \
cut -f2 -d =
```

## Linux \(curl\)<a name="get-activation-key-linux-curl"></a>

The following examples show you how to get the activation key using Linux \(curl\)\.

**Note**  
Replace the highlighted variables with actual values for your gateway\. Acceptable values are as follows:  
*gateway\_ip\_address* \- The IPv4 address of your gateway, for example `172.31.29.201`
*gateway\_type* \- The type of gateway you want to activate, such as `STORED`, `CACHED`, `VTL`, `VTL_SNOW`, `FILE_S3`, or `FILE_FSX_SMB`\.
*region\_code* \- The region in which you want to activate your gateway\. See [Regional endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#regional-endpoints) in the *AWS General Reference Guide*\.
*vpc\_endpoint* \- The VPC endpoint name for your gateway, for example `vpce-050f90485f28f2fd0-iep0e8vq.storagegateway.us-west-2.vpce.amazonaws.com`\.

**To get the activation key for a public endpoint:**

```
curl "http://gateway_ip_address/?gatewayType=gateway_type&activationRegion=region_code&no_redirect"
```

**To get the activation key for a VPC endpoint:**

```
curl "http://gateway_ip_address/?gatewayType=gateway_type&activationRegion=region_code&vpcEndpoint=vpc_endpoint&no_redirect"
```

## Linux \(bash/zsh\)<a name="get-activation-key-linux"></a>

The following example shows you how to use Linux \(bash/zsh\) to fetch the HTTP response, parse HTTP headers, and get the activation key\.

```
  function get-activation-key() {
  local ip_address=$1
  local activation_region=$2
  local gatewaytype=$3
  local 
  if [[ -z "$ip_address" || -z "$activation_region" ]]; then
    echo "Usage: get-activation-key ip_address activation_region"
    return 1
  fi
  if redirect_url=$(curl -f -s -S -w '%{redirect_url}' "http://$ip_address/?activationRegion=$activation_region&gatewayType=$3"); then
    activation_key_param=$(echo "$redirect_url" | grep -oE 'activationKey=[A-Z0-9-]+')
    echo "$activation_key_param" | cut -f2 -d=
  else
    return 1
  fi
}
```

## Microsoft Windows PowerShell<a name="get-activation-key-powershell"></a>

The following example shows you how to use Microsoft Windows PowerShell to fetch the HTTP response, parse HTTP headers, and get the activation key\.

```
function Get-ActivationKey {
  [CmdletBinding()]
  Param(
    [parameter(Mandatory=$true)][string]$IpAddress, 
    [parameter(Mandatory=$true)][string]$ActivationRegion,
    [parameter(Mandatory=$true)][string]$GatewayType
  )
  PROCESS {
    $request = Invoke-WebRequest -UseBasicParsing -Uri "http://$IpAddress/?activationRegion=$ActivationRegion&gatewayType=$GatewayType" -MaximumRedirection 0 -ErrorAction SilentlyContinue
    if ($request) {
      $activationKeyParam = $request.Headers.Location | Select-String -Pattern "activationKey=([A-Z0-9-]+)"
      $activationKeyParam.Matches.Value.Split("=")[1]
    }
  }
}
```