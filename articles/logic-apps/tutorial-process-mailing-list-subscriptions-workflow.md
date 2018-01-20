---
title: "Tworzenie przepływów pracy do przetwarzania żądań listy adresowej - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób tworzenia przepływów pracy zatwierdzania automatycznego przetwarzania listy adresowej subskrypcji z usługi Azure Logic Apps"
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
ms.openlocfilehash: 26ef6f69ef2f2d50628f4d0b021159526c9a04a7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-mailing-list-requests-with-a-logic-app"></a>Zarządzaj żądaniami listy adresowej z aplikacji logiki

Aplikacje logiki platformy Azure pozwala automatyzować przepływy pracy i integrowanie danych między usługami Azure, usług firmy Microsoft, inne oprogramowanie jako usługa (SaaS) aplikacje i systemami lokalnymi. Ten samouczek pokazuje, jak można tworzyć [aplikacji logiki](../logic-apps/logic-apps-overview.md) który przetwarza żądania subskrypcji dla listy adresowej zarządza [MailChimp](https://mailchimp.com/) usługi.
Ta aplikacja logiki monitoruje konta e-mail na potrzeby te żądania, wysyła te żądania na zatwierdzenie i dodaje zatwierdzonych członków do tej listy.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenia pustej aplikacji logiki.
> * Dodaj wyzwalacz, który monitoruje wiadomości e-mail dla żądań subskrypcji.
> * Dodaj akcję, która wysyła wiadomości e-mail do zatwierdzenia lub odrzucenia te żądania.
> * Dodaj warunek, który sprawdza odpowiedź zatwierdzenia.
> * Dodaj akcję, która dodaje zatwierdzonych członków do tej listy.
> * Dodaj warunek, który sprawdza, czy te elementy członkowskie została pomyślnie dołączona do listy.
> * Dodaj akcję, która wysyła wiadomości e-mail z potwierdzeniem, czy tych członków została pomyślnie dołączona do listy.

Gdy wszystko będzie gotowe, aplikację logiki wygląda ten przepływ pracy na wysokim poziomie:

![Aplikacja logiki Zakończono wysokiego poziomu](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-overview.png)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem <a href="https://azure.microsoft.com/free/" target="_blank">zarejestruj się w celu założenia bezpłatnego konta platformy Azure</a>.

## <a name="prerequisites"></a>Wymagania wstępne

* Konto MailChimp. Utwórz listę o nazwie "test członków ML" gdzie aplikację logiki można dodawać adresów e-mail dla zatwierdzonych członków. Jeśli nie masz konta, [Załóż bezpłatne konto](https://login.mailchimp.com/signup/) i Dowiedz się [Tworzenie listy](https://us17.admin.mailchimp.com/lists/#). 

* Konto e-mail z programu Outlook pakietu Office 365 lub Outlook.com, który obsługuje przepływów pracy. W tym artykule wykorzystano program Outlook pakietu Office 365. Jeśli używasz konta innego adresu e-mail, ogólne kroki nie zmieniają się, ale interfejsu użytkownika może wyglądać nieco inaczej.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się do <a href="https://portal.azure.com" target="_blank">portalu Azure</a> przy użyciu poświadczeń konta platformy Azure.

## <a name="create-your-logic-app"></a>Tworzenie aplikacji logiki

1. Z głównego menu platformy Azure wybierz kolejno pozycje **Nowy** > **Integracja dla przedsiębiorstw** > **Aplikacja logiki**.

   ![Tworzenie aplikacji logiki](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app.png)

2. W obszarze **tworzenie aplikacji logiki**, podaj te informacje dotyczące aplikacji logiki jako pokazano, jak i opisano. Gdy skończysz, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Podaj informacje o aplikacji logiki](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa** | LA MailingList | Nazwa aplikacji logiki | 
   | **Subskrypcja** | <*your-Azure-subscription-name*> | Nazwa dla subskrypcji platformy Azure | 
   | **Grupa zasobów** | LA-MailingList-RG | Nazwa [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) używane do organizowania zasoby pokrewne | 
   | **Lokalizacja** | Wschodnie stany USA 2 | Region miejsce przechowywania informacji na temat aplikacji logiki | 
   | **Log Analytics** | Wyłączone | Zachowaj **poza** ustawienia rejestrowania diagnostycznego. | 
   |||| 

3. Po Azure wdraża aplikację, Projektant aplikacji logiki otwiera i pokazuje stronę wprowadzenie wideo oraz szablonów dla typowych wzorców aplikacji logiki. W obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**.

   ![Wybieranie szablonu pustej aplikacji logiki](./media/tutorial-process-mailing-list-subscriptions-workflow/choose-logic-app-template.png)

Następnie dodaj [wyzwalacza](../logic-apps/logic-apps-overview.md#logic-app-concepts) która nasłuchuje przychodzących wiadomości e-mail przy użyciu żądania subskrypcji.
Każda aplikacja logiki musi rozpoczynać się od wyzwalacza, które odbywa się uruchamiany, gdy wystąpi określone zdarzenie lub gdy nowe dane spełniają określony warunek. Aby uzyskać więcej informacji, zobacz [tworzenie pierwszej aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>Dodawanie monitorowania wiadomości e-mail

1. W Projektancie wprowadź "po odebraniu wiadomości e-mail" w polu wyszukiwania. Wybierz wyzwalacza dla dostawcy usługi poczty e-mail:  **< *dostawcy swój adres e-mail*> — w momencie nadejścia nowych wiadomości e-mail**
   
   ![Wybierz tego wyzwalacza dla dostawcy poczty e-mail: "Po odebraniu nowej wiadomości e-mail"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Dla kont służbowych platformy Azure wybierz pozycję Office 365 Outlook.
   * Dla osobistych kont Microsoft wybierz pozycję Outlook.com.

2. Jeśli jest wyświetlany monit o poświadczenia, zaloguj się na Twoje konto e-mail, aby Logic Apps można utworzyć połączenie do Twojego konta e-mail.

3. Teraz można określić kryteria, które sprawdza wyzwalacza w wszystkich nowych wiadomości e-mail.

   1. Określ folder, interwał i częstotliwość sprawdzania wiadomości e-mail.

      ![Określ folder, interwał i częstotliwość sprawdzania wiadomości](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Ustawienie | Wartość | Opis | 
      | ------- | ----- | ----------- | 
      | **Folder** | Skrzynka odbiorcza | Folderu poczty e-mail do monitorowania | 
      | **Interval** | 1 | Liczba interwałów między kontroli | 
      | **Częstotliwość** | Godzina | Sprawdza jednostki czasu dla każdego interwału między  | 
      |  |  |  | 

   2. Wybierz **Pokaż zaawansowane opcje**. W **filtr podmiotu** wpisz tekst tego wyzwalacza można znaleźć w temacie wiadomości e-mail:```subscribe-test-members-ML```

      ![Ustaw opcje zaawansowane](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-advanced-options.png)

4. Aby ukryć szczegóły tego wyzwalacza teraz, kliknij przycisk paska tytułu tego wyzwalacza.

   ![Zwiń kształt, aby ukryć szczegóły](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

5. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   Aplikację logiki jest teraz na żywo, ale nie są wykonywane inne niż wyboru przychodzącej poczty e-mail. 
   Tak Dodaj akcję, która reaguje, gdy wyzwalacz uruchamia.

## <a name="send-approval-email"></a>Wyślij wiadomość e-mail dotyczącą zatwierdzenia

Teraz, gdy masz wyzwalacz dodać [akcji](../logic-apps/logic-apps-overview.md#logic-app-concepts) który wysyła wiadomość e-mail, aby zatwierdzić lub odrzucić żądanie. 

1. W obszarze wyzwalacza, wybierz **+ nowy krok** > **Dodaj akcję**. Wyszukaj "zatwierdzenia" i wybierz tę akcję:  **< *dostawcy swój adres e-mail*>-wysyłania wiadomości e-mail zatwierdzania**

   ![Wybierz pozycję "< swój adres e-mail dostawcy > — Wysyłanie wiadomości e-mail zatwierdzania"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

2. Dostarcza informacji dotyczących tej akcji, jak pokazano i opisano: 

   ![Ustawienia poczty e-mail zatwierdzenia](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Do** | <*approver-email-address*> | Adres e-mail osoby zatwierdzającej. Do celów testowych, możesz użyć własnego adresu. | 
   | **Opcje użytkownika** | Zatwierdź, Odrzuć | Opcje odpowiedzi, które można wybrać osoby zatwierdzającej. Domyślnie osoba zatwierdzająca można wybrać albo "Zatwierdź" lub "Odrzuć" jako ich odpowiedzi. | 
   | **Temat** | Zatwierdź żądanie elementu członkowskiego dla testu członków ML | Temat opisową poczty e-mail | 
   |  |  |  | 

   Na razie Pomiń listy zawartości dynamicznej lub listy parametrów tekście wyświetlanym po kliknięciu wewnątrz pola edycji określonych. 
   Ta lista umożliwia Wybierz parametry z poprzednich działań, których można użyć jako dane wejściowe w przepływie pracy. 
   Szerokość Twojej przeglądarki określa, które zostanie wyświetlone. 
 
4. Zapisz aplikację logiki.

Następnie dodaj warunek do sprawdzenia osoby zatwierdzającej wybranej odpowiedzi.

## <a name="check-approval-response"></a>Sprawdź odpowiedzi

1. W obszarze **wysyłania wiadomości e-mail zatwierdzania** akcji, wybierz **+ nowy krok** > **Dodaj warunek**.

   Kształt warunku pojawi się wraz z dostępnych parametrów, obejmujących jako dane wejściowe do przepływu pracy. 

2. Zmień nazwę warunku z dokładniejszy opis.

   1. Na pasku tytułu warunku, wybierz **wielokropek** (**...** ) przycisk > **zmienić**.

      Na przykład, jeśli przeglądarka jest w widoku wąskie:

      ![Zmień nazwę warunku](./media/tutorial-process-mailing-list-subscriptions-workflow/condition-rename.png)

      Jeśli przeglądarka jest w widoku szerokości i listy zawartości dynamicznej zablokuje dostęp do przycisk z wielokropkiem, Zamknij listę, wybierając **dodać zawartość dynamiczną** wewnątrz warunku.

   2. Zmień nazwę warunku z tym opisem:```If request approved```

3. Tworzenie warunku, który sprawdza, czy osoba zatwierdzająca zaznaczone **Zatwierdź**:

   1. Wewnątrz warunek, kliknij wewnątrz **wybierz wartość** pole, które jest po lewej stronie (Widok szeroki przeglądarki) lub na górze (Widok wąskie przeglądarki).
   Wybierz z listy parametrów lub listy zawartości dynamicznej **SelectedOption** pole w obszarze **wysyłania wiadomości e-mail zatwierdzania**.

      Na przykład podczas pracy w widoku szeroki warunku wygląda w tym przykładzie:

      ![W obszarze "Wyślij e-mail zatwierdzenia" Wybierz "SelectedOption"](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   2. W polu operator porównania wybierz tego operatora: **jest równa**

   3. W prawo (wide widok) lub dolnej (wąskie widok) **wybierz wartość** wprowadź tej wartości:```Approve```

      Gdy wszystko będzie gotowe, warunku wygląda następująco:

      ![Pełny warunek](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

4. Zapisz aplikację logiki.

Następnie określ akcję, która wykonuje aplikację logiki, gdy osoba dokonująca przeglądu zatwierdza żądanie. 

## <a name="add-member-to-mailchimp-list"></a>Dodaj członka do listy MailChimp

Teraz Dodaj akcję, która dodaje zatwierdzonych elementu członkowskiego do listy adresowej.

1. W ten stan **w przypadku wartości PRAWDA** gałęzi, wybierz **Dodaj akcję**.
Wyszukaj "mailchimp" i wybierz tę akcję: **MailChimp — Dodawanie członka do listy**

   ![Wybierz opcję "MailChimp — Dodawanie członka do listy"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

3. Jeśli użytkownik jest proszony o Zaloguj się do swojego konta MailChimp, zaloguj się przy użyciu poświadczeń MailChimp.

4. Dostarcza informacji dotyczących tej akcji, co pokazano opisane tutaj:

   ![Podaj informacje dotyczące "Dodaj członka do listy"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Identyfikator listy** | Test członków ML | Nazwa dla listy adresowej MailChimp | 
   | **Stan** | subskrypcję | Stan subskrypcji dla nowego elementu członkowskiego. Aby uzyskać więcej informacji, zobacz <a href="https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/" target="_blank">Zarządzanie subskrybentów z interfejsem API MailChimp</a>. | 
   | **Adres e-mail** | <*new-member-email-address*> | Wybierz z listy parametrów albo listy zawartości dynamicznej, **z** w obszarze **po odebraniu nowej wiadomości e-mail**, która przechodzi w adresie e-mail dla nowego elementu członkowskiego. 
   |  |  |  | 

5. Zapisz aplikację logiki.

Następnie dodaj warunek, dzięki czemu można sprawdzić, czy nowy element członkowski została pomyślnie dołączona do listy adresowej. W ten sposób aplikację logiki powiadamia użytkownika, czy ta operacja zakończy się powodzeniem lub nie powiedzie się.

## <a name="check-for-success-or-failure"></a>Sprawdź, czy powodzenie lub niepowodzenie

1. W **w przypadku wartości PRAWDA** gałęzi, w obszarze **Członkowskie Dodaj do listy** akcji, wybierz **więcej...**   >  **Dodaj warunek**.

2. Zmień nazwę warunku zawierającego opis tego:```If add member succeeded```

3. Utwórz warunek, który sprawdza, czy zatwierdzone elementu członkowskiego powiedzie się lub kończy się niepowodzeniem w dołączenie listy adresowej:

   1. Wewnątrz warunek, kliknij wewnątrz **wybierz wartość** pole, które jest po lewej stronie (Widok szeroki przeglądarki) lub na górze (Widok wąskie przeglądarki).
   Wybierz z listy parametrów lub listy zawartości dynamicznej **stan** pole w obszarze **Członkowskie Dodaj do listy**.

      Na przykład podczas pracy w widoku szeroki warunku wygląda w tym przykładzie:

      ![W obszarze "Dodawanie członka do listy", wybierz "Status"](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   2. W polu operator porównania wybierz tego operatora: **jest równa**

   3. W prawo (wide widok) lub dolnej (wąskie widok) **wybierz wartość** wprowadź tej wartości:```subscribed```

   Gdy wszystko będzie gotowe, warunku wygląda następująco:

   ![Warunek ukończone](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Następnie skonfiguruj wiadomości e-mail do wysyłania po pomyślnym lub niepomyślnym zatwierdzonych elementu członkowskiego w dołączenie listy adresowej.

## <a name="send-email-if-member-added"></a>Wyślij wiadomość e-mail w przypadku dodania elementu członkowskiego

1. W **w przypadku wartości PRAWDA** gałęzi dla warunku **Jeśli dodać członka zakończyło się pomyślnie**, wybierz **Dodaj akcję**.

   ![W gałęzi "w przypadku true" w warunku, wybierz opcję "Dodaj akcję"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

2. Wyszukaj "outlook wysyłanie wiadomości e-mail" i wybierz tę akcję:  **< *dostawcy swój adres e-mail*> — Wyślij wiadomość e-mail**

   ![Dodaj akcję "Wyślij usługi poczty e-mail"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

3. Zmień nazwę działania o tym opisie:```Send email on success```

4. Dostarcza informacji dotyczących tej akcji, jak pokazano i opisano:

   ![Podaj informacje dotyczące powodzenia poczty e-mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Do** | <*your-email-address*> | Adres e-mail, gdzie do wysyłania wiadomości e-mail Powodzenie. Do celów testowych możesz użyć własnego adresu e-mail. | 
   | **Temat** | <*subject-for-success-email*> | Temat wiadomości e-mail Powodzenie. W tym samouczku, wprowadź tekst, a następnie wybierz określone pole w obszarze **Członkowskie Dodaj do listy** z listy parametrów lub dynamicznej zawartości listy: <p>"Sukces Został dodany do "test członków ML": **adres E-mail**" | 
   | **Treść** | <*body-for-success-email*> | Treść wiadomości e-mail Powodzenie. W tym samouczku, wprowadź tekst, a następnie wybierz określone pola w obszarze **Członkowskie Dodaj do listy** z listy parametrów lub dynamicznej zawartości listy:  <p>"Nowy element członkowski został przyłączony"test członków ML": **adres E-mail**"</br>"Stan akceptacji elementu członkowskiego: **stan**" | 
   | | | | 

5. Zapisz aplikację logiki.

## <a name="send-email-if-member-not-added"></a>Wyślij wiadomość e-mail, jeśli element członkowski nie dodany.

1. W **w przypadku wartości FAŁSZ** gałęzi dla warunku **Jeśli dodać członka zakończyło się pomyślnie**, wybierz **Dodaj akcję**.

   ![W gałęzi "w przypadku wartości FAŁSZ" w warunku, wybierz opcję "Dodaj akcję"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

2. Wyszukaj "outlook wysyłanie wiadomości e-mail" i wybierz tę akcję:  **< *dostawcy swój adres e-mail*> — Wyślij wiadomość e-mail**

   ![Dodaj akcję "Wyślij usługi poczty e-mail"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

3. Zmień nazwę działania o tym opisie:```Send email on failure```

4. Dostarcza informacji dotyczących tej akcji, co pokazano opisane tutaj:

   ![Podaj informacje dotyczące poczty email zakończonych niepowodzeniem](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Do** | <*your-email-address*> | Adres e-mail, gdzie do wysyłania wiadomości e-mail awarii. Do celów testowych możesz użyć własnego adresu e-mail. | 
   | **Temat** | <*subject-for-failure-email*> | Temat wiadomości e-mail awarii. W tym samouczku, wprowadź tekst, a następnie wybierz określone pole w obszarze **Członkowskie Dodaj do listy** z listy parametrów lub dynamicznej zawartości listy: <p>"Nie powiodło się, element członkowski nie dodany do"test członków ML": **adres E-mail**" | 
   | **Treść** | <*body-for-failure-email*> | Treść wiadomości e-mail awarii. W tym samouczku wprowadź tekst: <p>"Elementu członkowskiego już istnieje. Sprawdź swoje konto MailChimp." | 
   | | | | 

5. Zapisz aplikację logiki. 

Następnie przetestuj aplikację logiki, która powinna wyglądać następująco:

 ![Zakończono logiki aplikacji](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-complete.png)

## <a name="run-your-logic-app"></a>Uruchom aplikację logiki

1. Wyślij do siebie żądania poczty e-mail do dołączenia do listy adresowej.
Poczekaj na żądanie, które mają być widoczne w Twojej skrzynce odbiorczej.

3. Aby ręcznie uruchomić swoją aplikację logiki, na pasku narzędzi projektanta wybierz pozycję **Uruchom**. 

   Jeśli adres e-mail ma którego podmiot odpowiada filtru podmiotu wyzwalacza, aplikację logiki wysyła wiadomości e-mail do zatwierdzenia żądania subskrypcji.

4. W wiadomości e-mail zatwierdzania, wybierz **Zatwierdź**.

5. Jeśli adres e-mail subskrybenta nie istnieje na liście adresowej, aplikację logiki dodaje adresu e-mail tej osoby i wysyła do Ciebie wiadomość e-mail, tak jak ten przykład:

   ![Powodzenie poczty e-mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-success.png)

   Aplikację logiki nie można dodać na subskrybencie, otrzymasz wiadomość e-mail, tak jak ten przykład:

   ![Poczty email zakończonych niepowodzeniem](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-failed.png)

   Jeśli nie otrzymasz żadnych wiadomości e-mail, sprawdź folder wiadomości-śmieci adresu e-mail. 
   Filtr wiadomości-śmieci wiadomości e-mail może przekierować tego rodzaju wiadomości. 
   W przeciwnym razie, jeśli nie masz pewności, czy aplikacja logiki została poprawnie uruchomiona, zobacz [Troubleshoot your logic app (Rozwiązywania problemów z aplikacją logiki)](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulacje, został już utworzony i uruchom aplikację logiki która integruje się informacji w usługach Microsoft Azure i innymi aplikacjami SaaS.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupę zasobów, która zawiera aplikację logiki i powiązanych zasobów. W menu głównym Azure, przejdź do **grup zasobów**i wybierz grupę zasobów aplikacji logiki. Wybierz **Usuń grupę zasobów**. Wprowadź nazwę grupy zasobów jako potwierdzenie, a następnie wybierz pozycję **usunąć**.

!["Overview" > "Usuń grupę zasobów"](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać pomysły dotyczące funkcji lub zagłosować na nie, odwiedź [witrynę opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzono aplikację logiki, która zarządza zatwierdzenia żądań listy adresowej. Teraz jak utworzyć aplikację logiki, procesów i przechowująca załączników wiadomości e-mail przez integrację z usługami Azure, takich jak usługi Azure Storage i Azure Functions.

> [!div class="nextstepaction"]
> [Proces załączników wiadomości e-mail](../logic-apps/tutorial-process-email-attachments-workflow.md)