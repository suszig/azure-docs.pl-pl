---
title: "Eksportowanie do usługi Power BI z usługi Application Insights | Dokumentacja firmy Microsoft"
description: "Zapytania analityczne mogą być wyświetlane w usłudze Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: mbullwin
ms.openlocfilehash: fe708b14fac971d18d95fd1619907023ec35af89
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Źródła danych usługi Power BI z usługi Application Insights
[Power BI](http://www.powerbi.com/) jest pakiet narzędzia do analizy biznesowe, które ułatwiają analizowanie danych i udostępniać informacji. Pulpity nawigacyjne sformatowanego są dostępne na każdym urządzeniu. Można połączyć dane z wielu źródeł, takich jak analiza zapytania z [Azure Application Insights](app-insights-overview.md).

Istnieją trzy metody zalecane eksportowania danych usługi Application Insights do usługi Power BI. Można ich używać oddzielnie lub razem.

* [**Power BI karty** ](#power-pi-adapter) — skonfiguruj Tworzenie pulpitu nawigacyjnego dane telemetryczne z aplikacji. Wstępnie zdefiniowane zbiór wykresy, ale można dodać własne zapytania z innych źródeł.
* [**Eksportuj zapytania analityczne** ](#export-analytics-queries) -pisać zapytania przy użyciu analizy i eksportowania ich do usługi Power BI. To zapytanie można umieścić na pulpicie nawigacyjnym, wraz z innymi danymi.
* [**Eksport ciągły i analiza strumienia** ](app-insights-export-stream-analytics.md) — proces ten obejmuje więcej pracy, aby skonfigurować. Jest przydatne, jeśli chcesz zachować dane przez dłuższy czas. W przeciwnym razie inne metody są zalecane.

## <a name="power-bi-adapter"></a>Power BI karty
Ta metoda tworzy pełne pulpitu nawigacyjnego dane telemetryczne dla Ciebie. Wstępnie zdefiniowane początkowego zestawu danych, ale można dodać więcej danych do niego.

### <a name="get-the-adapter"></a>Pobierz karty
1. Zaloguj się do [Power BI](https://app.powerbi.com/).
2. Otwórz **Pobierz dane**, **usług**, **usługi Application Insights**
   
    ![Pobierz ze źródła danych usługi Application Insights](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Podaj szczegóły zasobu usługi Application Insights.
   
    ![Pobierz ze źródła danych usługi Application Insights](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Zaczekaj minutę lub dwie na dane do zaimportowania.
   
    ![Power BI karty](./media/app-insights-export-power-bi/010.png)

Pulpit nawigacyjny, można edytować połączenie wykresów usługi Application Insights z systemami innych źródeł oraz z zapytaniami Analytics. Brak galerii wizualizacji, gdzie uzyskasz więcej wykresów, a każdy wykres ma parametry, które można ustawić.

Po zaimportowaniu początkowej pulpit nawigacyjny i raporty nadal aktualizowane codziennie. Można kontrolować harmonogram odświeżania w zestawie danych.

## <a name="export-analytics-queries"></a>Zapytania analityczne eksportu
Tej trasy pozwala na każde zapytanie Analytics, którą chcesz zapisać, a następnie wyeksportować który do pulpitu nawigacyjnego usługi Power BI. (Możesz dodać do pulpitu nawigacyjnego utworzone przez adapter.)

### <a name="one-time-install-power-bi-desktop"></a>Jeden raz: Zainstaluj Power BI Desktop
Aby zaimportować zapytanie usługi Application Insights, należy użyć tej wersji usługi Power BI. Ale następnie można opublikować go w sieci Web lub do swojego obszaru roboczego chmury usługi Power BI. 

Zainstaluj [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Eksportuj zapytania analityka
1. [Otwórz Analytics i zapisać zapytanie](app-insights-analytics-tour.md).
2. Testowanie i uściślenie kwerendy, dopóki zakończeniu modyfikowania wyniki.

   **Upewnij się, że zapytanie działa poprawnie w module analiz przed wyeksportowaniem.**
3. Na **wyeksportować** menu, wybierz **Power BI (M)**. Zapisz plik tekstowy.
   
    ![Eksportuj zapytania usługi Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Wybierz Power BI Desktop w **pobieranie danych, pustą zapytania** , a następnie w edytorze zapytań w obszarze **widoku** wybierz **zaawansowany Edytor zapytań**.

    Wklej wyeksportowanego skryptu języka M do zaawansowany Edytor zapytań.

    ![Edytor zaawansowanych zapytań](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Należy podać poświadczenia, aby umożliwić usłudze Power BI na dostęp do platformy Azure. Używanie "konta organizacyjnego" się zalogować za pomocą konta Microsoft.
   
    ![Podaj poświadczenia platformy Azure, aby zezwolić usłudze Power BI do uruchamiania kwerendy usługi Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Jeśli musisz zweryfikować poświadczenia, użyj polecenia menu Ustawienia źródła danych w edytorze zapytań. Należy określić poświadczenia, używanego dla platformy Azure, która może być inny niż poświadczenia dla usługi Power BI.)
2. Wybierz wizualizacji zapytania i wybierz pola dla osi x, y i podzielenie wymiaru.
   
    ![Wybierz wizualizację](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Publikowanie raportu do obszaru roboczego chmury usługi Power BI. Z tego miejsca można osadzić zsynchronizowanej wersji do innych stron sieci web.
   
    ![Wybierz wizualizację](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Ręcznie odświeżyć raport w odstępach czasu, lub skonfigurowania zaplanowanego odświeżania na stronie opcji.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="401-or-403-unauthorized"></a>401 lub 403 bez autoryzacji 
Może to nastąpić, jeśli refesh token nie został zaktualizowany. Spróbuj wykonać następujące kroki, aby upewnić się, że nadal mieć dostęp. Jeśli masz dostęp i refershing poświadczenia nie działa, otwórz bilet pomocy technicznej.

1. Zaloguj się do portalu Azure i upewnij się, że ma dostęp do zasobu
2. Spróbuj odświeżyć poświadczenia dla pulpitu nawigacyjnego

### <a name="502-bad-gateway"></a>502 Niewłaściwa brama
Jest to zazwyczaj spowodowane Analytics kwerendę, która zwraca za dużo danych. Spróbuj użyć przy użyciu mniejszego zakresu czasu lub za pomocą [temu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) lub [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) działa tylko [projektu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) potrzebne pola.

Jeśli zmniejszenie zestawu danych przesyłanych przez zapytanie Analytics nie spełnia wymagań należy rozważyć użycie [interfejsu API](https://dev.applicationinsights.io/documentation/overview) ściągania większy zestaw danych. Poniżej przedstawiono instrukcje dotyczące sposobu przekonwertować eksportu M zapytania do za pomocą interfejsu API.

1. Utwórz [klucz interfejsu API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Zaktualizuj skrypt Power BI M wyeksportowany z usługi Analytics zamieniając adres URL ARM AI interfejsu API (zobacz poniższy przykład)
   * Zastąp **https://management.azure.com/subscriptions/...**
   * **https://api.applicationinsights.io/beta/apps/...**
3. Na koniec Zaktualizuj poświadczenia do warstwy podstawowa i korzystania z klucza interfejsu API
  

**Istniejący skrypt**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Zaktualizowany skrypt**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>O pobierania próbek
Jeśli aplikacja wyśle dużą ilość danych, funkcję adaptacyjną próbkowanie może działać i wysłać określonego odsetka telemetrii. Jest taka sama wartość true, jeśli ręcznie ustawiono próbkowania w zestawie SDK lub na wprowadzanie. [Dowiedz się więcej na temat próbkowania.](app-insights-sampling.md)


## <a name="next-steps"></a>Następne kroki
* [Power BI — informacje](http://www.powerbi.com/learning/)
* [Samouczek analityka](app-insights-analytics-tour.md)

