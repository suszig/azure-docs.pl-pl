---
title: "Eksploruj dzienniki śledzenia w usłudze Azure Application Insights w języku Java | Dokumentacja firmy Microsoft"
description: "Wyszukiwanie narzędzia Log4J lub Logback dane śledzenia w usłudze Application Insights"
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: 6e441c9cbd15bb1528ea8e8a781f90900af90cf2
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Eksploruj dzienniki śledzenia w usłudze Application Insights w języku Java
Jeśli używasz Logback lub Log4J (1.2 lub 2.0) śledzenia, może mieć dzienników śledzenia automatycznie przesyłane do usługi Application Insights, gdzie można eksplorować i ich wyszukiwanie.

## <a name="install-the-java-sdk"></a>Instalowanie oprogramowania Java SDK

Zainstaluj [zestaw SDK usługi Application Insights dla języka Java][java], jeśli jeszcze nie który.

(Jeśli nie chcesz śledzić żądania HTTP, można pominąć większość plik XML konfiguracji, ale musi zawierać co najmniej `InstrumentationKey` elementu. Należy także wywołać `new TelemetryClient()` zainicjować zestawu SDK.)


## <a name="add-logging-libraries-to-your-project"></a>Dodaj do projektu biblioteki rejestrowania
*Wybierz odpowiedni sposób dla danego projektu.*

#### <a name="if-youre-using-maven"></a>Jeśli używasz narzędzia Maven...
Projekt jest skonfigurowana do używania Maven dla kompilacji, jeden z następujących fragmentów kodu scalić plik pom.xml.

Następnie Odśwież zależności projektu, aby uzyskać pobierane pliki binarne.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*1.2 Log4J*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Jeśli używasz narzędzia Gradle...
Jeśli projekt jest skonfigurowana do używania narzędzia Gradle dla kompilacji, Dodaj jedną z poniższych wierszy, aby `dependencies` w pliku build.gradle:

Następnie Odśwież zależności projektu, aby uzyskać pobierane pliki binarne.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**1.2 Log4J**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>W innym przypadku...
Pobierz i Wyodrębnij appendera odpowiednie, a następnie dodaj odpowiednią bibliotekę do projektu:

| Rejestratora | Do pobrania | Biblioteka |
| --- | --- | --- |
| Logback |[Zestaw SDK z appendera Logback](https://aka.ms/xt62a4) |applicationinsights — rejestrowanie logback |
| Log4J v2.0 |[Zestaw SDK z narzędzia Log4J appendera v2](https://aka.ms/qypznq) |applicationinsights — rejestrowanie log4j2 |
| 1.2 Log4j |[Zestaw SDK z narzędzia Log4J appendera 1.2](https://aka.ms/ky9cbo) |applicationinsights — rejestrowanie log4j1_2 |

## <a name="add-the-appender-to-your-logging-framework"></a>Dodawanie appendera do Twojej struktury rejestrowania
Aby rozpocząć pobieranie danych śledzenia, Scal odpowiedni fragment kodu do pliku konfiguracji narzędzia Log4J lub Logback: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*1.2 Log4J*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Appenders usługi Application Insights można odwoływać się przez wszystkie skonfigurowane rejestratora i niekoniecznie rejestratora głównego (jak pokazano w powyższych przykładach kodu).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Eksploruj dane śledzenia w portalu usługi Application Insights
Teraz, gdy skonfigurowano z projektem, aby wysyłał informacje śledzenia do usługi Application Insights, można wyświetlać i wyszukać te operacje śledzenia w portalu usługi Application Insights w [wyszukiwania] [ diagnostic] bloku.

![Otwórz wyszukiwanie w portalu usługi Application Insights](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Następne kroki
[Wyszukiwanie diagnostycznych][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


