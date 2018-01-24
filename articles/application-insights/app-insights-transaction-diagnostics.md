---
title: Diagnostyka transakcji Insights aplikacji Azure | Dokumentacja firmy Microsoft
description: "Usługa Application diagnostics transakcji end-to-end Insights"
services: application-insights
documentationcenter: .net
author: SoubhagyaDash
manager: victormu
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2018
ms.author: sdash
ms.openlocfilehash: b090699cf90c74af8480b811901b6e3078b007b3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/23/2018
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Diagnostyka ujednoliconego transakcji między składnikami

*To środowisko jest obecnie w wersji zapoznawczej i zastępuje istniejące bloków diagnostyki dla żądania po stronie serwera, zależności i wyjątki.*

Wersja zapoznawcza wprowadza nową funkcjonalność ujednoliconego diagnostyki odpowiadająca automatycznie telemetrii po stronie serwera z dotyczące wszystkich składników usługi Application Insights monitorowane w jednym widoku. Nie ma znaczenia, jeśli masz wiele zasobów z kluczami oddzielne Instrumentacji; Usługa Application Insights wykrycia relacji podstawowej i umożliwia łatwe diagnozowanie składnika aplikacji, zależności lub wyjątek, która spowodowała spowolnienie transakcji lub niepowodzenie.

## <a name="what-does-component-mean-in-the-context-of-application-insights"></a>Co oznacza składnik w kontekście usługi Application Insights?

Składniki są niezależnie można wdrożyć części aplikacji rozproszonych mikrousług. Zespoły deweloperów i operacji mają widoczność na poziomie kodu lub dostępu do danych telemetrycznych generowane przez te składniki aplikacji. 

* Składniki różnią się od "obserwowanych" zależności zewnętrzne, takie jak SQL, EventHub itp., które zespół/organizacji nie mogą mieć dostęp do (kod lub dane telemetryczne).
* Składniki uruchamiane na dowolnej liczbie wystąpień kontenera server/roli.
* Składniki mogą być osobne klucze Instrumentacji usługi Application Insights (nawet jeśli różnią się subskrypcje) lub różne role podlegające jednego klucza Instrumentacji usługi Application Insights. Nowe środowisko przedstawia szczegółowe informacje dotyczące wszystkich składników, niezależnie od tego, jak ich zostały zdefiniowane.

> [!Tip]
> Aby uzyskać najlepsze wyniki upewnij się, że wszystkie składniki są instrumentowane przy użyciu najnowszych SDK stabilna usługi Application Insights. W przypadku różnych zasobów usługi Application Insights, upewnij się, że masz odpowiednie uprawnienia, aby wyświetlić ich telemetrii.

## <a name="enable-and-access"></a>Włączanie i dostępu
Włącz "Unified szczegółów: Diagnostyka transakcji E2E" z [listy podglądy](app-insights-previews.md)

![Włącz podgląd](media/app-insights-e2eTxn-diagnostics/previews.png)

Ta wersja zapoznawcza jest obecnie dostępny dla żądania po stronie serwera, zależności i wyjątki. Są dostępne nowe środowisko w **wyniki wyszukiwania**, **wydajności**, lub **błąd** klasyfikowanie środowiska. Wersja zapoznawcza zastępuje odpowiednie bloki szczegóły klasycznego. 

![Próbek wydajności](media/app-insights-e2eTxn-diagnostics/performanceSamplesClickThrough.png)

## <a name="transaction-diagnostics-experience"></a>Wyniki diagnostyki transakcji 
Ten widok zawiera trzy części klucza: wykres transakcji między składnikami, listę sekwencji czas wszystkie dane telemetryczne z operacją określonego składnika, a w okienku szczegółów dowolnego elementu wybrane dane telemetryczne po lewej stronie.

![Części klucza](media/app-insights-e2eTxn-diagnostics/3partsCrossComponent.png)

### <a name="1-cross-component-transaction-chart"></a>[1] między składnikami transakcji wykresu

Ten wykres zawiera oś czasu z słupkach poziomych na czas trwania żądania i zależności między składnikami. Wszelkie wyjątki, które są zbierane są również oznaczane na osi czasu.

* Pierwszy wiersz na tym wykresie reprezentuje punkt wejścia żądania przychodzącego, jeśli na pierwszym składnikiem o nazwie w tej transakcji. Czas trwania jest łączny czas do ukończenia transakcji.
* Wszystkie wywołania zależności zewnętrznych są proste-zwijanej wiersze, w których ikony reprezentujący typ zależności.
* Wywołania z innymi składnikami są zwijane wierszy. Każdy wiersz odpowiada określonej operacji wywołać składnika.
* Domyślnie żądania, zależności lub wyjątek, który początkowo zaznaczone jest wyświetlane na wykresie.
* Wybierz wszystkie wiersze, aby wyświetlić jego szczegóły po prawej stronie. Kliknij przycisk "ślady profilera Otwórz" lub "debugowania Otwórz migawkę" dla poziomu diagnostyki kodu w odpowiednich okienka szczegółów.

