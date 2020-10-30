---
keyword: [quota, ECS quotas, insufficient ECS quotas, quota, ECS quota, ECS quotas, ECS limits, ECS quotas, ECS resource quotas]
---

# Limits

This topic describes the limits on ECS and how to apply for extensions on some limits.

## Overview

ECS has the following limits:

-   You cannot install virtualization software such as VMware Workstation or use ECS instances for secondary virtualization. Only ECS bare metal instances and Super Computing Clusters \(SCCs\) support secondary virtualization.
-   ECS does not support sound card applications.
-   External hardware devices such as hardware dongles, USB flash drives, external hard disks, and hardware tokens, cannot be directly attached to ECS instances. Software verification methods such as two-factor authentication and dynamic passwords can be used.
-   ECS does not support multicast protocols. We recommend that you use unicast protocols instead.
-   Log Service does not support 32-bit Linux ECS instances.

    For information about the ECS instances that support Log Service, see [Logtail overview](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md).

-   To apply for ICP filings for websites that are deployed on your ECS instance, ensure that the instance meets ICP filing requirements. You can apply for a limited number of ICP filing service numbers for each ECS instance. For more information, see [Prepare and check the instance and access information](). For information about how to apply for ICP filings, see [ICP filing application overview]().

## View quotas

You can click **Privileges** on the **Overview** page of the ECS console, and select a region to view the resource usage and quotas in that region. If the quota for a resource type is insufficient, submit a ticket to request a quota increase. For more information about how to view privileges and quotas, see [View and increase quotas \(new version\)](/intl.en-US/Tag & Resource/Resource/View and increase quotas (new version).md) or [DescribeAccountAttributes](/intl.en-US/API Reference/Others/DescribeAccountAttributes.md).

## Instance limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Permissions to create an ECS instance|To create an ECS instance within a region in mainland China, you must first complete real-name verification.|None.|
|Instance types that can be used to create pay-as-you-go instances|Instance types that have less than 16 vCPUs.|Submit a ticket.|
|Quota for vCPUs available for pay-as-you-go instances per region in an account|50 vCPUs|Submit a ticket.|
|Quota for vCPUs available for preemptible instances per region in an account|Submit a ticket to apply for a quota of up to 50 vCPUs.|Submit a ticket.|
|Quota for instance launch templates per region in an account|30|None.|
|Quota for versions for an instance launch template|30|None.|
|Permissions to change the billing method from pay-as-you-go to subscription|The following instance types or families do not support billing method changes: t1, s1, s2, s3, c1, c2, m1, m2, n1, n2, and e3.|None.|
|Permissions to change the billing method from subscription to pay-as-you-go|-   Whether you have these permissions depends on your ECS usage.
-   5,000 vCPUs × Number of hours per month.
-   The billing method change from subscription to pay-as-you-go may result in a refund. Each account is limited to a maximum monthly refund amount. The actual refund details are displayed on the Switch to Pay-As-You-Go page in the ECS console.

|None.|

## Reserved instance limits

|Item|Limit|How to apply for an extension on the limit|
|----|-----|------------------------------------------|
|Quota for regional reserved instances in an account|20|Submit a ticket.|
|Quota for zonal reserved instances per zone in an account|20|Submit a ticket.|
|Instance families that support reserved instances|Instance families that support reserved instances include:-   General purpose instance families: g6e, g6, g5, and sn2ne
-   Compute optimized instance families: c6e, c6, c5, ic5, and sn1ne
-   Memory optimized instance families: r6e, r6, r5, and se1ne
-   Big data instance family: d2s
-   Instance families with local SSDs: i2 and i2g
-   Instance families with high clock speed: hfc6, hfc5, hfg6, hfg5, and hfr6
-   GPU-accelerated compute optimized instance families: gn6i and gn6e
-   ECS Bare Metal Instance families: ebmc6, ebmg6, ebmr6, ebmhfc6, ebmhfg6, and ebmhfr6
-   Burstable instance families: t6 and t5

**Note:** The t5 instance family supports only zonal reserved instances.

