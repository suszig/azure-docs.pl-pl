---
title: "Tworzenie przepływów pracy można przetworzyć wiadomości e-mail i załączników - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób tworzenia automatycznych przepływów pracy do przetwarzania wiadomości e-mail i załączników za pomocą usługi Azure Logic Apps, usługi Azure Storage i Azure Functions"
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
ms.openlocfilehash: 210731ce2e792452650b7a92cfc542c78a0e8014
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>Przetwarzaj wiadomości e-mail i załączniki z aplikacji logiki

Aplikacje logiki platformy Azure pozwala automatyzować przepływy pracy i integrowanie danych między usługami Azure, usług firmy Microsoft, inne oprogramowanie jako usługa (SaaS) aplikacje i systemami lokalnymi. Ten samouczek pokazuje, jak można tworzyć [aplikacji logiki](../logic-apps/logic-apps-overview.md) obsługująca przychodzących wiadomości e-mail i załączniki. To procesy aplikacji logiki tej zawartości, zapisuje zawartość do magazynu Azure i wysyła powiadomienia do przeglądania zawartości. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie [magazynu Azure](../storage/common/storage-introduction.md) i zapisać Eksploratora usługi Storage sprawdzania wiadomości e-mail i załączników.
> * Utwórz [funkcji Azure](../azure-functions/functions-overview.md) , które spowoduje usunięcie HTML z wiadomości e-mail. Ten samouczek zawiera kod, który można użyć dla tej funkcji.
> * Tworzenia pustej aplikacji logiki.
> * Dodaj wyzwalacz, który monitoruje wiadomości e-mail dla załączników.
> * Dodaj warunek, który sprawdza, czy załączników wiadomości e-mail.
> * Dodaj akcję wywołującą funkcji platformy Azure po załączników wiadomości e-mail.
> * Dodaj akcję, która tworzy obiekty BLOB magazynu dla wiadomości e-mail i załączników.
> * Dodaj akcję wysyła powiadomienia e-mail.

Gdy wszystko będzie gotowe, aplikację logiki wygląda ten przepływ pracy na wysokim poziomie:

![Aplikacja logiki Zakończono wysokiego poziomu](./media/tutorial-process-email-attachments-workflow/overview.png)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem <a href="https://azure.microsoft.com/free/" target="_blank">zarejestruj się w celu założenia bezpłatnego konta platformy Azure</a>. 

## <a name="prerequisites"></a>Wymagania wstępne

