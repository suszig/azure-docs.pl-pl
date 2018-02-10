---
title: "Monitorowanie aplikacji platformy Azure i zasobów | Dokumentacja firmy Microsoft"
description: "Omówienie różnych usług firmy Microsoft i możliwości składających się na pełną strategię monitorowania usług systemu Azure i aplikacji."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: 505e92b5fc63f570bc4d0f8899ae977b93850356
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Monitorowanie aplikacji platformy Azure i zasobów

Monitorowanie jest czynnością zbierania i analizowania danych w celu ustalenia wydajności, kondycji i dostępności aplikacji biznesowych i zasobów, od których zależy. Strategia monitorowania skuteczne pomoże poznać szczegółowe funkcjonowanie poszczególnych składników aplikacji i zwiększenia Twojej pracy aktywnego informujący poważnych problemów, dzięki czemu można je rozwiązać, zanim wystąpią problemy.

Azure zawiera wiele usług, które indywidualnie wykonania określonych ról i zadań w obszarze monitorowania i jednocześnie dostarczyć kompleksowe rozwiązanie umożliwiające obsługę zbierania, analizowanie i działające na dane telemetryczne z aplikacji i zasobów platformy Azure Obsługa.  Może również współpracować do monitorowania zasobów lokalnych krytyczne zapewnić monitorowanie środowiska hybrydowego.   Opis narzędzi i dane, które są dostępne jest pierwszym etapem opracowanie pełnej strategii monitorowania aplikacji. 

Na poniższym diagramie przedstawiono koncepcję różne składniki, które współpracują ze sobą, aby zapewnić monitorowanie zasobów platformy Azure.  Każdy z tych jest opisane w poniższych sekcjach linki do szczegółowych informacji technicznych.

