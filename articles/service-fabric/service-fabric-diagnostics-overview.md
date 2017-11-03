---
title: "Omówienie diagnostyki i monitorowanie sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat monitorowania i diagnostyki dla klastrów, aplikacje i usługi sieć szkieletowa usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 88f4a23f89a1c8fd88db1df3a7ff03ae5df64c0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorowania i diagnostyki dla sieci szkieletowej usług Azure

Monitorowania i diagnostyki są krytyczne dla programowania, testowania i wdrażania aplikacji i usług w każdym środowisku. Rozwiązania sieci szkieletowej usług najlepiej podczas planowanie i implementowanie monitorowania i diagnostyki, które pomagają upewnij się, aplikacji i usług działają zgodnie z oczekiwaniami w środowisku projektowym lokalnej lub w środowisku produkcyjnym.

Główne cele monitorowania i diagnostyki są:
* Wykrywanie i diagnozowanie problemów dotyczących sprzętu i infrastruktury
* Wykrywaj problemy związane z oprogramowania i aplikacji, skrócić przestoje związane z usługi
* Podejmowanie świadomych zasobów i zmniejsza zużycie dysku operacje decyzji
* Optymalizowanie aplikacji, usług i infrastruktury
* Generuj informacje biznesowe i zidentyfikuj obszary wymagające poprawy


Ogólny przepływ pracy monitorowania i diagnostyki obejmuje trzy kroki:

1. **Generowanie zdarzeń**: na poziomie aplikacji / usługi infrastruktury (klaster), platformy i obejmuje to zdarzenia (dzienników, śledzenie zdarzeń niestandardowych)
2. **Zdarzenie agregacji**: generowanych zdarzeń muszą być zbierane i agregować przed mogą być wyświetlane
3. **Analiza**: zdarzeń muszą być wizualizowanego i jest dostępna w niektórych formacie, aby umożliwić analizę i wyświetlane w razie potrzeby

Wiele produktów są dostępne, który obejmuje te trzy obszary i wolnych do wyboru różnych technologii dla każdego. Należy się upewnić, czy różnych elementów współdziałać, aby dostarczać rozwiązanie monitorowania end-to-end dla klastra.

## <a name="event-generation"></a>Generowanie zdarzeń

Pierwszym krokiem w przepływie pracy monitorowania i diagnostyki jest tworzenie i generowania zdarzeń i dzienniki. Te zdarzenia, dzienników i danych śledzenia są generowane na dwa poziomy: warstwa platformy (w tym klastrze, maszyny lub akcje sieci szkieletowej usług) lub warstwy aplikacji (wszelkie Instrumentacji dodany do aplikacji i usługi wdrożone do klastra). Zdarzenia na każdym z tych poziomów są można dostosowywać, chociaż sieci szkieletowej usług zapewniają niektóre Instrumentacji domyślnie.

Przeczytaj więcej na temat [zdarzenia na poziomie platformy](service-fabric-diagnostics-event-generation-infra.md) i [zdarzenia na poziomie aplikacji](service-fabric-diagnostics-event-generation-app.md) zrozumieć, jaka jest dostępna oraz sposób zwiększenia instrumentacji.

Po podejmowania decyzji w sprawie dostawcy logowania chcesz użyć, należy się upewnić, że dzienniki są agregowane i przechowywane poprawnie.

## <a name="event-aggregation"></a>Agregacja zdarzeń

Do zbierania dzienników i zdarzenia generowane przez aplikacje i klastra, zwykle zaleca się używanie [diagnostyki Azure](service-fabric-diagnostics-event-aggregation-wad.md) (więcej podobnie do zbierania dzienników opartej o agentów) lub [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) () zbieranie danych dziennika w trakcie).

Zbieranie dzienników aplikacji przy użyciu rozszerzenia diagnostyki Azure jest dobra opcja w przypadku usługi Service Fabric, jeśli zestaw źródeł dziennika oraz miejsc docelowych nie zmieniają się często, istnieje bezpośrednie mapowanie między źródeł oraz ich miejsc docelowych. Powód dla diagnostyki Azure to konfiguruje odbywa się w warstwie Menedżera zasobów dzięki wprowadzeniu istotnych zmian w konfiguracji wymaga klastra ponownego aktualizowania/wdrażania. Ponadto, najlepiej jest wykorzystywany w upewnieniu się, czy dzienniki są przechowywane w innym nieco bardziej trwałego, z którym są one dostępne na różnych platformach analizy. Oznacza to, że kończy się ono są mniej wydajne potoku niż korzystanie z opcji, takich jak EventFlow.

