---
title: "Resetowanie nie powiodło się obwodu Azure ExpressRoute: programu PowerShell | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomoże Ci zresetować obwodu usługi ExpressRoute, który znajduje się w stanie awarii."
documentationcenter: na
services: expressroute
author: anzaman
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 0e017200193de3e4a02275cec3b09c32f1fa5c31
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="reset-a-failed-expressroute-circuit"></a>Resetowanie nie powiodło się obwodu usługi expressroute

Podczas operacji obwodu usługi ExpressRoute nie zostanie zakończona pomyślnie, obwodu przejść w stan "nie powiodło się". Ten artykuł pomoże Ci zresetować obwodu Azure ExpressRoute nie powiodło się.

## <a name="reset-a-circuit"></a>Resetowanie obwodu

1. Zainstaluj najnowszą wersję poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

2. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

  ```powershell
  Login-AzureRmAccount
  ```
3. Jeśli masz wiele subskrypcji platformy Azure, wyświetl subskrypcje dla konta.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Wskaż subskrypcję, której chcesz użyć.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Uruchom następujące polecenia, aby zresetować obwód, który jest w stanie niepowodzenia:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Obwód powinno być teraz w dobrej kondycji. Otwórz bilet pomocy technicznej z [pomocy technicznej firmy Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Jeśli obwodu jest nadal w stanie niepowodzenia.

## <a name="next-steps"></a>Następne kroki

Otwórz bilet pomocy technicznej z [pomocy technicznej firmy Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Jeśli nadal występują problemy.
