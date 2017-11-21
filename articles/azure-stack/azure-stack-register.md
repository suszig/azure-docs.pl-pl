---
title: Zarejestruj stosu Azure | Dokumentacja firmy Microsoft
description: "Zarejestruj stosu Azure z subskrypcją platformy Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: erikje
ms.openlocfilehash: 977630741b8424c4c6bd5f5d492e33b9981b9cb5
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Zarejestruj stosu Azure z subskrypcją platformy Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Możesz zarejestrować [stosu Azure](azure-stack-poc.md) z platformy Azure, aby pobrać elementów marketplace z platformy Azure i ustaw commerce dane raportowania z powrotem do firmy Microsoft. 

> [!NOTE]
>Rejestracja jest zalecane, ponieważ umożliwia testowanie ważne funkcje stosu Azure, takich jak zespolonego marketplace i raportowanie użycia. Po zarejestrowaniu stosu Azure użycia jest zgłaszane do handlu platformy Azure. Można to sprawdzić w ramach subskrypcji, używanego do rejestracji. Azure użytkowników stosu Development Kit nie są naliczane opłaty za bez użycia, które zgłaszają.
>


## <a name="get-azure-subscription"></a>Uzyskiwanie subskrypcji platformy Azure

Przed zarejestrowaniem stosu Azure przy użyciu platformy Azure, musi mieć:

- Identyfikator subskrypcji dla subskrypcji platformy Azure. Można uzyskać Identyfikatora, logowanie do platformy Azure, kliknij przycisk **więcej usług** > **subskrypcje**, kliknij subskrypcję, którego chcesz użyć, a następnie w obszarze **Essentials** można znaleźć **Identyfikator subskrypcji**. Chin, Niemczech i instytucji rządowych Stanów Zjednoczonych subskrypcje w chmurze nie są obecnie obsługiwane.
- Nazwa użytkownika i hasło dla konta, które jest właścicielem subskrypcji (kont MSA/2FA są obsługiwane).
- Azure Active Directory dla subskrypcji platformy Azure. Ten katalog można znaleźć na platformie Azure, ustawiając kursor nad Awatar w prawym górnym rogu portalu Azure. 

Jeśli nie masz subskrypcji platformy Azure, która spełnia te wymagania, możesz [utworzyć tutaj bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/free/?b=17.06). Rejestrowanie stosu Azure wiąże się bez kosztów w ramach subskrypcji platformy Azure.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Procedura Rejestruj dostawcę zasobów Azure stosu na platformie Azure
> [!NOTE] 
> Ten krok należy wykonać tylko raz w środowisku Azure stosu.
>

1. Uruchom sesję programu Powershell jako administrator.
2. Zaloguj się do konta platformy Azure, który jest właścicielem subskrypcji platformy Azure (służy polecenie cmdlet Login-AzureRmAccount do logowania i podczas logowania, należy ustawić parametr - EnvironmentName "AzureCloud").
3. Rejestrowanie dostawcy zasobów platformy Azure "Microsoft.AzureStack."

**Przykład:** 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```

## <a name="register-azure-stack-with-azure"></a>Zarejestruj stosu Azure przy użyciu platformy Azure

> [!NOTE]
> Kroki te należy uruchomić z na komputerze, który ma dostęp do uprzywilejowanych punktu końcowego. Dla usługi Azure stosu zestawu SDK byłoby komputera-hosta. Jeśli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure.
>

1. Otwórz konsolę programu PowerShell jako administrator i [zainstalować program PowerShell Azure stosu](azure-stack-powershell-install.md).  

2. Dodaj konto platformy Azure, który będzie używany do rejestrowania stosu Azure. Aby to zrobić, uruchom `Add-AzureRmAccount` polecenia cmdlet bez parametrów. Zostanie wyświetlony monit o wprowadzenie poświadczeń konta platformy Azure i może być konieczne użycie uwierzytelniania wieloskładnikowego 2 na podstawie konfiguracji Twoje konto.  

3. Jeśli masz wiele subskrypcji, uruchom następujące polecenie, wybierz ten, który ma być używany:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Usuń istniejące wersje moduły programu Powershell, które odpowiadają rejestracji i [Pobierz najnowszą wersję z witryny GitHub](azure-stack-powershell-download.md).  

5. W katalogu "AzureStack-Tools-master", utworzony w poprzednim kroku przejdź do folderu "Rejestracji" i zaimportuj moduł ".\RegisterWithAzure.psm1":  

   ```powershell 
   Import-Module .\RegisterWithAzure.psm1 
   ```

6. W tej samej sesji programu PowerShell Uruchom następujący skrypt. Po wyświetleniu monitu o poświadczenia, określ `azurestack\cloudadmin` jako użytkownika i hasła jest taka sama jak użytą dla administratora lokalnego podczas wdrażania.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Add-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -AzureSubscriptionId $AzureContext.Subscription.Id `
       -AzureDirectoryTenantName $AzureContext.Tenant.TenantId `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development 
   ```

   | Parametr | Opis |
   | -------- | ------------- |
   | CloudAdminCredential | Poświadczenia domeny chmury, które są używane do [dostęp uprzywilejowany punktu końcowego](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Nazwa użytkownika jest w formacie "\<domeny Azure stosu\>\cloudadmin". Dla zestawu SDK nazwa użytkownika jest równa "azurestack\cloudadmin". Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość.|
   | AzureSubscriptionId | Subskrypcji Azure, która służy do rejestrowania stosu Azure.|
   | AzureDirectoryTenantName | Nazwa katalogu dzierżawą platformy Azure, który jest skojarzony z subskrypcją platformy Azure. W tej dzierżawie katalogu zostanie utworzony zasób rejestracji. |
   | PrivilegedEndpoint | Wstępnie skonfigurowane zdalnego programu PowerShell konsoli, która dostarcza funkcje, takie jak zbierania dzienników oraz innych post zadań wdrażania. Dla zestawu SDK uprzywilejowane punktu końcowego znajduje się na maszynie wirtualnej "AzS ERCS01". Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość. Aby dowiedzieć się więcej, zapoznaj się [przy użyciu punktu końcowego uprzywilejowanych](azure-stack-privileged-endpoint.md) tematu.|
   | BillingModel | Modelu rozliczeń, który korzysta z subskrypcji. Dozwolone wartości dla tego parametru są "Pojemność", "PayAsYouUse" i "Programowanie". Dla zestawu SDK ta wartość jest równa "Programowanie". Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość. |

7. Po ukończeniu działania skryptu, zostanie wyświetlony komunikat "Azure uaktywnianie stosu (ten krok może potrwać do 10 minut)." 

## <a name="verify-the-registration"></a>Sprawdź rejestrację

1. Zaloguj się do portalu administratora (https://adminportal.local.azurestack.external).
2. Kliknij przycisk **więcej usług** > **zarządzania Marketplace** > **dodać z platformy Azure**.
3. Jeśli zobaczysz listę dostępnych z platformy Azure (na przykład WordPress) elementów, proces aktywacji zakończyło się pomyślnie.

> [!NOTE]
> Po zakończeniu rejestracji aktywne ostrzeżenie nie rejestracji nie będzie dłużej widoczny.

## <a name="next-steps"></a>Następne kroki

[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

