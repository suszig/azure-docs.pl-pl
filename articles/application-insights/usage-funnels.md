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
ms.openlocfilehash: bbb25af888f34737f6a61cf43890ff248c4cc4de
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Wykryj, jak klienci używają aplikacji z Lejki Insights aplikacji

Zrozumienie obsługi klienta jest największe znaczenie dla Twojej firmy. Jeśli aplikacja obejmuje kilka etapów, musisz wiedzieć, jeśli większość klientów postępu są przez cały proces lub są one zakończenia procesu w pewnym momencie. Przejście przez kilka czynności w aplikacji sieci web nazywa się *lejka*. Użyj Azure Application Insights Lejki, aby uzyskać wgląd w użytkowników i monitorowanie kursy wymiany krok po kroku. 

## <a name="create-your-funnel"></a>Utwórz użytkownika lejka.
Przed utworzeniem sieci lejka zdecydować się na pytanie, które chcesz odpowiedzieć. Na przykład można wiedzieć, ilu użytkowników jest wyświetlana strona główna profil klienta wyświetlania i tworzenia biletu. W tym przykładzie właściciele firmy Fabrikam Fiber zapoznać się odsetek klientów, którzy pomyślnie utworzyć bilet klienta.

Poniżej przedstawiono kroki, które podejmują tworzenia ich lejka.

1. W narzędziu Application Insights Lejki wybierz **nowy**.
1. Z **zakres czasu** menu rozwijanego wybierz **ostatnich 90 dni temu**. Wybierz opcję **Moje Lejki** lub **udostępnionych Lejki**.
1. Z **krok 1** listy rozwijanej wybierz **indeksu**. 
1. Z **krok 2** listy, wybierz **klienta**.
1. Z **kroku 3** listy, wybierz **Utwórz**.
1. Dodaj nazwę do lejka, a następnie wybierz **zapisać**.

Poniższy zrzut ekranu przedstawia przykład tego rodzaju danych narzędzie Lejki generuje. Właściciele Fabrikam można stwierdzić, że w ciągu ostatnich 90 dni, 54.3 odsetek klientów, którzy odwiedzona strona główna utworzone biletu klienta. Można również sprawdzić 2,700 klientom pochodzi ze strony głównej do indeksu. Może to wskazywać problem odświeżania.


![Zrzut ekranu Lejki narzędzie z danymi](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnels-features"></a>Funkcje Lejki
Poprzedni zrzut ekranu zawiera pięciu obszarów. Są to funkcje lejki. Poniżej opisano bardziej dotyczące każdego odpowiadający mu obszar na zrzucie ekranu:
1. Jeśli jest próbkowany aplikacji, zostanie wyświetlony Baner próbkowania. Wybieranie transparentu powoduje otwarcie okienka kontekstu, wyjaśniający, jak wyłączyć próbkowania. 
2. Możesz wyeksportować z lejka do [usługi Power BI](app-insights-export-power-bi.md).
3. Zaznacz krok, aby zobaczyć więcej szczegółów po prawej stronie. 
4. Wykres historyczny konwersji pokazuje kursy wymiany w ciągu ostatnich 90 dni. 
5. Dowiedz się użytkownikom lepsze, uzyskując dostęp do narzędzia użytkowników. Możesz użyć filtrów w każdym kroku. 

## <a name="next-steps"></a>Następne kroki
  * [Przegląd wykorzystania](app-insights-usage-overview.md)
  * [Użytkownikami, sesjami i zdarzenia](app-insights-usage-segmentation.md)
  * [Przechowywanie](app-insights-usage-retention.md)
  * [Skoroszyty](app-insights-usage-workbooks.md)
  * [Dodaj kontekstu użytkownika](app-insights-usage-send-user-context.md)
  * [Eksportowanie do usługi Power BI](app-insights-export-power-bi.md)