|None.|

**Note:** For more information, see the "Limits" section in [Reserved instance overview](/intl.en-US/Instance/Instance purchasing options/Reserved Instances/Reserved instance overview.md).

## Elastic Block Storage limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Permissions to create a pay-as-you-go disk|You must complete real-name verification before you can create a disk within a region in mainland China.|None.|
|Quota for pay-as-you-go disks in all regions for an account|This quota is calculated by using the following formula: Number of ECS instances across all regions × 5. A minimum of ten pay-as-you-go disks can be created in each account. If only one instance exists in your account, you can create a maximum of ten pay-as-you-go disks in all regions, instead of five pay-as-you-go disks.|Submit a ticket.|
|Quota for the capacity of pay-as-you-go disks that are used as data disks in an account|This limit is subject to ECS resource usage, region, and disk category. You can go to the Privileges & Quotas page in the ECS console to view this information. For more information, see [View and increase quotas \(new version\)](/intl.en-US/Tag & Resource/Resource/View and increase quotas (new version).md).|Submit a ticket.|
|Quota for system disks on an instance|1|None.|
|Quota for data disks on an instance|16.|None.|
|Capacity of a basic disk|5 GiB to 2,000 GiB.|None.|
|Capacity of a standard SSD|20 GiB to 32,768 GiB.|None.|
|Capacity of an ultra disk|20 GiB to 32,768 GiB.|None.|
|Capacity of an enhanced SSD \(ESSD\)|20 GiB to 32,768 GiB.|None.|
|Capacity of a local SSD|5 GiB to 800 GiB.|None.|
|Total capacity of all local SSDs on an instance|1,024 GiB.|None.|
|Capacity of a system disk|-   Windows Server: 40 GiB to 500 GiB
-   CoreOS and FreeBSD: 30 GiB to 500 GiB
-   Linux systems excluding CoreOS: 20 GiB to 500 GiB

|None.|
|Permissions to attach new local disks to instances that are equipped with local disks|Not allowed.|None.|
|Permissions to change configurations of instances that are equipped with local disks|Only bandwidth configurations of instances that are equipped with local disks can be changed.|None.|
|Mount points of system disks|/dev/vda|None.|
|Mount points of data disks|/dev/vd\[b-z\]|None.|

**Note:** Block storage capacity is measured in binary units. In binary units, 1 KiB equals 1,024 bytes. Example: 1 GiB = 1,024 MiB.

## Snapshot limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for user-created snapshots \(also called manual snapshots\) that can be created for each disk|256|None.|
|Quota for automatic snapshots that can be created for each disk|1,000|None.|
|Quota for automatic snapshot policies that can be created per region in an account|100|None.|

## Image limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for custom images that can be created per region in an account|100|Submit a ticket.|
|Quota for users to whom a single image can be shared|50|Submit a ticket.|
|Support of instance types for images|Instance types that have 4 GiB or larger memory do not support 32-bit images.|None.|

## SSH key pair limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for SSH key pairs per region in an account|500|None.|
|Instance types that support SSH key pairs|Non-I/O optimized instances of Generation I instance families do not support SSH key pairs|None.|
|Images that support SSH key pairs|Only Linux images|None.|

## Public bandwidth limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Peak inbound bandwidth|-   If the purchased peak outbound bandwidth is less than or equal to 10 Mbit/s, Alibaba Cloud allocates an inbound bandwidth of 10 Mbit/s.
-   If the purchased peak outbound bandwidth is greater than 10 Mbit/s, Alibaba Cloud allocates an inbound bandwidth that is equal to the purchased peak outbound bandwidth.

|None.|
|Peak outbound bandwidth|-   Subscription instance: 200 Mbit/s
-   Pay-as-you-go instance: 100 Mbit/s

|None.|
|Change to the public IP address of an instance|The public IP address of an instance can be changed within six hours after the instance is created, and can be changed a maximum of three times.|None.|

