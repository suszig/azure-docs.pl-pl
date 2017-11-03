---
title: "Monitorowanie wydajności aplikacji sieci web Java w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Rozszerzone wydajności monitorowania użycia witryny sieci Web Java z usługą Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: ecfcf7a3b3698435f98b74474d0ca7223ab2b46c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Monitor zależności, wyjątków i czasu wykonywania w aplikacji sieci web Java


Jeśli masz [instrumentacji aplikacji sieci web Java za pomocą usługi Application Insights][java], agenta programu Java można użyć, aby uzyskać bardziej szczegółowe bez wprowadzania żadnych zmian kodu:

* **Zależności:** dane dotyczące wywołania, które umożliwia aplikacji do innych składników, w tym:
  * **Wywołania REST** wprowadzone za pośrednictwem HttpClient, OkHttp i RestTemplate (Spring).
  * **Redis** wywołań za pomocą klienta Jedis. Jeśli wywołanie trwa dłużej niż 10s, agent również pobiera argumenty wywołania.
  * **[Wywołania JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, bazy danych Oracle lub Apache DOM DB. wywołania "executeBatch" są obsługiwane. MySQL i PostgreSQL Jeśli wywołanie trwa dłużej niż 10s, agenci będą raportować planu zapytania.
* **Przechwycono wyjątkami:** dane dotyczące wyjątków, które są obsługiwane w kodzie.
* **Czas wykonania metody:** dane dotyczące czas potrzebny do wykonywania określonych metod.

Aby użyć agenta programu Java, należy ją zainstalować na serwerze. Aplikacje sieci web musi być instrumentowane przy użyciu [zestaw SDK Java usługi Application Insights][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Zainstaluj agenta usługi Application Insights dla języka Java
1. Na komputerze z serwerem Java [Pobierz agenta](https://aka.ms/aijavasdk).
2. Edytowanie skryptu uruchamiania serwera aplikacji, a następnie dodaj poniższe JVM:
   
    `javaagent:`*Pełna ścieżka do pliku JAR agenta*
   
    Na przykład w Tomcat na komputerze z systemem Linux:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Uruchom ponownie serwer aplikacji.

## <a name="configure-the-agent"></a>Konfigurowanie agenta
Utwórz plik o nazwie `AI-Agent.xml` i umieścić go w tym samym folderze co plik JAR agenta.

Ustaw zawartość pliku xml. Edytuj poniższy przykład można uwzględnić lub pominąć funkcje, które chcesz.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Należy włączyć wyjątek raporty i metody chronometrażu dla poszczególnych metod.

Domyślnie `reportExecutionTime` ma wartość true i `reportCaughtExceptions` ma wartość false.

## <a name="view-the-data"></a>Wyświetlanie danych
W zasobu usługi Application Insights zostanie wyświetlone zagregowane zdalnego zależności i metody wykonywania razy [w obszarze kafelka wydajności][metrics].

Aby wyszukać poszczególne wystąpienia raporty zależności, wyjątków i metody, otwórz [wyszukiwania][diagnostic].

[Diagnozowanie problemów z zależności — Dowiedz się więcej](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Pytania? Problemy?
* Brak danych? [Wyjątki zapory zestawu](app-insights-ip-addresses.md)
* [Rozwiązywanie problemów z technologią Java](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