![Omówienie monitorowania](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Podstawowe monitorowanie
Podstawowe monitorowanie udostępnia podstawowe wymagane monitorowanie z przekraczaniem zasobów platformy Azure.  Te usługi wymagają minimalnej konfiguracji i zbierać dane telemetryczne podstawowej, która jest wykorzystywana przez monitorowanie usług premium.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) włącza podstawowe monitorowanie dla usługi Azure, zezwalając Kolekcja [metryki](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [Dzienniki aktywności](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), i [dzienniki diagnostyczne](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Na przykład dziennika aktywności informuje po utworzeniu lub zmodyfikowaniu nowych zasobów.  Metryki są dostępne statystyki które zapewniają różne zasoby i nawet systemu operacyjnego wewnątrz maszyny wirtualnej.  Można wyświetlić te dane z jednego z eksploratorów w portalu Azure, wysyłania do analizy dzienników dla analizy trendów i szczegółowe lub tworzyć reguły alertów w celu aktywnego powiadomienia poważnych problemów.

### <a name="service-health"></a>Service Health
Kondycji aplikacji korzysta z usług Azure, których ona zależy.  [Kondycja usługi Azure](../service-health/service-health-overview.md) identyfikuje wszelkie problemy z usługami Azure, które mogą mieć wpływ na Twojej aplikacji, a także ułatwia planowanie żadnych harmonogram konserwacji.

### <a name="azure-advisor"></a>Azure Advisor
[Klasyfikator Azure](../advisor/advisor-overview.md) monitoruje telemetrii konfiguracji i użycia zasobów zapewnienie spersonalizowanych zalecenia dotyczące najlepszych rozwiązań.  Następujące zalecenia te pomóc zwiększyć wydajność, zabezpieczeń i dostępności zasobów obsługi aplikacji.


## <a name="premium-monitoring-services"></a>Monitorowanie usług Premium
Następujących usług platformy Azure zapewniają bogate możliwości do zbierania i analizowania danych monitorowania.  Tworzenie na podstawowe funkcje wspólnego monitorowania i wykorzystać na platformie Azure, a Udostępnianie zaawansowane analizy zebranych danych ma zostać przypisany unikatowy insights do aplikacji i infrastruktury.  Zawierają one dane w kontekście konkretnego scenariusze przeznaczone dla różnych odbiorców.

### <a name="application-insights"></a>Application Insights
[Usługa Application Insights](http://azure.microsoft.com/documentation/services/application-insights) pozwala na monitorowanie dostępności, wydajności i użycia aplikacji, czy jest ona hostowana w chmurze lub lokalnie.  Dzięki instrumentacji aplikacji do pracy z usługą Application Insights, można osiągnąć szczegółowych, dzięki czemu można szybko zidentyfikować i diagnozowanie błędów bez oczekiwania na użytkownika, aby zgłosić je. Informacje zbierane możesz wprowadzić świadomych wyborów na konserwację aplikacji i ulepszenia.  Oprócz szeroką gamę narzędzi do interakcji z danymi, które są zbierane usługi Application Insights przechowuje dane w typowych repozytorium wykorzystać udostępnionego funkcji, takich jak alerty, pulpity nawigacyjne i szczegółowa analiza przy użyciu języka zapytań usługi Analiza dzienników.

### <a name="log-analytics"></a>Log Analytics
[Zaloguj się Analytics](http://azure.microsoft.com/documentation/services/log-analytics) odgrywa kluczową rolę w Azure monitorowanie, zbieranie danych z różnych zasobów w jednym repozytorium, w którym można ją analizować z językiem zaawansowanych zapytań.  Usługa Application Insights i Centrum zabezpieczeń Azure są przechowywane w analizy dzienników danych przechowywania i wykorzystać silnik analizy danych.  To połączone z danych zbieranych z monitora Azure, rozwiązania do zarządzania i agentów zainstalowanych na maszynach wirtualnych w chmurze lub lokalnie pozwalają na formularzu pełnego obrazu całego środowiska. 


### <a name="service-map"></a>Mapa usługi
[Mapa usług](../operations-management-suite/operations-management-suite-service-map.md) zapewnia wgląd w środowisku IaaS, analizując maszyny wirtualne z różnych procesów i zależności na innych komputerach i procesów zewnętrznych.  Integruje zdarzenia, dane dotyczące wydajności i rozwiązań do zarządzania w analizy dzienników, dzięki czemu można wyświetlić te dane w kontekście każdego komputera, a jego powiązanie z resztą środowiska.  Mapa usługi jest podobny do [mapowanie aplikacji w usłudze Application Insights](../application-insights/app-insights-app-map.md) , ale koncentruje się na składników infrastruktury obsługi aplikacji.

### <a name="network-watcher"></a>Network Watcher
[Monitor sieci](../network-watcher/network-watcher-monitoring-overview.md) udostępnia oparta na scenariuszu monitorowania i diagnostyki dla różnych scenariuszach sieciowych na platformie Azure.  Przechowuje dane w Azure metryki i informacji diagnostycznych do analiz i działa z następującej sieci monitorowanie rozwiązań monitorowania różnych aspektów sieci:
* [Sieci monitora wydajności (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) — sieci oparte na chmurze rozwiązanie monitorujące, które monitoruje łączność między chmur publicznych, centrów danych i środowiskami lokalnymi
* [Monitor usługi ExpressRoute](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) -możliwości NPM, który monitoruje łączność end-to-end i wydajności za pośrednictwem obwody usługi ExpressRoute.
* Analiza ruchu — rozwiązania opartego na chmurze, która zapewnia wgląd w działania użytkowników i aplikacji w sieci w chmurze.
* [Analiza DNS](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns) — zapewnia bezpieczeństwa, wydajności i operacji powiązanych szczegółowe informacje, na podstawie serwerów DNS.

### <a name="management-solutions"></a>Rozwiązania do zarządzania
[Rozwiązania do zarządzania](../log-analytics/log-analytics-add-solutions.md) są utworzone zestawy logiki zawierające szczegółowe informacje dla określonej aplikacji lub usługi.  Opierają się na analizy dzienników do przechowywania i analizowania danych monitorowania, które pobierają.  Rozwiązania do zarządzania są dostępne z firmy Microsoft i partnerów, które zapewniają monitorowanie różnych usług platformy Azure i innych firm. Obejmują na przykład monitorowanie rozwiązań [monitorowania kontenera](../log-analytics/log-analytics-containers.md) umożliwiająca wyświetlanie i zarządzanie nimi hosty kontenera i [analiza SQL Azure](../log-analytics/log-analytics-azure-sql.md) który zbiera i wizualizuje metryki wydajności dla bazy danych SQL Bazy danych Azure.


## <a name="shared-functionality"></a>Funkcja udostępnionego
Następujące narzędzia Azure udostępnia krytyczne funkcji monitorowania usług premium.  Są one używane przez wiele usług, co umożliwia wykorzystanie typowe funkcje i konfiguracje w wielu usługach.

### <a name="alerts"></a>Alerty
[Alerty Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktywnie powiadamia użytkownika o krytycznych warunków i potencjalnie podjęcia działań naprawczych.  Reguły alertów można wykorzystać danych z wielu źródeł, takich jak dzienniki i metryki. Używają [grupy akcji](../monitoring-and-diagnostics/monitoring-action-groups.md) zawierającym unikatowe zestawy adresatów i akcji w odpowiedzi na alert.  W zależności od wymagań może mieć alertów, Uruchamianie zewnętrznego akcji za pomocą elementów webhook i integracja z narzędziami Zarządzanie usługami IT —.

### <a name="dashboards"></a>Pulpity nawigacyjne
[Pulpity nawigacyjne Azure](../azure-portal/azure-portal-dashboards.md) umożliwiają łączenie różnych typów danych w jednym okienku w portalu Azure i udziału innym użytkownikom Azure.  Na przykład można utworzyć pulpit nawigacyjny, który łączy Kafelki przedstawiający wykres metryki, spis Dzienniki aktywności wykres użycia za pomocą usługi Application Insights i dane wyjściowe wyszukiwania dziennika w analizy dzienników.

Można również eksportować dane analizy dzienników do [usługi Power BI](https://docs.microsoft.com/power-bi/) skorzystać z dodatkowych wizualizacje i udostępnić je innym użytkownikom w i poza siecią firmową.

### <a name="metrics-explorer"></a>Eksploratora metryk
[Metryki](../monitoring-and-diagnostics/monitoring-overview-metrics.md) są wartości liczbowe generowane przez zasobów platformy Azure, które ułatwiają zrozumienie działania i wydajności zasobu. Metryki można wysyłać do analizy dzienników dla analizy danych z innych źródeł.



### <a name="activity-logs"></a>Dzienniki aktywności
[Dzienniki aktywności](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) zawierają dane dotyczące operacji zasobów platformy Azure.  W tym takie informacje jak zmiany konfiguracji zasobów, zdarzenia kondycji usługi, zalecenia na lepsze wykorzystanie zasobów oraz informacje dotyczące operacji skalowania automatycznego.  Dzienniki dla określonego zasobu można wyświetlać na stronie w dziennikach widoku lub portalu Azure z wielu zasobów w Eksploratorze dziennika aktywności.  Można również wysłać Dzienniki aktywności do analizy dzienników, więc można analizować dane zbierane przez rozwiązania do zarządzania, agentów na maszynach wirtualnych i innych źródeł.


## <a name="example-scenarios"></a>Przykładowe scenariusze
Poniżej przedstawiono przykłady wysokiego poziomu, ilustrujące, jak można wykorzystanie różnych narzędzi do monitorowania na platformie Azure dla różnych scenariuszy.

### <a name="monitoring-a-web-application"></a>Monitorowanie aplikacji sieci web
Należy wziąć pod uwagę aplikacji sieci web wdrożona na platformie Azure przy użyciu usług aplikacji, usługi Azure Storage i bazy danych SQL.  Można uruchomić po zalogowaniu się do [metryki](../monitoring-and-diagnostics/monitoring-overview-metrics.md) i [Dzienniki aktywności](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) dla każdego z tych poszczególnych zasobów na swoich stronach w portalu Azure.  Obejmuje to krytyczne informacje, takie jak liczba żądań do aplikacji i Średni czas odpowiedzi oprócz identyfikacji jakiekolwiek zmiany konfiguracji.

W portalu można następnie przejdź do monitorowania, w Aby wyświetlić dzienniki dla różnych zasobów i metryk ze sobą.  W trakcie określania parametrów standardowe na podstawie metryk, możesz [tworzyć reguły alertów](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) do aktywnego powiadomienie, gdy na przykład średni czas odpowiedzi przekroczy próg.  Aby uzyskać szybki przegląd codzienne wydajności aplikacji, możesz utworzyć Azure pulpit nawigacyjny, aby pokazać wykresy metryki reprezentujący krytyczne kluczowych wskaźników wydajności.

Aby wykonać dokładniejsze monitorowanie aplikacji, możesz [skonfigurować go do usługi Application Insights](../application-insights/quick-monitor-portal.md).  Teraz można zbierać dodatkowe dane zawierające dalsze wgląd w działanie i wydajność aplikacji.  Usługa Application Insights wykrywa podstawowej relacji między składnikami aplikacji, co pozwala na wizualną reprezentację za pomocą [mapowanie aplikacji](../application-insights/app-insights-app-map.md) połączeniu z [śledzenia end-to-end](../application-insights/app-insights-transaction-diagnostics.md) do diagnozowania dokładne składnika, zależności lub wyjątek, w którym wystąpił problem.  Możesz utworzyć [testów dostępności](../application-insights/app-insights-monitor-web-app-availability.md) do aktywnego testowania aplikacji w różnych regionach.  Aby ułatwić deweloperów, możesz [włączyć profilera](../application-insights/enable-profiler-compute.md) co pozwala na śledzenie żądań i wszelkie wyjątki do określonego wiersza kodu.  

W celu uzyskania dalszych widoczność usług używanych w aplikacji, należy dodać [rozwiązania analizy SQL](../log-analytics/log-analytics-azure-sql.md) Aby zbierać dodatkowe dane do analizy dzienników. Po pewnym czasie zdecydujesz się Znajdź jego główną przyczynę okresów, kiedy wydajności w witrynie spadła poniżej progu.  Możesz zapisać zapytania przy użyciu analizy dzienników do skorelowania danych użycia i wydajności zbieranych przez usługi Application Insights z konfiguracją i wydajności danych między zasobami Azure obsługi aplikacji.


### <a name="monitoring-virtual-machines"></a>Monitorowanie maszyn wirtualnych
Masz kombinację systemu Windows i Linux maszyn wirtualnych działających na platformie Azure.  Azure Monitor umożliwia wyświetlanie [Dzienniki aktywności](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) i [metryki na poziomie hosta](../monitoring-and-diagnostics/monitoring-overview-metrics.md) , a następnie dodaj [rozszerzenia diagnostyki Azure](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) do maszyn wirtualnych, aby zbierać metryki z systemu operacyjnego gościa.  Następnie można utworzyć [reguły alertów](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) do aktywnego powiadomienia w przypadku podstawowych metryki takie wykorzystanie procesora i pamięci między progów.

Zbierać więcej szczegółów dotyczących maszyn wirtualnych uruchomionych aplikacji biznesowej, można [utworzyć obszaru roboczego analizy dzienników i włączyć rozszerzenia maszyny Wirtualnej](../log-analytics/log-analytics-quick-collect-azurevm.md) na każdym komputerze.  Możesz skonfigurować [kolekcji różnych źródeł danych](../log-analytics/log-analytics-data-sources.md) aplikacji i [tworzyć widoki](../log-analytics/log-analytics-view-designer.md) do raport o jego codziennych operacji i wydajności.  Następnie możesz [tworzyć reguły alertów](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) informujący, po odebraniu zdarzenia określonego błędu.  Aby można było ciągłego monitorowania kondycji agenta, możesz dodać [rozwiązania do zarządzania agenta kondycji](../operations-management-suite/oms-solution-agenthealth.md).

Do dalszego uzyskiwać wgląd w aplikacji możesz [dodać agenta zależności](../operations-management-suite/operations-management-suite-service-map-configure.md) do maszyn wirtualnych, aby dodać je do [mapy usługi](../operations-management-suite/operations-management-suite-service-map.md).  Odnajduje krytyczne procesów, a identyfikuje połączenia między komputerami z innymi usługami.  Po awarii zgłoszone można użyć mapy usługi do wykonania przechowanie do identyfikowania maszynach z konkretnego wystąpienia problemu.  Następnie można utworzyć [zapytania na danych analizy dzienników](../log-analytics/log-analytics-log-search-new.md) identyfikowanie problemu w przyszłości, i Utwórz regułę alertu do aktywnego powiadomienia po wykryciu warunku.



## <a name="next-steps"></a>Kolejne kroki
Dowiedz się więcej o

* [Azure Monitor wideo z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Wprowadzenie do platformy Azure monitora](monitoring-get-started.md)
* [Diagnostyka Azure](../azure-diagnostics.md) Jeśli próbujesz do diagnozowania problemów w usłudze chmury maszyny wirtualnej maszyny wirtualnej skalowanie aplikacji sieci szkieletowej ustawiona lub usługi.
* [Usługa Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Jeśli próbujesz diagnostycznych problemy w aplikacji sieci Web usługi aplikacji.
* [Zaloguj się Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) dla analizowanie zebranych danych monitorowania.
