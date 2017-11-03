---
title: "Przejrzyj użycia i kosztów zarządzania koszt Azure | Dokumentacja firmy Microsoft"
description: "Przejrzyj użycia i kosztów w celu śledzenia trendów, wykrywania wydajność i tworzyć alerty."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 36ebffb41211e443cc1619df46f50247945cc57c
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="review-usage-and-costs"></a>Przejrzyj użycia i kosztów

Azure kosztów zarządzania Cloudyn pokazuje użycie i kosztów, dzięki czemu można śledzić trendy, wykrywanie wydajność i tworzyć alerty. Wszystkie dane użycia i koszty są wyświetlane w Cloudyn pulpity nawigacyjne i raporty. Przykłady w tym samouczku opisano, chociaż użycia i kosztów za pomocą pulpitów nawigacyjnych i raportów. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Śledź trendy użycia i kosztów
> * Wykryj wydajność użycia
> * Tworzenie alertów dotyczących nietypowych wydatków lub przekroczenia wydatków



## <a name="open-the-cloudyn-portal"></a>Otwórz Cloudyn portal

Należy przejrzeć wszystkie użycia i kosztów w portalu Cloudyn. Otwórz Cloudyn portal z portalu Azure lub przejdź do https://app.cloudyn.com i logowania.

## <a name="track-usage-and-cost-trends"></a>Śledź trendy użycia i kosztów

Możesz śledzić rzeczywiste pieniędzy przeznaczony do użycia i kosztów raporty w miarę upływu czasu, aby zidentyfikować trendy. Aby rozpocząć wyszukiwanie informacji trendy, raport rzeczywisty koszt w miarę upływu czasu. W menu Raporty w górnej części portalu kliknij **koszt** > **analizy kosztów** > **rzeczywisty koszt w miarę upływu czasu**. Przy pierwszym otwarciu raportu, żadnym grupom ani filtry są stosowane do niego.

Poniżej przedstawiono przykładowy raport:

![przykładowy raport](./media/tutorial-review-usage/actual-cost01.png)

Raport zawiera wszystkie wydatków w ciągu ostatnich 30 dni. Aby wyświetlić tylko wydatków dla usług Azure, zastosowanie grupy usług, a następnie przeprowadź filtrowanie dla wszystkich usług platformy Azure. Na poniższej ilustracji przedstawiono usług filtrowane.

![filtrowane usług](./media/tutorial-review-usage/actual-cost02.png)

W powyższym przykładzie niższą cenę był poświęcony na uruchamianie na 2017-08-31 niż przed. Trend ten koszt składa się z różnych usług o dziewięć dni. Następnie dodatkowe wydatków kontynuuje jak poprzednio. Jednak za dużo kolumn mogą przesłaniać trend oczywiste. Do wiersza lub obszaru wykresu, aby wyświetlić dane zawarte w innych widokach, można zmienić widoku raportu. Na poniższej ilustracji przedstawiono sposób trendu.

![Trend w raporcie](./media/tutorial-review-usage/actual-cost03.png)

W tym przykładzie wyraźnie pojawić czy Magazyn Azure koszt porzuconych uruchamianie na 2017-08-31, podczas gdy wydatków związanych z innymi usługami Azure pozostała poziom. Tak co spowodowało redukcja w wydatków? W tym przykładzie niektórzy pracownicy na urlopie od pracy i nie używa usługi magazynu.