**Note:** When the pay-by-traffic billing method is used, the peak inbound and outbound bandwidths indicate the upper limits for bandwidths and are only for reference. In the event of resource contention, the peak bandwidths cannot be guaranteed. If you want guaranteed bandwidths for your instance, use the pay-by-bandwidth billing method.

## Security group limits

|Item|Basic security group|Advanced security group|
|----|--------------------|-----------------------|
|Quota for security groups that can be created per region in an account|100 \(1\)|Same as the limit on basic security groups.|
|Quota for classic network-type ECS instances that can be contained in a classic network-type security group|1,000 \(2\)|The classic network is not supported.|
|Quota for VPC-type ECS instances that can be contained in a VPC-type security group|Depends on the number of private IP addresses that can be contained in the VPC-type security group.|No limits.|
|Quota for security groups to which an ECS instance can belong|5 You can submit a ticket to raise the limit to 10 or 16 security groups.

|Same as the limit on basic security groups.|
|Quota for security groups to which an ENI of an ECS instance can belong|
|Quota for rules \(including inbound and outbound rules\) in a security group|200 \(3\)|Same as the limit on basic security groups.|
|Quota for rules \(including inbound and outbound rules\) in all security groups to which an ENI belongs|1,000|Same as the limit on basic security groups.|
|Quota for private IP addresses that can be contained in a VPC-type security group|2,000 \(4\)|65,536.|
|Internet access port|The default SMTP port for outbound traffic is port 25, which is disabled by default. It cannot be enabled by security group rules.|Same as the limit on basic security groups.|

-   \(1\) The following regions share the quota for security groups that can be created in each account: China \(Hangzhou\), China \(Shanghai\), China \(Qingdao\), China \(Beijing\), China \(Shenzhen\), China \(Hong Kong\), US \(Silicon Valley\), and Singapore \(Singapore\). A maximum of 100 security groups can be created in all of these regions in an account.
-   \(2\) If more than 1,000 classic network-type instances need mutual access over the internal network, you can assign them to multiple security groups and authorize mutual access among these security groups.
-   \(3\) If you increase the quota for security groups to which an ECS instance can belong, the quota for rules in each security group decreases. The product of the quota for security groups to which an ECS instance belongs and the quota for rules in each security group cannot exceed 1,000. For example, if the quota for security groups to which the ECS instance can belong is 5, 10, or 16, the corresponding quota for rules in each security group is 200, 100, or 60, which are verified by using the following formulas: 5 × 200 = 1000, 10 × 100 = 1000, and 16 × 60 ≤ 1000.
-   \(4\) If more than 2,000 private IP addresses need mutual access over the internal network, you can distribute the ECS instances of these private IP addresses to multiple security groups and authorize mutual access among these security groups.

## Deployment set limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for deployment sets per region in an account|2|Submit a ticket.|
|Quota for instances that can be contained in a deployment set|A maximum of seven instances are allowed in each zone. The number of instances allowed in a deployment set within a region is calculated by using the following formula: 7 × Number of zones in the region.|None.|
|Instance families that support deployment sets|Instances in the following instance families can be created in deployment sets: c6, g6, r6, hfc6, hfg6, hfr6, d2, d2s, d2c, c5, d1, d1ne, g5, hfc5, hfg5, i2, i2g, i1, ic5, r5, se1ne, sn1ne, and sn2ne. For more information about instance types and their performance, see [Instance families](/intl.en-US/Instance/Instance families.md).|None.|

## Cloud Assistant limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for Cloud Assistant commands that can be created per region in an account|100|Submit a ticket.|
|Quota for Cloud Assistant commands that can be run each day by an account within a region|5000|Submit a ticket.|

## ENI limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for elastic network interfaces \(ENIs\) per region in an account|100|Submit a ticket.|

## Tag limits

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for tags that can be bound to an instance|20|None.|

## API

|Item|Limit|How to apply for an extension on the limit|
|:---|:----|:-----------------------------------------|
|Quota for calls to the CreateInstance operation|200 calls per minute|Submit a ticket.|

**Note:** For more information about the limits on VPCs, see [Limits](/intl.en-US/Product Introduction/Limits.md).

