---
title: "Dotyczące monitorowania sieci w Log Analytics | Dokumentacja firmy Microsoft"
description: "Omówienie rozwiązania NPM, zarządzać sieciami środowiskach chmury, lokalne i hybrydowe monitorowania sieci."
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 6793bd8d2c561554213e9fe645aab018dba7a925
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="network-monitoring-solutions"></a>Monitorowanie rozwiązań sieci 

System Azure oferuje hosta rozwiązań do monitorowania zasobów sieciowych. Platforma Azure ma rozwiązania i narzędzia do monitorowania łączność z siecią, kondycję obwody usługi ExpressRoute i analizowania ruchu sieciowego w chmurze.

## <a name="network-performance-monitor-npm"></a>Monitor wydajności sieci (NPM)

Monitor wydajności sieci (NPM) to zestaw funkcji, z których każdy jest przeznaczone dla monitorowanie kondycji sieci, połączenie sieciowe z aplikacjami i zapewnia wgląd w wydajności sieci. NPM jest oparta na chmurze i udostępnia sieci hybrydowe rozwiązanie monitorujące, które monitoruje łączność między:
 
* Lokalizacje wdrożeń i lokalnej w chmurze
* Wiele centrów danych oraz biura oddziałów
* Misji wielowarstwowe aplikacje/micro-obsługi usług krytycznych
* Lokalizacji użytkownika i aplikacji sieci web (HTTP/HTTPs) 

## <a name="performance-monitor"></a>Monitorowanie wydajności

Monitor wydajności jest częścią programu NPM i jest monitorowanie sieci dla chmury, hybrydowej i lokalnych środowisk. Można monitorować łączność sieciową między gałęzi zdalnej i pola oddziałów, lokalizacji magazynu centrów danych i chmur. Użytkownicy skarżą się, można wykrywać problemy z siecią. Dostępne są następujące kluczowe korzyści:

* Monitorowanie utraty i opóźnienia za pośrednictwem różnych podsieci i Ustaw alerty
* Monitorowanie wszystkich ścieżek (w tym nadmiarowe ścieżki) w sieci
* Przejściowych i w momencie Rozwiązywanie problemów z sieci, które są trudne do replikacji
* Określić określonego segmentu w sieci, która jest odpowiedzialna za pogorszenie wydajności
* Monitorowanie kondycji sieci, bez konieczności SNMP

Aby uzyskać więcej informacji Wyświetl następujące artykuły:

* [Konfigurowanie rozwiązania monitora wydajności sieci w analizy dzienników](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Przypadki użycia](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Aktualizacje produktu: [lutego 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [2017 sierpnia](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>Monitor usługi ExpressRoute

NPM dla ExpressRoute oferuje kompleksowe monitorowanie usługi ExpressRoute dla prywatnej komunikacji równorzędnej połączeń. Można monitorować łączność E2E i wydajności między oddziałach i Azure za pośrednictwem usługi ExpressRoute. Do najważniejszych możliwości należą:

* Automatyczne wykrywanie obwody ER skojarzone z subskrypcją
* Wykrywanie topologii sieci z lokalnej do aplikacji w chmurze
* Planowanie pojemności, analiza wykorzystania
* Monitorowanie i alerty na głównych i dodatkowych ścieżek
* Wykryj spadek łączność sieci wirtualnych

Aby uzyskać więcej informacji zobacz następujące artykuły:

* [Konfigurowanie monitora wydajności sieci dla usługi ExpressRoute](../expressroute/how-to-npm.md)
* [wpis w blogu](https://aka.ms/NPMExRmonitorGA)

## <a name="next-steps"></a>Kolejne kroki

* [Konfigurowanie monitora wydajności sieci](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor)
* [Konfigurowanie monitora wydajności sieci dla usługi ExpressRoute](../expressroute/how-to-npm.md)