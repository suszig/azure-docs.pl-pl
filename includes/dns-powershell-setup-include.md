---
title: Uwzględnij plik dla środowiska PowerShell dla usługi Azure DNS
description: Uwzględnij plik dla środowiska PowerShell dla usługi Azure DNS
services: dns
author: subsarma
ms.service: dns
ms.topic: include file for PowerShell for Azure DNS
ms.date: 03/21/2018
ms.author: subsarma
ms.custom: include file for PowerShell for Azure DNS
ms.openlocfilehash: e96b312f03069256396261bd6efe2f2586cdadea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Konfigurowanie programu Azure PowerShell dla usługi Azure DNS

### <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami:

* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
* Musisz zainstalować najnowszą wersję poleceń cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs).

Ponadto do korzystania ze stref prywatne (publicznej wersji zapoznawczej), należy upewnić się, że poniższe moduły programu PowerShell i wersje. 
* AzureRM.Dns - [wersji 4.1.0](https://www.powershellgallery.com/packages/AzureRM.Dns/4.1.0) lub nowszy
* AzureRM.Network - [wersji 5.4.0](https://www.powershellgallery.com/packages/AzureRM.Network/5.4.0) lub nowszy

Pobierz powyżej moduły z galerii programu PowerShell przy użyciu łączy powyżej obok wersji modułu. Następnie można zainstalować przy użyciu poniższych poleceń. Moduły oba są wymagane i są w pełni wstecznie zgodne. 

```powershell
Install-Module -Name AzureRM.Dns -Force
```

```powershell
Install-Module -Name AzureRM.Network -Force
```

### <a name="sign-in-to-your-azure-account"></a>Zaloguj się do swojego konta platformy Azure

Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Aby uzyskać więcej informacji, zobacz [przy użyciu programu PowerShell z usługą Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Wybierz subskrypcję
 
Sprawdź subskrypcje dostępne na koncie.

```powershell
Get-AzureRmSubscription
```

Wybierz subskrypcję platformy Azure do użycia.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Ponieważ jednak wszystkie zasoby DNS są globalne, a nie regionalne, wybór lokalizacji grupy zasobów nie ma wpływu na usługę Azure DNS.

Ten krok można pominąć, jeśli używasz istniejącej grupy zasobów.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Rejestrowanie dostawcy zasobów

Usługa Azure DNS jest zarządzana przez dostawcę zasobów Microsoft.Network. Aby można było korzystać z usługi Azure DNS, Twoja subskrypcja platformy Azure musi być zarejestrowana do używania tego dostawcy zasobów. Jest to jednorazowa operacja dla każdej subskrypcji.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```