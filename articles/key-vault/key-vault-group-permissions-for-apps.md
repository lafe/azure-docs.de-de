---
title: 'Gewähren der Berechtigung zum Zugreifen auf einen Azure-Schlüsseltresor für viele Anwendungen: Azure Key Vault | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie vielen Anwendungen die Berechtigung zum Zugreifen auf einen Schlüsseltresor gewähren.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: 187d455003cf8b1c9402e24755c5f15b703cd9ad
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114398"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Gewähren des Zugriffs auf einen Schlüsseltresor für mehrere Anwendungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Mithilfe der Zugriffssteuerungsrichtlinie kann mehreren Anwendungen der Zugriff auf einen Schlüsseltresor gewährt werden. Eine Zugriffssteuerungsrichtlinie kann bis zu 1024 Anwendungen unterstützen und wird wie folgt konfiguriert:

1. Erstellen Sie eine Azure Active Directory-Sicherheitsgruppe. 
2. Fügen Sie alle zugehörigen Dienstprinzipale der Anwendungen zur Sicherheitsgruppe hinzu.
3. Gewähren Sie der Sicherheitsgruppe Zugriff auf Ihre Key Vault-Instanz.

## <a name="prerequisites"></a>Voraussetzungen

Hier finden Sie die Voraussetzungen:
* [Installieren Sie Azure PowerShell](/powershell/azure/overview).
* [Installieren Sie das Azure Active Directory V2-PowerShell-Modul](https://www.powershellgallery.com/packages/AzureAD).
* Berechtigungen zum Erstellen/Bearbeiten von Gruppen im Azure Active Directory-Mandanten. Falls Sie nicht über die entsprechenden Berechtigungen verfügen, müssen Sie sich ggf. an Ihren Azure Active Directory-Administrator wenden. Unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md) finden Sie nähere Informationen zur Richtlinie für Berechtigungen zum Zugriff auf Schlüsseltresore.

## <a name="granting-key-vault-access-to-applications"></a>Gewähren von Key Vault-Zugriff auf Anwendungen

Führen Sie die folgenden Befehle in PowerShell aus:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Wenn Sie einer Gruppe von Anwendungen einen anderen Berechtigungssatz gewähren müssen, können Sie für diese Anwendungen eine separate Azure Active Directory-Sicherheitsgruppe erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Schützen einer Key Vault-Instanz](key-vault-secure-your-key-vault.md).
