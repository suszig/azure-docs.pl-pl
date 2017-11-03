---
title: Brama starszej wersji sieci wirtualnej platformy Azure jednostki SKU | Dokumentacja firmy Microsoft
description: Stary wirtualnych sieci jednostki SKU bramy.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2017
ms.author: cherylmc
ms.openlocfilehash: d5127c7fa512bad49817fa4c8edf3a16ca2f7d60
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Praca z bramy sieci wirtualnej jednostki SKU (starszej wersji jednostki SKU)

Ten artykuł zawiera informacje dotyczące starszych bramy sieci wirtualnej (stare) jednostki SKU. Starszego jednostki SKU nadal działa w obu modelach wdrażania dla bram sieci VPN, które zostały już utworzone. Klasycznego bramy sieci VPN w dalszym ciągu używać starszej wersji jednostki SKU, zarówno dla bramy istniejących i nowych bram. Podczas tworzenia nowego Menedżera zasobów sieci VPN bramy, użyj nowej jednostki SKU bramy. Informacje o nowej wersji produktu, zobacz [o bramy sieci VPN](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Jednostki SKU bramy

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Szacowany łącznej przepływności przez jednostki SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Obsługiwane konfiguracje według typu jednostki SKU i sieci VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Zmień rozmiar bramy (zmienianie jednostka SKU bramy)

Można zmienić rozmiar jednostka SKU bramy w ramach tej samej rodziny SKU. Na przykład jeśli masz wersji Standard, możesz zmienić rozmiar do SKU wysokowydajnej. Nie można zmienić rozmiaru z bramy sieci VPN między starym jednostki SKU i nowe rodziny SKU. Nie można na przykład przejść z wersji Standard, do wersji VpnGw2.

>[!IMPORTANT]
>Podczas zmiany rozmiaru bramy będzie mieć 20-30 minut przestoju dla tej bramy, gdy jest zmieniany.
>
>

Aby zmienić rozmiar jednostki SKU bramy dla klasycznym modelu wdrożenia, użyj następującego polecenia:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Aby zmienić rozmiar jednostki SKU bramy dla modelu wdrażania usługi Resource Manager, użyj następującego polecenia:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migracja do nowej bramy jednostki SKU

Jeśli pracujesz z modelu wdrażania usługi Resource Manager, można migrować do nowej bramy jednostki SKU. Jeśli pracujesz z klasycznym modelu wdrożenia, nie można migrować do nowej jednostki SKU i zamiast tego należy nadal używać starszej wersji jednostki SKU.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o nowej jednostki SKU bramy, zobacz [jednostki SKU bramy](vpn-gateway-about-vpngateways.md#gwsku).

Aby uzyskać więcej informacji na temat ustawień konfiguracji, zobacz [o bramy sieci VPN, ustawienia konfiguracji](vpn-gateway-about-vpn-gateway-settings.md).
