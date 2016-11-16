---
title: "Wprowadzenie do tworzenia modułu równoważenia obciążenia mającego połączenie z Internetem w trybie klasycznym przy użyciu programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostępny z Internetu moduł równoważenia obciążenia w trybie klasycznym przy użyciu programu PowerShell"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7344f2c3eeb7d52f8bc60e564d66b2cc51f10f75

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Wprowadzenie do tworzenia dostępnego z Internetu modułu równoważenia obciążenia (klasycznego) przy użyciu programu PowerShell

[!INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano klasyczny model wdrażania. Możesz też zapoznać się z artykułem na temat [tworzenia dostępnego z Internetu modułu równoważenia obciążenia za pomocą usługi Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>Konfiguracja modułu równoważenia obciążenia za pomocą programu PowerShell

Aby skonfigurować moduł równoważenia obciążenia za pomocą programu PowerShell, wykonaj poniższe kroki:

1. Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami aż do momentu logowania się w programie Azure i wyboru subskrypcji.
2. Po utworzeniu maszyny wirtualnej możesz użyć poleceń cmdlet programu PowerShell, aby dodać moduł równoważenia obciążenia do maszyny wirtualnej w ramach tej samej usługi w chmurze.

W poniższym przykładzie opisano sposób dodania zestawu modułu równoważenia obciążenia o nazwie „webfarm” do usługi w chmurze „mytestcloud” (lub myctestcloud.cloudapp.net), przy jednoczesnym dodaniu punktów końcowych modułu równoważenia obciążenia do maszyn wirtualnych o nazwach „web1” i „web2”. Moduł równoważenia obciążenia odbiera ruch sieciowy przez port 80, a równoważenie obciążenia między maszynami wirtualnymi jest definiowane przez lokalny punkt końcowy (w tym przypadku port 80) przy użyciu protokołu TCP.

### <a name="step-1"></a>Krok 1

Tworzenie punktu końcowego o zrównoważonym obciążeniu dla pierwszej maszyny wirtualnej „web1”

```powershell
    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>Krok 2

Tworzenie kolejnego punktu końcowego dla drugiej maszyny wirtualnej „web2” za pomocą zestawu modułu równoważenia obciążenia o tej samej nazwie

```powershell
    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Usuwanie maszyny wirtualnej z modułu równoważenia obciążenia

Możesz wykorzystać polecenie Remove-AzureEndpoint, aby usunąć punkt końcowy maszyny wirtualnej z modułu równoważenia obciążenia

```powershell
    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM
```

## <a name="next-steps"></a>Następne kroki

Możesz też zapoznać się z artykułem na temat [wprowadzenia do tworzenia wewnętrznego modułu równoważenia obciążenia](load-balancer-get-started-ilb-classic-ps.md) i określić typ [trybu dystrybucji](load-balancer-distribution-mode.md) dotyczącego danego ruchu sieciowego modułu równoważenia obciążenia.

Jeśli zastosowanie wymaga zachowania działających połączeń z serwerami za modułem równoważenia obciążenia, zapoznaj się z informacjami o [ustawieniach limitu czasu bezczynności protokołu TCP modułu równoważenia obciążenia](load-balancer-tcp-idle-timeout.md). Pozwoli to zrozumieć zachowanie bezczynnego połączenia podczas używania usługi Azure Load Balancer.



<!--HONumber=Nov16_HO2-->


