---
title: "DeleteCertificate Method | Microsoft Docs"
ms.custom: ""
ms.date: "09/20/2016"
ms.prod: "configuration-manager"
ms.reviewer: ""
ms.suite: ""
ms.technology:
  - "configmgr-other"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 246d0ad5-582b-4c6d-9611-0e200c47f354
caps.latest.revision: 3
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# DeleteCertificate Method in Class SMS_CertificateData
The `DeleteCertificate` Windows Management Instrumentation (WMI) class method, in Configuration Manager, that deletes the certificate from the database.  

 The following syntax is simplified from Managed Object Format (MOF) code and defines the method.  

## Syntax  

```  
sint32 DeleteCertificate(  
    [IN] UInt32 CertType  
);  

```  

## Parameters  
 `CertType`  
 Data type: `UInt32`  

 Qualifiers: [in]  

 Certificate type.  

## Remarks  

## Requirements  

## Runtime Requirements  
 For more information, see [Configuration Manager Server Runtime Requirements](../../../develop/core/reqs/server-runtime-requirements.md).  

## Development Requirements  
 For more information, see [Configuration Manager Server Development Requirements](../../../develop/core/reqs/server-development-requirements.md).
