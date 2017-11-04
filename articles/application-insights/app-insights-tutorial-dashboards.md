---
title: "Tworzenie niestandardowych pulpitów nawigacyjnych w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Samouczek tworzenia niestandardowych KPI pulpitów nawigacyjnych przy użyciu usługi Azure Application Insights."
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0d2f98ca2fb39289b2916ddd24590924856507d6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Tworzyć niestandardowe pulpity nawigacyjne kluczowego wskaźnika wydajności, za pomocą usługi Azure Application Insights

Można utworzyć wiele pulpitów nawigacyjnych w portalu Azure który każdego Kafelki include wizualizacji danych z wielu zasobów platformy Azure w różnych grupach zasobów i subskrypcje.  Można przypiąć różnych wykresów i widoków z Azure Application Insights, aby tworzyć niestandardowe pulpity nawigacyjne, które dostarczają pełny obraz kondycji i wydajności aplikacji.  Ten samouczek przedstawia tworzenie niestandardowy pulpit nawigacyjny, który zawiera wiele typów danych i wizualizacji z usługi Azure Application Insights.  Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz niestandardowy pulpit nawigacyjny w systemie Azure
> * Dodaj kafelka z galerii kafelka
> * Dodawanie standardowych metryki w usłudze Application Insights do pulpitu nawigacyjnego 
> * Dodaj wykres metryki niestandardowe usługi Application Insights do pulpitu nawigacyjnego
> * Dodaj wyników kwerendy Analytics do pulpitu nawigacyjnego 



## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Wdrażanie aplikacji .NET na platformie Azure i [włączyć zestaw SDK usługi Application Insights](app-insights-asp-net.md). 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-new-dashboard"></a>Utwórz nowy pulpit nawigacyjny
Pojedynczy pulpit nawigacyjny może zawierać zasobów z wielu aplikacji, subskrypcji i grupy zasobów.  Uruchom samouczek tworzenia nowego pulpitu nawigacyjnego aplikacji.  

2.  Na stronie głównej portalu, wybierz **nowego pulpitu nawigacyjnego**.

    ![Nowy pulpit nawigacyjny](media/app-insights-tutorial-dashboards/new-dashboard.png)

3. Wpisz nazwę pulpitu nawigacyjnego.
4. Obejrzyj **galerii kafelka** dla różnych Kafelki, które można dodać do pulpitu nawigacyjnego.  Oprócz dodania Kafelki z galerii możesz przypinać wykresy i innych widoków bezpośrednio z usługi Application Insights do pulpitu nawigacyjnego.
5. Zlokalizuj **Markdown** Kafelek, a następnie przeciągnij go do pulpitu nawigacyjnego.  Ten Kafelek pozwala na dodawanie tekstu sformatowanego w języku znaczników markdown, który jest idealne rozwiązanie w przypadku dodawania tekst opisowy do pulpitu nawigacyjnego.
6. Dodawanie tekstu na kafelku właściwości i jej rozmiar na kanwie pulpitu nawigacyjnego.
    
    ![Edytowanie kafelka języka znaczników markdown](media/app-insights-tutorial-dashboards/edit-markdown.png)

