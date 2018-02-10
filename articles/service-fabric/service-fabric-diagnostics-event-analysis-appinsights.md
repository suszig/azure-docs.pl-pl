---
title: "Analiza zdarzeń sieci szkieletowej usług Azure z usługą Application Insights | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat wizualizacji i analizowania zdarzeń za pomocą usługi Application Insights, monitorowania i diagnostyki klastrów sieci szkieletowej usług Azure."
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 479e486dca432020d5fcbaf98971a9803888bf98
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Analiza zdarzeń i wizualizacji z usługą Application Insights

Azure Application Insights to rozszerzalna platforma aplikacji monitorowania i diagnostyki. Obejmuje zaawansowane analizy i badania narzędzia, można dostosować pulpit nawigacyjny i wizualizacje i dalsze opcje w tym automatycznego tworzenia alertu. Jest zalecana platformy do monitorowania i diagnostyki dla aplikacji platformy Service Fabric i usług.

## <a name="setting-up-application-insights"></a>Konfigurowanie usługi Application Insights

### <a name="creating-an-ai-resource"></a>Tworzenie zasobu AI

Aby utworzyć AI zasobu siedzibę za pośrednictwem portalu Azure Marketplace i wyszukaj "Usługi Application Insights". Powinien on wyświetlane jako pierwsze rozwiązanie (znajduje się w kategorii "Web + Mobile"). Kliknij przycisk **Utwórz** podczas szukania na prawo zasobów (Sprawdź, czy ścieżka zgodna obraz poniżej).

