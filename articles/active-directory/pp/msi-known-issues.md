---
title: "Często zadawane pytania i znane problemy z zarządzania usługi tożsamości (MSI) dla usługi Azure Active Directory"
description: "Znane problemy z zarządzanych tożsamości usługi dla usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 447844d1779c537eb9e336a32575cb68ac9ad9eb
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="faq-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Często zadawane pytania i znane problemy z zarządzania usługi tożsamości (MSI) dla usługi Azure Active Directory

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

## <a name="frequently-asked-questions"></a>Często zadawane pytania

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

### <a name="are-there-rbac-roles-for-user-assigned-identities"></a>Dostępne są role RBAC dla tożsamości przypisanych użytkowników?
Tak:
1. MSI Contributor: 
- Można: CRUD przypisane tożsamości użytkowników. 
- Nie można: Przypisz użytkownika przypisanego do zasobu tożsamości. (tj. Przypisz tożsamości do maszyny Wirtualnej)
2. MSI Operator: 
- Może: Przypisywanie tożsamości użytkownika przypisanego do zasobu. (tj. Przypisz tożsamości do maszyny Wirtualnej)
- Nie można: CRUD przypisane tożsamości użytkowników.

Uwaga: Rola współautora wbudowane mogą wykonywać wszystkie działania opisane powyżej: 
- Tożsamość użytkownika z przypisanym CRUD
- Przypisz użytkownika przypisanego do zasobu tożsamości. (tj. Przypisz tożsamości do maszyny Wirtualnej)


## <a name="known-issues"></a>Znane problemy

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>"Skrypt automatyzacji" kończy się niepowodzeniem podczas próby eksportowania schematu dla rozszerzenia pliku MSI

Po włączeniu zarządzane tożsamości usługi na maszynie Wirtualnej następujący błąd jest wyświetlany podczas próby użycia funkcji "Skrypt automatyzacji" dla maszyny Wirtualnej lub jego grupa zasobów:

![Wystąpił błąd MSI automatyzacji skrypt eksportu](~/articles/active-directory/media/msi-known-issues/automation-script-export-error.png)

Rozszerzenia zarządzane wirtualna tożsamość usługi nie obsługuje obecnie możliwość eksportowania schematem do szablonu grupy zasobów. W związku z tym wygenerowanego szablonu nie są wyświetlane parametry konfiguracji, Włącz zarządzane tożsamość usługi zasobu. Poniższe sekcje mogą być dodawane ręcznie, wykonując przykłady w [konfigurowania tożsamości usługi maszyn wirtualnych zarządzanych za pomocą szablonu](msi-qs-configure-template-windows-vm.md).

Gdy funkcja eksportu schematu stają się dostępne dla rozszerzenia maszyny Wirtualnej MSI, będzie wyświetlane w [eksportowanie grupy zasobów, która zawiera rozszerzenia maszyny Wirtualnej](~/articles/virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

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

## <a name="known-issues-with-user-assigned-msi-private-preview-feature"></a>Znane problemy związane z MSI przypisany użytkownik *(prywatnej wersji zapoznawczej)*

- Jedynym sposobem, aby usunąć wszystkich użytkowników przypisanych msi przypisano systemowi MSI. 
- Inicjowanie obsługi rozszerzenia maszyny Wirtualnej do maszyny Wirtualnej może zakończyć się niepowodzeniem z powodu błędów wyszukiwania DNS. Uruchom ponownie maszynę Wirtualną, a następnie spróbuj ponownie. 
- Azure CLI: `Az resource show` i `Az resource list` zakończy się niepowodzeniem na maszynie Wirtualnej z użytkownikiem przypisane MSI. Jako obejście Użyj`az vm/vmss show`
- Samouczek usługi Azure magazynu jest dostępna tylko w centralnej nam EUAP w tej chwili. 
- Gdy MSI przypisany użytkownik uzyskuje dostęp do zasobu, bloku IAM dla tego zasobu pokazuje "Nie można uzyskać dostępu do danych". Jako obejście Użyj interfejsu wiersza polecenia, aby przeglądanie i edytowanie przypisania roli dla tego zasobu.
- Tworzenie użytkownika przypisanego MSI podkreślenia w nazwie, nie jest obsługiwane.
- Dodawanie drugiego użytkownika przypisany tożsamości, clientID może być dostępne dla żądań tokenów dla niego. Jako ograniczenie Uruchom ponownie rozszerzenia MSI maszyny Wirtualnej za pomocą następujących poleceń dwóch bash:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- VMAgent w systemie Windows nie obsługuje obecnie MSI przypisany użytkownik. 
- Przypisania ról Instalatora MSI w celu uzyskania dostępu do zasobu, obecnie nie wymaga specjalnych uprawnień. 
- Gdy użytkownik, któremu przypisano MSI ma Maszynę wirtualną, ale żaden system przydzielonych MSI, portalu interfejsu użytkownika będzie widoczna MSI jako włączona. Aby włączyć system przypisane MSI, użyj szablonu usługi Azure Resource Manager, Azure CLI lub zestawu SDK.
