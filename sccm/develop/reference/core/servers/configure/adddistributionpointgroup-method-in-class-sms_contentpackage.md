---
title: "AddDistributionPointGroup Method | Microsoft Docs"
ms.custom: ""
ms.date: "09/20/2016"
ms.prod: "configuration-manager"
ms.reviewer: ""
ms.suite: ""
ms.technology:
  - "configmgr-other"
ms.tgt_pltfrm: ""
ms.topic: "article"
applies_to:
  - "System Center Configuration Manager (current branch)"
ms.assetid: ad858478-ae62-42be-ad8c-38d16c5c4d26
caps.latest.revision: 8
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# AddDistributionPointGroup Method in Class SMS_ContentPackage
The `AddDistributionPointGroup` Windows Management Instrumentation (WMI) class method, in Configuration Manager, adds the content package to a set of distribution point groups.  

 The following syntax is simplified from Managed Object Format (MOF) code and defines the method.  

## Syntax  

```  
sint32 AddDistributionPointGroup (  
     string DistributionPointGroup[],  
);  
```  

#### Parameters  
 `DistributionPointGroup`  
 Data type: `String` Array  

 Qualifiers: `[in]`  

 Array of distribution point groups.   

## Return Values  
 An `SInt32` data type that is 0 to indicate success or non-zero to indicate failure.  

 For more information about handling returned errors, see [About Configuration Manager Errors](../../../../../develop/core/understand/about-configuration-manager-errors.md).  

## Requirements  

## Runtime Requirements  
 For more information, see [Configuration Manager Server Runtime Requirements](../../../../../develop/core/reqs/server-runtime-requirements.md).  

## Development Requirements  
 For more information, see [Configuration Manager Server Development Requirements](../../../../../develop/core/reqs/server-development-requirements.md).  

## See Also  
 [SMS_Application Server WMI Class](../../../../../develop/reference/apps/sms_application-server-wmi-class.md)   
 [Application Model Server WMI Classes](../../../../../develop/reference/apps/application-management-server-wmi-classes.md)