> [!NOTE]
Wywołania z innymi składnikami mają dwa wiersze: jeden wiersz reprezentuje wywołanie wychodzącego (zależności) ze składnika wywołującego i drugiego wiersza odpowiada żądania przychodzącego na składnik o nazwie. Ikona początkowych i różne style pasków czas trwania pomoże rozróżnianie między nimi.

### <a name="2-time-sequenced-telemetry-of-the-selected-component-operation"></a>[2] czas sekwencjonowania telemetrii operacji wybranego składnika.

Każdy wiersz na wykresie między składnikami transakcji jest powiązany z operacji wywoływanej w poszczególnych składników. Ta operacja wybranego składnika znajduje odzwierciedlenie w tytule dolnej części. Otworzyć tę sekcję, aby zobaczyć sekwencji płaskiej czas wszystkie dane telemetryczne dotyczące tej operacji. Można wybrać dowolny element telemetrii na tej liście, aby wyświetlić szczegóły po prawej stronie.

![Wszystkie dane telemetryczne czasu sekwencji](media/app-insights-e2eTxn-diagnostics/allTelemetryDrawerOpened.png)

### <a name="3-details-pane"></a>[3] w okienku szczegółów

Szczegóły wybranego elementu z jednej z dwóch części w tym okienku są wyświetlane po lewej stronie. "Pokaż wszystko" zawiera wszystkie standardowe atrybuty, które są zbierane. Atrybuty niestandardowe oddzielnie są wymienione poniżej standardowego zestawu.

![Szczegóły wyjątku](media/app-insights-e2eTxn-diagnostics/exceptiondetail.png)

## <a name="profiler-and-snapshot-debugger"></a>Debuger profilera i migawki

[Application Insights profilera](app-insights-profiler.md) lub [debugera migawki](app-insights-snapshot-debugger.md) pomoc w diagnostyce poziomie kodu problemów z wydajnością i niepowodzenie. Z tym środowiskiem widać ślady profilera, lub kliknij przycisk migawek z każdego składnika za pomocą pojedynczej.

![Integracji debugera](media/app-insights-e2eTxn-diagnostics/debugSnapshot.png)

## <a name="faq"></a>Często zadawane pytania

*Jeden składnik są widoczne na wykresie, a inne tylko są wyświetlane jako zależności zewnętrzne bez szczegółowości co się stało, w ramach tych składników.*

Potencjalne przyczyny:

* Czy inne składniki są instrumentowane przy użyciu usługi Application Insights?
* Są one przy użyciu najnowszych stabilna Insights zestaw SDK usługi Application?
* Jeśli te składniki są osobne zasobów usługi Application Insights, czy jest wymagany dostęp do swoich danych telemetrycznych?

Jeśli masz dostęp i składniki są instrumentowane przy użyciu najnowszych zestawów SDK Insights aplikacji, Daj nam znać za pośrednictwem kanału górnym prawym opinii.

*Masz tylko zależności zewnętrzne. Należy się o tej wersji zapoznawczej?*

Tak. Nowe środowisko łączy wszystkie powiązane dane telemetryczne po stronie serwera w ramach jednego widoku. Starsze bloków szczegółów zostanie zastąpione przez to doświadczenie w przyszłości umożliwiające wypróbować jej możliwości i przekaż nam swoją opinię. Oto wygląda następująco dla transakcji pojedynczego składnika:

![Środowisko pojedynczego składnika](media/app-insights-e2eTxn-diagnostics/singleComponent.png)

*Widać zduplikowane wiersze dla zależności. Jest to oczekiwane?*

W tej chwili możemy są wyświetlane wywołania zależności wychodzącego oddzielony od ruchu przychodzącego żądania. Zazwyczaj dwa wywołania Szukaj taka sama, jak są różne z powodu sieci Rundy tylko wartości czasu trwania. Ikona początkowych i różne style pasków czas trwania pomoże rozróżnianie między nimi. Jest myląca ze tej prezentacji danych? Przekaż nam swoją opinię!

*Jak zegar pochyla w wystąpieniach różnych składników?*

Osiach czasu są dostosowane do zegara pochyla na wykresie transakcji. Widać dokładne sygnatury czasowe, w okienku szczegółów lub za pomocą analizy.

*Dlaczego nowe środowisko brakuje większości kwerend powiązane elementy?*

Jest to celowe. Wszystkie powiązane elementy dotyczące wszystkich składników są już dostępne po lewej stronie (sekcje górny i dolny). Nowe środowisko zawiera dwie pozycje powiązane, które po lewej stronie nie obejmuje: wszystkie dane telemetryczne z pięciu minut przed i po to zdarzenie i harmonogram użytkownika.

*Dlaczego nowe środowisko nie ma funkcji I loved w blokach starsze?*

Prześlij nam swoją opinię! Chcemy w celu rozwiązania problemów, zanim to środowisko jest ogólnie dostępna po tym czasie zostaną wycofane starsze widoków. Teraz możesz wyłączyć podgląd, aby powrócić do starszych bloków.

## <a name="known-issues"></a>Znane problemy

* Błąd próbek z łącza mapowanie aplikacji do starszej karty szczegółów.
* Na podstawie autocluster szczegółowych danych w wynikach wyszukiwania łącze do starszej karty szczegółów.
* Integrację elementu roboczego nie jest dostępna w nowym środowisku.