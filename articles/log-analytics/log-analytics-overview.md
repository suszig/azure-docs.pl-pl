---
title: "Co to jest usługa Log Analytics na platformie Azure? | Microsoft Docs"
description: "Log Analytics to usługa platformy Azure ułatwiająca zbieranie i analizę danych operacyjnych generowanych przez zasoby w środowisku chmurowym i lokalnym.  Ten artykuł zawiera krótkie omówienie różnych składników usługi Log Analytics i linki do szczegółowych treści na ich temat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: bwren
ms.openlocfilehash: a95528f5bd259a36ea96c7bc0660ca082c09d6e6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="what-is-log-analytics"></a>Co to jest usługa Log Analytics?
Log Analytics to usługa platformy Azure, która monitoruje środowiska chmurowe i lokalne w celu zachowania ich dostępności i wydajności.  Zbiera ona dane generowane przez zasoby w środowiskach chmurowych i lokalnych oraz inne narzędzia do monitorowania, aby przeprowadzać analizę na podstawie wielu źródeł.  Ten artykuł zawiera krótkie omówienie wartości oferowanej przez usługę Log Analytics i sposobu jej działania oraz linki do bardziej szczegółowej zawartości, dzięki której możesz dowiedzieć się więcej na ten temat.

## <a name="is-log-analytics-for-you"></a>Czy usługa Log Analytics jest przeznaczona dla Ciebie?
Jeśli obecnie środowisko platformy Azure nie jest monitorowane, warto rozpocząć od pracy z usługą [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md), która zbiera i analizuje dane monitorowania zasobów platformy Azure.  Usługa Log Analytics może [zbierać dane z usługi Azure Monitor](log-analytics-azure-storage.md), aby korelować je z innymi danymi i przeprowadzać dodatkowe analizy.

Jeśli chcesz monitorować środowisko lokalne lub obecnie przeprowadzasz monitorowanie za pomocą usług, takich jak Azure Monitor lub System Center Operations Manager, usługa Log Analytics może mieć dla Ciebie dużą wartość.  Może ona zbierać dane bezpośrednio od agentów oraz innych narzędzi w obrębie pojedynczego repozytorium.  Narzędzia analityczne w usłudze Log Analytics, takie jak operacje wyszukiwania w dziennikach, widoki i rozwiązania, działają na wszystkich zebranych danych i oferują scentralizowaną analizę całego środowiska.


## <a name="using-log-analytics"></a>Korzystanie z usługi Log Analytics
Dostęp do usługi Log Analytics można uzyskiwać za pośrednictwem witryny Azure Portal, które działa w dowolnej przeglądarce i zapewnia dostęp do ustawień konfiguracji oraz wielu narzędzi służących do analizowania zebranych danych i przeprowadzania na nich działań.  Z poziomu portalu można skorzystać z [wyszukiwania w dziennikach](log-analytics-log-searches.md), za pomocą którego można tworzyć zapytania służące do analizowania zebranych danych, z [pulpitów nawigacyjnych](log-analytics-dashboards.md), które można dostosować za pomocą widoków graficznych dotyczących najbardziej wartościowych wyszukiwań, i z [rozwiązań](log-analytics-add-solutions.md) zapewniających dodatkowe funkcje i narzędzia analizy.

