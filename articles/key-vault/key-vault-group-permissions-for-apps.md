---
title: "Przyznanie uprawnień do wielu aplikacjom dostęp do usługi Azure key vault | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można udzielić uprawnienia do wielu aplikacjom dostęp do magazynu kluczy"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14da9256def60d678ef5cae795fef1c373914b5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Udziel uprawnień do wielu aplikacjom dostęp do magazynu kluczy

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>Pytanie: czy masz kilka aplikacji (ponad 16), które trzeba uzyskać dostępu do magazynu kluczy. Ponieważ usługa Key Vault zezwala jedynie na 16 wpisów kontroli dostępu, w jaki sposób mogę sobie z tym poradzić?

Zasady kontroli dostępu do magazynu kluczy obsługuje tylko wpisy 16. Można jednak utworzyć grupy zabezpieczeń usługi Azure Active Directory. Dodaj do tej grupy zabezpieczeń wszystkich podmiotów zabezpieczeń skojarzona usługa, a następnie przyznać dostęp do tej grupy zabezpieczeń do magazynu kluczy.

Poniżej przedstawiono wymagania wstępne:
* [Zainstaluj moduł programu PowerShell usługi Azure Active Directory w wersji 2](https://www.powershellgallery.com/packages/AzureAD).
* [Zainstalowanie programu Azure PowerShell](/powershell/azure/overview).
* Aby uruchomić następujące polecenia, musisz mieć uprawnienia Utwórz/Edytuj grup w dzierżawie usługi Azure Active Directory. Jeśli nie masz uprawnień, może być konieczne skontaktuj się z administratorem usługi Azure Active Directory.

Teraz uruchom następujące polecenia w programie PowerShell.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Jeśli musisz przyznać inny zestaw uprawnień do grupy aplikacji, należy utworzyć osobnej grupy zabezpieczeń usługi Azure Active Directory dla takich aplikacji.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o sposobie [bezpiecznego magazynu kluczy](key-vault-secure-your-key-vault.md).
