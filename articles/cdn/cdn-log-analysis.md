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
ms.openlocfilehash: 04e5499011e72dfcc20dff370d5d837227ed29b6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analizować wzorce użycia usługi Azure CDN

Po włączeniu sieci CDN w warstwie aplikacji monitorowania użycia sieci CDN w warstwie, Sprawdź kondycję firmy i rozwiązać potencjalne problemy. Usługi Azure CDN zapewnia te możliwości w następujących dwóch sposobów: 

## <a name="verizon-core-reports"></a>Verizon podstawowych raportów

Jako użytkownik usługi Azure CDN Verizon standard lub profilu premium Verizon Verizon Core raporty można wyświetlić w portalu uzupełniającym Verizon. Verizon podstawowych raportów jest dostępny za pośrednictwem **Zarządzaj** opcji w portalu Azure i oferuje wiele widoków i wykresy. Aby uzyskać więcej informacji, zobacz [raporty Core from Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Raporty niestandardowe Verizon

Jako użytkownik usługi Azure CDN Verizon standard lub profilu premium Verizon można wyświetlić w portalu uzupełniającym Verizon Verizon raporty niestandardowe. Raporty niestandardowe Verizon jest dostępny za pośrednictwem **Zarządzaj** opcji w portalu Azure. Pokazuje stronę raporty niestandardowe Verizon liczba trafień lub danych transferu dla każdego krawędzi CName należących do profilu usługi Azure CDN. Dane można przedstawić w rozbiciu stanu odpowiedzi HTTP kod lub pamięci podręcznej w dowolnym czasie. Aby uzyskać więcej informacji, zobacz [raporty niestandardowe from Verizon](cdn-verizon-custom-reports.md).

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Podstawowa analiza za pomocą dzienników diagnostycznych platformy Azure

Podstawowa analiza jest dostępna dla wszystkich punktów końcowych usługi CDN należących do profilów sieci CDN (standardowe) Akamai i Verizon (Standard i Premium). Dzienniki diagnostyczne platformy Azure umożliwiają podstawowa analiza mają być eksportowane do usługi Azure storage, centra zdarzeń lub analizy dzienników Operations Management Suite (OMS). Analiza dzienników OMS oferuje rozwiązanie o wykresy użytkownika można skonfigurować i dostosować. Aby uzyskać więcej informacji, zobacz [Azure dzienników diagnostycznych](cdn-azure-diagnostic-logs.md).

