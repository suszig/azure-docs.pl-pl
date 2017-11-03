---
title: Monitorowanie w Microsoft Azure | Dokumentacja firmy Microsoft
description: "Opcje, jeśli chcesz monitorować w Microsoft Azure. Azure Monitor, usługi Application Insights i analizy dzienników"
author: rboucher
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
ms.date: 10/04/2017
ms.author: robb
ms.openlocfilehash: e164cbd910ccc38610c7aef37d25ff1b4413038d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Omówienie monitorowania na platformie Microsoft Azure
Ten artykuł zawiera omówienie narzędzi i usług związanych z monitorowaniem całościowo Microsoft Azure. Dotyczy:
- Do monitorowania infrastruktury platformy Azure i aplikacji przy użyciu usług Azure
- Przy użyciu usług Azure do monitora hybrydowego i innych niż Azure infrastruktury i aplikacji
- Za pomocą usług Azure z systemem innym niż do monitorowania infrastruktury platformy Azure i aplikacji

Go w tym artykule omówiono różne produkty i usługi dostępne i ich współpracę. Można ustalić, jakie narzędzia są najbardziej odpowiednie dla Ciebie w jakich przypadkach może być pomocny.  

## <a name="why-use-azures-monitoring-services"></a>Dlaczego warto używać monitorowania usług systemu Azure?

Problemy z wydajnością w Twojej aplikacji w chmurze może wpłynąć na Twojej firmy. Z wielu powiązanych elementów i częste wersjach degradations może nastąpić w dowolnej chwili. I w przypadku tworzenia aplikacji, użytkowników, zazwyczaj odnajdywanie problemy, które nie udało się znaleźć podczas testowania. Należy od razu wiedzieć o tych problemów i narzędzia do diagnozowania i rozwiązywania problemów. Ponadto problemy w aplikacji często wynika z podstawowej infrastruktury, na którym te aplikacje są uruchamiane, więc o holistyczny widok aplikacji i infrastruktury ma kluczowe znaczenie dla monitorowania środowiska platformy Azure. Microsoft Azure ma szeroką gamę narzędzi do identyfikacji i rozwiązywania problemów.

## <a name="how-do-i-monitor-my-azure-environment"></a>Jak monitorować Moje środowiska platformy Azure?

Istnieją różne narzędzia do monitorowania środowiska platformy Azure, z kod aplikacji działający na platformie Azure do usługi i infrastruktury uruchomienia tego kodu. Te narzędzia współpracują ze sobą oferuje kompleksowe chmury monitorowania i obejmują:

-   **Azure Monitor** — usługa Azure, która działa jako skonsolidowanych potoku dla wszystkich danych monitorowania z usług Azure. Umożliwia on dostęp do metryki wydajności i zdarzeń, które opisują działania infrastruktury platformy Azure i dowolnych usług Azure, którego używasz. Azure Monitor jest monitorowania potoku danych do środowiska Azure i udostępnia dane bezpośrednio do analizy dzienników, jak również 3 innych narzędzi, gdzie można uzyskać wgląd w dane i połączyć ją z danych z lokalnego lub innych zasobów chmury.

-   **Usługa Application Insights** — usługa Azure, która oferuje analizy wydajności aplikacji monitorowania i użytkownika. Monitoruje napisanych kodu i aplikacje zostały wdrożone na platformie Azure lub lokalnie / innych chmur. Instrumentując aplikacji przy użyciu zestawu SDK usługi Application Insights można uzyskać dostęp do danych w tym czasy odpowiedzi zależności, śledzenia wyjątków, migawek debugowania i wykonywania profilów. Udostępnia zaawansowane narzędzia do analizowania telemetrii tej aplikacji podczas tworzenia i obsługi aplikacji. Głęboko go integruje się z programem Visual Studio umożliwiające uzyskanie prawo, aby wiersze problem kodu, więc można go naprawić i oferuje analizy użycia do analizy użycia aplikacji z myślą o kierownikach produktu, jak również przez klientów.

-   **Dziennika analizy** — wcześniej znana jako OMS analizy dzienników, jest usługą platformy Azure, który wysyła strumień dziennika i metryki danych z usług Azure (za pośrednictwem Monitora Azure), maszynach wirtualnych platformy Azure oraz w lokalnym lub innych wyszukiwania elastyczne dziennika chmury, jak infrastruktury i oferty i wyjście z pole analizy na podstawie tych danych. Oferuje zaawansowane narzędzia do analizy danych między źródłami, umożliwia złożonych zapytań przez wszystkie dzienniki i może aktywnie alert po wystąpieniu określonego warunku.  Można nawet zebrać dane niestandardowe w jego centralnym repozytorium, dzięki czemu może zapytania i wizualizacji go. Można również wykonać korzystają z dziennika analityczne wbudowanego rozwiązania natychmiast Uzyskaj wgląd w zabezpieczenia i funkcje infrastruktury.

## <a name="accessing-monitoring-in-the-azure-portal"></a>Uzyskiwanie dostępu do monitorowania w portalu Azure
Wszystkie usługi monitorowania Azure są teraz dostępne w jednym okienku interfejsu użytkownika. Aby uzyskać więcej informacji na temat sposobu dostępu do tego obszaru, zobacz [Rozpoczynanie pracy z monitorem Azure](monitoring-get-started.md). 

Funkcje monitorowania dla określonych zasobów można także przejść przez wyróżnianie tych zasobów i przechodzenie od ich opcje monitorowania. 

