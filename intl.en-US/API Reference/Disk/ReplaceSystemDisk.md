# ReplaceSystemDisk {#doc_api_1030728 .reference}

Replaces the system disk or operating system of an ECS instance.

## Description {#description .section}

When you call this operation, note that:

-   You cannot change the category of the system disk for your ECS instance.
-   This operation changes the ID of the system disk but does not change its billing method. The original system disk is released when the operation is completed.
-   The instance to which the specified disk is attached must be in the Stopped state. You must disable No Fees for Stopped Instances \(VPC-Connected\) when you stop the instance. This prevents the instance from being unable to be restarted because of insufficient ECS resources after the system disk is replaced. For more information, see [StopInstance](~~25501~~).
-   The instance must not be locked by [security control](~~25695~~) \(the OperationLocks parameter is "LockReason": "security"\).
-   The instance cannot have overdue payments.
-   You can specify a new capacity by setting the SystemDisk.Size parameter for the system disk. The parameter value must be at least 20 GiB and greater than or equal to the disk capacity before replacement. Extra fees are charged for the size that exceeds max\{20, the disk capacity before replacement\}.

## Debugging {#apiExplorer .section}

You can use [API Explorer](https://api.aliyun.com/#product=Ecs&api=ReplaceSystemDisk) to perform debugging. API Explorer allows you to perform various operations to simplify API usage. For example, you can retrieve APIs, call APIs, and dynamically generate SDK example code.

## Request parameters {#parameters .section}

|Name|Type|Required|Example|Description|
|----|----|--------|-------|-----------|
|InstanceId|String|Yes|i-instanceid1| The ID of the instance.

 |
|Action|String|No|ReplaceSystemDisk| The operation that you want to perform. Set the value to ReplaceSystemDisk.

 |
|Architecture|String|No|i386| The system architecture. Valid values: i386 | x86\_64.

 |
|ClientToken|String|No|123e4567-e89b-12d3-a456-426655440000| A client token. It is used to ensure the idempotency of requests. The value of this parameter is generated by the client and is unique among different requests. The **ClientToken** parameter must be no more than 64 ASCII characters in length. For more information, see [How to ensure idempotency](~~25693~~).

 |
|DiskId|String|No|d-23b3p4r8x| The ID of an existing disk. The system disk is replaced based on this disk.

 |
|ImageId|String|No|m-imageid1| The ID of the image used to replace the system disk.

 |
|KeyPairName|String|No|JoshuaCentOS| The name of the key pair.

 -   For a Windows ECS instance, this parameter is ignored. This parameter is not specified by default. The Password parameter is still effective even when the KeyPairName parameter is specified.
-   For a Linux ECS instance, the username and password authentication method is disabled by default.

 |
|Password|String|No|EcsV587!| The password of the instance.

 **Note:** If the Password parameter is specified, use HTTPS to call the operation to avoid password leakage.

 The password must be 8 to 30 characters in length and must contain uppercase or lowercase letters, digits, and special characters. The password of a Windows instance cannot start with a forward slash \(/\). The password can contain any of the following special characters:

 ``` {#codeblock_51j_1er_fs8}
()` ~! @#$%^&*-_+=|{}[]:;‘<>,.? /
```

 |
|PasswordInherit|Boolean|No|false| Indicates whether to use the pre-configured password of the specified image. When the PasswordInherit parameter is specified, the Password parameter must be null. For secure access, make sure that the selected image has password configured.

 Default value: false.

 |
|Platform|String|No|CentOS| The release version of the operating system. Valid values: CentOS | Ubuntu.

 |
|SecurityEnhancementStrategy|String|No|Active| Indicates whether to enable security hardening and install Alibaba Cloud Security on the system disk. Valid values:

 -   Active: enables security hardening and installs Alibaba Cloud Security for free. This value is applicable to only public images.
-   Deactive: disables security hardening and does not install Alibaba Cloud Security. This value is applicable to all images.

 |
|SystemDisk.Size|Integer|No|80| The capacity of the new system disk. Unit: GiB. Valid values: Max\{20, the size of the specified image\} to 500.

 Default value: Max\{40, the size of the specified image\}.

 |
|UseAdditionalService|Boolean|No|true| Indicates whether to use the VM system configuration provided by Alibaba Cloud. Windows: NTP and KMS. Linux: NTP and YUM.

 **Note:** You can specify this parameter when you attach a system disk \(device is /dev/xvda\).

 |

## Response parameters {#resultMapping .section}

|Name|Type|Example|Description|
|----|----|-------|-----------|
|DiskId|String|d-j6cam2z21u4ks3dj6flb| The ID of the new system disk.

 |
|RequestId|String|473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E| The ID of the request.

 |

## Examples {#demo .section}

Sample requests

``` {#request_demo}
https://ecs.aliyuncs.com/?Action=ReplaceSystemDisk
&InstanceId=i-instanceid1
&ImageId=m-imageid1
&SystemDisk.Size=80
&ClientToken=123e4567-e89b-12d3-a456-426655440000
&Password=EcsV587!
&PasswordInherit=false
&KeyPairName=JoshuaCentOS
&SecurityEnhancementStrategy=Active
&<Common request parameters>
```

Successful response examples

`XML` format

``` {#xml_return_success_demo}
<ReplaceSystemDiskResponse>
  <DiskId>d-23jbf2v5m</DiskId>
  <RequestId>F3CD6886-D8D0-4FEE-B93E-1B73239673DE</RequestId>
</ReplaceSystemDiskResponse>
```

`JSON` format

``` {#json_return_success_demo}
{
	"RequestId":"337568C5-64F3-4B76-8CDD-D3D8C57B5B8C",
	"DiskId":"d-j6cam2z21u4ks3dj6flb"
}
```

## Error codes {#section_8uh_vuq_vdm .section}

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|400|InvalidSystemDiskSize.ValueNotSupported|The specified parameter SystemDisk.Size is invalid.|The error message returned when the specified value of the SystemDisk.Size parameter is invalid.|
|404|InvalidImageId.NotFound|The specified ImageId does not exist.|The error message returned when the specified image does not exist under this account. Check whether the image ID is correct.|
|403|IncorrectInstanceStatus|The current status of the resource does not support this operation.|The error message returned when the specified resource is in a state that does not support the current operation.|
|403|InstanceLockedForSecurity|The instance is locked due to security.|The error message returned when the specified instance is locked for security reasons and the operation cannot be completed.|
|403|ImageRemovedInMarket|The specified market image is not available, Or the specified user defined image includes product code because it is based on an image subscribed from marketplace, and that image in marketplace including exact the same product code has been removed.|The error message returned when the specified image from Marketplace is unavailable.|
|500|OperationDenied|Internal Error.|The error message returned when an unknown internal error occurs.|
|403|InstanceExpiredOrInArrears|The specified operation is denied as your prepay instance is expired \(prepay mode\) or in arrears \(afterpay mode\).|The error message returned when the subscription of the instance has expired. You must renew the subscription before proceeding.|
|403|ChargeTypeViolation|The operation is not permitted due to charge type of the instance.|The error message returned when the billing method of the instance does not support this operation.|
|403|DiskCreatingSnapshot|The operation is denied due to a snapshot of the specified disk is not completed yet.|The error message returned when a snapshot of the specified disk is being created.|
|403|QuotaExceed.BuyImage|The specified image is from the image market,You have not bought it or your quota has been exceeded.|The error message returned when you are not authorized to use the specified image from Marketplace.|
|404|InvalidSystemDiskSize.LessThanImageSize|The specified parameter SystemDisk.Size is less than the image size.|The error message returned when the specified system disk size is smaller than the size of the image.|
|404|NoSuchResource|The specified resource is not found.|The error message returned when the specified resource does not exist.|
|403|INST\_HAS\_UNPAID\_ORDER|The instance has unpaid order.|The error message returned when the instance has overdue payments.|
|400|InvalidSystemDiskSize.ValueNotSupported|The specified parameter SystemDisk.Size is invalid|The error message returned when the specified value of the SystemDisk.Size parameter is invalid.|
|400|InvalidPasswordParam.Mismatch|The input password should be null when passwdInherit is true.|The error message returned when the PasswdInherit parameter is specified but the Password parameter is not left null.|
|400|OperationDenied|The specified image contains the snapshot of the data disk,does not support this operation.|The error message returned when the specified image contains the snapshot of the data disk and cannot be exported.|
|400|InvalidDiskCategory.ValueNotSupported|The specified parameter DiskCategory is not valid.|The error message returned when the specified value of the DiskCategory parameter is invalid.|
|400|InvalidParameter.Conflict|%s|The error message returned when a specified parameter conflicts with another parameter.|
|400|InvalidSystemDiskSize.ValueNotSupported|%s|The error message returned when a specified parameter is not supported.|
|403|InvalidParameter.NotMatch|%s|The error message returned when a specified parameter conflicts with another parameter.|
|400|MissingParameter.Architecture|Architecture should not be null.|The error message returned when a required parameter is not specified.|
|400|InvalidArchitecture.Malformed|Architecture is not valid.|The error message returned when the value of a specified parameter is invalid.|
|400|MissingParameter.Platform|Platform should not be null.|The error message returned when a required parameter is not specified.|
|400|InvalidPlatform.Malformed|Platform is not valid.|The error message returned when the specified value of the Platform parameter is invalid.|
|400|InvalidParameter.AllEmpty|%s|The error message returned when a required parameter is not specified.|
|400|InvalidDiskId.NotFound|The specified disk do not exist.|The error message returned when the specified disk does not exist.|
|400|InvalidDatadisk.DiskStatusViolation|The operation is not permitted due to status of the Datadisk.|The error message returned when the specified data disk type does not support this operation.|
|400|InvalidDatadisk.DiskCategoryViolation|The operation is not permitted due to category of the Datadisk.|The error message returned when the specified data disk type does not support this operation.|
|400|InvalidSystemDiskSize.ValueNotSupported|The specified SystemDiskSize is not valid.|The error message returned when the specified value of the SystemDisk.Size parameter is invalid.|
|403|ImageNotSupportInstanceType|The specified instanceType is not supported by instance with marketplace image.|The error message returned when the specified image from Marketplace does not support the instance type.|
|500|InternalError|The request processing has failed due to some unknown error, exception or failure.|The error message returned when an unknown error occurs.|

[View error codes](https://error-center.aliyun.com/status/product/Ecs)

