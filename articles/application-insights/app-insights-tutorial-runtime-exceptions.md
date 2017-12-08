---
title: "Diagnozowanie wyjątków czasu wykonywania przy użyciu usługi Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Samouczek można znaleźć i diagnozowanie wyjątków środowiska wykonawczego w aplikacji przy użyciu usługi Azure Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 11e0f2f19acc843f1c558b5d0cfe84291035a6a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Znajdź i diagnozowanie wyjątków środowiska wykonawczego z usługą Azure Application Insights

Azure Application Insights zbiera dane telemetryczne z aplikacji do identyfikacji i diagnozowanie wyjątków w czasie wykonywania.  Ten samouczek przedstawia ten proces z aplikacją.  Omawiane kwestie:

> [!div class="checklist"]
> * Modyfikowanie projektu, aby włączyć śledzenie wyjątków
> * Rozpoznanie wyjątków dla różnych składników aplikacji
> * Wyświetl szczegóły wyjątku
> * Pobierz migawkę wyjątek dla programu Visual Studio do debugowania
> * Analizować szczegółowe informacje dotyczące żądań zakończonych niepowodzeniem przy użyciu języka zapytań
> * Utwórz nowy element roboczy, aby skorygować błędny kod


## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
    - Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych
    - Tworzenie aplikacji na platformie Azure
