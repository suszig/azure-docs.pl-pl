---
title: "Zautomatyzować procesy Azure Application Insights przy użyciu Logic Apps."
description: "Dowiedz się, jak szybko można zautomatyzować powtarzalnych procesów przez dodanie łącznika usługi Application Insights do aplikacji logiki."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 36df5bc0a019f4197d40fd6fa5a2a9957820c8b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Zautomatyzować procesy usługi Application Insights przy użyciu aplikacji logiki

Czy zaczniesz wielokrotnego uruchamiania tej samej zapytań na podstawie danych telemetrii, aby sprawdzić, czy usługa działa prawidłowo? Szukasz zautomatyzować te zapytania do znajdowania trendów i anomalii i późniejszego kompilowania własne przepływy pracy wokół nich? Łącznik usługi Azure Application Insights (wersja zapoznawcza) dla usługi Logic Apps jest właściwych narzędzi, w tym celu.

Dzięki tej integracji można zautomatyzować wiele procesów bez pisania pojedynczy wiersz kodu. Można utworzyć aplikację logiki z łącznikiem usługi Application Insights, aby szybko zautomatyzować każdy proces usługi Application Insights. 

Można dodać również dodatkowe akcje. Funkcję Logic Apps w usłudze Azure App Service udostępnia setki akcje. Na przykład za pomocą aplikacji logiki, możesz można automatycznie wysłać wiadomość e-mail z powiadomieniem lub tworzenie usterki w programie Visual Studio Team Services. Można także użyć jednego z wielu dostępne [szablony](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) aby przyspieszyć proces tworzenia aplikacji logiki. 

## <a name="create-a-logic-app-for-application-insights"></a>Tworzenie aplikacji logiki do usługi Application Insights

W tym samouczku Dowiedz się jak utworzyć aplikację logiki, która używa algorytmu autocluster Analytics grupy atrybutów danych dla aplikacji sieci web. Przepływ automatycznie wysyła wyniki za pośrednictwem poczty e-mail, tylko jeden przykład sposobu użycia Application Insights analizy i Logic Apps razem. 

### <a name="step-1-create-a-logic-app"></a>Krok 1: Tworzenie aplikacji logiki
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W **nowy** okienku wybierz **sieci Web i mobilność**, a następnie wybierz **aplikacji logiki**.

    ![Nowe okno aplikacji logiki](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Krok 2: Tworzenie aplikacji logiki wyzwalacza
1. W **projektanta aplikacji logiki** okna, w obszarze **rozpoczynać wyzwalacz wspólnej**, wybierz pozycję **cyklu**.

    ![Okna Projektant aplikacji logiki](./media/automate-with-logic-apps/logicapp2.png)

2. W **częstotliwość** wybierz opcję **dzień** , a następnie w **interwał** wpisz **1**.

    ![Projektant aplikacji logiki "Cyklu" okna](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Krok 3: Dodaj akcję usługi Application Insights
1. Kliknij przycisk **nowy krok**, a następnie kliknij przycisk **Dodaj akcję**.

2. W **wybierz akcję** pole wyszukiwania, wpisz **Azure Application Insights**.

3. W obszarze **akcje**, kliknij przycisk **Azure Application Insights — wizualizacji analizy zapytania Podgląd**.

    ![Okno "Wybierz akcję" Projektant aplikacji logiki](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Krok 4: Łączenie z zasobu usługi Application Insights

Aby ukończyć ten krok, należy identyfikator i klucz interfejsu API dla zasobu. Można je pobrać z portalu Azure, jak pokazano na poniższym diagramie:

![Identyfikator aplikacji w portalu Azure](./media/automate-with-logic-apps/appid.png) 

Podaj nazwę połączenia, identyfikator i klucz interfejsu API.

![Logika Projektant aplikacji przepływu połączenia okna](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Krok 5: Określ typ zapytania i wykres analizy
W poniższym przykładzie zapytanie wybiera żądań zakończonych niepowodzeniem w ciągu ostatniego dnia i są powiązane z nimi wyjątków, które wystąpiły w ramach operacji. Analiza skorelowany nieudanych żądań, na podstawie identyfikatora operation_Id. Zapytanie następnie segmentów wyników za pomocą algorytmu autocluster. 

Podczas tworzenia własnych zapytań, sprawdź, czy działają prawidłowo w module analiz przed dodaniem go do Twojego przepływu.

1. W **zapytania** Dodaj następujące zapytanie Analytics: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. W **typ wykresu** wybierz opcję **tabeli Html**.

    ![Okno konfiguracji kwerendy analityka](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Krok 6: Skonfigurowanie aplikacji logiki do wysyłania wiadomości e-mail

1. Kliknij przycisk **nowy krok**, a następnie wybierz **Dodaj akcję**.

2. W polu wyszukiwania wpisz **Office 365 Outlook**.

3. Kliknij przycisk **Office 365 Outlook — Wyślij usługi poczty e-mail**.

    ![Wybór Outlook pakietu Office 365](./media/automate-with-logic-apps/flow2b.png)

4. W **wysłać wiadomość e-mail** okna, wykonaj następujące czynności:

   a. Wpisz adres e-mail odbiorcy.

   b. Wpisz temat wiadomości e-mail.

   c. Kliknij w dowolnym miejscu **treści** polu, a następnie na dynamicznej zawartości wyświetlonym menu z prawej strony, wybierz **treści**.

   d. Kliknij przycisk **Pokaż zaawansowane opcje**.

      ![Konfiguracja programu Outlook pakietu Office 365](./media/automate-with-logic-apps/flow5.png)

5. W menu zawartości dynamicznej wykonaj następujące czynności:

    a. Wybierz **Nazwa załącznika**.

    b. Wybierz **zawartości załącznika**.
    
    c. W **HTML jest** wybierz opcję **tak**.

      ![Ekran konfiguracji poczty e-mail usługi Office 365](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Krok 7: Zapisz i przetestuj aplikację logiki
* Kliknij przycisk **zapisać** Aby zapisać zmiany.

Możesz poczekać trigger, aby uruchomić aplikację logiki, lub uruchom aplikację logiki natychmiast po wybraniu **Uruchom**.

![Ekran Tworzenie aplikacji logiki](./media/automate-with-logic-apps/step7.png)

Po uruchomieniu aplikacji logiki, odbiorcy, określone na liście wiadomości e-mail będą otrzymywać wiadomości e-mail, która wygląda następująco:

![Wiadomość e-mail aplikacji logiki](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o tworzeniu [zapytania analityczne](app-insights-analytics-using.md).
- Dowiedz się więcej o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





