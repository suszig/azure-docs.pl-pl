---
title: "Tworzenie harmonogramu automatycznych przepływów pracy opartych na — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak utworzyć na podstawie harmonogramu cyklicznego, automatyczne przepływu pracy z usługą Azure Logic Apps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: deb2572de363ca5d0dec0f78f2e30ad648e9b5f8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="check-traffic-with-a-scheduler-based-logic-app"></a>Sprawdź ruchu z aplikacji logiki na podstawie harmonogramu

Aplikacje logiki platformy Azure pomaga zautomatyzować przepływy pracy uruchamiane zgodnie z harmonogramem. Ten samouczek pokazuje, jak można tworzyć [aplikacji logiki](../logic-apps/logic-apps-overview.md) z wyzwalacz harmonogram, który jest uruchamiany codziennie dzień tygodnia i sprawdza czasu podróży, w tym ruch, między dwiema miejsca. Jeśli czas przekracza limit określonych, aplikację logiki wysyła wiadomość e-mail z podczas podróży i dodatkowy czas niezbędne do folderu docelowego.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenia pustej aplikacji logiki. 
> * Dodaj wyzwalacz, który działa jako harmonogramu aplikacji logiki.
> * Dodaj akcję, która pobiera podczas podróży dla trasy.
> * Dodaj akcję, która tworzy zmienną, konwertuje podczas podróży z sekund na minut i zapisuje wynik w zmiennej.
> * Dodaj warunek, który porównuje podczas podróży względem określonego limitu.
> * Dodaj akcję, która wysyła wiadomość e-mail, jeśli czas podróży przekracza limit.

Gdy wszystko będzie gotowe, aplikację logiki wygląda ten przepływ pracy na wysokim poziomie:

