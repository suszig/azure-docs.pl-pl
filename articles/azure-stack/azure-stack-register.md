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
ms.date: 10/27/2017
ms.author: erikje
ms.openlocfilehash: 24cde66a132ae2e1ba0eb9b1564915746e5ca448
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
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
- [Zarejestrowano dostawcę zasobów Azure stosu](#register-azure-stack-resource-provider-in-azure).

Jeśli nie masz subskrypcji platformy Azure, która spełnia te wymagania, możesz [utworzyć tutaj bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/free/?b=17.06). Rejestrowanie stosu Azure wiąże się bez kosztów w ramach subskrypcji platformy Azure.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Procedura Rejestruj dostawcę zasobów Azure stosu na platformie Azure
> [!NOTE] 
> Ten krok tylko musi zostać zakończona raz w środowisku Azure stosu.
>

1. Uruchom ISE programu Powershell jako administrator.
2. Zaloguj się do konta platformy Azure, który jest właścicielem subskrypcji platformy Azure z parametrem - EnvironmentName ustawioną wartość "AzureCloud".
3. Rejestrowanie dostawcy zasobów platformy Azure "Microsoft.AzureStack".

Przykład: 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```


## <a name="register-azure-stack-with-azure"></a>Zarejestruj stosu Azure przy użyciu platformy Azure

> [!NOTE]
>Wszystkie te czynności musi wykonać na komputerze hosta.
>

1. [Instalowanie programu PowerShell dla usługi Azure stosu](azure-stack-powershell-install.md). 
2. Kopiuj [skryptu RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) do folderu (na przykład C:\Temp).
3. Uruchom program PowerShell ISE jako administrator, a następnie zaimportuj moduł RegisterWithAzure.    
4. W skrypcie RegisterWithAzure.psm1 uruchom moduł AzsRegistration Dodaj. Zastąp symbole zastępcze następujące: 
    - *YourCloudAdminCredential* jest obiekt programu PowerShell, który zawiera poświadczenia domeny lokalnej domain\cloudadmin (dla zestawu SDK, jest to azurestack\cloudadmin).
    - *YourAzureSubscriptionID* jest identyfikator subskrypcji Azure, która ma być używany do rejestrowania stosu Azure.
    - *YourAzureDirectoryTenantName* jest nazwą katalogu dzierżawą platformy Azure skojarzone z subskrypcją platformy Azure. W tej dzierżawie katalogu zostanie utworzony zasób rejestracji. 
    - *YourPrivilegedEndpoint* jest nazwą [uprzywilejowanych punktu końcowego](azure-stack-privileged-endpoint.md).

    ```powershell
    Add-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -AzureDirectoryTenantName $YourAzureDirectoryTenantName  -AzureSubscriptionId $YourAzureSubscriptionId -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Development 
    ```
5. W oknie podręcznym logowania wprowadź swoje poświadczenia subskrypcji platformy Azure.

## <a name="verify-the-registration"></a>Sprawdź rejestrację

1. Zaloguj się do portalu administratora (https://adminportal.local.azurestack.external).
2. Kliknij przycisk **więcej usług** > **zarządzania Marketplace** > **dodać z platformy Azure**.
3. Jeśli zobaczysz listę dostępnych z platformy Azure (na przykład WordPress) elementów, proces aktywacji zakończyło się pomyślnie.

> [!NOTE]
> Po zakończeniu rejestracji aktywne ostrzeżenie nie rejestracji nie będzie dłużej widoczny.

## <a name="next-steps"></a>Następne kroki

[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

