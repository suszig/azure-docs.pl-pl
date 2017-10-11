---
title: "Lejki wgląd w aplikacji Azure"
description: "Dowiedz się, jak można użyć Lejki, aby dowiedzieć się, jak klienci są interakcji z aplikacją."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 59c4dfafc102b26e3b9873f433065715f4aec9ec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Wykryj, jak klienci używają aplikacji z rozdzielaczy Application Insights

Opis klientów są największe znaczenie dla Twojej firmy. Jeśli aplikacja obejmuje kilka etapów, musisz wiedzieć, jeśli większość klientów postępu są przez cały proces lub są one zakończenia procesu w pewnym momencie. Przejście przez kilka czynności w aplikacji sieci web nazywa się "lejka". Rozdzielaczy Application Insights umożliwia uzyskać wgląd w użytkowników i monitor kursy wymiany krok po kroku. 

## <a name="get-started-with-the-funnels-blade"></a>Rozpoczynanie pracy z bloku Lejki
Najprostszym sposobem, aby dowiedzieć się więcej o Lejki jest przeprowadzenie jednak przykładem. Poniższe ilustracje wykazanie, że kroki właściciele firm handlu elektronicznego zajmie się sposób interakcji klientów z aplikacją sieci web.  

### <a name="create-your-funnel"></a>Utwórz użytkownika lejka.
Przed utworzeniem sieci lejka należy zdecydować się na pytanie, które chcesz odpowiedzieć. Można na przykład wiedzieć, ilu użytkowników Wyświetlanie przez stronę główną, kliknij na anonsu. W tym przykładzie właściciele firmy Fabrikam Fiber zapoznać się odsetek klientów, którzy tworzą zakupu po dodaniu elementów do ich koszyk w ciągu ostatniego miesiąca.

Poniżej przedstawiono kroki, które podejmują tworzenia ich lejka.

1. Kliknij przycisk Nowy blok lejki.
1. Wybierz przedział czasu "Ostatni miesiąc" z **zakres czasu** listy rozwijanej. 
1. Wybierz **stronę produktu** zdarzenia z **krok 1** listy rozwijanej. 
1. Wybierz **Dodaj do koszyka** zdarzenia z **krok 2** listy rozwijanej.
1. Wybierz **kliknij zakupu** zdarzenia z **kroku 3** listy rozwijanej.
1. Dodaj nazwę do lejka i kliknij przycisk **zapisać**.

Poniższa ilustracja przedstawia się, że generuje Lejki bloku danych. W tym miejscu Fabrikam właścicieli widoczny w zeszłym tygodniu 22.7% dla klientów, którzy dodać element do ich koszyk ukończone zakupu. Można również sprawdzić 1% klientów kliknięty anonsu, aby odwiedzić stronę produktu i 20% klientów wylogowany po zakończeniu ich zakupu.


![Blok Lejki z danymi](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Następne kroki
  * [Przegląd wykorzystania](app-insights-usage-overview.md)
  * [Użytkownikami, sesjami i zdarzenia](app-insights-usage-segmentation.md)
  * [Przechowywanie](app-insights-usage-retention.md)
  * [Skoroszyty](app-insights-usage-workbooks.md)
  * [Dodaj kontekstu użytkownika](app-insights-usage-send-user-context.md)