![Aplikacja logiki wysokiego poziomu](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem <a href="https://azure.microsoft.com/free/" target="_blank">zarejestruj się w celu założenia bezpłatnego konta platformy Azure</a>.

## <a name="prerequisites"></a>Wymagania wstępne

* Konto e-mail od dostawcy poczty e-mail obsługiwane przez logikę aplikacji, takich jak program Outlook pakietu Office 365, Outlook.com lub Gmail. W przypadku innych dostawców [przeglądu łączniki tutaj](https://docs.microsoft.com/connectors/). Ta opcja szybkiego startu używa konta usługi Outlook.com. Jeśli używasz konta innego adresu e-mail, ogólne kroki nie zmieniają się, ale interfejsu użytkownika może wyglądać nieco inaczej.

* Aby uzyskać czas podróży trasy, należy klucz dostępu dla interfejsu API map Bing. Aby pobrać ten klucz, wykonaj kroki <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">jak uzyskać klucz usługi mapy Bing</a>. 

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się do <a href="https://portal.azure.com" target="_blank">portalu Azure</a> przy użyciu poświadczeń konta platformy Azure.

## <a name="create-your-logic-app"></a>Tworzenie aplikacji logiki

1. Z głównego menu platformy Azure wybierz kolejno pozycje **Nowy** > **Integracja dla przedsiębiorstw** > **Aplikacja logiki**.

   ![Tworzenie aplikacji logiki](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. W obszarze **tworzenie aplikacji logiki**, podaj te informacje dotyczące aplikacji logiki jako pokazano, jak i opisano. Gdy skończysz, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Podaj informacje o aplikacji logiki](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa** | LA TravelTime | Nazwa aplikacji logiki | 
   | **Subskrypcja** | <*your-Azure-subscription-name*> | Nazwa dla subskrypcji platformy Azure | 
   | **Grupa zasobów** | LA-TravelTime-zarządcy zasobów | Nazwa [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) używane do organizowania zasoby pokrewne | 
   | **Lokalizacja** | Wschodnie stany USA 2 | Region miejsce przechowywania informacji na temat aplikacji logiki | 
   | **Log Analytics** | Wyłączone | Zachowaj **poza** ustawienia rejestrowania diagnostycznego. | 
   |||| 

3. Po Azure wdraża aplikację, Projektant aplikacji logiki otwiera i pokazuje stronę wprowadzenie wideo oraz szablonów dla typowych wzorców aplikacji logiki. W obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**.

   ![Wybieranie szablonu pustej aplikacji logiki](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Następnie Dodaj cykl [wyzwalacza](../logic-apps/logic-apps-overview.md#logic-app-concepts), który uruchamiany zgodnie z określonym harmonogramem. Każda aplikacja logiki musi rozpoczynać się od wyzwalacza, które odbywa się uruchamiany, gdy wystąpi określone zdarzenie lub gdy nowe dane spełniają określony warunek. Aby uzyskać więcej informacji, zobacz [tworzenie pierwszej aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Dodaj wyzwalacza harmonogramu

1. W Projektancie wprowadź "cyklu" w polu wyszukiwania. Wybierz ten wyzwalacz: **harmonogram - cyklu**

   ![Znajdź i dodać wyzwalacza "Harmonogram cyklu"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. Na **cyklu** kształtu, wybierz **wielokropek** (**...** ) przycisk, a następnie wybierz pozycję **zmienić**. Zmień nazwę wyzwalacza z takim opisem:```Check travel time every weekday morning```

   ![Zmień nazwę wyzwalacza](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Wewnątrz wyzwalacza, wybierz **Pokaż zaawansowane opcje**.

4. Podaj szczegóły harmonogram i cyklu wyzwalacza jako pokazano, jak i opisem:

   ![Podaj szczegóły harmonogramu i cyklu](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Interval** | 1 | Liczba interwałów między kontroli | 
   | **Częstotliwość** | Tydzień | Jednostka czasu dla cyklu | 
   | **Strefa czasowa** | None | Ma zastosowanie tylko wtedy, gdy należy określić godzinę rozpoczęcia. Ułatwia określenie strefy czasu lokalnego. | 
   | **Godzina rozpoczęcia** | None | Opóźnienie ponownego do określonej daty i godziny. Aby uzyskać więcej informacji, zobacz [Planowanie zadań i przepływy pracy uruchamiane regularnie](../connectors/connectors-native-recurrence.md). | 
   | **W następujące dni** | Poniedziałek, Wtorek, środę, czwartek i piątek | Dostępne tylko wtedy, gdy **częstotliwość** ma ustawioną wartość "Tydzień" | 
   | **W tych godzinach** | 7,8,9 | Dostępne tylko wtedy, gdy **częstotliwość** jest ustawiony na "Tygodnia" lub "Day". Wybierz godziny, dnia, aby uruchomić ten cykl. W tym przykładzie jest uruchamiane w 7, 8 i znaczniki 9 godzin. | 
   | **W tych minutach** | 0,15,30,45 | Dostępne tylko wtedy, gdy **częstotliwość** jest ustawiony na "Tygodnia" lub "Day". Wybierz minut dnia do uruchomienia tego cyklu. W tym przykładzie jest uruchamiany co 15 minut, zaczynając od znaku godzinie zero. | 
   ||||

   Wyzwalacza każdy dzień tygodnia, co 15 minut, zaczynając od 7:00 AM i kończąc godzinie 9:45. 
   **Podgląd** pole zawiera harmonogram cyklu. 
   Aby uzyskać więcej informacji, zobacz [harmonogramu zadań i przepływów pracy](../connectors/connectors-native-recurrence.md) i [działania przepływu pracy i wyzwalaczy](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Aby ukryć szczegóły tego wyzwalacza teraz, kliknij wewnątrz paska tytułu kształtu.

   ![Zwiń kształt, aby ukryć szczegóły](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**. 

Aplikację logiki jest teraz na żywo, ale nie są wykonywane inne Powtórz. Tak Dodaj akcję, która reaguje, gdy wyzwalacz uruchamia.

## <a name="get-the-travel-time-for-a-route"></a>Pobierz czas podróży trasy

Teraz, gdy masz wyzwalacz dodać [akcji](../logic-apps/logic-apps-overview.md#logic-app-concepts) który pobiera podczas podróży między dwóch miejscach. Logic Apps oferuje łącznika dla interfejsu API map Bing, dzięki czemu można łatwo uzyskać te informacje. Przed rozpoczęciem tego zadania upewnij się, że jest klucz interfejsu API map Bing, zgodnie z opisem w tym samouczku wymagania wstępne.

1. W Projektancie aplikacji logiki w ramach wyzwalacz, wybierz **+ nowy krok** > **Dodaj akcję**.

2. Wyszukaj "map", a następnie wybierz tę akcję: **mapy Bing - Get trasy**

3. Jeśli nie masz połączenie z usługą mapy Bing monicie o utworzyć połączenie. Podaj te informacje dotyczące połączenia, a następnie wybierz pozycję **Utwórz**.

   ![Wybierz opcję "Mapy Bing - Get trasy" akcji](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Ustawienie | Wartość | Opis |
   | ------- | ----- | ----------- |
   | **Nazwa połączenia** | BingMapsConnection | Podaj nazwę dla połączenia. | 
   | **Klucz interfejsu API** | <*your-Bing-Maps-key*> | Wprowadź klucz mapy Bing, który wcześniej otrzymały. Dowiedz się, jeśli nie masz klucza usługi mapy Bing <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">jak uzyskać klucz</a>. | 
   | | | |  

4. Zmień nazwę działania o tym opisie:```Get route and travel time with traffic```

5. Podaj szczegóły **trasy Get** akcji, jak pokazano i opisanych tutaj, na przykład:

   ![Podaj informacje dotyczące "Mapy Bing - Get trasy" akcji](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Ustawienie | Wartość | Opis |
   | ------- | ----- | ----------- |
   | **Waypoint 1** | <*start-location*> | Źródła programu trasy | 
   | **Waypoint 2** | <*end-location*> | Twoje trasy docelowego | 
   | **Należy unikać** | None | Wszystkie elementy, aby uniknąć na trasy, na przykład autostrady, przejazd i tak dalej | 
   | **Optimize** | timeWithTraffic | Parametr zoptymalizować trasy, na przykład odległość, czas z bieżącego ruchu podróży i tak dalej. Wybierz ten parametr: "timeWithTraffic" | 
   | **Odległość jednostki** | <*Twoje — preferencji*> | Jednostka odległość z trasy. W tym artykule używa tej jednostki: "Mil"  | 
   | **Tryb podróży** | Zwiększa | Tryb podróży z trasy. Wybierz ten tryb: "Kierowaniu" | 
   | **Przesyłania daty i godziny** | None | Dotyczy tylko tryb przesyłania | 
   | **Przesyłania typu Date — typ** | None | Dotyczy tylko tryb przesyłania | 
   |||| 

   Aby uzyskać więcej informacji na temat tych parametrów, zobacz [obliczyć trasę](https://msdn.microsoft.com/library/ff701717.aspx).

6. Zapisz aplikację logiki.

Następnie należy utworzyć zmienną, dzięki czemu można przekonwertować i przechowywać bieżący czas podróży minut, a nie w sekundach. Dzięki temu można uniknąć powtarzania konwersji i łatwiej Użyj wartości w kolejnych krokach. 

## <a name="create-variable-to-store-travel-time"></a>Utwórz zmienną do przechowywania czas podróży

Czasami można wykonywać operacje na danych w przepływie pracy i korzystanie z wyników w późniejsze akcje. Aby zapisać wyniki te można łatwo ponowne użycie lub ich odwołanie, można tworzyć zmienne do przechowywania tych wyników po przetworzeniu ich. Zmienne można utworzyć tylko na najwyższym poziomie w aplikacji logiki.

Domyślnie poprzedniej **trasy Get** akcji zwraca bieżącą godzinę podróży z ruchem w ciągu sekund za pomocą **podróży ruchu czas trwania** pola. Konwertowanie i zamiast tego przechowywania tej wartości jako minut, można ułatwić wartość późniejszego bez konwertowania ponownie.

1. W obszarze **trasy Get** akcji, wybierz **+ nowy krok** > **Dodaj akcję**.

2. Wyszukaj "zmiennych" i wybierz tę akcję: **zmienne - zainicjować zmiennej**

   ![Wybierz opcję "Zmienne - zainicjować zmiennej" akcji](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Zmień nazwę tej akcji z tym opisem:```Create variable to store travel time```

4. Podaj szczegóły zmiennej użytkownika zgodnie z opisem w tym miejscu:

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa** | travelTime | Nazwę zmiennej użytkownika | 
   | **Typ** | Liczba całkowita | Typ danych zmiennej użytkownika | 
   | **Wartość** | Wyrażenie konwertuje bieżący czas podróży sekundach minut (zobacz kroki opisane w tej tabeli). | Wartość początkowa zmiennej użytkownika | 
   |||| 

   1. Aby utworzyć wyrażenie **wartość** kliknij wewnątrz pola tak, aby pojawi się lista zawartości dynamicznej. 
   W razie potrzeby rozszerzenia przeglądarki, dopóki nie zostanie wyświetlona lista. 
   Na liście zawartości dynamicznej, wybierz **wyrażenia**. 

      ![Podaj informacje dotyczące "Zmienne - zainicjować zmiennej" akcji](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Po kliknięciu wewnątrz niektóre pola edycji, dynamicznej zawartości listy lub listy parametrów wbudowanego zostaną wyświetlone. Ta lista zawiera wszystkie parametry z poprzednich działań, których można użyć jako dane wejściowe w przepływie pracy. 
      Listy zawartości dynamicznej ma edytora wyrażeń, którym można wybrać funkcje dla operacji. 
      Ten edytor wyrażenie pojawi się tylko na liście zawartości dynamicznej.

      Szerokość Twojej przeglądarki określa, które zostanie wyświetlone. 
      Jeśli przeglądarka jest szeroka, pojawi się lista zawartości dynamicznej. 
      Jeśli przeglądarka jest ograniczone, zostanie wyświetlona lista parametrów wbudowany w polu edycji, który aktualnie ma fokus.

   2. W edytorze wyrażenie wprowadź wyrażenie:```div(,60)```

      ![Wprowadź wyrażenie: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. Umieść kursor wewnątrz wyrażenia między lewy nawias (**(**) i przecinkiem (**,**). 
   Wybierz **zawartości dynamicznej**.

      ![Umieść kursor, wybierz polecenie "Zawartość dynamiczna"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Na liście zawartości dynamicznej, wybierz **ruchu czas trwania podróży**.

      ![Wybierz pole "Ruchu czas trwania podróży"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. Po pole rozpoznaje wewnątrz wyrażenia, wybierz **OK**.

      ![Wybierz przycisk "OK"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      **Wartość** pola pojawi się teraz, jak pokazano poniżej:

      ![Pole "Value" za pomocą wyrażenia rozwiązany](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Zapisz aplikację logiki.

Następnie dodaj warunek, który sprawdza, czy bieżący czas podróży jest większy niż limit określone.

## <a name="compare-travel-time-with-limit"></a>Porównanie czasu podróży limit

1. W obszarze poprzednią czynność, wybierz **+ nowy krok** > **Dodaj warunek**. 

2. Zmień nazwę warunku zawierającego opis tego:```If travel time exceeds limit```

3. Tworzenie warunku, który sprawdza, czy **travelTime** przekracza limit określony jak opisano i przedstawiono w tym miejscu:

   1. Wewnątrz warunek, kliknij wewnątrz **wybierz wartość** pole, które jest po lewej stronie (Widok szeroki przeglądarki) lub na górze (Widok wąskie przeglądarki).

   2. Wybierz z listy zawartości dynamicznej lub listy parametrów **travelTime** pole w obszarze **zmienne**.

   3. W polu Porównanie wybierz tego operatora: **jest większa niż**

   4. W **wybierz wartość** do prawej (wide widok) lub dolnej (wąskie widok), wprowadź ten limit:```15```

   Na przykład podczas pracy w widoku wąskie poniżej przedstawiono sposób tworzenia tego warunku:

   ![Tworzenie warunku w widoku wąskie](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Zapisz aplikację logiki.

Następnie dodaj akcję do wykonania, gdy czas podróży przekracza limit.

## <a name="send-email-when-limit-exceeded"></a>Wyślij wiadomość e-mail po przekroczeniu limitu

Teraz Dodaj akcję, która zostanie wiadomości e-mail, gdy czas podróży przekracza limit. Ten adres e-mail zawiera bieżący czas podróży i dodatkowy czas niezbędny do określonej trasy są przesyłane. 

1. W tym stanie **w przypadku wartości PRAWDA** gałęzi, wybierz **Dodaj akcję**.

2. Wyszukaj "Wysyłanie wiadomości e-mail", a następnie wybierz łącznik poczty e-mail i "Wyślij e-mail akcji", który ma być używany.

   ![Znajdź i wybierz akcję "Wyślij wiadomość e-mail"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * W przypadku osobistego konta Microsoft, wybierz opcję **Outlook.com**. 
   * Dla platformy Azure pracy lub szkołą kont, wybierz **Office 365 Outlook**.

3. Jeśli nie masz już połączenie, użytkownik jest proszony o Zaloguj się do swojego konta poczty e-mail.

   Usługa Logic Apps utworzy połączenie z kontem e-mail.

4. Zmień nazwę działania o tym opisie:```Send email with travel time```

5. W polu **Do** wprowadź adres e-mail adresata. Do celów testowych, użyj adresu e-mail.

6. W **podmiotu** polu Określ temat wiadomości e-mail, a obejmują **travelTime** zmiennej.

   1. Wprowadź tekst ```Current travel time (minutes): ``` z spacje końcowe. 
   
   2. Wybierz z listy parametrów lub listy zawartości dynamicznej **travelTime** w obszarze **zmienne**. 
   
      Na przykład, jeśli przeglądarka jest w widoku wąskie:

      ![Wprowadź tekst tematu i wyrażenie zwracające podczas podróży](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. W **treści** Określ zawartości dla treści wiadomości e-mail. 

   1. Wprowadź tekst ```Add extra travel time (minutes): ``` z spacje końcowe. 
   
   2. W razie potrzeby rozszerzenia przeglądarki, dopóki nie zostanie wyświetlona lista zawartości dynamicznej. 
   Na liście zawartości dynamicznej, wybierz **wyrażenia**.

      ![Zbuduj wyrażenie dla treści wiadomości e-mail](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. W edytorze wyrażenie wprowadź wyrażenie, dzięki czemu można obliczyć liczbę minut, które przekraczają limit:```sub(,15)```

      ![Wprowadź wyrażenie do obliczenia czas podróży dodatkowych minut](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. Umieść kursor wewnątrz wyrażenia między lewy nawias (**(**) i przecinkiem (**,**). Wybierz **zawartości dynamicznej**.

      ![Kontynuować tworzenie wyrażenie do obliczenia czas podróży dodatkowych minut](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. W obszarze **zmienne**, wybierz pozycję **travelTime**.

      ![Wybierz pole "travelTime" można używać w wyrażeniach](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. Po pole rozpoznaje wewnątrz wyrażenia, wybierz **OK**.

      ![Pole "Treść" za pomocą wyrażenia rozwiązany](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      **Treści** pola pojawi się teraz, jak pokazano poniżej:

      ![Pole "Treść" za pomocą wyrażenia rozwiązany](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Zapisz aplikację logiki.

Następnie przetestuj aplikację logiki, która powinna wyglądać następująco:

![Zakończono logiki aplikacji](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Uruchom aplikację logiki

Aby ręcznie uruchomić swoją aplikację logiki, na pasku narzędzi projektanta wybierz pozycję **Uruchom**. Jeśli bieżący podróży pozostaje czasu w obszarze limitu, aplikację logiki nie nic i czeka przed ponownym sprawdzeniem następnym interwale czasowym.
Jednak jeśli bieżący czas podróży przekracza limit, otrzymasz wiadomość e-mail z bieżącym czasem podróży i liczbę minut powyżej limitu. Oto przykład wiadomości e-mail, wysyłanej przez aplikację logiki:

![Wiadomości e-mail wysyłane z czasem w podróży](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

Jeśli nie otrzymasz żadnych wiadomości e-mail, sprawdź folder wiadomości-śmieci adresu e-mail. Filtr wiadomości-śmieci wiadomości e-mail może przekierować tego rodzaju wiadomości. W przeciwnym razie, jeśli nie masz pewności, czy aplikacja logiki została poprawnie uruchomiona, zobacz [Troubleshoot your logic app (Rozwiązywania problemów z aplikacją logiki)](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulacje, utworzeniu teraz i uruchamianie aplikacji na podstawie harmonogramu cyklicznego logiki. 

Aby utworzyć inne aplikacje logiki, które używają **harmonogram - cyklu** wyzwalania, zapoznaj się z tych szablonów, które dostępnych po należy utworzyć aplikację logiki:

* Pobierz codzienne przypomnienia wysłane do użytkownika.
* Usuwanie starszych obiektów blob Azure.
* Dodaj komunikat do kolejki usługi Azure Storage.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupę zasobów, która zawiera aplikację logiki i powiązanych zasobów. W menu głównym Azure, przejdź do **grup zasobów**i wybierz grupę zasobów aplikacji logiki. Wybierz **Usuń grupę zasobów**. Wprowadź nazwę grupy zasobów jako potwierdzenie, a następnie wybierz pozycję **usunąć**.

!["Overview" > "Usuń grupę zasobów"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać pomysły dotyczące funkcji lub zagłosować na nie, odwiedź [witrynę opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzyć aplikację logiki, która sprawdza ruch na podstawie określonego harmonogramu (na mornings dzień tygodnia), a akcja ma (wysyła wiadomości e-mail) gdy czas podróży przekracza określony limit. Teraz informacje o sposobie tworzenia aplikacji logiki, który wysyła żądania listy adresowej na zatwierdzenie przez integrację z usługami Azure, usług firmy Microsoft i innymi aplikacjami SaaS.

> [!div class="nextstepaction"]
> [Zarządzaj żądaniami listy adresowej](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)