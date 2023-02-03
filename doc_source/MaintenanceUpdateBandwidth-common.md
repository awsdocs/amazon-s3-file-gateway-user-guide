--------

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Managing Bandwidth for Your Amazon S3 File Gateway<a name="MaintenanceUpdateBandwidth-common"></a>

You can limit the upload throughput from your gateway to AWS to control the amount of network bandwidth the gateway uses\. By default, an activated gateway has no rate limits\.

You can configure a bandwidth\-rate\-limit schedule using the AWS Management Console, an AWS Software Development Kit \(SDK\), or the AWS Storage Gateway API \(see [UpdateBandwidthRateLimitSchedule](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimitSchedule.html) in the *AWS Storage Gateway API Reference*\.\)\. Using a bandwidth rate limit schedule, you can configure limits to change automatically throughout the day or week\. For more information, see [View and edit the bandwidth\-rate\-limit schedule for your gateway using the Storage Gateway console](#SchedulingBandwidthThrottling)\.

You can monitor your gateway's upload throughput using the `CloudBytesUploaded` metric on the **Monitoring** tab in the Storage Gateway console, or in Amazon CloudWatch\. 

**Note**  
Bandwidth rate limits apply to Storage Gateway file uploads only\. Other gateway operations are not affected\.  
Bandwidth rate limiting works by balancing the throughput of all files being uploaded, averaged over every second\. While it is possible for uploads to cross the bandwidth rate limit briefly for any given micro\- or millisecond, this does not typically result in large spikes over longer periods of time\.  
Configuring bandwidth rate limits and schedules is not currently supported for the Amazon FSx File Gateway type\.

**Topics**
+ [View and edit the bandwidth\-rate\-limit schedule for your gateway using the Storage Gateway console](#SchedulingBandwidthThrottling)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java](#MaintenanceUpdateBandwidthJava-common)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for \.NET](#MaintenanceUpdateBandwidthDotNet-common)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS Tools for Windows PowerShell](#MaintenanceUpdateBandwidthPowerShell-common)

## View and edit the bandwidth\-rate\-limit schedule for your gateway using the Storage Gateway console<a name="SchedulingBandwidthThrottling"></a>

This section describes how to view and edit the bandwidth rate limit schedule for your gateway\.

**To view and edit the bandwidth rate limit schedule**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the left navigation pane, choose **Gateways**, and then choose the gateway that you want to manage\.

1. For **Actions**, choose **Edit bandwidth rate limit schedule**\.

   The gateway's current bandwidth\-rate\-limit schedule is displayed on the **Edit bandwidth rate limit schedule** page\. By default, a new gateway has no defined bandwidth\-rate limits\. 

1. \(Optional\) Choose **Add new bandwidth rate limit** to add a new configurable interval to the schedule\. For each interval you add, enter the following information:
   + **Upload rate** – Enter the upload rate limit, in megabits per second \(Mbps\)\. The minimum value is 100 Mbps\. 
   + **Days of week** – Select the day or days during each week when you want the interval to apply\. You can apply the interval on weekdays \(Monday through Friday\), weekends \(Saturday and Sunday\), every day of the week, or on one specific day each week\. To apply the bandwidth\-rate limit uniformly and constantly on all days and at all times, choose **No schedule**\.
   + **Start time** – Enter the start time for the bandwidth interval, using the HH:MM format and the time\-zone offset from UTC for your gateway\.
**Note**  
Your bandwidth\-rate\-limit interval begins at the start of the minute that you specify here\.
   + **End time** – Enter the end time for the bandwidth interval, using the HH:MM format and the time\-zone offset from GMT for your gateway\. 
**Important**  
The bandwidth\-rate\-limit interval ends at the end of the minute specified here\. To schedule an interval that ends at the end of an hour, enter **59**\.  
 To schedule consecutive continuous intervals, transitioning at the start of the hour, with no interruption between the intervals, enter **59** for the end minute of the first interval\. Enter **00** for the start minute of the succeeding interval\. 

1. \(Optional\) Repeat the previous step as necessary until your bandwidth\-rate\-limit schedule is complete\. If you need to delete an interval from your schedule, choose **Remove**\.
**Important**  
 Bandwidth\-rate\-limit intervals cannot overlap\. The start time of an interval must occur after the end time of a preceding interval, and before the start time of a following interval\. 

1. When finished, choose **Save changes**\.

## Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java<a name="MaintenanceUpdateBandwidthJava-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust these limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits using the AWS SDK for Java\. To use the example code, you should be familiar with running a Java console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/java-dg-setup.html) in the *AWS SDK for Java Developer Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java**  
The following Java code example updates a gateway's bandwidth\-rate limits\. To use this example code, you must provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload limit\. For a list of AWS service endpoints that you can use with Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.  

```
    import java.io.IOException;
     
    import com.amazonaws.AmazonClientException;
    import com.amazonaws.auth.PropertiesCredentials;
    import com.amazonaws.services.storagegateway.AWSStorageGatewayClient;
    import com.amazonaws.services.storagegateway.model. UpdateBandwidthRateLimitScheduleRequest;
    import com.amazonaws.services.storagegateway.model. UpdateBandwidthRateLimitScheduleReturn;
     
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
     
    public class UpdateBandwidthExample {
     
        public static AWSStorageGatewayClient sgClient;
        
        // The gatewayARN
        public static String gatewayARN = "*** provide gateway ARN ***";
     
        // The endpoint
        static String serviceURL = "https://storagegateway.us-east-1.amazonaws.com";
        
        // Rates
        static long uploadRate = 100 * 1024 * 1024;  // Bits per second, minimum 100 Megabits/second
        
        public static void main(String[] args) throws IOException {
     
            // Create a Storage Gateway client
            sgClient = new AWSStorageGatewayClient(new PropertiesCredentials(
                    UpdateBandwidthExample.class.getResourceAsStream("AwsCredentials.properties")));    
            sgClient.setEndpoint(serviceURL);
            
            UpdateBandwidth(gatewayARN, uploadRate, null); // download rate not supported by S3 File Gateways
            
        }
     
        private static void UpdateBandwidth(String gatewayArn, long uploadRate, long downloadRate) {
            try
            {
                BandwidthRateLimit bandwidthRateLimit = new BandwidthRateLimit(downloadRate, uploadRate);
                BandwidthRateLimitInterval noScheduleInterval = new BandwidthRateLimitInterval()
                    .withBandwidthRateLimit(bandwidthRateLimit)
                    .withDaysOfWeek(Arrays.asList(1, 2, 3, 4, 5, 6, 0))
                    .withStartHourOfDay(0)
                    .withStartMinuteOfHour(0)
                    .withEndHourOfDay(23)
                    .withEndMinuteOfHour(59);
                UpdateBandwidthRateLimitScheduleRequest updateBandwidthRateLimitScheduleRequest =
                    new UpdateBandwidthRateLimitScheduleRequest()
                    .withGatewayARN(gatewayArn)
                    .with BandwidthRateLimitIntervals(Collections.singletonList(noScheduleInterval));
     
                UpdateBandwidthRateLimitScheduleReturn updateBandwidthRateLimitScheuduleResponse = sgClient.UpdateBandwidthRateLimitSchedule(updateBandwidthRateLimitScheduleRequest);
     
                String returnGatewayARN = updateBandwidthRateLimitScheuduleResponse.getGatewayARN();
                System.out.println("Updated the bandwidth rate limits of " + returnGatewayARN);
                System.out.println("Upload bandwidth limit = " + uploadRate + " bits per second");
            }
            catch (AmazonClientException ex)
            {
                System.err.println("Error updating gateway bandwith.\n" + ex.toString());
            }
        }        
    }
```

## Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for \.NET<a name="MaintenanceUpdateBandwidthDotNet-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust these limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits by using the AWS Software Development Kit \(SDK\) for \.NET\. To use the example code, you should be familiar with running a \.NET console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits by Using the AWS SDK for \.NET**  
The following C\# code example updates a gateway's bandwidth\-rate limits\. To use this example code, you must provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload limit\. For a list of AWS service endpoints that you can use with Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.  

```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using Amazon.StorageGateway;
    using Amazon.StorageGateway.Model;
     
    namespace AWSStorageGateway
    {
        class UpdateBandwidthExample
        {
            static AmazonStorageGatewayClient sgClient;
            static AmazonStorageGatewayConfig sgConfig;
     
            // The gatewayARN
            public static String gatewayARN = "*** provide gateway ARN ***";
     
            // The endpoint
            static String serviceURL = "https://storagegateway.us-east-1.amazonaws.com";
     
            // Rates
            static long uploadRate = 100 * 1024 * 1024;  // Bits per second, minimum 100 Megabits/second
     
            public static void Main(string[] args)
            {
                // Create a Storage Gateway client
                sgConfig = new AmazonStorageGatewayConfig();
                sgConfig.ServiceURL = serviceURL;
                sgClient = new AmazonStorageGatewayClient(sgConfig);
     
                UpdateBandwidth(gatewayARN, uploadRate, null);
     
                Console.WriteLine("\nTo continue, press Enter.");
                Console.Read();
            }
     
            public static void UpdateBandwidth(string gatewayARN, long uploadRate, long downloadRate)
            {
                try
                {
                   BandwidthRateLimit bandwidthRateLimit = new BandwidthRateLimit(downloadRate, uploadRate);
                   BandwidthRateLimitInterval noScheduleInterval = new BandwidthRateLimitInterval()
                    .withBandwidthRateLimit(bandwidthRateLimit)
                    .withDaysOfWeek(Arrays.asList(1, 2, 3, 4, 5, 6, 0))
                    .withStartHourOfDay(0)
                    .withStartMinuteOfHour(0)
                    .withEndHourOfDay(23)
                    .withEndMinuteOfHour(59);
                  List <BandwidthRateLimitInterval> bandwidthRateLimitIntervals = new List<BandwidthRateLimitInterval>();
                  bandwidthRateLimitIntervals.Add(noScheduleInterval);
                  UpdateBandwidthRateLimitScheduleRequest updateBandwidthRateLimitScheduleRequest = 
                    new UpdateBandwidthRateLimitScheduleRequest()
                       .withGatewayARN(gatewayARN)
                       .with BandwidthRateLimitIntervals(bandwidthRateLimitIntervals);
     
                    UpdateBandwidthRateLimitScheduleReturn updateBandwidthRateLimitScheuduleResponse = sgClient.UpdateBandwidthRateLimitSchedule(updateBandwidthRateLimitScheduleRequest);
                    String returnGatewayARN = updateBandwidthRateLimitScheuduleResponse.GatewayARN;
                    Console.WriteLine("Updated the bandwidth rate limits of " + returnGatewayARN);
                    Console.WriteLine("Upload bandwidth limit = " + uploadRate + " bits per second");
                }
                catch (AmazonStorageGatewayException ex)
                {
                    Console.WriteLine("Error updating gateway bandwith.\n" + ex.ToString());
                }
            }
        }
    }
```

## Updating Gateway Bandwidth\-Rate Limits Using the AWS Tools for Windows PowerShell<a name="MaintenanceUpdateBandwidthPowerShell-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust these limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits using the AWS Tools for Windows PowerShell\. To use the example code, you should be familiar with running a PowerShell script\. For more information, see [Getting Started](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html) in the *AWS Tools for Windows PowerShell User Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits by Using the AWS Tools for Windows PowerShell**  
The following PowerShell script example updates a gateway's bandwidth\-rate limits\. To use this example script, you must provide your gateway Amazon Resource Name \(ARN\) and the upload limit\.  

```
    <#
    .DESCRIPTION
        Update Gateway bandwidth limits schedule
        
    .NOTES
        PREREQUISITES:
        1) AWS Tools for PowerShell from https://aws.amazon.com/powershell/
        2) Credentials and region stored in session using Initialize-AWSDefault.
        For more info, see https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html
        
     
    .EXAMPLE
        powershell.exe .\SG_UpdateBandwidth.ps1 
    #>
     
    $UploadBandwidthRate = 100 * 1024 * 1024 
    $gatewayARN = "*** provide gateway ARN ***"
     
    $bandwidthRateLimitInterval = New-Object Amazon.StorageGateway.Model.BandwidthRateLimitInterval
    $bandwidthRateLimitInterval.StartHourOfDay = 0
    $bandwidthRateLimitInterval.StartMinuteOfHour = 0
    $bandwidthRateLimitInterval.EndHourOfDay = 23
    $bandwidthRateLimitInterval.EndMinuteOfHour = 59
    $bandwidthRateLimitInterval.DaysOfWeek = 0,1,2,3,4,5,6
    $bandwidthRateLimitInterval.AverageUploadRateLimitInBitsPerSec = $UploadBandwidthRate
     
    #Update Bandwidth Rate Limits
    Update-SGBandwidthRateLimitSchedule -GatewayARN $gatewayARN `
                                        -BandwidthRateLimitInterval @($bandwidthRateLimitInterval)
     
    $schedule =  Get-SGBandwidthRateLimitSchedule -GatewayARN $gatewayARN
     
    Write-Output("`nGateway: " + $gatewayARN);
    Write-Output("`nNew bandwidth throttle schedule: " + $schedule.BandwidthRateLimitIntervals.AverageUploadRateLimitInBitsPerSec)
```