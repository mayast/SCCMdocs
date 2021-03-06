---
title: "SMS_TaskSequence_RunCommandLineAction Class | Microsoft Docs"
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
ms.assetid: b0f7d2c6-ca63-4f73-82d9-1f7f3efbca25
caps.latest.revision: 11
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# SMS_TaskSequence_RunCommandLineAction Server WMI Class
The `SMS_TaskSequence_RunCommandLineAction` Windows Management Instrumentation (WMI) class is an SMS Provider server class, in Configuration Manager, that represents a task sequence action that runs a user-specified command line.  

 The following syntax is simplified from Managed Object Format (MOF) code and includes all inherited properties.  

## Syntax  

```  
Class SMS_TaskSequence_RunCommandLineAction : SMS_TaskSequence_Action   
{   
      String CommandLine;   
      SMS_TaskSequence_Condition Condition;   
      Boolean ContinueOnError;   
      String Description;   
      Boolean DisableWow64Redirection;   
      Boolean Enabled;   
      String Name;   
      String PackageID;   
      Boolean RunAsUser;  
      String SuccessCodes;   
      String SupportedEnvironment;   
      UInt32 Timeout;   
      String UserName;  
      String UserPassword;  
      String WorkingDirectory;   
};  
```  

## Methods  
 The `SMS_TaskSequence_RunCommandLineAction` class does not define any methods.  

## Properties  
 `CommandLine`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [Not_Null, CommandLineArg(2), AllowedLen("1-32000")]  

 User-specified command-line application name. The name length can be between 1 and 32,000 characters.  

 `Condition`  
 Data type: `SMS_TaskSequence_Condition`  

 Access type: Read/Write  

 Qualifiers: None  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `ContinueOnError`  
 Data type: `Boolean`  

 Access type: Read/Write  

 Qualifiers: None  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `Description`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [AllowedLen("0-255")]  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `DisableWow64Redirection`  
 Data type: `Boolean`  

 Access type: Read/Write  

 Qualifiers: [Not_Null, VariableName("SMSTSDisableWow64Redirection")]  

 `true` if the execution engine disables Wow64 file redirection and 64-bit registry redirection when the task sequence is evaluating file, folder, and registry conditions on a 64-bit operating system. The default value is `false`.  

 The task sequence variable associated with this property is SMSTSDisableWow64Redirection. For more information, see the MSDN documentation for [Operating System Deployment Task Sequence Variables](http://go.microsoft.com/fwlink/?LinkId=100711).  

 `Enabled`  
 Data type: `Boolean`  

 Access type: Read/Write  

 Qualifiers: None  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `Name`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [AllowedLen("1-100")]  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `PackageID`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [TaskSequencePackage, CommandLineArg(1)]  

 The ID of the task sequence package associated with the action.  

 `RunAsUser`  
 Data type: `Boolean`  

 Access type: Read/Write  

 Qualifiers: [VariableName("_SMSTSRunCommandLineAsUser"), RequireR2]  

 When set to `true`, the command line will be run under the credentials specified by the `UserName` property.  

 The default value is: `false`  

 `SuccessCodes`  
 Data type: `String`  

 Access type: `Read/Write`  

 Qualifiers: [SuccessCodes, Not_Null]  

 Exit codes that indicate success. The default setting is "0 3010".  

 `SupportedEnvironment`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [Not_Null:ToInstance]  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `Timeout`  
 Data type: `UInt32`  

 Access type: Read/Write  

 Qualifiers: [Not_Null:ToInstance]  

 See [SMS_TaskSequence_Action Server WMI Class](../../../develop/reference/osd/sms_tasksequence_action-server-wmi-class.md).  

 `UserName`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [VariableName("SMSTSRunCommandLineUserName"]  

 The user account to run the command line under when the `RunAsUser` property is set to `true`.  

 `UserPassword`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [VariableName("SMSTSRunCommandLineUserPassword", Secret]  

 Masked password associated with the user account that is used to run the command line when the `RunAsUser` property is set to `true`.  

 `WorkingDirectory`  
 Data type: `String`  

 Access type: Read/Write  

 Qualifiers: [AllowedLen("0-255")]  

 The directory from which to run the command line. Set this property to an absolute path or a relative path. The path length must be between 0 and 255 characters.  

## Remarks  
 Class qualifiers for this class include:  

 [CommandLine("smsswd.exe /run:%1 %2"),  

 ActionCategory("General,1,1"),ActionUI{"AdminUI.TaskSequenceEditor.dll", "Microsoft.ConfigurationManagement.AdminConsole.TaskSequenceEditor", "RunCommandLineControl", "TaskSequenceOptionControl"}]  

 For more information about both the class qualifiers and the property qualifiers included in the Properties section, see [Configuration Manager Class and Property Qualifiers](../../../develop/reference/misc/class-and-property-qualifiers.md).  

## Requirements  

## Runtime Requirements  
 For more information, see [Configuration Manager Server Runtime Requirements](../../../develop/core/reqs/server-runtime-requirements.md).  

## Development Requirements  
 For more information, see [Configuration Manager Server Development Requirements](../../../develop/core/reqs/server-development-requirements.md).  

## See Also  
 [Operating System Deployment Server WMI Classes](../../../develop/reference/osd/operating-system-deployment-server-wmi-classes.md)