Przy użyciu [EventFlow](https://github.com/Azure/diagnostics-eventflow) umożliwia wysyłanie ich dzienników bezpośrednio do analizy i wizualizacji platformy i/lub do magazynu usług. Inne biblioteki (ILogger, Serilog itp.) mogą być używane do tego celu, ale EventFlow przynosi korzyści związane z został zaprojektowany specjalnie z myślą o zbierania dzienników w trakcie i do obsługi usługi sieć szkieletowa usług. Zwykle ma wiele potencjalnych korzyści:

* Łatwe konfigurowanie i wdrażanie
    * Konfiguracja zbierania danych diagnostycznych jest tylko część konfiguracji usługi. Łatwo jest zawsze Zachowuj "synchronizację" z pozostałą częścią aplikacji
    * Dla każdej aplikacji lub na usługi konfiguracji jest łatwo osiągalne
    * Konfigurowanie miejsc docelowych danych za pośrednictwem EventFlow jest wystarczy dodanie odpowiedniego pakietu NuGet i zmianę *eventFlowConfig.json* pliku
* Elastyczność
    * Aplikacja może wysyłać dane wszędzie tam, gdzie należy przejść, tak długo, jak jest biblioteka klienta, która obsługuje system magazynowania wybranych danych. Można dodać nowych miejsc docelowych pożądane
    * Złożone zasady przechwytywania, filtrowania i agregacja danych może być zaimplementowany.
* Dostęp do danych aplikacji wewnętrznych i kontekstu
    * Podsystem diagnostycznych uruchomiony proces aplikacji/usługi można łatwo rozszerzyć śladów z informacje kontekstowe

Jedyną operacją, należy pamiętać, jest te dwie opcje nie wykluczają oraz w trakcie można uzyskać podobne zadania wykonane przy użyciu jednego lub drugiego go może okazać, że można skonfigurować jednocześnie. W większości sytuacji należy połączenie agenta z kolekcji w trakcie może prowadzić do bardziej niezawodny monitorowania przepływu pracy. Rozszerzenia Azure Diagnostics (agent) może być ścieżki wybranego poziomu dzienników platformy podczas można używać EventFlow (w trakcie kolekcji) dla poziomu dzienników aplikacji. Gdy użytkownik znalezienia co najlepiej będzie działać dla Ciebie, nadszedł czas na traktować jak mają być wyświetlane i analizowane danych.

## <a name="event-analysis"></a>Analiza zdarzeń

Istnieje kilka dużą platformy, które istnieją w rynku, jeśli chodzi o analizy i wizualizacji danych monitorowania i diagnostyki. Są dwa, które są zalecane [OMS](service-fabric-diagnostics-event-analysis-oms.md) i [usługi Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) z powodu ich lepszą integrację z sieci szkieletowej usług, ale również powinien wyglądać w [stosu elastycznych](https://www.elastic.co/products) () szczególnie w przypadku, gdy rozważane jest uruchomiony klaster w środowisku w trybie offline), [Splunk](https://www.splunk.com/), lub inne platformy swoich preferencji.

Klucz wskazuje na dowolnej platformie wybierz powinna zawierać jak doświadczenia są z interfejsem użytkownika i badania opcje możliwość wizualizacji danych i tworzenie czytelny pulpitów nawigacyjnych i dodatkowe narzędzia zapewniają do rozszerzania monitorowania, automatyczne takich jak alerty.

Oprócz platformy, należy wybrać podczas konfigurowania klastra sieci szkieletowej usług jako zasobów platformy Azure, również uzyskać dostęp do platformy Azure poza pole monitorowanie dla maszyn, które mogą być przydatne w przypadku dotyczące wydajności i metryki monitorowania.

### <a name="azure-monitor"></a>Azure Monitor

Można użyć [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) do monitorowania wielu zasobów platformy Azure, na których jest zbudowany klastra sieci szkieletowej usług. Zestaw metryki dla [zestaw skali maszyny wirtualnej](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) i indywidualnych [maszyn wirtualnych](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) automatycznie są zbierane i wyświetlane w portalu Azure. Aby wyświetlić zebrane informacje w portalu Azure, wybierz grupę zasobów, zawierającą klaster sieci szkieletowej usług. Wybierz zestaw skali maszyny wirtualnej, który chcesz wyświetlić. W **monitorowanie** zaznacz **metryki** do wyświetlania wykresu wartości.

![Widok portalu Azure zebranych informacji metryki](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Aby dostosować wykresy, postępuj zgodnie z instrukcjami [metryk w Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Można również tworzyć alerty oparte na tych metryk, zgodnie z opisem w [w monitorze Azure tworzyć alerty dla usług Azure](../monitoring-and-diagnostics/insights-alerts-portal.md). Możesz wysłać alerty usługi powiadomień za pomocą punkty zaczepienia sieci web, zgodnie z opisem w [skonfigurować haku sieci web na alert metryki Azure](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure Monitor obsługuje tylko jedną subskrypcję. Jeśli chcesz monitorować wiele subskrypcji lub jeśli potrzebujesz dodatkowych funkcji [analizy dzienników](https://azure.microsoft.com/documentation/services/log-analytics/), część programu Microsoft Operations Management Suite, przewiduje całościowe rozwiązanie zarządzania zarówno lokalnych i oparte na chmurze infrastruktura. Pozwala zobaczyć dzienniki i metryki dla całego środowiska w jednym miejscu, mogą przesyłać dane z monitora Azure bezpośrednio do analizy dzienników.

## <a name="next-steps"></a>Następne kroki

### <a name="watchdogs"></a>Watchdogs

Programu alarmowego jest oddzielny usługa można obejrzeć kondycji i obciążenie usługi i raportu kondycji dla wszystkich elementów w hierarchii modelu kondycji. Może to pomóc zapobiec wystąpieniu błędów, które nie można wykryć na podstawie widoku jednej usługi. Watchdogs są również dobrym miejscem do hosta kod, który wykonuje czynności zaradczych bez interakcji z użytkownikiem (na przykład czyszczenie plików dziennika w magazynie, co pewien czas). Można znaleźć implementacji usługi programu alarmowego próbki [tutaj](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Wprowadzenie do zrozumienia, jak pobrać generowany zdarzeń i dzienniki na [poziom platformy](service-fabric-diagnostics-event-generation-infra.md) i [poziomie aplikacji](service-fabric-diagnostics-event-generation-app.md).