---
title: "Lejki wgląd w aplikacji Azure"
description: "Dowiedz się, jak można użyć Lejki, aby dowiedzieć się, jak klienci są interakcji z aplikacją."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: mbullwin
ms.openlocfilehash: 0396c59d9d95ab71f0af04029d87afbb6e47dc35
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Wykryj, jak klienci używają aplikacji z rozdzielaczy Application Insights

Opis klientów są największe znaczenie dla Twojej firmy. Jeśli aplikacja obejmuje kilka etapów, musisz wiedzieć, jeśli większość klientów postępu są przez cały proces lub są one zakończenia procesu w pewnym momencie. Przejście przez kilka czynności w aplikacji sieci web nazywa się "lejka". Rozdzielaczy Application Insights umożliwia uzyskać wgląd w użytkowników i monitor kursy wymiany krok po kroku. 

## <a name="create-your-funnel"></a>Utwórz użytkownika lejka.
Przed utworzeniem sieci lejka należy zdecydować się na pytanie, które chcesz odpowiedzieć. Na przykład można wiedzieć, ilu użytkowników jest wyświetlana strona główna profil klienta wyświetlania i tworzenia biletu. W tym przykładzie właściciele firmy Fabrikam Fiber zapoznać się odsetek klientów, którzy pomyślnie utworzyć bilet klienta.

Poniżej przedstawiono kroki, które podejmują tworzenia ich lejka.

1. Kliknij przycisk Nowy narzędzie lejki.
1. Wybierz przedział czasu "Ostatnich 90 dni" z **zakres czasu** listy rozwijanej. Wybierz opcję "Mój Lejki" lub "Udostępniania Lejki"
1. Wybierz **indeksu** zdarzenia z **krok 1** listy rozwijanej. 
1. Wybierz **klienta** zdarzenia z **krok 2** listy rozwijanej.
1. Wybierz **Utwórz** zdarzenia z **kroku 3** listy rozwijanej.
1. Dodaj nazwę do lejka i kliknij przycisk **zapisać**.

Poniższa ilustracja przedstawia się, że dane narzędzie Lejki generuje. W tym miejscu Fabrikam właścicieli znajduje się w ciągu ostatnich 90 dni, 54.3% dla klientów, którzy odwiedzona strona główna utworzenia biletu klienta. Można również sprawdzić czy pochodzi 2.7k klientów do indeksu strony głównej, może to wskazywać problem odświeżania.


![Narzędzie Lejki z danymi](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnel-features"></a>Funkcje lejka
1. Jeśli jest próbkowany aplikacji, zostanie wyświetlony Baner próbkowania. Kliknięcie na banerze spowoduje otwarcie okienka kontekstu poinstruowanie jak wyłączyć próbkowania. 
2. Możesz wyeksportować z lejka do [usługi Power BI](app-insights-export-power-bi.md).
3. Kliknij krok, aby uzyskać bardziej szczegółowe po prawej stronie. 
4. Konwersja historycznych pokazuje konwersji w ciągu ostatnich 90 dni. 
5. Zrozumienie użytkownikom lepsze, przechodząc do narzędzia do użytkowników z lejki. Każdy krok zapewni możesz wyselekcjonowanych filtry użytkowników. 

## <a name="next-steps"></a>Następne kroki
  * [Przegląd wykorzystania](app-insights-usage-overview.md)
  * [Użytkownikami, sesjami i zdarzenia](app-insights-usage-segmentation.md)
  * [Przechowywanie](app-insights-usage-retention.md)
  * [Skoroszyty](app-insights-usage-workbooks.md)
  * [Dodaj kontekstu użytkownika](app-insights-usage-send-user-context.md)
  * [Eksportowanie do usługi Power BI](app-insights-export-power-bi.md)

