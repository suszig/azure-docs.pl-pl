---
title: "Integracja programu SCOM z usługi Application Insights | Dokumentacja firmy Microsoft"
description: "Jeśli jesteś użytkownikiem SCOM monitorowania wydajności i diagnozowanie problemów z usługą Application Insights. Kompleksowe pulpity nawigacyjne, inteligentne alertów, zaawansowanych narzędzi diagnostycznych i zapytań analiz."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: mbullwin
ms.openlocfilehash: 35ea37b751909e14e616a965462b832e4e51bae0
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Monitorowanie wydajności aplikacji za pomocą usługi Application Insights dla oprogramowania SCOM
Jeśli używasz programu System Center Operations Manager (SCOM) do zarządzania serwerami, można monitorować wydajność i diagnozowanie problemów z wydajnością przy pomocy [Azure Application Insights](app-insights-asp-net.md). Usługi Application Insights monitoruje aplikację sieci web żądania przychodzące, wychodzące REST i wywołania SQL, wyjątków i ślady dziennika. Zapewnia on pulpity nawigacyjne z wykresami metryki i inteligentne alerty, a także zaawansowane wyszukiwanie diagnostycznych i zapytania analityczne przez ten telemetrii. 

Można przełączyć na monitorowanie usługi Application Insights przy użyciu pakietu administracyjnego programu SCOM.

## <a name="before-you-start"></a>Przed rozpoczęciem
Przyjęto założenie:

* Serwery sieci web znasz SCOM i użyj SCOM 2012 R2 lub 2016 do zarządzania programu IIS.
* Zostały już zainstalowane na serwerach aplikacji sieci web, który chcesz monitorować za pomocą usługi Application Insights.
* Wersja platformy aplikacji jest .NET 4.5 lub nowszej.
* Masz dostęp do subskrypcji w [Microsoft Azure](https://azure.com) i zalogować się do [portalu Azure](https://portal.azure.com). Twoja organizacja może mieć subskrypcję, a do niej dodać konto Microsoft.

(Zespół deweloperów może zbudować [zestaw SDK usługi Application Insights](app-insights-asp-net.md) do aplikacji sieci web. Instrumentacja to czas kompilacji daje im większa elastyczność w pisaniu telemetria niestandardowa. Jednak nie ma znaczenia: możesz wykonać kroki opisane w tym miejscu lub bez wbudowany zestaw SDK.)

## <a name="one-time-install-application-insights-management-pack"></a>(Jeden raz) Zainstaluj pakiet administracyjny usługi Application Insights
Na komputerze, na którym uruchamiasz programu Operations Manager:

1. Odinstaluj wszelkie starą wersję pakietu administracyjnego:
   1. W programie Operations Manager Otwórz administracji, pakietów administracyjnych. 
   2. Usuń starą wersję.
2. Pobierz i zainstaluj pakiet administracyjny z katalogu.
3. Ponowne uruchomienie programu Operations Manager.

## <a name="create-a-management-pack"></a>Utwórz pakiet administracyjny
1. W programie Operations Manager, otwórz **tworzenie**, **.NET... with usługi Application Insights**, **Kreator dodawania monitorowania**i ponownie wybierz **.NET... z aplikacją Informacje na temat technologii**.
   
    ![](./media/app-insights-scom/020.png)
2. Nazwa konfiguracji po aplikacji. (Należy Instrumentacja jedną aplikację naraz.)
   
    ![](./media/app-insights-scom/030.png)
3. Na tej samej stronie kreatora Utwórz nowy pakiet administracyjny albo wybierz pakiet, który wcześniej utworzony dla usługi Application Insights.
   
     (W usłudze Application Insights [pakiet administracyjny](https://technet.microsoft.com/library/cc974491.aspx) szablonem, z którego można utworzyć wystąpienia. Można ponownie użyć tego samego wystąpienia później.)

    ![Na karcie Ogólne właściwości wpisz nazwę aplikacji. Kliknij przycisk Nowy, a następnie wpisz nazwę dla pakietu administracyjnego. Kliknij przycisk OK, a następnie kliknij przycisk Dalej.](./media/app-insights-scom/040.png)

1. Wybierz jedną aplikację, która ma być monitorowany. Funkcja wyszukiwania wyszukuje między aplikacji zainstalowanych na serwerach.
   
    ![Co na karcie monitorowanie kliknij przycisk Dodaj, wpisz część nazwy aplikacji, kliknij przycisk Wyszukaj, wybierz aplikację, a następnie dodaj OK.](./media/app-insights-scom/050.png)
   
    Pole opcjonalne zakresu monitorowania można określić podzestaw serwerów, jeśli nie chcesz monitorować aplikację na wszystkich serwerach.
2. Na następnej stronie kreatora musisz podać swoje poświadczenia, aby zalogować się do systemu Microsoft Azure.
   
    Na tej stronie wybierz polecenie zasobu usługi Application Insights miejscu danych telemetrycznych i przeanalizowane. 
   
   * Jeśli aplikacja została skonfigurowana pod kątem usługi Application Insights podczas tworzenia, wybierz jej istniejącego zasobu.
   * W przeciwnym razie utwórz nowy zasób o nazwie aplikacji. Jeśli istnieją inne aplikacje, które są składnikami tego samego systemu, umieść je w tej samej grupie zasobów, aby ułatwić dostęp na podstawie danych telemetrycznych do zarządzania.
     
     Te ustawienia można zmienić później.
     
     ![Na karcie Ustawienia usługi Application Insights kliknij przycisk "Zarejestruj" i podaj poświadczenia konta Microsoft Azure. Następnie wybierz pozycję subskrypcji, grupy zasobów i zasobów.](./media/app-insights-scom/060.png)
3. Ukończ pracę kreatora.
   
    ![Kliknięcie pozycji Utwórz](./media/app-insights-scom/070.png)

Powtórz tę procedurę dla każdej aplikacji, które chcesz monitorować.

Jeśli trzeba zmienić ustawienia, ponownie otwórz właściwości monitora z okna Tworzenie.

![W tworzenie, wybierz opcję monitorowanie wydajności aplikacji .NET z usługi Application Insights, wybierz monitora, a następnie kliknij przycisk Właściwości.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Sprawdź monitorowania
Monitor, na każdym serwerze zainstalowano wyszukiwania dla aplikacji. Gdy znajdzie aplikacji, ponieważ konfiguruje Monitor stanu usługi Application Insights do monitorowania aplikacji. W razie potrzeby najpierw instaluje Monitor stanu na serwerze.

Aby sprawdzić, które wystąpienia aplikacji znalazła:

![Monitorowanie, otwórz usługę Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Dane telemetryczne wyświetleń w usłudze Application Insights
W [portalu Azure](https://portal.azure.com), przejdź do zasobów dla aplikacji. Możesz [zobaczyć wykresy przedstawiający telemetrii](app-insights-dashboards.md) z aplikacji. (Jeśli go nie nie pojawiają na stronie głównej jeszcze, kliknij strumień na żywo metryk).

## <a name="next-steps"></a>Następne kroki
* [Konfigurowanie pulpitu nawigacyjnego](app-insights-dashboards.md) ze sobą najważniejszych wykresy monitorowania to i inne aplikacje.
* [Dowiedz się więcej o metryk](app-insights-metrics-explorer.md)
* [Konfigurowanie alertów](app-insights-alerts.md)
* [Diagnozowanie problemów z wydajnością](app-insights-detect-triage-diagnose.md)
* [Zaawansowane zapytania analityczne](app-insights-analytics.md)
* [Dostępność testy sieci web](app-insights-monitor-web-app-availability.md)