Poniższy obraz pochodzi z ekranu omówienia ze zbiorczymi informacjami na temat [rozwiązań](#add-functionality-with-management-solutions) zainstalowanych w obszarze roboczym.  Aby uzyskać dostęp do szczegółowych danych wybranego rozwiązania, można kliknąć odpowiedni kafelek.

![Portal pakietu OMS](media/log-analytics-overview/portal.png)

Usługa Log Analytics oferuje język zapytań, za pomocą którego można szybko pobierać i konsolidować dane w repozytorium.  Możliwe jest tworzenie i zapisywanie [wyszukiwań w dziennikach](log-analytics-log-searches.md), aby bezpośrednio analizować dane w portalu lub automatycznie uruchamiać wyszukiwania w dziennikach w celu tworzenia alertów, jeśli wyniki zapytania wskazują spełnienie ważnego warunku.

![Wyszukiwanie w dzienniku](media/log-analytics-overview/log-search.png)

Aby analizować dane poza usługą Log Analytics, można wyeksportować dane do narzędzi, takich jak usługa [Power BI](log-analytics-powerbi.md) lub program Excel.  Możesz również wykorzystywać [interfejs API wyszukiwania w dzienniku](log-analytics-log-search-api.md), aby tworzyć niestandardowe rozwiązania korzystające z danych usługi Log Analytics lub przeprowadzać integrację z innymi systemami.

## <a name="add-functionality-with-management-solutions"></a>Dodawanie funkcjonalności dzięki rozwiązaniom do zarządzania
[Rozwiązania do zarządzania](log-analytics-add-solutions.md) dodają funkcjonalność do usługi Log Analytics i udostępniają usłudze Log Analytics dodatkowe narzędzia do obsługi danych i analizy.  Mogą również definiować nowe typy rekordów do pobrania, które można przeanalizować za pomocą funkcji wyszukiwań w dziennikach lub przy użyciu dodatkowego interfejsu użytkownika udostępnionego przez rozwiązanie w ramach pulpitu nawigacyjnego.  Na poniższym obrazie przedstawiono przykładowe [rozwiązanie do śledzenia zmian](log-analytics-change-tracking.md)

![Rozwiązanie do śledzenia zmian](media/log-analytics-overview/change-tracking.png)

Dostępne są już rozwiązania związane z różnorodnymi funkcjami i cały czas dodajemy kolejne rozwiązania.  Dostępne rozwiązania można łatwo przeglądać i [dodawać do obszaru roboczego](log-analytics-add-solutions.md) z poziomu witryny Azure Marketplace.  Wiele rozwiązań zostanie automatycznie wdrożonych i od razu zacznie działać, ale w przypadku innych może być konieczne wykonanie umiarkowanie złożonych czynności konfiguracyjnych.

![Galeria rozwiązań](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Składniki usługi Log Analytics
Centralnym punktem usługi Log Analytics jest repozytorium zebranych danych, które jest hostowane w chmurze platformy Azure.  Dane są zbierane z połączonych źródeł przez konfigurowanie źródeł danych i dodawanie rozwiązań do subskrypcji.  Za pomocą źródeł danych i rozwiązań będą tworzone różne typy rekordów mających własne zestawy właściwości, ale które mogą być analizowane razem w zapytaniach do repozytorium.  Pozwala to korzystać z tych samych narzędzi i metod do pracy z różnymi rodzajami danych zbieranych przez różne źródła.

![Składniki usługi Log Analytics](media/log-analytics-overview/overview.png)

Połączone źródła to komputery i inne zasoby, które generują dane zbierane przez usługę Log Analytics.  Może to obejmować agentów zainstalowanych na komputerach z systemem [Windows](log-analytics-windows-agent.md) i [Linux](log-analytics-linux-agents.md) nawiązujących połączenie bezpośrednio lub agentów w [podłączonej grupie zarządzania programu System Center Operations Manager](log-analytics-om-agents.md).  W przypadku zasobów platformy Azure usługa Log Analytics zbiera dane z usług [Azure Monitor i Azure Diagnostics](log-analytics-azure-storage.md).

[Źródła danych](log-analytics-data-sources.md) to różne rodzaje danych zbieranych ze wszystkich połączonych źródeł.  Obejmuje to [zdarzenia](log-analytics-data-sources-windows-events.md) i [dane dotyczące wydajności](log-analytics-data-sources-performance-counters.md) z agentów systemu [Windows](log-analytics-data-sources-windows-events.md) i Linux, a także źródła, takie jak [dzienniki usług IIS](log-analytics-data-sources-iis-logs.md) i [niestandardowe dzienniki tekstowe](log-analytics-data-sources-custom-logs.md).  Każde źródło danych, które ma być zbierane, jest konfigurowane, a konfiguracja jest automatycznie dostarczana do każdego z nich.

Jeśli masz wymagania niestandardowe, a następnie możesz użyć [interfejsu API modułu zbierającego dane HTTP](log-analytics-data-collector-api.md) w celu zapisania danych do repozytorium z klienta interfejsu API REST.

## <a name="log-analytics-architecture"></a>Architektura usługi Log Analytics
Wymagania dotyczące wdrażania usługi Log Analytics są minimalne, ponieważ centralne składniki są hostowane w chmurze platformy Azure.  Oprócz repozytorium obejmuje to usługi, które umożliwiają korelowanie i analizowanie zebranych danych.  Do portalu można uzyskać dostęp z dowolnej przeglądarki, więc nie jest wymagane żadne oprogramowanie klienckie.

Konieczne jest zainstalowanie agentów na komputerach z systemem [Windows](log-analytics-windows-agent.md) i [Linux](log-analytics-linux-agents.md), ale dla komputerów, które są już członkami [połączonej grupy zarządzania programu System Center Operations Manager](log-analytics-om-agents.md), nie są wymagani żadni dodatkowi agenci.  Agenci programu Operations Manager będą nadal komunikować się z serwerami zarządzania, które będą przekazywać swoje dane do usługi Log Analytics.  Niektóre rozwiązania będą jednak wymagać agentów w celu bezpośredniego komunikowania się z usługą Log Analytics.  W dokumentacji poszczególnych rozwiązań będą określone ich wymagania dotyczące komunikacji.

Po [utworzeniu konta usługi Log Analytics](log-analytics-get-started.md) zostanie utworzony obszar roboczy.  Obszar roboczy można traktować jako unikatowe środowisko usługi Log Analytics z własnym repozytorium danych, źródłami danych i rozwiązaniami. W ramach subskrypcji można tworzyć wiele obszarów roboczych do obsługi wielu środowisk, takich jak środowisko produkcyjne i testowe.

![Architektura usługi Log Analytics](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Następne kroki
* [Załóż bezpłatne konto usługi Log Analytics](log-analytics-get-started.md), aby przetestować ją we własnym środowisku.
* Wyświetl poszczególne [źródła danych](log-analytics-data-sources.md), za pomocą których można zbierać dane do usługi Log Analytics.
* [Przeglądaj dostępne rozwiązania w galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje do usługi Log Analytics.