![Nowy zasób usługi Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Konieczne będzie podanie niektórych informacji poprawnie zainicjować zasobu. W *typu aplikacji* pola, użyj "Aplikacja sieci web ASP.NET" Jeśli będziesz używać dowolną sieć szkieletowa usług modele programowania lub publikowanie aplikacji .NET do klastra. Użyj "Ogólne", jeśli będziesz wdrażać pliki wykonywalne gościa i kontenerów. Ogólnie rzecz biorąc, domyślnie przy użyciu "Aplikacja sieci web ASP.NET" Aby opcje otwarte w przyszłości. Nazwa zależy od swoich preferencji, a zarówno grupy zasobów, jak i subskrypcji są włączone po wdrożeniu zasobu. Zaleca się, że zasób AI znajduje się w tej samej grupie zasobów co klaster. Aby uzyskać więcej informacji, zobacz [utworzyć zasobu usługi Application Insights](../application-insights/app-insights-create-new-resource.md)

Należy klucza Instrumentacji AI w celu skonfigurowania AI narzędziem agregacji zdarzeń. Po zasobu AI (zajmuje kilka minut po zweryfikowaniu jest wdrożenie), przejdź do niego i Znajdź **właściwości** sekcji na pasku nawigacyjnym po lewej stronie. Zostanie otwarty nowy blok pokazujący *klucza INSTRUMENTACJI*. Jeśli musisz zmienić subskrypcji lub grupy zasobów zasobu, mogą to robić tutaj również.

### <a name="configuring-ai-with-wad"></a>Konfigurowanie AI z WAD

>[!NOTE]
>Dotyczy tylko klastry z systemem Windows w tej chwili.

Istnieją dwa podstawowe sposoby wysyłania danych z WAD do AI Azure, która jest to osiągane przez dodanie zbiornika AI w konfiguracji WAD, zgodnie z opisem w [w tym artykule](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Dodaj klucz Instrumentacji AI, podczas tworzenia klastra w portalu Azure

![Dodawanie AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Podczas tworzenia klastra, jeśli włączono diagnostyki "Włączone", zostaną wyświetlone pole opcjonalne, aby wprowadzić klucz Instrumentacji wgląd w aplikacji. Po wklejeniu Twojej IKey AI tutaj zbiornika AI zostanie skonfigurowana automatycznie dla Ciebie w szablonie usługi Resource Manager, który służy do wdrażania klastra.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>Dodaj obiekt Sink AI do szablonu usługi Resource Manager

W "WadCfg" w szablonie usługi Resource Manager należy dodać "Sink", umieszczając w niej następujące dwie zmiany:

1. Dodaj konfigurację zbiornika bezpośrednio po deklarowanie z `DiagnosticMonitorConfiguration` zostało zakończone:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Obejmują zbiornika w `DiagnosticMonitorConfiguration` , dodając następujący wiersz w `DiagnosticMonitorConfiguration` z `WadCfg` (tuż przed `EtwProviders` są deklarowane jako):

    ```json
    "sinks": "applicationInsights"
    ```

W obu fragmenty kodu powyżej nazwa "applicationInsights" została użyta do opisywania sink. Nie jest wymagane i tak długo, jak nazwa sink znajduje się w "sink", można ustawić nazwę do dowolnego ciągu.

Obecnie dzienników z klastra będzie wyświetlany jako dane śledzenia w przeglądarce dzienników AI firmy. Ponieważ większość śladów pochodzących od platformy jest poziom "Informacyjny", możesz również należy rozważyć zmianę konfiguracji zbiornika tylko do wysyłania dzienników typu "Krytyczne" lub "Error". Można to zrobić przez dodanie "węzła kanały" do obiektu sink, jak pokazano w [w tym artykule](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Użycie niepoprawne IKey AI w portalu lub w szablonie usługi Resource Manager należy ręcznie zmienić wartość klucza i zaktualizować klaster / Wdróż go ponownie. 

### <a name="configuring-ai-with-eventflow"></a>Konfigurowanie AI z EventFlow

Jeśli używasz EventFlow celu agregowania zdarzeń, upewnij się, że importowanie `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`pakietu NuGet. Następujące musi zostać uwzględniony w *generuje* sekcji *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace the following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Upewnij się wprowadzić wymagane zmiany w filtry, a także zawierać innych danych wejściowych (wraz z ich odpowiednich pakietów NuGet).

## <a name="aisdk"></a>AI.SDK

Ogólnie zaleca się użyć EventFlow i WAD jako rozwiązania agregacji, ponieważ umożliwiają one dla bardziej podejściu Diagnostyka i monitorowanie, tj. Jeśli chcesz zmienić Twoje dane wyjściowe z EventFlow wymaga nie zmieniono Twojego rzeczywiste Instrumentacji tylko proste modyfikację do pliku konfiguracji. Jeśli jednak podjąć decyzję o inwestycji w za pomocą usługi Application Insights i prawdopodobnie nie można zmienić na innej platformie, należy rozważyć w celu agregowania zdarzeń i wysyłania ich do AI przy użyciu jego AI nowego zestawu SDK. Oznacza to, przestaną być skonfigurować EventFlow do wysyłania danych do AI, ale zamiast tego zostanie zainstalowany pakiet NuGet usługi Service Fabric ApplicationInsight. Szczegółowe informacje dotyczące pakietu można znaleźć [tutaj](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Obsługa usługi Application Insights Mikrousług i kontenery](https://azure.microsoft.com/en-us/blog/app-insights-microservices/) przedstawiono niektóre nowe funkcje działające na (nadal obecnie w wersji beta), które umożliwiają są bardziej rozbudowane możliwości monitorowania poza pole z AI. Obejmują one śledzenia zależności (używanego podczas konstruowania AppMap wszystkich usług i aplikacji w klastrze i komunikację między nimi) oraz lepsze korelacji śladów pochodzących z Twoimi usługami (pomaga w lepiej trafić problemu w przepływie pracy programu aplikacji lub usługi).

Jeśli opracowujesz w .NET i zostanie przy użyciu niektóre usługi sieć szkieletowa programowania modeli i czy chce używać AI jako platformy do wizualizacji i analizowania danych zdarzeń i dzienników, następnie zaleca się możesz przejść za pomocą trasy AI SDK jako monitorowania i diagnos e przepływ pracy. Odczyt [to](../application-insights/app-insights-asp-net-more.md) i [to](../application-insights/app-insights-asp-net-trace-logs.md) wprowadzenie gromadzenie i wyświetlanie dzienników przy użyciu AI.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Nawigowanie po zasobu AI w portalu Azure

Po skonfigurowaniu AI jako dane wyjściowe dla zdarzeń i dzienniki informacji należy zacząć wyświetlani w zasobie AI za kilka minut. Przejdź do zasobu AI, co spowoduje przejście do pulpitu nawigacyjnego zasobów AI. Kliknij przycisk **wyszukiwania** AI pasku zadań, aby zobaczyć najnowsze dane śledzenia, które otrzymała i można filtrować za ich pośrednictwem.

*Eksploratora metryk* stanowi przydatne narzędzie do tworzenia niestandardowych pulpitów nawigacyjnych w oparciu metryki aplikacji, usług i klaster mogą raportowania. Zobacz [metryki Eksplorowanie w usłudze Application Insights](../application-insights/app-insights-metrics-explorer.md) do skonfigurowania kilku wykresów dla siebie na podstawie danych są zbierane.

Kliknięcie przycisku **Analytics** spowoduje przejście do portalu Application Insights Analytics, gdzie można zbadać zdarzenia i dane śledzenia z większą zakresu i opcjonalność. Dowiedz się więcej o tym w [analizy w usłudze Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Kolejne kroki

* [Konfigurowanie alertów w AI](../application-insights/app-insights-alerts.md) Aby otrzymywać powiadomienia o zmianach wprowadzonych w wydajności i użycia
* [Inteligentne wykrywania w usłudze Application Insights](../application-insights/app-insights-proactive-diagnostics.md) wykonuje aktywnego analizy telemetrii są wysyłane do AI, aby ostrzec użytkownika o potencjalnych problemów z wydajnością
