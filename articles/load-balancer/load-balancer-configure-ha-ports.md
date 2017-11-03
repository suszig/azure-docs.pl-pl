---
title: "Konfigurowanie wysokiej dostępności portów dla usługi równoważenia obciążenia Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie korzystania z portów wysokiej dostępności do wewnętrznego ruchu na wszystkich portach równoważenia obciążenia"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: 7256548b988812c64ca9a9f8a84fec377646635d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>Jak skonfigurować porty wysokiej dostępności dla wewnętrznego modułu równoważenia obciążenia

W tym artykule przedstawiono przykładowe wdrożenie wysokiej dostępności (HA) porty na wewnętrzny moduł równoważenia obciążenia. Urządzenie wirtualne sieci określonej konfiguracji, można znaleźć odpowiedniej witryny sieci Web dostawcy.

>[!NOTE]
> Funkcja wysokiej dostępności portów jest obecnie w przeglądzie. W okresie obowiązywania wersji zapoznawczej ta funkcja może nie oferować dostępności i niezawodności na tym samym poziomie, co funkcje w wersji ogólnodostępnej. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Rysunek 1 pokazuje następującą konfigurację przykład wdrożenia, opisane w tym artykule:
- NVAs są wdrażane w puli zaplecza wewnętrznego modułu równoważenia obciążenia za konfiguracji portów wysokiej dostępności. 
- PRZEZ stosowane na trasach podsieci DMZ cały ruch do <>? Dzięki następnego przeskoku jako wewnętrznego wirtualnego IP usługi równoważenia obciążenia. 
- Wewnętrzny moduł równoważenia obciążenia dystrybuuje ruch do jednego z active NVAs zgodnie z algorytmem LB.
- Analizę NVA przetwarza ruchu i przekazuje je do początkowego miejsca docelowego w podsieci wewnętrznej bazy danych.
- Ścieżka zwracanego mogą również czerpać tej samej ścieżki, jeśli odpowiednie przez jest skonfigurowany w podsieci wewnętrznej bazy danych. 

![ha porty Przykładowe wdrożenie](./media/load-balancer-configure-ha-ports/haports.png)

Rysunek 1 — wirtualnych urządzeń sieciowych wdrożony za wewnętrzny moduł równoważenia obciążenia z portami wysokiej dostępności 

## <a name="preview-sign-up"></a>Podgląd rejestracji

Aby wziąć udział w wersji zapoznawczej funkcji porty wysokiej dostępności w wersji Standard usługi równoważenia obciążenia, zarejestruj subskrypcji do uzyskania dostępu za pomocą programu PowerShell lub 2.0 interfejsu wiersza polecenia platformy Azure.

- Zaloguj przy użyciu programu PowerShell

   ```powershell
   Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```

- Zarejestruj się przy użyciu usługi Azure CLI 2.0

    ```cli
  az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network  
    ```

## <a name="configuring-ha-ports"></a>Konfigurowanie portów wysokiej dostępności

Konfiguracja portów HA obejmuje Konfigurowanie wewnętrzny moduł równoważenia obciążenia, z NVAs w puli zaplecza odpowiedniej konfiguracji usługi równoważenia obciążenia kondycji sondowania wykrywanie kondycji NVA i reguły równoważenia obciążenia z portami wysokiej dostępności. Ogólne elementami konfiguracji usługi równoważenia obciążenia jest objęte [wprowadzenie](load-balancer-get-started-ilb-arm-portal.md). W tym artykule omówiono konfiguracji portów wysokiej dostępności.

Konfiguracja obejmuje zasadniczo, ustawiając wartość portu portu i wewnętrznej bazy danych serwera sieci Web **0**i wartość protokołu **wszystkich**. W tym artykule opisano sposób konfigurowania portów wysokiej dostępności przy użyciu portalu Azure, programu PowerShell i Azure CLI 2.0.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Skonfiguruj reguły modułu równoważenia obciążenia porty wysokiej dostępności przy użyciu portalu Azure

Azure portal zawiera **HA porty** opcji za pomocą pola wyboru dla tej konfiguracji. Po wybraniu elementami konfiguracji portu i protokołu jest wypełniane automatycznie. 

![ha porty konfiguracji za pomocą portalu Azure](./media/load-balancer-configure-ha-ports/haports-portal.png)

Rysunek 2 - HA konfiguracji portów za pomocą portalu

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>Skonfiguruj reguły modułu równoważenia obciążenia porty wysokiej dostępności przy użyciu programu PowerShell

Aby utworzyć regułę HA portów usługi równoważenia obciążenia podczas tworzenia wewnętrznego modułu równoważenia obciążenia przy użyciu programu PowerShell, użyj następującego polecenia:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Skonfiguruj reguły modułu równoważenia obciążenia porty HA 2.0 interfejsu wiersza polecenia platformy Azure

W kroku nr 4 [tworzenia wewnętrznego obciążenia równoważenia ustawić](load-balancer-get-started-ilb-arm-cli.md), użyj następującego polecenia, można utworzyć porty HA reguły modułu równoważenia obciążenia.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [porty wysokiej dostępności](load-balancer-ha-ports-overview.md)