* Konto e-mail od dostawcy poczty e-mail obsługiwane przez logikę aplikacji, takich jak program Outlook pakietu Office 365, Outlook.com lub Gmail. W przypadku innych dostawców [przeglądu łączniki tutaj](https://docs.microsoft.com/connectors/).

  Ta aplikacja logiki przy użyciu konta programu Outlook pakietu Office 365. 
  Jeśli używasz konta innego adresu e-mail, ogólne kroki nie zmieniają się, ale interfejsu użytkownika może wyglądać nieco inaczej.

* Pobierz i zainstaluj <a href="http://storageexplorer.com/" target="_blank">Eksploratora magazynu Microsoft Azure w warstwie bezpłatna</a>. To narzędzie ułatwia Sprawdź, czy Twoje kontenera magazynu jest poprawnie skonfigurowany.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się do <a href="https://portal.azure.com" target="_blank">portalu Azure</a> przy użyciu poświadczeń konta platformy Azure.

## <a name="set-up-storage-to-save-attachments"></a>Konfigurowanie magazynu, aby zapisać załączników

Możesz zapisać przychodzących wiadomości e-mail i załączników jako obiekty BLOB w [kontenera magazynu Azure](../storage/common/storage-introduction.md). 

1. Aby można było utworzyć kontenera magazynu, [Utwórz konto magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account) przy użyciu tych ustawień:

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa** | attachmentstorageacct | Nazwa konta magazynu | 
   | **Model wdrażania** | Menedżer zasobów | [Modelu wdrażania](../azure-resource-manager/resource-manager-deployment-model.md) zarządzania zasobu wdrożenia | 
   | **Typ konta** | Zastosowania ogólne | [Typu konta magazynu](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Performance** | Standardowa (Standard) | To ustawienie określa typy danych obsługiwane i nośniki do przechowywania danych. Zobacz [typy kont magazynu](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replikacja** | Magazyn lokalnie nadmiarowy (LRS) | To ustawienie określa, jak danych jest kopiowana, przechowywane zarządzane i zsynchronizowane. Zobacz [replikacji](../storage/common/storage-introduction.md#replication). | 
   | **Zapewnienia bezpiecznego transferu wymagane** | Disabled (Wyłączony) | To ustawienie określa zabezpieczeń wymaganych dla żądań z połączenia. Zobacz [wymaga zapewnienia bezpiecznego transferu](../storage/common/storage-require-secure-transfer.md). | 
   | **Subskrypcja** | <*your-Azure-subscription-name*> | Nazwa dla subskrypcji platformy Azure | 
   | **Grupa zasobów** | LA — samouczek-zarządcy zasobów | Nazwa [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) używany do organizowania i zarządzania nimi powiązane zasoby. <p>**Uwaga:** grupy zasobów istnieje w określonym regionie. Mimo że elementy w tym samouczku mogą nie być dostępne we wszystkich regionach, spróbuj użyć tego samego regionu, gdy jest to możliwe. | 
   | **Lokalizacja** | Wschodnie stany USA 2 | Region miejsce przechowywania informacji o koncie magazynu | 
   | **Konfigurowanie sieci wirtualnych** | Disabled (Wyłączony) | W tym samouczku, Zachowaj **wyłączone** ustawienie. | 
   |||| 

   Można również użyć [programu Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) lub [interfejsu wiersza polecenia Azure](../storage/common/storage-quickstart-create-storage-account-cli.md).
  
2. Po Azure wdraża konta magazynu, Pobierz klucz dostępu konta magazynu:

   1. W magazynie konta menu, w obszarze **ustawienia**, wybierz **klucze dostępu**. 
   2. Znajdź **klucz1** w obszarze **domyślne klucze** i nazwa konta magazynu.

      ![Skopiuj i Zapisz nazwę konta magazynu i klucz](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Można również użyć [programu Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) lub [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list). 

3. Utwórz kontener magazynu dla załączników do wiadomości e-mail.
   
   1. W menu konta magazynu na **omówienie** okienku wybierz **obiekty BLOB** w obszarze **usług**, a następnie wybierz **+ kontener**.

   2. Wprowadź "załączniki" jako nazwę kontenera. W obszarze **poziom dostępu publicznego**, wybierz pozycję **kontenera (anonimowy dostęp do odczytu do kontenerów i obiektów blob.)**i wybierz polecenie **OK**.

   Można również użyć [programu Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), lub [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create). 
   Gdy wszystko będzie gotowe, możesz znaleźć z kontenera magazynu na koncie magazynu w tym miejscu w portalu Azure:

   ![Kontener magazynu zakończone](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Następnie podłącz Eksploratora magazynu do konta magazynu.

## <a name="set-up-storage-explorer"></a>Konfigurowanie Eksploratora usługi Storage

Teraz połączenia Eksploratora magazynu do konta magazynu, dzięki któremu można potwierdzić, że aplikację logiki poprawnie zapisuje załączniki jako obiekty BLOB w kontenerze z magazynu.

1. Otwórz Eksploratora usługi Storage platformy Microsoft Azure. Eksplorator usługi Storage monit dla połączenia z magazynem platformy Azure, wybierz **użyć nazwy konta magazynu i klucza** > **dalej**.
Jeśli nie zostanie wyświetlony monit, wybierz **Dodaj konto** na pasku narzędzi Eksplorator.

2. W obszarze **dołączyć przy użyciu nazwy i klucza**, wprowadź nazwę konta magazynu i klucza dostępu, który został wcześniej zapisany. Wybierz **dalej** > **połączyć**.

3. Sprawdź, czy Twoje konto magazynu i kontener są wyświetlane poprawnie w Eksploratorze usługi Storage:

   1. W obszarze **Explorer**, rozwiń węzeł **(lokalny i dołączonego)** > 
    **kont magazynu** > **attachmentstorageaccount** > 
    **Obiektu blob kontenery**.

   2. Upewnij się, że kontener "załączniki" jest teraz wyświetlany. 
   Na przykład:

      ![Eksplorator usługi Storage - Potwierdź kontenera magazynu](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Następnie należy utworzyć [funkcji Azure](../azure-functions/functions-overview.md) , które spowoduje usunięcie HTML przychodzących wiadomości e-mail.

## <a name="create-a-function-to-clean-html"></a>Tworzy funkcję, aby wyczyścić HTML

Teraz Użyj fragment kodu udostępniane przez te kroki, aby utworzyć Azure funkcja, która usuwa HTML z każdej przychodzących wiadomości e-mail. W ten sposób treść wiadomości e-mail jest czyszczący i łatwiejsze do procesu. Następnie można wywołać tej funkcji z aplikacji logiki.

1. Aby można było utworzyć funkcję, [tworzenia aplikacji funkcji](../azure-functions/functions-create-function-app-portal.md) przy użyciu tych ustawień:

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa aplikacji** | CleanTextFunctionApp | Globalnie unikatowa i opisowa nazwa aplikacji — funkcja | 
   | **Subskrypcja** | <*your-Azure-subscription-name*> | Tej samej subskrypcji platformy Azure, który wcześniej używany | 
   | **Grupa zasobów** | LA — samouczek-zarządcy zasobów | Tej samej grupy zasobów platformy Azure, który wcześniej używany | 
   | **Plan hostingu** | Plan Zużycie | To ustawienie określa sposób przydzielania i zasoby skali, takie jak obliczanie zasilania do uruchamiania aplikacji funkcji. Zobacz [hosting plany porównanie](../azure-functions/functions-scale.md). | 
   | **Lokalizacja** | Wschodnie stany USA 2 | Tym samym regionie, który wcześniej używany | 
   | **Storage** | cleantextfunctionstorageacct | Utwórz konto magazynu dla funkcji aplikacji. Użyj tylko małe litery i cyfry. <p>**Uwaga:** to konto magazynu zawiera aplikacji funkcji i różni się od konta magazynu wcześniej utworzonej dla załączników wiadomości e-mail. | 
   | **Application Insights** | Wyłączone | Włącza monitorowanie aplikacji za pomocą [usługi Application Insights](../application-insights/app-insights-overview.md), zachowując w tym samouczku **poza** ustawienie. | 
   |||| 

   Jeśli funkcja aplikacji nie zostanie automatycznie otwarte po wdrożeniu, postrzegać twoją aplikację w <a href="https://portal.azure.com" target="_blank">portalu Azure</a>. W menu głównym Azure wybierz **usługi aplikacji**i wybierz aplikację funkcji.

   ![Funkcja utworzonej aplikacji](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Jeśli **usługi aplikacji** nie są wyświetlane w Azure menu, przejdź do **więcej usług** zamiast tego. W polu wyszukiwania Znajdź i zaznacz pozycję **aplikacji funkcji**. Aby uzyskać więcej informacji, zobacz [Tworzenie funkcji](../azure-functions/functions-create-first-azure-function.md).

   Można również użyć [interfejsu wiersza polecenia Azure](../azure-functions/functions-create-first-azure-function-azure-cli.md), lub [szablonów programu PowerShell i Menedżera zasobów](../azure-resource-manager/resource-group-template-deploy.md).

2. W obszarze **aplikacji funkcji**, rozwiń węzeł **CleanTextFunctionApp**i wybierz **funkcji**. Na pasku narzędzi funkcji wybierz **+ nową funkcję**.

   ![Tworzenie nowej funkcji](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. W obszarze **wybrać szablon poniżej lub przejdź do procedury szybkiego startu**, wybierz pozycję **HttpTrigger - C#** szablonu funkcji.

   ![Wybierz szablon — funkcja](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. W obszarze **nazwy funkcji**, wprowadź ```RemoveHTMLFunction```. W obszarze **wyzwalacza HTTP** > **poziom autoryzacji**, zachowaj ustawienie domyślne **funkcja** wartość, a następnie wybierz **Utwórz**.

   ![Nadaj nazwę funkcji](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Po otwarciu edytora, Zastąp ten kod, który usuwa HTML i zwraca wyniki do obiektu wywołującego kod szablonu:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Gdy wszystko będzie gotowe, wybierz pozycję **zapisać**. Aby przetestować funkcję, należy wybrać **Test** w obszarze strzałkę (**<**) ikonę w edytorze prawej krawędzi. 

   ![Otwieranie okienka "Test"](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. W **testu** okienku w obszarze **treść żądania**, wprowadź ten wiersz i wybierz **Uruchom**.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Testuj funkcję](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   **Dane wyjściowe** okna przedstawia wynik z funkcji:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Po sprawdzeniu, że funkcja działa, należy utworzyć aplikację logiki. Chociaż w tym samouczku przedstawiono sposób tworzenia funkcji, które spowoduje usunięcie HTML z wiadomości e-mail, ma także Logic Apps **HTML, Text** łącznika.

## <a name="create-your-logic-app"></a>Tworzenie aplikacji logiki

1. W menu głównym Azure wybierz **nowy** > **integracji przedsiębiorstwa** > **aplikacji logiki**.

   ![Tworzenie aplikacji logiki](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. W obszarze **tworzenie aplikacji logiki**, podaj te informacje dotyczące aplikacji logiki jako pokazano, jak i opisano. Gdy skończysz, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Podaj informacje o aplikacji logiki](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa** | LA-ProcessAttachment | Nazwa aplikacji logiki | 
   | **Subskrypcja** | <*your-Azure-subscription-name*> | Tej samej subskrypcji platformy Azure, który wcześniej używany | 
   | **Grupa zasobów** | LA — samouczek-zarządcy zasobów | Tej samej grupy zasobów platformy Azure, który wcześniej używany |
   | **Lokalizacja** | Wschodnie stany USA 2 | Tym samym regionie, który wcześniej używany | 
   | **Log Analytics** | Wyłączone | W tym samouczku, Zachowaj **poza** ustawienie. | 
   |||| 

3. Po Azure wdraża aplikację, Projektant aplikacji logiki otwiera i pokazuje stronę wprowadzenie wideo oraz szablonów dla typowych wzorców aplikacji logiki. W obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**.

   ![Wybieranie szablonu pustej aplikacji logiki](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Następnie dodaj [wyzwalacza](../logic-apps/logic-apps-overview.md#logic-app-concepts) która nasłuchuje przychodzących wiadomości e-mail, które mają załączników. Każda aplikacja logiki musi rozpoczynać się od wyzwalacza, które odbywa się uruchamiany, gdy wystąpi określone zdarzenie lub gdy nowe dane spełniają określony warunek. Aby uzyskać więcej informacji, zobacz [tworzenie pierwszej aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Monitorowanie przychodzących wiadomości e-mail

1. W Projektancie wprowadź "po odebraniu wiadomości e-mail" w polu wyszukiwania. Wybierz tego wyzwalacza dla dostawcy usługi poczty e-mail:  **< *dostawcy swój adres e-mail*> — w momencie nadejścia nowych wiadomości e-mail**, na przykład:

   ![Wybierz tego wyzwalacza dla dostawcy poczty e-mail: "Po odebraniu nowej wiadomości e-mail"](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Dla kont służbowych platformy Azure wybierz pozycję Office 365 Outlook. 
   * Dla osobistych kont Microsoft wybierz pozycję Outlook.com. 

2. Jeśli jest wyświetlany monit o poświadczenia, zaloguj się na Twoje konto e-mail dzięki czemu Logic Apps można łączyć się konto poczty e-mail.

3. Teraz należy podać kryteria wyzwalacza używane do filtrowania nowych wiadomości e-mail.

   1. Określ folder, interwał i częstotliwość sprawdzania wiadomości e-mail.

      ![Określ folder, interwał i częstotliwość sprawdzania wiadomości](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Ustawienie | Wartość | Opis | 
      | ------- | ----- | ----------- | 
      | **Folder** | Skrzynka odbiorcza | Sprawdź folder wiadomości e-mail | 
      | **Interval** | 1 | Liczba interwałów między kontroli | 
      | **Częstotliwość** | Minuta | Sprawdza jednostki czasu dla każdego interwału między | 
      |  |  |  | 
  
   2. Wybierz **Pokaż zaawansowane opcje** i określ następujące ustawienia:

      | Ustawienie | Wartość | Opis | 
      | ------- | ----- | ----------- | 
      | **Zawiera załącznik** | Yes | Pobierz tylko wiadomości e-mail z załącznikami. <p>**Uwaga:** wyzwalacz nie powoduje usunięcia żadnych wiadomości e-mail z Twojego konta, sprawdzenie tylko nowe wiadomości i przetwarzanie tylko wiadomości e-mail, zgodne z filtrem podmiotu. | 
      | **Załączniki** | Yes | Pobierz załączniki jako dane wejściowe dla przepływu pracy, a nie tylko sprawdzania załączników. | 
      | **Filtr podmiotu** | ```Business Analyst 2 #423501``` | Tekst, który można znaleźć w temacie wiadomości e-mail | 
      |  |  |  | 

4. Aby ukryć szczegóły tego wyzwalacza teraz, kliknij wewnątrz paska tytułu tego wyzwalacza.

   ![Zwiń kształt, aby ukryć szczegóły](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   Aplikację logiki jest teraz na żywo, ale nie wszystko inne Sprawdź wiadomości e-mail. 
   Następnie dodaj warunek, który określa kryteria, aby kontynuować przepływ pracy.

## <a name="check-for-attachments"></a>Sprawdź, czy załączników

1. W obszarze wyzwalacza, wybierz **+ nowy krok** > **Dodaj warunek**.

   Gdy pojawi się kształt warunku, domyślnie na liście parametrów lub listy zawartości dynamicznej pojawia się i pokazuje wszystkie parametry z poprzedniego kroku, który można dołączyć jako danych wejściowych przepływu pracy. 
   Szerokość Twojej przeglądarki określa, które zostanie wyświetlone.

2. Zmień nazwę warunku z dokładniejszy opis.

   1. Na pasku tytułu warunku, wybierz **wielokropek** (**...** ) przycisk > **zmienić**.

      Na przykład, jeśli przeglądarka jest w widoku wąskie:

      ![Zmień nazwę warunku](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Jeśli przeglądarka jest w widoku szerokości i listy zawartości dynamicznej zablokuje dostęp do przycisk z wielokropkiem, Zamknij listę, wybierając **dodać zawartość dynamiczną** wewnątrz warunku. 
      
      ![Zamknij listy zawartości dynamicznej](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Zmień nazwę warunku z tym opisem:```If email has attachments and key subject phrase```

3. Opis warunku zapewniając wyrażenia. 

   1. Wewnątrz kształtu warunku wybierz **edytowanie w trybie zaawansowanym**.

      ![Edytuj warunek w trybie zaawansowanym](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. W polu tekstowym wprowadź wyrażenie:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      To wyrażenie porównuje **HasAttachment** wartości właściwości z treści wyzwalacza, czyli wiadomości e-mail z tego samouczka, logiczna obiektu ```True```. 
      Jeśli obie wartości są równe, wiadomości e-mail zawiera co najmniej jeden załącznik, przekazuje warunku, i kontynuuje przepływ pracy.

      Warunek teraz wygląda następująco:

      ![Wyrażenie warunku](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Wybierz **edytowania w trybie podstawowe**. Teraz w wyrażeniu rozwiązuje, jak pokazano poniżej:

      ![Wyrażenie rozwiązany](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Aby ręcznie utworzyć wyrażenie, musi działać w trybie podstawowym i dynamiczne listy, Otwórz, dzięki czemu można pracować z Konstruktor wyrażeń. W obszarze **wyrażenie**, można wybrać funkcje. W obszarze **zawartości dynamicznej**, możesz wybrać pola parametr do użycia w tych funkcji.
      > Później w tym samouczku pokazano, jak ręcznie utworzyć wyrażenia.

4. Zapisz aplikację logiki.

### <a name="test-your-condition"></a>Testowanie warunku

Teraz test sprawdzający, czy warunek działa prawidłowo:

1. Jeśli aplikację logiki nie jest już uruchomiony, wybierz **Uruchom** na pasku narzędzi projektanta.

   W tym kroku ręcznie uruchamia aplikację logiki bez konieczności poczekaj, aż przekazuje określonego interwału. 
   Jednak nic się nie dzieje aż do nadejścia testowa wiadomość e-mail w skrzynce odbiorczej. 

2. Wyślij do siebie wiadomość e-mail, która spełnia te kryteria:

   * Temat wiadomości e-mail ma tekst, który określono w wyzwalacza **filtr podmiotu**:```Business Analyst 2 #423501```

   * Adres e-mail ma jeden załącznik. 
   Teraz po prostu utworzyć jeden pusty plik tekstowy i dołączyć ten plik do wiadomości e-mail.

   Po odebraniu wiadomości e-mail, aplikacji logiki sprawdza załączników i tekst określonego tematu.
   Jeśli warunek zakończy się pomyślnie, wyzwalacz wyzwala i powoduje, że aparat Logic Apps utworzyć wystąpienie aplikacji logiki i uruchomić przepływ pracy. 

3. Aby sprawdzić, czy uruchamiany wyzwalacza i aplikacji logiki został uruchomiony pomyślnie, w menu aplikacji logiki wybierz **omówienie**.

   ![Sprawdź historię wyzwalacza i działa](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Jeśli aplikację logiki nie wyzwolenia lub uruchom pomimo wyzwalacz pomyślnie, zobacz [rozwiązywania problemów z aplikacją logiki](../logic-apps/logic-apps-diagnosing-failures.md).

Następnie określ akcje do wykonania **w przypadku wartości PRAWDA** gałęzi. Aby zapisać wiadomości e-mail oraz załączniki, Usuń kod HTML z treści wiadomości e-mail, a następnie utwórz obiekty BLOB w kontenerze magazynu dla wiadomości e-mail i załączników.

> [!NOTE]
> Nie musi wykonywać żadnych czynności, aby uzyskać aplikację logiki **w przypadku wartości FAŁSZ** gałęzi, jeśli wiadomość e-mail nie ma załączników. Jako wykonywaniu podwyższenie po ukończeniu tego samouczka można dodać odpowiednią akcję, która ma być wykonywana **w przypadku wartości FAŁSZ** gałęzi.

## <a name="call-the-removehtmlfunction"></a>Wywołanie RemoveHTMLFunction

1. W menu aplikacji logiki, wybierz **projektanta aplikacji logiki**. W **w przypadku wartości PRAWDA** gałęzi, wybierz **Dodaj akcję**.

2. Wyszukaj "azure functions" i wybierz tę akcję: **usługi Azure Functions — należy wybrać funkcję platformy Azure**

   ![Wybierz akcję dla "Azure Functions — wybierz funkcję platformy Azure"](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Wybierz aplikację utworzonej wcześniej funkcja: **CleanTextFunctionApp**

   ![Wybierz opcję aplikacji Azure — funkcja](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Teraz wybierz funkcji: **RemoveHTMLFunction**

   ![Wybierz funkcji platformy Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Zmień nazwę funkcji kształt z tym opisem:```Call RemoveHTMLFunction to clean email body``` 

6. Funkcja kształtu wprowadź dane wejściowe dla funkcji do przetwarzania. Określ treść wiadomości e-mail jako pokazano, jak i opisane tutaj:

   ![Określ treści żądania dla tej funkcji można oczekiwać](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. W obszarze **treść żądania**, wprowadź tekst: 
   
      ```{ "emailBody": ``` 

      Dopiero po zakończeniu tego wpisu w następnych krokach pojawia się błąd o nieprawidłowy JSON.
      Testach wcześniej tej funkcji określone dla tej funkcji dane wejściowe używane JavaScript Object Notation (JSON). 
      Tak treści żądania musi za użycie tego samego formatu. 

   2. Wybierz z listy parametrów lub listy zawartości dynamicznej **treści** pole w obszarze **po odebraniu nowej wiadomości e-mail**.
   Po **treści** pola, Dodaj zamykający nawias klamrowy:```}```

      ![Określ treści żądania do przekazania do funkcji](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      W definicji aplikacji logiki ten wpis jest wyświetlany w następującym formacie:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Zapisz aplikację logiki.

Następnie dodaj akcję, która tworzy obiektu blob w kontenerze z magazynu zapisywania treść wiadomości e-mail.

## <a name="create-blob-for-email-body"></a>Tworzenie obiektu blob dla treści wiadomości e-mail

1. W obszarze kształtu funkcji platformy Azure, wybierz **Dodaj akcję**. 

2. W obszarze **wybierz akcję**, wyszukaj "blob" i wybierz tę akcję: **magazynu obiektów Blob Azure — Tworzenie obiektów blob**

   ![Dodaj akcję w celu utworzenia obiektu blob dla treści wiadomości e-mail](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Jeśli nie masz połączenie z kontem magazynu platformy Azure, utworzyć połączenie do konta magazynu przy użyciu tych ustawień wyświetlanych i opisane tutaj. Gdy wszystko będzie gotowe, wybierz pozycję **Utwórz**.

   ![Utwórz połączenie z kontem magazynu](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Nazwa połączenia** | AttachmentStorageConnection | Nazwę opisową dla połączenia | 
   | **Konto magazynu** | attachmentstorageacct | Nazwa konta magazynu utworzoną wcześniej zapisywania załączników | 
   |||| 

4. Zmień nazwę **Utwórz obiekt blob** akcji z tym opisem:```Create blob for email body```

5. W **Utwórz obiekt blob** akcji, podaj te informacje, a następnie wybierz tych parametrów do utworzenia obiektu blob jako pokazano, jak i opisem:

   ![Podaj informacje o obiekcie blob dla treści wiadomości e-mail](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Ścieżka folderu** | /Attachments | Ścieżkę i nazwę kontenera, która została wcześniej utworzona. Można również Przeglądaj i wybierz kontener. | 
   | **Nazwa obiektu blob** | **Z** pola | Podaj nazwę nadawcy wiadomości e-mail jako nazwa obiektu blob. Wybierz z listy parametrów albo listy zawartości dynamicznej, **z** w obszarze **po odebraniu nowej wiadomości e-mail**. | 
   | **Zawartość obiektu blob** | **Zawartości** pola | Przekaż w treści wiadomości e-mail bez HTML jako zawartości obiektu blob. Wybierz z listy parametrów albo listy zawartości dynamicznej, **treści** w obszarze **RemoveHTMLFunction wywołać, aby wyczyścić treść wiadomości e-mail**. |
   |||| 

6. Zapisz aplikację logiki. 

### <a name="check-attachment-handling"></a>Obsługa załączników wyboru

Teraz test sprawdzający, czy aplikację logiki obsługi wiadomości e-mail w taki sposób jak:

1. Jeśli aplikację logiki nie jest już uruchomiony, wybierz **Uruchom** na pasku narzędzi projektanta.

2. Wyślij do siebie wiadomość e-mail, która spełnia te kryteria:

   * Temat wiadomości e-mail ma tekst, który określono w wyzwalacza **filtr podmiotu**:```Business Analyst 2 #423501```

   * Adres e-mail ma co najmniej jeden załącznik. 
   Teraz po prostu utworzyć jeden pusty plik tekstowy i dołączyć ten plik do wiadomości e-mail.

   * Adres e-mail ma zawartość testową na przykład w treści: 

     ```
     Testing my logic app
     ```

   Jeśli aplikację logiki nie wyzwolenia lub uruchom pomimo wyzwalacz pomyślnie, zobacz [rozwiązywania problemów z aplikacją logiki](../logic-apps/logic-apps-diagnosing-failures.md).

3. Sprawdź aplikację logiki zapisania wiadomości e-mail do kontenera magazynu poprawne. 

   1. W Eksploratorze usługi Storage, rozwiń węzeł **(lokalny i dołączonego)** > 
    **kont magazynu** > **attachmentstorageacct (zewnętrzne)** > 
    **Kontenerów obiektów blob** > **załączników**.

   2. Sprawdź **załączników** kontener dla wiadomości e-mail. 

      W tym momencie tylko wiadomości e-mail pojawia się w kontenerze, ponieważ aplikacji logiki nie jeszcze przetwarzać załączniki.

      ![Sprawdź, czy Eksploratora usługi Storage zapisanej wiadomości e-mail.](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Gdy wszystko będzie gotowe, Usuń poczty e-mail w Eksploratorze usługi Storage.

4. Opcjonalnie Aby przetestować **w przypadku wartości FAŁSZ** gałęzi, które ma nic w tym momencie, możesz wysłać wiadomość e-mail, który nie spełnia kryteriów.

Następnie dodaj pętlę do przetworzenia wszystkich załączników wiadomości e-mail.

## <a name="process-attachments"></a>Proces załączników

Korzysta z tej aplikacji logiki **dla każdego** pętli do przetworzenia wszystkich załączników wiadomości e-mail.

1. W obszarze **obiektu blob Utwórz dla treści wiadomości e-mail** kształtu, wybierz **... Więcej**i wybierz polecenie: **Dodaj dla każdego**

   ![Dodawanie pętli "for each"](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Zmiana nazwy Twojego pętli z opis tego:```For each email attachment```

3. Teraz można określić dla pętli do przetwarzania danych. Kliknij wewnątrz **wybierz wyjścia z poprzednich kroków** pole. Wybierz z listy parametrów lub listy zawartości dynamicznej **załączników**. 

   ![Wybierz opcję "Załączników"](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   **Załączników** pola przekazuje tablica zawierająca wszystkie załączniki dołączone do wiadomości e-mail. 
   **Dla każdego** pętli powtarza akcje na każdy element, który jest przekazywany za pomocą tablicy.

4. Zapisz aplikację logiki.

Następnie dodaj akcję, która zapisuje poszczególnych załączników w postaci obiektów blob w Twojej **załączników** kontenera magazynu.

## <a name="create-blobs-for-attachments"></a>Tworzenie obiektów blob dla załączników

1. W **dla każdego** pętli, wybierz **Dodaj akcję** umożliwiające określenie zadań do wykonania na każdej znaleziono załącznika.

   ![Dodaj akcję do pętli](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. W obszarze **wybierz akcję**, wyszukaj "blob", a następnie wybierz tę akcję: **magazynu obiektów Blob Azure — Tworzenie obiektów blob**

   ![Dodaj akcję w celu utworzenia obiektu blob](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Zmień nazwę **Tworzenie obiektów blob 2** akcji z tym opisem:```Create blob for each email attachment```

4. W **obiektu blob Utwórz dla wszystkich załączników wiadomości e-mail** akcji, podaj te informacje, a następnie wybierz parametrów do utworzenia jako pokazano, jak i opisano każdy obiekt blob:

   ![Podaj informacje o obiekcie blob](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Ustawienie | Wartość | Opis | 
   | ------- | ----- | ----------- | 
   | **Ścieżka folderu** | /Attachments | Ścieżka i nazwa kontenera wcześniej utworzony. Można również wybrać i wybierz kontener. | 
   | **Nazwa obiektu blob** | **Nazwa** pola | Wybierz z listy parametrów albo listy zawartości dynamicznej, **nazwa** do przekazania w nazwie załącznika nazwa obiektu blob. | 
   | **Zawartość obiektu blob** | **Zawartości** pola | Wybierz z listy parametrów albo listy zawartości dynamicznej, **zawartości** do uwzględnienia w zawartości załącznika dla zawartości obiektu blob. |
   |||| 

5. Zapisz aplikację logiki. 

### <a name="check-attachment-handling"></a>Obsługa załączników wyboru

Następnie test sprawdzający, czy aplikację logiki obsługuje załączniki w taki sposób jak:

1. Jeśli aplikację logiki nie jest już uruchomiony, wybierz **Uruchom** na pasku narzędzi projektanta.

2. Wyślij do siebie wiadomość e-mail, która spełnia te kryteria:

   * Temat wiadomości e-mail ma tekst, który określono w wyzwalacza **filtr podmiotu**:```Business Analyst 2 #423501```

   * Adres e-mail ma co najmniej dwa załączniki. 
   Teraz po prostu utworzyć dwa pliki pusty tekst i Dołącz te pliki do wiadomości e-mail.

   Jeśli aplikację logiki nie wyzwolenia lub uruchom pomimo wyzwalacz pomyślnie, zobacz [rozwiązywania problemów z aplikacją logiki](../logic-apps/logic-apps-diagnosing-failures.md).

3. Sprawdź zapisania aplikację logiki poczty e-mail i załączników do kontenera magazynu poprawne. 

   1. W Eksploratorze usługi Storage, rozwiń węzeł **(lokalny i dołączonego)** > 
    **kont magazynu** > **attachmentstorageacct (zewnętrzne)** > 
    **Kontenerów obiektów blob** > **załączników**.

   2. Sprawdź **załączników** kontenera dla wiadomości e-mail i załączniki.

      ![Sprawdź, czy zapisane wiadomości e-mail i załączników](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Gdy wszystko będzie gotowe, Usuń wiadomości e-mail i załączników w Eksploratorze usługi Storage.

Następnie dodaj akcję, aby aplikację logiki wysyła wiadomości e-mail, aby przejrzeć te załączniki.

## <a name="send-email-notifications"></a>Wysyłanie powiadomień e-mail

1. W **w przypadku wartości PRAWDA** gałęzi, w obszarze **dla wszystkich załączników wiadomości e-mail** pętli, wybierz **Dodaj akcję**. 

   ![Dodaj akcję w obszarze "for each" pętli](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. W obszarze **wybierz akcję**, wyszukaj "Wysyłanie wiadomości e-mail", a następnie wybierz akcję "Wyślij wiadomość e-mail" dla dostawcy poczty e-mail, który ma. Aby filtrować listę akcji do określonej usługi, można wybrać łącznik najpierw w obszarze **łączniki**.

   ![Wybierz akcję "Wyślij wiadomość e-mail" dla dostawcy usługi poczty e-mail](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Dla kont służbowych platformy Azure wybierz pozycję Office 365 Outlook. 
   * Dla osobistych kont Microsoft wybierz pozycję Outlook.com. 

3. Jeśli jest wyświetlany monit o poświadczenia, zaloguj się na Twoje konto e-mail, aby aplikacje logiki tworzy połączenie z kontem e-mail.

4. Zmień nazwę **wysłać wiadomość e-mail** akcji z tym opisem:```Send email for review```

5. Podaj informacje dotyczące tej akcji i wybierz pola, które chcesz uwzględnić w wiadomości e-mail, co pokazano opisane. Aby dodać puste wiersze w polu edycji, naciśnij klawisze Shift + Enter.  

   Na przykład, jeśli pracujesz z listy dynamicznej zawartości:

   ![Wyślij powiadomienie pocztą e-mail](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Jeśli nie możesz znaleźć oczekiwanego pola na liście, wybierz **zobaczyć więcej** obok **po odebraniu nowej wiadomości e-mail** na liście zawartości dynamicznej lub na końcu listy parametrów.

   | Ustawienie | Wartość | Uwagi | 
   | ------- | ----- | ----- | 
   | **Do** | <*recipient-email-address*> | Do celów testowych możesz użyć własnego adresu e-mail. | 
   | **Temat**  | ```ASAP - Review applicant for position: ```**Podmiotu** | Temat wiadomości e-mail, który chcesz dołączyć. Wybierz z listy parametrów albo listy zawartości dynamicznej, **podmiotu** pole w obszarze **po odebraniu nowej wiadomości e-mail**. | 
   | **Treść** | ```Please review new applicant:``` <p>```Applicant name: ```**z** <p>```Application file location: ```**Ścieżki** <p>```Application email content: ```**Treści** | Zawartość dla treści wiadomości e-mail. Z listy parametrów lub listy zawartości dynamicznej wybierz te pola: <p>- **z** pole w obszarze **po odebraniu nowej wiadomości e-mail** </br>- **Ścieżki** pole w obszarze **obiektu blob Utwórz dla treści wiadomości e-mail** </br>- **Treści** pole w obszarze **RemoveHTMLFunction wywołać, aby wyczyścić treść wiadomości e-mail** | 
   |||| 

   W przypadku wybierz pole, które zawiera tablicę, takie jak **zawartości**, która jest tablica zawierająca załączniki, Projektant automatycznie dodaje pętli "For each" wokół akcję, która odwołuje się do tego pola. 
   Dzięki temu Twoja aplikacja logiki może wykonać tę akcję dla każdego elementu tablicy. 
   Aby usunąć pętli, usuń pola tablicy, Przenieś Akcja odwołujący się do poza pętli, wybierz wielokropek (**...** ) w pętli tytułu paska, a następnie wybierz **usunąć**.
     
6. Zapisz aplikację logiki. 

Następnie przetestuj aplikację logiki, która teraz wygląda następująco:

![Zakończono logiki aplikacji](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Uruchom aplikację logiki

1. Wyślij do siebie wiadomość e-mail, która spełnia te kryteria:

   * Temat wiadomości e-mail ma tekst, który określono w wyzwalacza **filtr podmiotu**:```Business Analyst 2 #423501```

   * Adres e-mail ma jeden lub więcej załączników. 
   Można ponownie użyć pusty plik tekstowy z poprzednich testu. 
   Dla scenariusza bardziej realistyczne dołączyć plik wznowienia.

   * Treść wiadomości e-mail zawiera ten tekst, który możesz skopiować i wkleić:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Uruchom aplikację logiki. W przypadku powodzenia aplikację logiki wysyła wiadomość e-mail, który wygląda następująco:

   ![Powiadomienie e-mail wysyłanych przez aplikację logiki](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Jeśli nie otrzymasz żadnych wiadomości e-mail, sprawdź folder wiadomości-śmieci adresu e-mail. 
   Filtr wiadomości-śmieci wiadomości e-mail może przekierować tego rodzaju wiadomości. 
   W przeciwnym razie, jeśli nie masz pewności, czy aplikacja logiki została poprawnie uruchomiona, zobacz [Troubleshoot your logic app (Rozwiązywania problemów z aplikacją logiki)](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulacje, utworzeniu teraz i uruchom aplikację logiki, który automatyzuje zadania w różnych usługach Azure i wywołuje kodu niestandardowego.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupę zasobów, która zawiera aplikację logiki i powiązanych zasobów. W menu głównym Azure, przejdź do **grup zasobów**i wybierz grupę zasobów aplikacji logiki. Wybierz **Usuń grupę zasobów**. Wprowadź nazwę grupy zasobów jako potwierdzenie, a następnie wybierz pozycję **usunąć**.

![Usuń grupę zasobów aplikacji logiki](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać pomysły dotyczące funkcji lub zagłosować na nie, odwiedź [witrynę opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzono aplikację logiki, która przetwarza i przechowuje załączników wiadomości e-mail przez integrację z usługami Azure, takich jak usługi Azure Storage i Azure Functions. Teraz Dowiedz się więcej o innych łączników, które służą do tworzenia aplikacji logiki.

> [!div class="nextstepaction"]
> [Dowiedz się więcej o łączniki dla usługi Logic Apps](../connectors/apis-list.md)