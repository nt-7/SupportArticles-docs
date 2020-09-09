---
title: CAP in failover cluster doesn't come online
description: Fixes an issue where a Client Access Point (CAP) in a failover cluster does not come online as expected.
ms.date: 09/08/2020
author: Deland-Han
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: Errors when running the Validation Wizard
ms.technology: HighAvailability
---
# "Unable to get Computer Object using GUID" error message when a network name request fails in a Windows Server 2008 failover cluster

This article provides a solution to fix an issue where a Client Access Point (CAP) in a failover cluster does not come online as expected.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 2008654

## Symptoms

A CAP in a Windows Server 2008 failover cluster does not come online as expected. In this situation, the following Error event is logged: 

Log Name:      System
Source:        Microsoft-Windows-FailoverClustering
Event ID:      1207
Task Category: Network Name Resource
Level:         Error
Description:
Cluster network name resource '\<Resource Name>' cannot be brought online. The computer object associated with the resource could not be updated in domain '\<domain FQDN>' for the following reason:
 **Unable to get Computer Object using GUID.**  
 The text for the associated error code is: **The specified domain either does not exist or could not be contacted.**  
 The cluster identity '\<Cluster CNO or Node>$' may lack permissions required to update the object. Work with your domain administrator to ensure that the cluster identity can update computer objects in the domain. 
 Additionally, when you view the cluster log, you see the following error message: 
 Search for existing computer account failed. status 8007054B 

> [!NOTE]
> Error code 0x8007054b = "ERROR_NO_SUCH_DOMAIN // The specified domain either does not exist or could not be contacted." 

## Cause

During a cluster operation, maintenance tasks may be required on computer objects in Active Directory. For example passwords may have to be created, deleted, enabled, disabled, and rotated, and SPNs may have to be updated. When servers are located in perimeter networks (also known as DMZs, demilitarized zones, and screened subnets) that have access only to Read-Only Domain Controllers (RODC), you can work around some of these tasks by manually creating and deleting computer objects. Other operations are forwarded by RODCs to a writeable domain controller elsewhere in the domain.  However, there are some modification operations that the cluster performs in which a writeable domain controller is explicitly requested. 

## Resolution

To resolve this problem, provide the failover cluster access to a writeable domain controller.

 To learn more about the Active Directory requirements for failover clustering, visit the following Microsoft Web site: 
 [https://technet.microsoft.com/library/cc731002(WS.10).aspx](https://technet.microsoft.com/library/cc731002%28WS.10%29.aspx) 
For more information about RODCs, visit the following Microsoft Web site:
 [https://technet2.microsoft.com/windowsserver2008/en/library/ce82863f-9303-444f-9bb3-ecaf649bd3dd1033.mspx?mfr=true](https://technet2.microsoft.com/windowsserver2008/en/library/ce82863f-9303-444f-9bb3-ecaf649bd3dd1033.mspx?mfr=true)