- Pobierz i zainstaluj [Visual Studio Debugger migawki](http://aka.ms/snapshotdebugger).
- Włącz [programu Visual Studio migawki debugera](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-snapshot-debugger)
- Wdrażanie aplikacji .NET na platformie Azure i [włączyć zestaw SDK usługi Application Insights](app-insights-asp-net.md). 
- Samouczek śledzi identyfikacji Wystąpił wyjątek w aplikacji, więc modyfikowanie kodu w środowisku deweloperskich lub testowania można wygenerować wyjątek. 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Analizowanie błędów
Usługa Application Insights zbiera żadnych błędów w aplikacji i umożliwia wyświetlanie ich częstotliwość przez różne operacje, aby skupić się wysiłków na tych, których najlepiej wpłynąć.  Następnie można Przechodzenie do szczegółów tych błędów, aby zidentyfikować przyczynę.   

1. Wybierz **usługi Application Insights** , a następnie subskrypcja.  
1. Aby otworzyć **błędów** panelu wybierz opcję **błędów** w obszarze **zbadaj** menu lub kliknij przycisk **żądań zakończonych niepowodzeniem** wykresu.

    ![Żądań zakończonych niepowodzeniem](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

2. **Żądań zakończonych niepowodzeniem** panelu informuje o liczbie nieudanych żądań i liczbę użytkowników, dla każdej operacji dla aplikacji.  Sortując tych informacji przez użytkownika można zidentyfikować te błędy większość wpływ na użytkowników.  W tym przykładzie **UZYSKAĆ pracowników/Utwórz** i **UZYSKAĆ klientów, uzyskać szczegółowe informacje** prawdopodobnie nadają się do sprawdzania, czy z powodu dużej liczby błędów i wpływ na użytkowników.  Wybranie operacji zawiera dodatkowe informacje na temat tej operacji na prawym panelu.

    ![Nie powiodło się panelu żądań](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

3. Zmniejsza okno czasu powiększyć okres, w przypadku gdy współczynnik awaryjności przedstawiono kolekcji.

    ![Okno żądań zakończonych niepowodzeniem](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

4. Kliknij przycisk **Wyświetl szczegóły** Aby wyświetlić szczegóły dla tej operacji.  Dotyczy to również wykres Gantta przedstawiający dwa zależności nie powiodło się, co łącznie trwało prawie pół sekundy do ukończenia.  Można dowiedzieć się więcej na temat analizowania problemów z wydajnością, wykonując samouczka [znaleźć i diagnozowanie problemów z wydajnością z usługą Azure Application Insights](app-insights-tutorial-performance.md).

    ![Nie powiodło się szczegóły żądania](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

5. Szczegóły operacji przedstawiono również FormatException widocznego spowodował błąd.  Kliknij przycisk wyjątek lub na **typów wyjątków 3 pierwszych** liczbę, aby wyświetlić jego szczegóły.  Widoczny jest ze względu na nieprawidłowy kod pocztowy.

    ![Szczegóły wyjątku](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)



## <a name="identify-failing-code"></a>Zidentyfikuj w przypadku braku kodu
Debuger migawki zbiera dane migawki najczęstsze wyjątków w aplikacji, aby pomóc w zdiagnozowaniu jego główną przyczynę w środowisku produkcyjnym.  Migawki debugowania można wyświetlić w portalu w celu sprawdzenia wywołania stosu i sprawdzić zmiennych w każdej ramki stosu wywołań. Następnie można debugować kodu źródłowego pobieranie migawki i otwierając go w programie Visual Studio 2017 r.

1. W oknie Właściwości wyjątku kliknij **migawki Otwórz debugowania**.
2. **Debugowania migawki** panelu otwiera ze stosu wywołań dla żądania.  Kliknij dowolną metodę, aby wyświetlić wartości wszystkich zmiennych lokalnych w momencie żądania.  Począwszy od top — metoda w tym przykładzie, możemy stwierdzić, zmienne lokalne, które mają żadnej wartości.

    ![Debugowanie migawki](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. Pierwsze wywołanie, które znajdowały się prawidłowe wartości to **ValidZipCode**, i zobaczysz, że kod pocztowy podano z literami, nie można przetłumaczyć jej na liczbę całkowitą.  Wydaje się być błąd kodu, który musi zostać usunięty.

    ![Debugowanie migawki](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Aby pobrać tę migawkę do programu Visual Studio, w którym można znaleźć rzeczywisty kod, który musi zostać usunięty, kliknij przycisk **pobrać migawki**.
6. Migawka została załadowana do programu Visual Studio.
7. Teraz możesz uruchomić sesję debugowania w programie Visual Studio, która szybko identyfikuje wiersz kodu, który spowodował wyjątek.

    ![Wyjątek w kodzie](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Użyj analizy danych
Wszystkie dane zebrane przez usługę Application Insights są przechowywane w Azure Log Analytics, która zapewnia rozwinięty język zapytań umożliwiający analizowanie danych w różny sposób.  Możemy użyć tych danych do analizowania żądania, które spowodowało wyjątku, który możemy Cię badanie. 

8. Kliknij przycisk informacje wskaźników CodeLens powyżej kod, aby wyświetlić dane telemetryczne udostępniane przez usługi Application Insights.

    ![Kod](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Kliknij przycisk **analizy wpływu** otworzyć analizy wgląd w aplikacji.  Nagłówkowy kilka zapytań, które zawierają szczegółowe informacje dotyczące żądań zakończonych niepowodzeniem, takie jak wpływ na użytkowników, przeglądarki i regionach.<br><br>![Analiza](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Dodaj element roboczy
Jeśli system śledzenia, takiego jak Visual Studio Team Services lub GitHub połączenie usługi Application Insights, można utworzyć elementu roboczego bezpośrednio z usługi Application Insights.

1. Wróć do **właściwości wyjątku** panelu w usłudze Application Insights.
2. Kliknij przycisk **nowego elementu roboczego**.
3. **Nowego elementu roboczego** panelu otwiera się ze szczegółami wyjątek już wypełnione.  Można dodać żadnych dodatkowych informacji przed zapisaniem.

    ![Nowy element roboczy](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już identyfikowanie wyjątki czasu wykonywania, przejdź do następnego samouczek informacje na temat identyfikowania i diagnozowanie problemów z wydajnością.

> [!div class="nextstepaction"]
> [Identyfikowanie problemów z wydajnością](app-insights-tutorial-performance.md)