Aby obejrzeć film samouczka śledzenie trendów użycia i kosztów informacje, zobacz [analizowanie chmury rozliczeń danych, a czas z usługą Azure Management koszt przez Cloudyn](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Wykryj wydajność użycia

Raporty Optymalizator zwiększyć wydajność, optymalizować wykorzystanie i identyfikację metod zapisywania wydatków na zasobów w chmurze. Są one szczególnie przydatne wraz z zaleceniami ekonomicznego rozmiaru mają pomóc w zmniejszeniu bezczynności lub kosztowne maszyn wirtualnych.

To powszechny problem, która wpływa na organizacje łączącymi się początkowo zasobów chmurze jest ich strategii wirtualizacji. Często używane podejście podobny do używanego do tworzenia maszyn wirtualnych w środowisku wirtualizacji lokalnymi. I założono, że koszty zostały zredukowane przez przeniesienie ich lokalnych maszyn wirtualnych w chmurze jako — jest. Takie podejście nie jest jednak może zmniejszyć koszty.

Problem polega na tym, że istniejącej infrastruktury została już uregulowaniu płatności. Użytkownicy mogą tworzyć i zachować dużą maszyny wirtualne uruchomione, jeśli są zbędne — bezczynności lub nie, jak i z niewielkim konsekwencji. Przenoszenie dużych lub bezczynnych maszyn wirtualnych do chmury prawdopodobnie *zwiększyć* kosztów. Po wprowadzeniu umów z dostawcy usług w chmurze, ważne jest koszt alokacji zasobów. Należy opłacać zatwierdzić czy używać zasób całości lub nie.

Raport koszt wprowadzenia zmiany rozmiaru zalecenia identyfikuje potencjalne oszczędności roczne porównując pojemności typu wystąpienia maszyny Wirtualnej do ich historycznych procesora CPU i dane użycia pamięci.  

W menu Raporty w górnej części portalu kliknij **Optymalizator** > **cennik optymalizacji** > **zalecenia dotyczące rozmiaru obowiązującej koszt**. Filtr dostawcy na platformie Azure, aby przyjrzeć się tylko maszyn wirtualnych platformy Azure. Oto przykład obrazu.

![Maszyny wirtualne platformy Azure](./media/tutorial-review-usage/sizing01.png)

W tym przykładzie $3,114 może być zapisany, wykonując zalecenia, aby zmienić typy wystąpienia maszyny Wirtualnej. Kliknij znak plus (+) w obszarze **szczegóły** dla pierwszego zalecenia. Poniżej przedstawiono szczegółowe informacje dotyczące pierwszego zalecenia.

![Szczegóły zalecenia](./media/tutorial-review-usage/sizing02.png)

Wyświetl identyfikatory wystąpienie maszyny Wirtualnej, klikając znak plus obok **listy kandydatów**.

![Listy kandydatów](./media/tutorial-review-usage/sizing03.png)

Aby obejrzeć film samouczka o wykrywaniu wydajność użycia, zobacz [optymalizacji rozmiaru maszyny Wirtualnej Azure kosztów zarządzania przez Cloudyn](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Tworzenie alertów dotyczących nietypowych wydatków

Może generować alerty uczestników automatycznie dla anomalii wydatków i ryzyka przekroczenie wydatków. Możesz szybko i łatwo utworzyć za pomocą raportów, które Obsługa alerty oparte na budżet oraz koszt progi alertów.

Możesz tworzyć alert dla dowolnej wydatków żadnych raportów koszt za pomocą. W tym przykładzie wybierz raport rzeczywisty koszt w miarę upływu czasu i powiadamianie wydatków maszyny Wirtualnej platformy Azure będzie bliski całkowita budżetu. W menu Raporty w górnej części portalu kliknij **koszt** > **analizy kosztów** > **rzeczywisty koszt w miarę upływu czasu**. Ustaw **grup** do **usługi** i ustaw **filtru w usłudze** do **maszyny Wirtualnej i Azure**. W górnym rogu raportu, kliknij przycisk **akcje** , a następnie wybierz **zaplanować raport**.

Użyj **Planowanie** kartę, aby wysłać do siebie wiadomość e-mail przy użyciu częstotliwości, który ma raportu. Wszystkie tagi, grupowanie i filtrowanie użyte znajdują się w pocztę e-mail raport. Kliknij przycisk **próg** wybierz kartę i wybierz **rzeczywisty koszt vs. Próg**. Jeśli znajdował się całkowity budżet $ 500 000 i chcesz, aby powiadomienia, gdy koszty pobliżu w połowie, Utwórz **czerwonym kolorem** na 250 000 $ i **żółta ikona alertu** na 240,000 $. Następnie wybierz liczbę kolejnych alertów. Po wyświetleniu łączną liczbę alertów, które można określić dodatkowe alerty są wysyłane. Zapisz raport według harmonogramu.

![przykładowy raport](./media/tutorial-review-usage/schedule-alert01.png)

Możesz również vs procent kosztów. Metryka próg budżetu tworzyć alerty. Za pomocą tej metryki, można użyć wartości procentowych budget zamiast wartości waluty.


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Śledź trendy użycia i kosztów
> * Wykryj wydajność użycia
> * Tworzenie alertów dotyczących nietypowych wydatków lub przekroczenia wydatków


Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat kontrolowania dostępu do danych.

> [!div class="nextstepaction"]
> [Kontrola dostępu do danych](tutorial-user-access.md)