## <a name="examples-of-when-to-use-which-tool"></a>Przykłady zastosowania narzędzie, które 

W poniższych sekcjach przedstawiono niektóre podstawowe scenariusze i narzędzia, które powinny być używane razem. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scenariusz 1 — poprawki błędów w tworzonej aplikacji Azure   

**Najlepszym rozwiązaniem jest jednocześnie używać usługi Application Insights, Azure Monitor i programu Visual Studio**

Platforma Azure udostępnia teraz pełną moc debuger programu Visual Studio w chmurze. Konfigurowanie monitora Azure, aby wysłać dane telemetryczne z usługą Application Insights. Włącz Visual Studio, aby umieścić zestaw SDK usługi Application Insights w aplikacji. Raz w usłudze Application Insights, możesz użyć mapy aplikacji do odnajdywania wizualnie części uruchomionych aplikacji, które jest w złej kondycji lub też nie. Dla tych elementów, które nie są w dobrej kondycji błędy i wyjątki są już dostępne dla eksploracji. Przejść głębiej, można użyć różnych analityka w usłudze Application Insights. Jeśli nie masz pewności o błędzie, można użyć debuger programu Visual Studio do śledzenia do kodu i numer pin punktu dalsze problem. 

Aby uzyskać więcej informacji, zobacz [monitorowania aplikacji sieci Web](../application-insights/app-insights-azure-web-apps.md) i odwoływać się do tabeli treści po lewej stronie, aby uzyskać instrukcje na różnych typach aplikacji i języków.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scenariusz 2 — debugowania aplikacji sieci web Azure .NET błędów, które zawierają tylko w środowisku produkcyjnym 

> [!NOTE]
> Te funkcje są w wersji zapoznawczej. 

**Najlepszym rozwiązaniem jest użycie usługi Application Insights i jeśli wystąpi możliwości programu Visual Studio do pełnego debugowania.**

Debugowanie aplikacji przy użyciu debugera migawki Insights aplikacji. W przypadku pewnej wartości progowej błędu ze składnikami produkcji, system automatycznie przechwytuje dane telemetryczne w systemie windows w czasie o nazwie "migawki." Kwota przechwycone jest bezpieczne dla chmury produkcji, ponieważ jest za mały, nie mają wpływu na wydajność, ale istotne umożliwić śledzenie.  Systemie może przechwytywać wiele migawek. Można przyjrzeć się punktu w czasie w portalu Azure lub użyć programu Visual Studio, aby uzyskać pełne środowisko. Z programem Visual Studio deweloperzy mogą Przeprowadź przez tej migawki tak, jakby były one debugowanie w czasie rzeczywistym. Zmienne lokalne, parametry, pamięci i ramki będą dostępne. Deweloperzy muszą otrzymać dostęp do tych danych produkcyjnych za pomocą roli RBAC.  

Aby uzyskać więcej informacji, zobacz [debugowania migawki](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scenariusz 3 — debugowania aplikacji Azure, która używa kontenerów lub mikrousług 

**Taka sama jak Scenariusz 1. Jednocześnie używać usługi Application Insights, Azure Monitor i programu Visual Studio** usługi Application Insights obsługuje również zbieranie telemetrii z procesów uruchomionych wewnątrz kontenerów i mikrousług (Kubernetes, Docker, sieć szkieletowa usług Azure). Aby uzyskać więcej informacji [zobacz ten film na debugowanie kontenerów i mikrousług](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scenariusz 4 — wydajności napraw problemy w aplikacji Azure

[Profilera usługi Application Insights](../application-insights/app-insights-profiler.md) jest przeznaczony do rozwiązywania problemów tego typu problemów. Identyfikowanie i rozwiązywanie problemów z wydajnością aplikacji uruchomionych w usługach aplikacji (aplikacje sieci Web, Logic Apps, Mobile Apps, aplikacje interfejsu API) i innych zasobów obliczeniowych, takich jak maszyny wirtualne, maszyny wirtualnej zestawy skalowania (VMSS), usługi w chmurze i sieci szkieletowej usług . 

> [!NOTE]
> Możliwość profilu maszyn wirtualnych, zestawy skalowania maszyny wirtualnej (VMSS), usługi w chmurze i sieci szkieletowej usług jest w wersji zapoznawczej.   

Ponadto możesz są aktywne powiadomienie pocztą e-mail o niektórych typów błędów, takich jak czas ładowania strony powoli, przez narzędzie do wykrywania inteligentne.  Nie należy wykonać żadnej konfiguracji dotyczących tego narzędzia. Aby uzyskać więcej informacji, zobacz [inteligentne wykrywanie - anomalii wydajności](../application-insights/app-insights-proactive-performance-diagnostics.md).



## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o

* [Azure Monitor wideo z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Wprowadzenie do platformy Azure monitora](monitoring-get-started.md)
* [Diagnostyka Azure](../azure-diagnostics.md) Jeśli próbujesz do diagnozowania problemów w usłudze chmury maszyny wirtualnej maszyny wirtualnej skalowanie aplikacji sieci szkieletowej ustawiona lub usługi.
* [Usługa Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Jeśli próbujesz diagnostycznych problemy w aplikacji sieci Web usługi aplikacji.
* [Zaloguj się Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) i [Operations Management Suite](https://www.microsoft.com/oms/) produkcji rozwiązanie monitorujące,
