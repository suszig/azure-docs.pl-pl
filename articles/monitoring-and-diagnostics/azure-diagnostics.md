---
title: "Omówienie diagnostyki Azure | Dokumentacja firmy Microsoft"
description: "Użyj diagnostyki Azure dla debugowania pomiaru wydajności, monitorowanie, analiza ruchu w usługi w chmurze, maszyn wirtualnych i sieci szkieletowej usług"
services: multiple
documentationcenter: .net
author: rboucher
manager: 
editor: 
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 0c6e4d9d2a3744f607b72364f3944c700acd070c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-diagnostics"></a>Co to jest Azure Diagnostics
Diagnostyka Azure jest możliwość w ramach platformy Azure, która umożliwia zbieranie danych diagnostycznych na wdrożonej aplikacji. Można użyć rozszerzenia diagnostyki z wielu różnych źródeł. Obecnie obsługiwane są Azure Cloud Service w sieci Web i proces roboczy, maszynach wirtualnych platformy Azure z systemem Microsoft Windows i sieci szkieletowej usług. Innymi usługami Azure mają własne oddzielne diagnostyki.

## <a name="data-you-can-collect"></a>Dane, które można zbierać
Diagnostyka Azure można zebrać następujące typy danych:

| Źródło danych | Opis |
| --- | --- |
| Liczniki wydajności |System operacyjny i niestandardowe liczniki wydajności |
| Dzienniki aplikacji |Wiadomości śledzenia zapisanych przez aplikację |
| Dzienniki zdarzeń systemu Windows |Informacje wysyłane do rejestrowania zdarzeń systemu |
| Źródło zdarzenia platformy .NET |Kod zapisywania zdarzeń przy użyciu programu .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) — klasa |
| Dzienniki programu IIS |Informacje o witryn sieci web usług IIS |
| Manifest na podstawie ETW |Zdarzenia śledzenia dla systemu Windows zdarzenia generowane przez żaden proces |
| Zrzuty awaryjne |Informacje o stanie procesu w przypadku awarii aplikacji |
| Dzienniki błędów niestandardowych |Dzienniki tworzone przez usługi lub aplikacji |
| Dzienniki diagnostyczne infrastruktury platformy Azure |Informacje o diagnostyce sam |

Rozszerzenia diagnostyki Azure można transferu tych danych do konta magazynu Azure lub wysyłają je do usług, takich jak [usługi Application Insights](../application-insights/app-insights-cloudservices.md). Dane można użyć do debugowania i rozwiązywania problemów, pomiaru wydajności monitorowania użycia zasobów, analizy ruchu i Planowanie wydajności i inspekcji.

## <a name="versioning"></a>Obsługa wersji
Zobacz [historii Wersjonowania diagnostyki Azure](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Następne kroki
Wybierz usługi, które próbujesz na zbieranie danych diagnostycznych i skorzystaj z poniższych artykułów, aby rozpocząć pracę. Użyj łączy ogólne diagnostyki Azure dla odwołania do wykonywania określonych zadań.

## <a name="web-apps"></a>Web Apps
Należy pamiętać, że aplikacje sieci Web nie diagnostyki Azure. Znajdź równoważnych informacji w [aplikacji sieci Web](../app-service/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Usługi w chmurze za pomocą diagnostyki Azure
* Jeśli używasz programu Visual Studio, zobacz [program Visual Studio do śledzenia aplikacji usługi w chmurze](../vs-azure-tools-debug-cloud-services-virtual-machines.md) rozpocząć pracę. W przeciwnym razie zobacz
* [Jak monitorować usługi w chmurze za pomocą diagnostyki Azure](../cloud-services/cloud-services-how-to-monitor.md)
* [Konfigurowanie diagnostyki Azure w aplikacji usługi w chmurze](../cloud-services/cloud-services-dotnet-diagnostics.md)

Aby uzyskać bardziej zaawansowanych tematów zobacz

* [Za pomocą diagnostyki Azure z usługą Application Insights dla usługi w chmurze](../application-insights/app-insights-cloudservices.md)
* [Śledzenie przepływu aplikacji usługi w chmurze Diagnostyka Azure](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Aby skonfigurować diagnostykę na usługi w chmurze za pomocą programu PowerShell](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines-using-azure-diagnostics"></a>Maszyny wirtualne za pomocą diagnostyki Azure
* Jeśli używasz programu Visual Studio, zobacz [program Visual Studio do śledzenia maszyny wirtualne Azure](../vs-azure-tools-debug-cloud-services-virtual-machines.md) rozpocząć pracę. W przeciwnym razie zobacz
* [Konfigurowanie diagnostyki Azure na maszynie wirtualnej platformy Azure](../virtual-machines-dotnet-diagnostics.md)

Aby uzyskać bardziej zaawansowanych tematów zobacz

* [Aby skonfigurować diagnostykę na maszynach wirtualnych platformy Azure za pomocą programu PowerShell](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Utwórz maszynę wirtualną systemu Windows z monitorowania i diagnostyki za pomocą szablonu usługi Resource Manager Azure](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>Sieć szkieletowa usług za pomocą diagnostyki Azure
Rozpoczynanie pracy w [monitorowanie aplikacji sieci szkieletowej usług](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Wiele innych artykułów diagnostyki sieci szkieletowej usług są dostępne w drzewie nawigacji po lewej stronie, gdy pojawi się w tym artykule.

## <a name="general-azure-diagnostics-articles"></a>Artykuły diagnostyki Azure ogólne
* [Konfiguracja schematu diagnostyki Azure](https://msdn.microsoft.com/library/azure/mt634524.aspx) -Zmienianie pliku schematu i gromadzone dane diagnostyczne. Należy pamiętać, można również użyć programu Visual Studio do zmiany pliku schematu.
* [Sposób przechowywania danych diagnostyki Azure w usłudze Azure Storage](../cloud-services/cloud-services-dotnet-diagnostics-storage.md) -znać nazwy tabel i obiektów blob, w którym zapisywana jest danych diagnostycznych.
* Dowiedz się, jak [użyć liczników wydajności w diagnostyce Azure](../cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
* Dowiedz się, jak [Azure trasy informacje diagnostyczne do usługi Application Insights](azure-diagnostics-configure-application-insights.md)
* Jeśli masz problem z uruchomieniem diagnostyki lub wyszukiwanie danych w tabelach usługi Azure Storage, zobacz [rozwiązywania problemów Diagnostyka Azure](azure-diagnostics-troubleshooting.md)
