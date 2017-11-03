---
title: "Konfigurowanie usługi równoważenia obciążenia dla bazy danych SQL zawsze na | Dokumentacja firmy Microsoft"
description: "Konfigurowanie równoważenia obciążenia do pracy z SQL zawsze na i sposób korzystania z programu powershell do utworzenia modułu równoważenia obciążenia dla wdrożenia SQL"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3ebbf1c4009d89b1f18b2ff8ff5dd243c456dff8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurowanie usługi równoważenia obciążenia dla bazy danych SQL zawsze na

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Obecnie można uruchomić z ILB grup dostępności AlwaysOn programu SQL Server. Grupa dostępności jest rozwiązania najważniejszych programu SQL Server, aby uzyskać wysoką dostępność i odzyskiwanie po awarii. Odbiornik grupy dostępności pozwala klienta aplikacjom bezproblemowo połączyć się repliką podstawową, niezależnie od liczby replik w konfiguracji.

Nazwa odbiornika (DNS) jest mapowany na adres IP z równoważeniem obciążenia i modułowi równoważenia obciążenia Azure kieruje ruch przychodzący do serwera podstawowego zestawu replik.

Obsługa ILB można użyć dla punktów końcowych AlwaysOn programu SQL Server (odbiornika). Teraz kontrolują dostępność odbiornika i wybrać adres IP usługi równoważenia obciążenia z określonej podsieci w sieci wirtualnej (VNet).

Za pomocą ILB na obiektu nasłuchującego punktu końcowego serwera SQL (np. serwera = tcp:ListenerName, 1433; baza danych = DatabaseName) jest dostępny tylko dla:

* Usług i maszyn wirtualnych w tej samej sieci wirtualnej
* Usług i maszyn wirtualnych z sieci połączonych lokalnie
* Usług i maszyn wirtualnych z połączonych sieci wirtualnych

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Rysunek 1 — skonfigurowana z modułem równoważenia obciążenia internetowy funkcji SQL AlwaysOn

## <a name="add-internal-load-balancer-to-the-service"></a>Dodawanie wewnętrznego modułu równoważenia obciążenia z usługą

1. W poniższym przykładzie mamy konfiguruje sieci wirtualnej, która zawiera podsieć o nazwie "Podsieć 1":

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Dodawanie punktów końcowych ze zrównoważonym obciążeniem dla ILB na każdej maszynie Wirtualnej

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    W powyższym przykładzie masz 2 maszyny Wirtualnej o nazwie "sqlsvc1" i "sqlsvc2" działa w chmurze usługi "Sqlsvc". Po utworzeniu ILB z `DirectServerReturn` przełącznika, można dodać punktów końcowych ze zrównoważonym obciążeniem do ILB, aby umożliwić SQL skonfigurować odbiorników dla grupy dostępności.

Aby uzyskać więcej informacji o funkcji SQL AlwaysOn, zobacz [skonfigurować wewnętrzny moduł równoważenia obciążenia dla grupy dostępności AlwaysOn w usłudze Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zobacz też
[Przed rozpoczęciem konfigurowania internetowy modułu równoważenia obciążenia](load-balancer-get-started-internet-arm-ps.md)

[Przed rozpoczęciem konfigurowania wewnętrznego modułu równoważenia obciążenia](load-balancer-get-started-ilb-arm-ps.md)

[Configure a Load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)
