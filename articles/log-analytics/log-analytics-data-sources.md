---
title: "Konfigurowanie źródeł danych w OMS Log Analytics | Dokumentacja firmy Microsoft"
description: "Źródła danych do definiowania danych czy zbiera analizy dzienników z agentów i inne połączenie źródeł.  W tym artykule opisano pojęcia jak analizy dzienników korzysta ze źródeł danych, zawiera szczegółowe informacje o sposobach ich konfigurowania i zawiera podsumowanie dostępnych źródeł danych."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 00d030a502cf70ea9a5dea767f560cdf2919573e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="data-sources-in-log-analytics"></a>Źródła danych w analizy dzienników
Analiza dzienników zbiera dane z połączonych źródeł w obszarze roboczym pakietu OMS i zapisuje go w repozytorium OMS.  Dane są zbierane z każdej jest zdefiniowany przez źródła danych, które można skonfigurować.  Dane w repozytorium OMS są przechowywane jako zestaw rekordów.  Dla każdego źródła danych tworzy rekordy określonego typu z każdym typem o własny zestaw właściwości.

![Zbierania danych analizy dzienników](./media/log-analytics-data-sources/overview.png)

Źródła danych są inne niż OMS rozwiązania, które również zbieranie danych z połączonych źródeł i tworzenie rekordów w repozytorium OMS.  Rozwiązania można dodać do swojego obszaru roboczego z galerii rozwiązań i zwykle zapewnia dodatkowe analizy narzędzi w portalu OMS.  

## <a name="summary-of-data-sources"></a>Podsumowanie źródeł danych
W poniższej tabeli wymieniono źródeł danych, które są aktualnie dostępne w analizy dzienników.  Każdy ma łącze do oddzielnym artykule podanie szczegółów dla tego źródła danych.

| Źródło danych | Typ zdarzenia | Opis |
|:--- |:--- |:--- |
| [Niestandardowe dzienniki](log-analytics-data-sources-custom-logs.md) |\<Nazwa_dziennika\>_CL |Pliki tekstowe agentów systemu Windows lub Linux, zawierające informacje w dzienniku. |
| [Dzienniki zdarzeń systemu Windows](log-analytics-data-sources-windows-events.md) |Wydarzenie |Zbierane zdarzenia z dziennika zdarzeń na komputerach z systemem Windows. |
| [Liczniki wydajności systemu Windows](log-analytics-data-sources-performance-counters.md) |Wydajności |Liczniki wydajności są zbierane z komputerów z systemem Windows. |
| [Liczniki wydajności systemu Linux](log-analytics-data-sources-performance-counters.md) |Wydajności |Liczniki wydajności zebrane z komputerów z systemem Linux. |
| [Dzienniki usług IIS](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internetowe usługi informacyjne dzienniki w formacie W3C. |
| [Dziennik systemu](log-analytics-data-sources-syslog.md) |Dziennik systemu |Zdarzenia dziennika systemowego na komputerach z systemem Windows lub Linux. |

## <a name="configuring-data-sources"></a>Konfigurowanie źródeł danych
Konfigurowanie źródeł danych z **danych** menu analizy dzienników **ustawienia**.  Żadnej konfiguracji są dostarczane do wszystkich połączonych źródeł w obszarze roboczym pakietu OMS.  Nie można obecnie wykluczyć wszystkich agentów z tej konfiguracji.

![Skonfiguruj zdarzenia systemu Windows](./media/log-analytics-data-sources/configure-events.png)

1. Kliknij w konsoli OMS **ustawienia** Kafelek lub **ustawienia** w górnej części ekranu.
2. Wybierz **danych**.
3. Kliknij źródło danych do skonfigurowania.
4. Skorzystaj z linku do dokumentacji dla każdego źródła danych w tabeli powyżej, aby uzyskać szczegółowe informacje na ich konfiguracji.

> [!NOTE]
> Obecnie nie można skonfigurować źródła danych analizy dzienników w portalu Azure.

## <a name="data-collection"></a>Zbieranie danych
Konfiguracje źródła danych są dostarczane do agentów, które są podłączone bezpośrednio do analizy dzienników w ciągu kilku minut.  Określony dane są zbierane z agenta i dostarczane bezpośrednio do analizy dzienników w odstępach czasu określonych dla każdego źródła danych.  Zobacz dokumentację dla każdego źródła danych dla tych szczegółowych informacji.

Agenci programu System Center Operations Manager (SCOM) w dołączonej grupy zarządzania konfiguracji źródła danych są przetłumaczyć pakietów administracyjnych i dostarczonych w grupie zarządzania co 5 minut domyślnie.  Agent pobiera pakiet administracyjny jak każdy inny i zbiera określone dane. W zależności od źródła danych dane będą się, że albo wysłane do serwera zarządzania, który przesyła dane do analizy dzienników lub agent będzie wysyłać dane do analizy dzienników bez przechodzenia przez serwer zarządzania. Zapoznaj się [szczegóły zbierania danych w celu OMS funkcji i rozwiązań](log-analytics-add-solutions.md#data-collection-details) szczegółowe informacje.  Możesz przeczytać o szczegółach łączenia SCOM i OMS i modyfikowanie częstotliwości tej konfiguracji jest dostarczany w [skonfigurować integrację z programem System Center Operations Manager](log-analytics-om-agents.md).

Jeśli agent nie może nawiązać połączenia analizy dzienników lub Operations Manager, będzie zbierać dane, które będzie dostarczać, podczas nawiązywania połączenia.  Dane mogą zostać utracone, jeśli ilość danych osiągnie maksymalny rozmiar pamięci podręcznej klienta, lub jeśli agent nie jest w stanie nawiązać połączenia w ciągu 24 godzin.

## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics
Wszystkie dane zebrane przez analizy dzienników jest przechowywane w repozytorium OMS jako rekordów.  Rejestruje zebrane przez różnych źródeł danych będą mieć własny zestaw właściwości i zostać zidentyfikowane na podstawie ich **typu** właściwości.  Dla każdego typu rekordu, zobacz dokumentację dla każdego źródła danych i rozwiązania, aby uzyskać szczegółowe informacje.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [rozwiązań](log-analytics-add-solutions.md) czy dodać funkcje do analizy dzienników i również zbierać dane do repozytorium OMS.
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) analizować dane zebrane ze źródeł danych i rozwiązania.  
* Skonfiguruj [alerty](log-analytics-alerts.md) do aktywnego powiadomienia o krytyczne dane zebrane ze źródeł danych i rozwiązań.
