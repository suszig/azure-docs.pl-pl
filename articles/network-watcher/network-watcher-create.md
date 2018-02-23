---
title: "Utwórz wystąpienie obserwatora sieciowego Azure | Dokumentacja firmy Microsoft"
description: "Ta strona przedstawia kroki tworzenia wystąpienia obserwatora sieciowego przy użyciu portalu i interfejsu API REST Azure"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fb1e3f9a93d18e949cb42ac0a4e09129cfe414f6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-an-azure-network-watcher-instance"></a>Utwórz wystąpienie obserwatora sieciowego Azure

Monitor sieci jest regionalnych usługa, która umożliwia monitorowanie i diagnozowanie warunki na poziomie sieci scenariusz w, do i z platformy Azure. Scenariusz poziomu monitorowania umożliwia diagnozowanie problemów w sieci pełnego widoku poziomu. Diagnostyka sieci i narzędzi wizualizacji dostępnych z obserwatora sieciowego pomagają zrozumieć, diagnozowanie i Uzyskaj wgląd do sieci na platformie Azure.

> [!NOTE]
> Obserwatora sieciowego aktualnie obsługuje tylko 1.0 interfejsu wiersza polecenia, z instrukcjami, aby utworzyć nowe wystąpienie obserwatora sieciowego jest dostępna dla interfejsu wiersza polecenia 1.0.

## <a name="create-a-network-watcher-in-the-portal"></a>Utwórz obserwatora sieciowego w portalu

Przejdź do **wszystkie usługi** > **sieci** > **sieci obserwatora**. Możesz wybrać wszystkie subskrypcje, które chcesz włączyć Monitor sieci. Ta akcja tworzy obserwatora sieciowego w każdym regionie dostępnej.

![Utwórz obserwatora sieciowego][1]

Po włączeniu obserwatora sieciowego przy użyciu portalu automatycznie można ustawić nazwę wystąpienia obserwatora sieciowego do NetworkWatcher_region_name gdzie region_name odpowiada regionu Azure, gdy wystąpienie zostało włączone.  Na przykład będzie nazwę obserwatora sieciowego, włączona w regionie Zachód środkowe stany NetworkWatcher_westcentralus

Ponadto wystąpienia obserwatora sieciowego automatycznie zostanie dodany do grupy zasobów o nazwie NetworkWatcherRG.  Ta grupa zasobów zostanie utworzony, jeśli jeszcze nie istnieje.

Jeśli chcesz dostosować nazwę wystąpienia obserwatora sieciowego i grupy zasobów jest umieszczany w, możesz użyć programu Powershell, interfejsu API REST lub ARMClient metod opisanych poniżej.  W każdej opcji grupy zasobów musi istnieć przed umieszczeniem obserwatora sieciowego do niego.  

## <a name="create-a-network-watcher-with-powershell"></a>Utwórz obserwatora sieciowego przy użyciu programu PowerShell

Aby utworzyć wystąpienie obserwatora sieciowego, należy uruchomić poniższy przykład:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Utwórz obserwatora sieciowego przy użyciu interfejsu API REST

ARMclient służy do wywołania interfejsu API REST przy użyciu programu PowerShell. ARMClient znajduje się na chocolatey w [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Zaloguj się za pomocą ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Utwórz obserwatora sieciowego

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Kolejne kroki

Teraz, gdy masz wystąpienie obserwatora sieciowego Dowiedz się więcej o funkcjach dostępnych:

* [Topologia](network-watcher-topology-overview.md)
* [Przechwytywania pakietów](network-watcher-packet-capture-overview.md)
* [Weryfikowanie przepływu adresów IP](network-watcher-ip-flow-verify-overview.md)
* [Następny przeskok](network-watcher-next-hop-overview.md)
* [Widok grupy zabezpieczeń](network-watcher-security-group-view-overview.md)
* [Rejestrowanie przepływu NSG](network-watcher-nsg-flow-logging-overview.md)
* [Rozwiązywanie problemów bramy sieci wirtualnej](network-watcher-troubleshoot-overview.md)

Po utworzeniu wystąpienia obserwatora sieciowego przechwytywania pakietu można skonfigurować, postępując w artykule: [utworzyć przechwytywania alertów wyzwalanych pakietów](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











