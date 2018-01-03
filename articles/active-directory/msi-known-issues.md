---
title: "Często zadawane pytania i znane problemy z zarządzania usługi tożsamości (MSI) dla usługi Azure Active Directory"
description: "Znane problemy z zarządzanych tożsamości usługi dla usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/12/2017
ms.author: bryanla
ms.openlocfilehash: 2c29e93c0978250281fa489e53dcdf25f890a0a7
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Często zadawane pytania i znane problemy z zarządzania usługi tożsamości (MSI) dla usługi Azure Active Directory

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Często zadawane pytania

### <a name="is-there-a-private-preview-available-for-additional-features"></a>Dostępne dla dodatkowych funkcji jest prywatnej wersji zapoznawczej?

Tak. Jeśli chcesz wziąć pod uwagę dla rejestracji w prywatnej wersji zapoznawczej [odwiedź naszą stronę tworzenia konta](https://aka.ms/azuremsiprivatepreview).

### <a name="does-msi-work-with-azure-cloud-services"></a>MSI działa z usługami w chmurze Azure?

Nie, nie ma żadnych planów obsługi MSI w usługi w chmurze Azure.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>MSI działa z biblioteki uwierzytelniania usługi Active Directory (ADAL) lub biblioteki uwierzytelniania firmy Microsoft (MSAL)?

Nie, MSI nie jest jeszcze zintegrowana z biblioteką ADAL lub MSAL. Aby uzyskać więcej informacji na temat pozyskiwania token MSI przy użyciu punktu końcowego MSI REST, zobacz [sposobu użycia Azure VM zarządzane usługi tożsamości (MSI) do przejęcia tokenu](msi-how-to-use-vm-msi-token.md).

### <a name="what-are-the-supported-linux-distributions"></a>Co to są obsługiwane dystrybucje systemu Linux?

Następujące dystrybucje systemu Linux obsługują MSI: 

- Stały CoreOS
- CentOS 7.1
- RedHat 7.2
- Ubuntu 15.04

Inne dystrybucje systemu Linux są obecnie nieobsługiwane i rozszerzenia może zakończyć się niepowodzeniem na nieobsługiwany dystrybucji.

Rozszerzenie działa na 6,9 CentOS. Jednak z powodu braku obsługi systemu w 6,9, rozszerzenia nie automatycznie uruchom ponownie Jeśli awaria lub zatrzymana. Jej ponownym uruchomieniu, po ponownym uruchomieniu maszyny Wirtualnej. Aby ręcznie ponownie uruchomić rozszerzenia, zobacz [jak jest ponownym z rozszerzeniem MSI?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>Jak ponownym z rozszerzeniem MSI?
W systemach Windows i niektóre wersje systemu Linux Jeśli rozszerzenie zostanie zatrzymana, następującego polecenia cmdlet można ręcznie uruchomić ponownie:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Gdzie: 
- Nazwa rozszerzenia i typu dla systemu Windows: ManagedIdentityExtensionForWindows
- Nazwa rozszerzenia i typu dla systemu Linux jest: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Znane problemy

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>"Skrypt automatyzacji" kończy się niepowodzeniem podczas próby eksportowania schematu dla rozszerzenia pliku MSI

Po włączeniu zarządzane tożsamości usługi na maszynie Wirtualnej następujący błąd jest wyświetlany podczas próby użycia funkcji "Skrypt automatyzacji" dla maszyny Wirtualnej lub jego grupa zasobów:

![Wystąpił błąd MSI automatyzacji skrypt eksportu](media/msi-known-issues/automation-script-export-error.png)

Rozszerzenia zarządzane wirtualna tożsamość usługi nie obsługuje obecnie możliwość eksportowania schematem do szablonu grupy zasobów. W związku z tym wygenerowanego szablonu nie są wyświetlane parametry konfiguracji, Włącz zarządzane tożsamość usługi zasobu. Poniższe sekcje mogą być dodawane ręcznie, wykonując przykłady w [konfigurowania tożsamości usługi maszyn wirtualnych zarządzanych za pomocą szablonu](msi-qs-configure-template-windows-vm.md).

Gdy funkcja eksportu schematu stają się dostępne dla rozszerzenia maszyny Wirtualnej MSI, będzie wyświetlane w [eksportowanie grupy zasobów, która zawiera rozszerzenia maszyny Wirtualnej](../virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Blok konfiguracji nie są wyświetlane w portalu Azure

Jeśli nie ma bloku konfiguracji maszyny Wirtualnej na maszynie Wirtualnej, następnie MSI nie zostało włączone w portalu w danym regionie jeszcze.  Sprawdź ponownie później.  Można również włączyć MSI do maszyny Wirtualnej przy użyciu [PowerShell](msi-qs-configure-powershell-windows-vm.md) lub [interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Nie można przypisać dostęp do maszyn wirtualnych w bloku kontroli dostępu (IAM)

Jeśli **maszyny wirtualnej** nie są wyświetlane w portalu Azure jako rozwiązaniem dla **przypisany dostęp** w **kontroli dostępu (IAM)** > **Dodaj uprawnienia**, a następnie zarządzane tożsamość usługi nie został włączony w portalu w danym regionie jeszcze. Sprawdź ponownie później.  Możesz również zaznaczyć tożsamość usługi zarządzania dla przypisania roli, wyszukując dla MSI nazwy głównej usługi.  Wprowadź nazwę maszyny Wirtualnej w ramach **wybierz** pola i nazwy głównej usługi pojawia się w wynikach wyszukiwania.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Maszyna wirtualna nie została uruchomiona po przeniesieniu z grupy zasobów lub subskrypcji

Jeśli przenosisz Maszynę wirtualną w stanie uruchomienia, nadal działa podczas przenoszenia. Jednak po przeniesieniu, jeśli maszyna wirtualna zostanie zatrzymana i ponownie uruchomione, nie będzie można uruchomić. Ten problem odbywa się, ponieważ maszyna wirtualna nie jest aktualizowana odwołanie do tożsamości MSI i wskaż w starym grupy zasobów w dalszym ciągu.

**Obejście problemu** 
 
Wyzwalają aktualizację na maszynie Wirtualnej, aby go pobrać poprawne wartości dla MSI. Możliwość zmiany właściwości maszyny Wirtualnej, aby zaktualizować odwołania do tożsamości MSI. Na przykład można ustawić nową wartość tagu na Maszynie wirtualnej za pomocą następującego polecenia:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
To polecenie ustawia nowy znacznik "fixVM" o wartości 1 na maszynie Wirtualnej. 
 
Przez ustawienie dla tej właściwości, maszyna wirtualna aktualizuje poprawny identyfikator URI zasobu MSI i następnie można uruchomić maszyny Wirtualnej. 
 
Po uruchomieniu maszyny Wirtualnej, można usunąć tagu za pomocą następujących poleceń:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```