6. Kliknij przycisk **gotowe dostosowywanie** w górnej części ekranu, aby wyjść z trybu dostosowania kafelka, a następnie **Opublikuj zmiany** Aby zapisać zmiany.

    ![Pulpit nawigacyjny z kafelka języka znaczników markdown](media/app-insights-tutorial-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Dodaj informacje o kondycji
Pulpit nawigacyjny z właśnie statyczny tekst nie jest bardzo interesujące, więc teraz dodać kafelka z usługi Application Insights, aby wyświetlić informacje na temat aplikacji.  Można dodać usługi Application Insights Kafelki z galerii kafelka, lub Przypnij je bezpośrednio z usługi Application Insights ekranów.  Dzięki temu można skonfigurować wykresów i widoki, które znasz już przed przypięciem je do pulpitu nawigacyjnego.  Rozpocznij od dodania Przegląd kondycji standardowych aplikacji.  Nie wymaga konfiguracji i umożliwia dostosowanie minimalnego na pulpicie nawigacyjnym.


1. Wybierz **usługi Application Insights** w Azure menu, a następnie wybierz aplikację.
2. W **osi czasu omówienie**, wybierz menu kontekstowe i kliknij przycisk **Przypnij do pulpitu nawigacyjnego**.  Spowoduje to dodanie fragmentu ostatniego pulpit nawigacyjny, który była wyświetlana.  

    ![Oś czasu Przegląd kodu PIN](media/app-insights-tutorial-dashboards/pin-overview-timeline.png)
 
3. W górnej części ekranu kliknij **widoku pulpitu nawigacyjnego** aby powrócić do pulpitu nawigacyjnego.
4. Oś czasu omówienie zostało teraz dodane do pulpitu nawigacyjnego.  Kliknij i przeciągnij go do określonej pozycji, a następnie kliknij **gotowe dostosowywanie** i **Opublikuj zmiany**.  Pulpit nawigacyjny zawiera teraz kafelka niektóre przydatne informacje.

    ![Pulpit nawigacyjny z omówienie osi czasu](media/app-insights-tutorial-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Dodawanie niestandardowych metryk wykresu
**Metryki** panel umożliwia wykres metrykę zbieranych przez usługi Application Insights w czasie opcjonalne filtry i grupowania.  Podobnie jak wszystkie inne w usłudze Application Insights można dodać ten wykres do pulpitu nawigacyjnego.  Wymagane jest najpierw wykonać nieco dostosowania.

1. Wybierz **usługi Application Insights** w Azure menu, a następnie wybierz aplikację.
1. Wybierz **metryki**.  
2. Pusty wykresu już istnieje i zostanie wyświetlony monit, aby dodać metrykę.  Dodaj wskaźnik do wykresu, a następnie opcjonalnie Dodaj filtr i grupowania.  W poniższym przykładzie pokazano to liczba żądań serwera pogrupowane według Powodzenie.  Dzięki temu uruchomionych widoku żądania zakończone powodzeniem i niepowodzeniem.

    ![Dodaj wskaźnik](media/app-insights-tutorial-dashboards/metrics-chart.png)

4. Wybierz menu kontekstowe wykresu i wybierz **Przypnij do pulpitu nawigacyjnego**.  Spowoduje to dodanie widoku ostatniego pulpit nawigacyjny, który pracy z.

    ![Wykres metryki kodu PIN](media/app-insights-tutorial-dashboards/pin-metrics-chart.png)

3. W górnej części ekranu kliknij **widoku pulpitu nawigacyjnego** aby powrócić do pulpitu nawigacyjnego.

4. Oś czasu omówienie zostało teraz dodane do pulpitu nawigacyjnego.  Kliknij i przeciągnij go do określonej pozycji, a następnie kliknij **gotowe dostosowywanie** , a następnie **Opublikuj zmiany**. 

    ![pulpit nawigacyjny z metryk](media/app-insights-tutorial-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Eksploratora metryk
**Eksploratora metryk** jest podobny do metryki, mimo że umożliwia dostosowanie znacznie więcej po dodaniu do pulpitu nawigacyjnego.  Umożliwiają wykres Twoje metryki zależy od konkretnego preferencji i wymagań.

1. Wybierz **usługi Application Insights** w Azure menu, a następnie wybierz aplikację.
1. Wybierz **Eksploratora metryk**. 
2. Kliknij, aby edytować wykresu, a następnie wybierz co najmniej jedną metrykę i opcjonalnie szczegółową konfigurację.  W przykładzie przedstawiono wykres liniowy, śledzenie strony Średni czas odpowiedzi.
3. Kliknij ikonę pinezki na początku prawej strony, aby dodać wykres do pulpitu nawigacyjnego, a następnie przeciągnij go do określonej pozycji.

    ![Eksploratora metryk](media/app-insights-tutorial-dashboards/metrics-explorer.png)

4. Na kafelku Eksploratora metryk umożliwia dostosowanie więcej po dodaniu go do pulpitu nawigacyjnego.  Kliknij prawym przyciskiem myszy Kafelek i wybierz **edytować tytuł** Aby dodać tytuł niestandardowy.  Przejdź dalej i wprowadzić inne dostosowania, jeśli chcesz.

    ![Pulpit nawigacyjny z Eksploratora metryk](media/app-insights-tutorial-dashboards/dashboard-04a.png)

5. Istnieje już na wykresie Eksploratora metryk dodane do pulpitu nawigacyjnego.

    ![Pulpit nawigacyjny z Eksploratora metryk](media/app-insights-tutorial-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Dodawanie zapytania analityka
Rozwinięty język zapytań umożliwiający analizowanie wszystkie dane zebrane usługi Application Insights udostępnia analizy Insights aplikacji Azure.  Podobnie jak wykresy i innych widoków wyniki kwerendy Analytics można dodać do pulpitu nawigacyjnego.   

Ponieważ analiza Insights aplikacji Azure jest usługą w oddzielnych, trzeba Udostępnij swój pulpit nawigacyjny, aby wykonać kwerendę Analytics.  Po udostępnieniu Azure pulpitu nawigacyjnego, można opublikować go jako zasobów platformy Azure, które można udostępnić ją innym użytkownikom i zasobów.  

1. W górnej części ekranu pulpitu nawigacyjnego, kliknij przycisk **udziału**.

    ![Publikowanie pulpitu nawigacyjnego](media/app-insights-tutorial-dashboards/publish-dashboard.png)

2. Zachowaj **nazwa pulpitu nawigacyjnego** tych samych i wybierz **Nazwa subskrypcji** udostępnianie pulpitu nawigacyjnego.  Kliknij przycisk **Opublikuj**.  Pulpit nawigacyjny jest teraz dostępna do innych usług i subskrypcji.  Można opcjonalnie zdefiniować określonych użytkowników, którzy powinni mieć dostęp do pulpitu nawigacyjnego.
1. Wybierz **usługi Application Insights** w Azure menu, a następnie wybierz aplikację.
2. Kliknij przycisk **Analytics** w górnej części ekranu, aby otworzyć portal analityka.

    ![Uruchom analityka](media/app-insights-tutorial-dashboards/start-analytics.png)

3. Wpisz następujące zapytanie zwraca górny 10 najbardziej pożądanych stron i ich liczbę żądań:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Kliknij przycisk **Przejdź** Aby zweryfikować wyniki zapytania.
5. Kliknij ikonę pinezki i wybierz nazwę pulpitu nawigacyjnego.  Ta opcja ma wyboru pulpit nawigacyjny w przeciwieństwie do poprzednich kroków, których użyto ostatniego pulpitu nawigacyjnego przyczyną jest to ponieważ konsola Analytics jest usługą oddzielne i wymaga dokonać wyboru spośród wszystkich dostępnych udostępnionych pulpitów nawigacyjnych.

    ![Zapytanie analizy kodu PIN](media/app-insights-tutorial-dashboards/analytics-pin.png)

5. Przed możesz powrócić do pulpitu nawigacyjnego, należy dodać inne zapytanie, ale teraz renderować ją jako wykres, zobacz różne sposoby wizualizacji analizy zapytania na pulpicie nawigacyjnym.  Rozpoczynać się od następującej kwerendy sumującą top operacji 10 większość wyjątków.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Wybierz **wykresu** , a następnie zmień **pierścieniowego** do wizualizacji danych wyjściowych.

    ![Wykres analizy](media/app-insights-tutorial-dashboards/analytics-chart.png)

6. Kliknij ikonę numeru pin, aby przypiąć wykres do pulpitu nawigacyjnego i tym razem wybierz łącze, aby powrócić do pulpitu nawigacyjnego.
4. Wyniki zapytania są teraz dodawane do pulpitu nawigacyjnego w wybranym formacie.  Kliknij i przeciągnij każdą w określonej pozycji, a następnie kliknij **zakończeniu edytowania**.
5. Kliknij prawym przyciskiem myszy każdy z kafelkami i wybierz **edytować tytuł** nadanie im opisowy tytuł.

    ![Pulpit nawigacyjny z Analytics](media/app-insights-tutorial-dashboards/dashboard-05.png)

5. Kliknij przycisk **Opublikuj zmiany** można przekazać zmian do pulpitu nawigacyjnego, który zawiera teraz różnych wykresów i wizualizacji z usługi Application Insights.


## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz, jak tworzyć niestandardowe pulpity nawigacyjne, mają wygląd w pozostałej części dokumentacji usługi Application Insights, w tym analizę przypadku.

> [!div class="nextstepaction"]
> [Głębokie diagnostyki](app-insights-devops.md)