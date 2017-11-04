---
title: "Oddzielanie dane telemetryczne z programowania, testowania i wersji w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Bezpośrednie telemetrii do różnych zasobów dla rozwoju, testów i produkcji sygnatury."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 8d95958bce0597bfb16ef1c6b99b72ce9134e66f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Oddzielanie danych telemetrycznych z programowanie, testowego i produkcyjnego

Podczas tworzenia następnej wersji aplikacji sieci web, użytkownik nie chce pomylone [usługi Application Insights](app-insights-overview.md) dane telemetryczne z nowej wersji i już wydanej wersji. Aby uniknąć pomyłek, należy wysłać dane telemetryczne z różnych etapach rozwoju do oddzielania zasobów usługi Application Insights z kluczami oddzielne Instrumentacji (ikeys). Aby ułatwić zmienić klucza instrumentacji, ponieważ wersja są przenoszone z jednego etapu do innego, może być ustalenie ikey w kodzie, a nie w pliku konfiguracji. 

(Jeśli system jest usługi w chmurze Azure, Brak [innej metody ustawienia oddzielnych ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Dotyczące zasobów i klucze Instrumentacji

Po skonfigurowaniu monitorowanie usługi Application Insights dla aplikacji sieci web, tworzenia usługi Application Insights *zasobów* platformie Microsoft Azure. Możesz otworzyć ten zasób w portalu Azure, aby wyświetlić i analizować dane telemetryczne zebrane z aplikacji. Zasób jest identyfikowany przez *klucza Instrumentacji* (ikey). Po zainstalowaniu pakietu usługi Application Insights do monitorowania aplikacji, możesz ją skonfigurować z klucza Instrumentacji tak, aby miejsce wysyłania danych telemetrycznych.

Zwykle wybierzesz oddzielne zasoby lub pojedynczy zasób udostępniony w różnych scenariuszach:

* Aplikacje innych, niezależnie od - używać oddzielnego zasobów i ikey dla każdej aplikacji.
* Wiele składników lub role w aplikacji biznesowej jeden — użyj [pojedynczy zasób udostępniony](app-insights-monitor-multi-role-apps.md) dla wszystkich składników aplikacji. Można filtrować dane telemetryczne i segmentowanych przez właściwość cloud_RoleName.
* Programowanie, testów i Release - Użyj oddzielnych zasobów i ikey dla wersji systemu w "sygnatury" etapie produkcji.
* A | Testowanie B — użyj pojedynczego zasobu. Utwórz TelemetryInitializer można dodać właściwości do danych telemetrii, który identyfikuje wariantów.


## <a name="dynamic-ikey"></a>Klucz Instrumentacji dynamiczne

Aby ułatwić zmienić ikey, gdy kod przechodzi między etapami produkcji, ustaw go w kodzie zamiast w pliku konfiguracji.

Ustaw klucz w metodzie inicjowania, takich jak pliku global.aspx.cs w usługi ASP.NET:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

W tym przykładzie ikeys dla różnych zasobów są umieszczane w różnych wersjach pliku konfiguracji sieci web. Plik konfiguracji sieci web — co można zrobić w ramach skryptu - wymiany będą wymiany zasobu docelowego.

### <a name="web-pages"></a>Strony sieci Web
IKey jest również używany na stronach sieci web aplikacji, w [skryptu pochodzący z bloku szybki start](app-insights-javascript.md). Zamiast kodowania go bezpośrednio do skryptu, wygenerowany na podstawie stanu serwera. Na przykład w aplikacji ASP.NET:

*Język JavaScript w Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Tworzenie dodatkowych zasobów usługi Application Insights
Aby rozdzielić telemetrii składników w innej aplikacji lub inny sygnatur (deweloperów testu/produkcja) tego składnika, następnie musisz utworzyć nowy zasób usługi Application Insights.

W [portal.azure.com](https://portal.azure.com), Dodaj zasób usługi Application Insights:

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-separate-resources/01-new.png)

* **Typ aplikacji** wpływa na informacje wyświetlane na bloku omówienie i właściwości, które są dostępne w [Eksploratora metryk](app-insights-metrics-explorer.md). Jeśli nie widzisz typ aplikacji, wybierz jeden z typów sieci web dla stron sieci web.
* **Grupa zasobów** jest wygodne dla właściwości, takie jak zarządzanie [kontrola dostępu](app-insights-resources-roles-access-control.md). Można użyć oddzielnych grup zasobów dla rozwoju, testów i produkcji.
* **Subskrypcja** Twojego konta płatności na platformie Azure.
* **Lokalizacja** jest, gdzie możemy przechowywanie danych. Obecnie nie można zmienić. 
* **Dodaj do pulpitu nawigacyjnego** umieszcza kafelka szybkiego dostępu dla zasobu na stronie głównej Azure. 

Trwa tworzenie zasobu zajmuje kilka sekund. Zostanie wyświetlony alert po jego zakończeniu.

(Można napisać [skrypt programu PowerShell](app-insights-powershell-script-create-resource.md) automatyczne tworzenie zasobu.)

### <a name="getting-the-instrumentation-key"></a>Wprowadzenie klucza Instrumentacji
Klucz Instrumentacji identyfikatorem zasobu, który został utworzony. 

![Kliknij Essentials, kliknij przycisk klucza Instrumentacji klawisze CTRL + C](./media/app-insights-separate-resources/02-props.png)

Należy klucze Instrumentacji wszystkich zasobów, do której aplikacja będzie wysyłać dane.

## <a name="filter-on-build-number"></a>Filtrowanie według numeru kompilacji
Po opublikowaniu nowej wersji aplikacji chcesz mieć możliwość oddzielenia dane telemetryczne z różnych kompilacji.

Można ustawić właściwości wersji aplikacji, dzięki czemu można filtrować [wyszukiwania](app-insights-diagnostic-search.md) i [Eksploratora metryk](app-insights-metrics-explorer.md) wyników.

![Filtrowanie według właściwości](./media/app-insights-separate-resources/050-filter.png)

Istnieje kilka różnych metod ustawiania właściwości wersji aplikacji.

* Ustaw bezpośrednio:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Zawijaj tego wiersza w [inicjatora telemetrii](app-insights-api-custom-events-metrics.md#defaults) aby upewnić się, że wszystkie wystąpienia TelemetryClient są stale ustawione.
* [ASP.NET] Ustaw wersję `BuildInfo.config`. Wersja w węźle BuildLabel przejmą moduł sieci web. Dołączyć ten plik do projektu i pamiętaj, aby ustawić właściwości zawsze Kopiuj w Eksploratorze rozwiązań.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] BuildInfo.config ma być automatycznie wygenerowany w programie MSBuild. Aby to zrobić, dodaj kilka wierszy do Twojej `.csproj` pliku:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Spowoduje to wygenerowanie pliku o nazwie *yourProjectName*. BuildInfo.config. Proces publikowania zmienia ją BuildInfo.config.

    Etykieta kompilacji zawiera symbol zastępczy (AutoGen_...), podczas kompilacji z programem Visual Studio. Jednak podczas tworzenia przy użyciu programu MSBuild, jest wypełniana numer poprawnej wersji.

    Aby umożliwić MSBuild, aby wygenerować numery wersji, Ustaw wersję jak `1.0.*` w AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Śledzenie wersji i wydania
Aby śledzić wersje aplikacji, upewnij się, że plik `buildinfo.config` jest generowany przez proces aparatu Microsoft Build Engine. W pliku .csproj dodaj ten kod:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Jeśli plik zawiera informację o kompilacji, moduł sieci Web usługi Application Insights automatycznie dodaje **wersję aplikacji** jako właściwość do każdego elementu telemetrii. Pozwala to na filtrowanie według wersji podczas przeprowadzania [wyszukiwania diagnostycznego](app-insights-diagnostic-search.md) lub [eksplorowania metryk](app-insights-metrics-explorer.md).

Numer wersji kompilacji jest jednak generowany tylko przez aparat Microsoft Build Engine, a nie podczas kompilowania przez deweloperów w programie Visual Studio.

### <a name="release-annotations"></a>Adnotacje dotyczące wersji
Jeśli korzystasz z usługi Visual Studio Team Services, możesz [uzyskać znacznik adnotacji](app-insights-annotations.md) dodawany do Twoich wykresów, za każdym razem, gdy wydasz nową wersję. Na następującej ilustracji pokazano sposób wyświetlania tego znacznika.

![Zrzut ekranu przedstawiający przykładową adnotację dotyczącą wersji widoczną na wykresie](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Następne kroki

* [Udostępnione zasoby do wielu ról](app-insights-monitor-multi-role-apps.md)
* [Utwórz inicjatora Telemetrii, aby odróżnić A | Wariantów B](app-insights-api-filtering-sampling.md#add-properties)
