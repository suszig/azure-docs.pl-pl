---
title: "Analizować wzorce użycia usługi Azure CDN | Dokumentacja firmy Microsoft"
description: "Klienta można włączyć analizy dziennika dla usługi Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: ce9ec021250a3548e23ad87273a9225cdf700c70
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analizować wzorce użycia usługi Azure CDN

Po włączeniu sieci CDN w warstwie aplikacji monitorowania użycia sieci CDN w warstwie, Sprawdź kondycję firmy i rozwiązać potencjalne problemy. Usługi Azure CDN zapewnia te możliwości w następujący sposób: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Podstawowa analiza za pomocą dzienników diagnostycznych platformy Azure

Podstawowa analiza jest dostępna dla wszystkich punktów końcowych usługi CDN należących do profilów sieci CDN (standardowe) Akamai i Verizon (Standard i Premium). Dzienniki diagnostyczne platformy Azure umożliwiają podstawowa analiza mają być eksportowane do usługi Azure storage, centra zdarzeń lub analizy dzienników Operations Management Suite (OMS). Analiza dzienników OMS oferuje rozwiązanie o wykresy użytkownika można skonfigurować i dostosować. Aby uzyskać więcej informacji, zobacz [Azure dzienników diagnostycznych](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon podstawowych raportów

Jako użytkownik usługi Azure CDN Verizon standard lub profilu premium Verizon Verizon core raporty można wyświetlić w portalu uzupełniającym Verizon. Verizon podstawowych raportów jest dostępny za pośrednictwem **Zarządzaj** opcji w portalu Azure i oferuje wiele widoków i wykresy. Aby uzyskać więcej informacji, zobacz [raporty Core from Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Raporty niestandardowe Verizon

Jako użytkownik usługi Azure CDN Verizon standard lub profilu premium Verizon Verizon niestandardowych raportów można wyświetlić w portalu uzupełniającym Verizon. Verizon niestandardowych raportów jest dostępny za pośrednictwem **Zarządzaj** opcji w portalu Azure. Pokazuje stronę niestandardowych raportów Verizon liczba trafień lub danych transferu dla każdego krawędzi CName należących do profilu usługi Azure CDN. Dane można przedstawić w rozbiciu stanu odpowiedzi HTTP kod lub pamięci podręcznej w dowolnym czasie. Aby uzyskać więcej informacji, zobacz [raporty niestandardowe from Verizon](cdn-verizon-custom-reports.md).

## <a name="verizon-premium-reports"></a>Raporty premium Verizon

Z **Azure CDN Premium from Verizon**, można także przejść do następujących raportów:
   * [Zaawansowane raporty HTTP](cdn-advanced-http-reports.md)
   * [Statystyki w czasie rzeczywistym](cdn-real-time-stats.md)
   * [Wydajności węzłów brzegowych](cdn-edge-performance.md)

