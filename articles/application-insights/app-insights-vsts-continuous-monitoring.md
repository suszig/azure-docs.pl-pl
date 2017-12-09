---
title: "Ciągłe monitorowanie planowaną wersji DevOps z programu VSTS i Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Zawiera instrukcje umożliwiające szybkie ustawienia monitorowania ciągłego za pomocą usługi Application Insights"
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: article
manager: carmonm
ms.openlocfilehash: 5bfbdd0033f966422a84071a694845627827f016
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Dodaj Monitorowanie ciągłe do potoku sieci zlecenia

Visual Studio Team Services (VSTS) integruje się z usługą Azure Application Insights umożliwia ciągłego monitorowania potoku wersji sieci DevOps w całym cyklu programistycznym z oprogramowania. 

VSTS obsługuje teraz ciągłego monitorowania, według której wersji potoki dołączyć do nich danych monitorowania z usługi Application Insights i innych zasobów platformy Azure. Po wykryciu alert usługi Application Insights, wdrożenie może pozostawać warunkowe lub wycofane, do czasu rozwiązania alertu. Jeśli wszystkie wykryte, wdrożenia przejść automatycznie testu aż do środowiska produkcyjnego bez konieczności ręcznej interwencji. 

## <a name="configure-continuous-monitoring"></a>Skonfigurowanie funkcji ciągłego monitorowania

1. Wybierz istniejący projekt usługi VSTS.

2. Umieść kursor nad **kompilacji i wydania** > Wybierz **wersje** > kliknij **znak plus** > **Tworzenie wersji definicji** > Wyszukaj **monitorowania** > **wdrożenie usługi aplikacji Azure o ciągłego monitorowania.**

   ![Nowa definicja wersji programu VSTS](.\media\app-insights-continuous-monitoring\001.png)

3. Kliknij przycisk **zastosowania.**

4. Obok czerwony wykrzyknik zaznacz tekst na niebiesko do **wyświetlić zadania środowiska.**

   ![Wyświetl zadania środowiska](.\media\app-insights-continuous-monitoring\002.png)

   Pojawi się okno konfiguracji, skorzystaj z poniższej tabeli, aby wypełnić pole wejściowe.

    | Parametr        | Wartość |
   | ------------- |:-----|
   | **Nazwa środowiska**      | Nazwa opisująca środowiska definicji wersji |
   | **Subskrypcja platformy Azure** | Lista rozwijana wypełnia żadnej subskrypcji platformy Azure, połączony z kontem programu VSTS|
   | **Nazwa usługi aplikacji** | Ręczne wprowadzanie nową wartość może być wymagane dla tego pola, w zależności od innych opcji |
   | **Grupa zasobów**    | Lista rozwijana wypełnia dostępnych grup zasobów |
   | **Nazwa zasobu Application Insights** | Lista rozwijana wypełnia wszystkich zasobów usługi Application Insights, które odnoszą się do wcześniej wybranej grupy zasobów.

5. Wybierz **alerty Konfiguruj usługę Application Insights**

6. Domyślne reguły alertów, wybierz **zapisać** > wprowadź opisową komentarzu > kliknij **OK**

## <a name="modify-alert-rules"></a>Modyfikuj reguły alertu

1. Aby zmodyfikować wstępnie zdefiniowane ustawienia alertu, kliknij pole **wielokropek...**  z prawej strony **reguły alertów.**

   (Istnieją reguły alertów out-of-box czterech: dostępność, żądań zakończonych niepowodzeniem, czas odpowiedzi serwera, wyjątki serwera.)

2. Kliknij symbol listy rozwijanej obok pola **dostępności.**

3. Modyfikowanie dostępności **próg** zgodnie z wymaganiami poziomu usługi.

   ![Modyfikuj Alert](.\media\app-insights-continuous-monitoring\003.png)

4. Wybierz **OK** > **zapisać** > wprowadź opisową komentarzu > kliknij **OK.**

## <a name="add-deployment-conditions"></a>Dodawanie warunków wdrożenia

1. Kliknij przycisk **potoku** > Wybierz **sprzed** lub **po wdrożeniu warunków** symbol w zależności od tego, etap, który wymaga brama ciągłego monitorowania.

   ![Warunki wstępne wdrożenia](.\media\app-insights-continuous-monitoring\004.png)

2. Ustaw **bramy** do **włączone** > **zatwierdzania bramy**> kliknij **Dodaj.**

3. Wybierz **Azure Monitor** (Ta opcja pozwala na alerty dostęp zarówno z monitora Azure i usługi Application Insights)

    ![Azure Monitor](.\media\app-insights-continuous-monitoring\005.png)

4. Wprowadź **limitu czasu bramy** wartość.

5. Wprowadź **interwał próbkowania.**

## <a name="deployment-gate-status-logs"></a>Dzienniki stan bramy wdrożenia

Po dodaniu bramy wdrożenia w usłudze Application Insights, którego rozmiar przekracza z wcześniej określoną wartość progową alertu chroni wdrożenia z wersji niechciane podwyższania poziomu. Po usunięciu alert automatycznie przejść wdrożenia.

Aby sprawdzić to zachowanie, wybierz **wersje** > wersji kliknij prawym przyciskiem myszy nazwę **Otwórz** > **dzienniki.**

![Dzienniki](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o VSTS kompilacji i wydania Wypróbuj te [Przewodniki Szybki Start.](https://docs.microsoft.com/vsts/build-release/)
