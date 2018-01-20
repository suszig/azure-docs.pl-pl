---
title: "Monitorowane zmian maszyny wirtualnej — & siatki zdarzeń Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Sprawdź zmiany konfiguracji w maszynach wirtualnych (VM) przy użyciu siatki zdarzeń platformy Azure i Logic Apps"
keywords: "aplikacje logiki, siatki zdarzeń, w przypadku maszyny wirtualnej maszyny Wirtualnej"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 11/30/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e31f30e46c3a49ff9eca72cb82c16acb731427bf
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Monitoruj zmiany maszyny wirtualnej Azure zdarzeń siatki i Logic Apps

Można uruchomić zautomatyzowanych [przepływu pracy aplikacji logiki](../logic-apps/logic-apps-overview.md) po określonych zdarzeń w zasobów platformy Azure lub innych zasobów. Te zasoby mogą publikować te zdarzenia do [siatki usługi Azure event](../event-grid/overview.md). Z kolei siatki zdarzeń wypychanie tych zdarzeń do subskrybentów mających kolejek, elementów webhook, lub [usługi event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) jako punktów końcowych. Jako aplikację logiki można oczekiwania dla tych zdarzeń z siatki zdarzeń przed uruchomieniem automatycznych przepływów pracy do wykonywania zadań — bez pisania żadnego kodu.

Na przykład poniżej przedstawiono niektóre zdarzenia, które wydawców mogą wysyłać do subskrybentów za pomocą usługi Azure Event siatki:

* Tworzenia, odczytu, zaktualizować lub usunąć zasób. Na przykład można monitorować zmian, które mogą spowodować naliczenie opłat w ramach subskrypcji platformy Azure i mają wpływ na rachunku. 
* Dodaj lub Usuń osoby z subskrypcją platformy Azure.
* Aplikacja wykonuje określonej akcji.
* Wyświetleniu nowego komunikatu z kolejki.

W tym samouczku tworzy aplikację logiki, który śledzi zmiany w maszynie wirtualnej, a następnie wysyła powiadomienia dotyczące tych zmian. Podczas tworzenia aplikacji logiki z subskrypcji zdarzeń dla zasobów platformy Azure, zdarzenia przepływać z tego zasobu za pośrednictwem siatki zdarzeń do aplikacji logiki. Samouczek przeprowadza przez tworzenie aplikacji logiki:

![Przegląd — monitor maszyny wirtualnej z aplikacji logiki i siatki zdarzeń](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie aplikacji logiki, który monitoruje zdarzenia z siatki zdarzeń.
> * Dodaj warunek, które w szczególności sprawdza zmiany maszyny wirtualnej.
> * Wyślij wiadomość e-mail, gdy zmienia się na komputerze wirtualnym.

## <a name="prerequisites"></a>Wymagania wstępne

* Konto e-mail z [każdego dostawcy poczty e-mail obsługiwane przez usługi Azure Logic Apps](../connectors/apis-list.md), takie jak program Outlook pakietu Office 365, Outlook.com lub Gmail wysyłania powiadomień. W tym samouczku jest używana usługa Office 365 Outlook.

* A [maszyny wirtualnej](https://azure.microsoft.com/services/virtual-machines). Jeśli jeszcze tego nie zrobiono, utwórz maszynę wirtualną za pośrednictwem [utworzyć samouczek wirtualna](https://docs.microsoft.com/azure/virtual-machines/). Aby przełączyć maszynę wirtualną, publikowanie zdarzeń, możesz [nie trzeba nic robić](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Tworzenie aplikacji logiki, który monitoruje zdarzenia z siatki zdarzeń

Najpierw należy utworzyć aplikację logiki i dodać wyzwalacz siatki zdarzeń, który monitoruje grupy zasobów dla maszyny wirtualnej. 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). 

2. W lewym górnym rogu menu głównego Azure wybierz opcje **nowy** > **integracji przedsiębiorstwa** > **aplikacji logiki**.

   ![Tworzenie aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Utwórz aplikację logiki z ustawień określonych w poniższej tabeli:

   ![Podawanie szczegółów aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *{your-logic-app-name}* | Podaj unikatową nazwę aplikacji logiki. | 
   | **Subskrypcja** | *{your-Azure-subscription}* | Wybierz tej samej subskrypcji platformy Azure dla wszystkich usług, w tym samouczku. | 
   | **Grupa zasobów** | *{your-Azure-resource-group}* | Wybierz grupę zasobów platformy Azure dla wszystkich usług w tym samouczku. | 
   | **Lokalizacja** | *{your-Azure-region}* | Wybierz tego samego regionu dla wszystkich usług, w tym samouczku. | 
   | | | 

4. Jeśli wszystko jest gotowe, wybierz **Przypnij do pulpitu nawigacyjnego**i wybierz polecenie **Utwórz**.

   Teraz został utworzony zasób platformy Azure dla Twojej aplikacji logiki. 
   Po wdrożeniu Twojej aplikacji logiki na platformie Azure projektant aplikacji usługi Logic Apps pokaże szablony dla typowych wzorców, więc możesz szybciej rozpocząć pracę.

   > [!NOTE] 
   > Po wybraniu **Przypnij do pulpitu nawigacyjnego**, aplikację logiki automatycznie otwiera w Projektancie aplikacji logiki. W przeciwnym razie ręcznie znaleźć i otworzyć aplikację logiki.

5. Teraz można wybrać szablon aplikacji logiki. W obszarze **szablony**, wybierz **pustą aplikację logiki** , można utworzyć aplikację logiki od początku.

   ![Wybieranie szablonu aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Teraz pokazuje projektanta aplikacji logiki [ *łączniki* ](../connectors/apis-list.md) i [ *wyzwalaczy* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) można uruchomić aplikacji logiki i również akcje, które można dodać po wyzwalacz do wykonywania zadań. Wyzwalacz jest zdarzenie, które tworzy wystąpienie aplikacji logiki i uruchamia przepływ pracy aplikacji logiki. 
   Twoja aplikacja logiki powinna wyzwalacz jako pierwszy element.

6. W polu wyszukiwania wprowadź "zdarzeń siatki" jako filtr. Wybierz ten wyzwalacz: **siatki zdarzeń Azure — w przypadku zdarzeń zasobów**

   ![Wybierz ten wyzwalacz: "Azure zdarzeń Siatka — na zdarzenie typu zasobu"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Po wyświetleniu monitu zaloguj się do siatki zdarzeń platformy Azure przy użyciu poświadczeń platformy Azure.

   ![Zaloguj się przy użyciu poświadczeń platformy Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Jeśli logujesz się za pomocą osobistego konta Microsoft, takich jak @outlook.com lub @hotmail.com, wyzwalacz siatki zdarzenie może nie być wyświetlana poprawnie. Jako obejście, wybierz [Połącz z główną usługi](../azure-resource-manager/resource-group-create-service-principal-portal.md), lub Uwierzytelnij się jako członek usługi Azure Active Directory, skojarzone z subskrypcją platformy Azure, na przykład *nazwy użytkownika*@emailoutlook.onmicrosoft.com.

8. Teraz subskrypcji aplikację logiki, aby wydawca zdarzeń. Podaj szczegóły subskrypcji zdarzeń określoną w poniższej tabeli:

   ![Podaj szczegóły subskrypcji zdarzeń](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Subskrypcja** | *{virtual-machine-Azure-subscription}* | Wybierz subskrypcję platformy Azure wydawca zdarzeń. W tym samouczku Wybierz subskrypcję platformy Azure dla maszyny wirtualnej. | 
   | **Typ zasobu** | Microsoft.Resources.resourceGroups | Wybierz typ zasobu wydawca zdarzeń. W tym samouczku wybierz określoną wartość, aplikację logiki monitoruje tylko grupy zasobów. | 
   | **Nazwa zasobu** | *{virtual-machine-resource-group-name}* | Wybierz nazwę zasobu wydawcy. W tym samouczku wybierz nazwę grupy zasobów dla maszyny wirtualnej. | 
   | Wybierz opcjonalne ustawienia **Pokaż zaawansowane opcje**. | *{zobacz opisy}* | * **Prefiks filtru**: W tym samouczku, pozostaw to ustawienie puste. Domyślne zachowanie dopasowuje wszystkie wartości. Jednak można określić ciąg prefiksu jako filtru, na przykład ścieżkę i parametr dla określonego zasobu. <p>* **Sufiks filtru**: W tym samouczku, pozostaw to ustawienie puste. Domyślne zachowanie dopasowuje wszystkie wartości. Jednak można określić ciąg sufiksu jako filtru, na przykład rozszerzenie nazwy pliku, jeśli chcesz tylko określonych typów plików.<p>* **Nazwa subskrypcji**: Podaj unikatową nazwę dla Twojej subskrypcji zdarzeń. |
   | | | 

   Gdy wszystko będzie gotowe, wyzwalacz siatki zdarzeń może wyglądać w tym przykładzie:
   
   ![Przykład zdarzeń wyzwalacza siatki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**. Aby zwijanie i ukrywanie szczegóły akcji w aplikacji logiki, wybierz akcję paska tytułu.

   ![Zapisywanie aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Przy zapisywaniu aplikację logiki wyzwalacza siatki zdarzeń, Azure automatycznie tworzy subskrypcję zdarzeń aplikacji logiki do wybranego zasobu. Dlatego gdy zasób publikuje zdarzenia do siatki zdarzeń, tej siatki zdarzeń automatycznie wypycha dane zdarzenia do aplikacji logiki. To zdarzenie wyzwala aplikację logiki, a następnie tworzy i uruchamia wystąpienia przepływu pracy, który definiuje się w tych kroków.

Aplikację logiki jest teraz na żywo i wykrywa zdarzenia z siatki zdarzeń, ale nic nie robi do momentu dodania działań w przepływie pracy. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Dodaj warunek, który sprawdza, czy zmiany maszyny wirtualnej

Aby uruchomić przepływ pracy aplikacji logiki, tylko wtedy, gdy określone zdarzenie, Dodaj warunek, który sprawdza dla maszyny wirtualnej "" operacji zapisu. Jeśli ten warunek jest spełniony, aplikację logiki wysyła wiadomości e-mail ze szczegółami zaktualizowano maszynę wirtualną.

1. W Projektancie aplikacji logiki, w obszarze siatki wyzwalacza zdarzenia, wybierz **nowy krok** > **Dodaj warunek**.

   ![Dodaj warunek do aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Projektant aplikacji logiki dodaje pusty warunek do przepływu pracy, w tym ścieżki akcji do wykonania na podstawie, czy warunek jest równa true lub false.

   ![Pusty warunku](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. W **warunku** wybierz **edytowanie w trybie zaawansowanym**.
Wprowadź wyrażenie:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Warunek teraz wygląda następująco:

   ![Pusty warunku](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   To wyrażenie sprawdza zdarzenia `body` dla `data` obiektu gdzie `operationName` jest właściwość `Microsoft.Compute/virtualMachines/write` operacji. 
   Dowiedz się więcej o [schematu zdarzeń siatki zdarzeń](../event-grid/event-schema.md).

3. Aby podać opis warunku, wybierz **wielokropek** (**...** ) znajdującego się na kształt warunku, a następnie wybierz pozycję **zmienić**.

   > [!NOTE] 
   > Nowsze przykłady w tym samouczku również zawierają opisy kroków w przepływie pracy aplikacji logiki.

4. Teraz wybierz **edytowania w trybie podstawowe** tak, aby wyrażenie automatycznie rozwiązuje, jak pokazano:

   ![Stan aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Zapisz aplikację logiki.

## <a name="send-email-when-your-virtual-machine-changes"></a>Wyślij wiadomość e-mail, gdy zmienia się na komputerze wirtualnym

Teraz Dodaj [ *akcji* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) tak, aby pobrać wiadomość e-mail, gdy określony warunek jest spełniony.

1. W tym stanie **w przypadku wartości PRAWDA** wybierz **Dodaj akcję**.

   ![Dodaj akcję dla gdy warunek ma wartość true](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. W polu wyszukiwania wprowadź "e-mail" jako filtr. W oparciu o Twojego dostawcę poczty e-mail znajdź i wybierz zgodny łącznik. Następnie wybierz akcję „Wyślij wiadomość e-mail” dla Twojego łącznika. Na przykład: 

   * Dla konta służbowego platformy Azure wybierz łącznik usługi Office 365 Outlook. 
   * Dla osobistych kont Microsoft wybierz łącznik usługi Outlook.com. 
   * Dla kont usługi Gmail wybierz łącznik usługi Gmail. 

   Będziemy kontynuować z użyciem łącznika usługi Office 365 Outlook. 
   Jeśli używasz innego dostawcy, kroki pozostają takie same, ale Twój interfejs użytkownika może wyglądać inaczej. 

   ![Wybierz akcję "Wyślij wiadomość e-mail"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Jeśli masz już połączenia dla dostawcy usługi poczty e-mail, zaloguj się do swojego konta poczty e-mail w odpowiedzi na pytanie uwierzytelniania.

4. Należy podać szczegóły dotyczące wiadomości e-mail, jak określono w poniższej tabeli:

   ![Akcja pusty poczty e-mail](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Aby wybrać pola, które są dostępne w przepływie pracy, kliknij w polu edycji tak że **zawartości dynamicznej** liście otwiera lub wybierz **dodać zawartość dynamiczną**. Więcej pól, wybierz **zobaczyć więcej** dla każdej sekcji na liście. Aby zamknąć **zawartości dynamicznej** wybierz **dodać zawartość dynamiczną**.

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Do** | *{recipient-email-address}* |Wprowadź adres e-mail adresata. Do celów testowych możesz użyć własnego adresu e-mail. | 
   | **Temat** | Zasób zaktualizowane: **podmiotu**| Wprowadź zawartość w polu tematu wiadomości e-mail. W tym samouczku, wprowadź tekst sugerowane i wybierz zdarzenie **podmiotu** pola. W tym miejscu temat wiadomości e-mail zawiera nazwę do zaktualizowanego zasobu (maszyny wirtualnej). | 
   | **Treść** | Grupa zasobów: **tematu** <p>Typ zdarzenia: **typ zdarzenia**<p>Identyfikator zdarzenia: **ID**<p>Czas: **czas trwania zdarzenia** | Wprowadź zawartość w polu treści wiadomości e-mail. W tym samouczku, wprowadź tekst sugerowane i wybierz zdarzenie **tematu**, **typ zdarzenia**, **identyfikator**, i **czas trwania zdarzenia** pola tak, aby Twój adres e-mail zawiera nazwa grupy zasobów, typ zdarzenia, sygnatura czasowa zdarzenia i Identyfikatora zdarzenia dla aktualizacji. <p>Aby dodać puste wiersze w zawartości, naciśnij klawisze Shift + Enter. | 
   | | | 

   > [!NOTE] 
   > Po zaznaczeniu pola, które reprezentuje tablicę, Projektant automatycznie dodaje **dla każdego** pętlę wokół akcję, która odwołuje się do tablicy. W ten sposób Twoja aplikacja logiki wykonuje tę akcję dla każdego elementu tablicy.

   Twoja Akcja poczty e-mail może wyglądać tak jak w tym przykładzie:

   ![Wybierz dane wyjściowe do uwzględnienia w wiadomości e-mail](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   I aplikację logiki Zakończono może wyglądać następująco:

   ![Zakończono logiki aplikacji](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Zapisz aplikację logiki. Aby zwijanie i ukrywanie szczegóły każdej akcji w aplikacji logiki, wybierz pasek tytułu akcji.

   ![Zapisywanie aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Aplikację logiki jest teraz na żywo, ale czeka przed wykonaniem jakichkolwiek zmian do maszyny wirtualnej. 
   Aby teraz przetestować Twoją aplikację logiki, przejdź do następnej sekcji.

## <a name="test-your-logic-app-workflow"></a>Testowanie przepływu pracy aplikacji logiki

1. Aby sprawdzić, czy aplikacji logiki będzie niedługo określonych zdarzeń, należy zaktualizować maszyny wirtualnej. 

   Na przykład można zmienić rozmiar maszyny wirtualnej w portalu Azure lub [Zmień rozmiar maszyny Wirtualnej przy użyciu programu Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Po kilku chwilach otrzymasz wiadomość e-mail. Na przykład:

   ![Wyślij wiadomość e-mail o aktualizacji maszyny wirtualnej](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Aby przejrzeć uruchamia i wyzwolić historii aplikacji logiki, w menu aplikacji logiki, wybierz **omówienie**. Aby wyświetlić więcej szczegółów na temat uruchomienia, wybierz wiersz dla tego uruchomienia.

   ![Historia uruchomieniu aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Aby wyświetlić dane wejściowe i wyjściowe dla każdego kroku, rozwiń krok, który chcesz przejrzeć. Te informacje ułatwiają diagnozowanie i debugowanie problemów w aplikacji logiki.
 
   ![Szczegóły historii uruchomienia aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Gratulacje, utworzeniu i uruchom aplikację logiki, który monitoruje zdarzenia zasobów za pomocą siatki zdarzeń i wiadomości e-mail możesz przypadku tych zdarzeń. Również wiesz, jak łatwo mogą tworzyć przepływy pracy, które automatyzację procesów i integrować systemy oraz usług w chmurze.

Inne zmiany w konfiguracji z siatki zdarzeń i logic apps można monitorować na przykład:

* Maszyna wirtualna uzyskuje dostęp opartej na rolach prawa kontroli (RBAC).
* Zmiany zostały wprowadzone do grupy zabezpieczeń sieci (NSG) dla interfejsu sieciowego (NIC).
* Dysków dla maszyny wirtualnej są dodawane lub usuwane.
* Publiczny adres IP jest przypisany do maszyny wirtualnej karty sieciowej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Ten samouczek używa zasobów i wykonuje akcje, które naliczenie opłat w ramach subskrypcji platformy Azure. Dlatego gdy ukończysz pracę z tego samouczka i testowania, upewnij się, że zostanie wyłączone lub usunięcie wszystkich zasobów, których nie chcesz naliczenie opłat.

* Aby zatrzymać aplikację logiki bez usuwania swojej pracy, wyłącz ją. W menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku narzędzi wybierz pozycję **Wyłącz**.

  ![Wyłączanie aplikacji logiki](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Jeśli nie widzisz menu aplikacji logiki, spróbuj wrócić do pulpitu nawigacyjnego platformy Azure i ponownie otworzyć aplikację logiki.

* Aby trwale usunąć aplikację logiki w menu aplikacji logiki, wybierz **omówienie**. Na pasku narzędzi wybierz pozycję **Usuń**. Potwierdź, że chcesz usunąć aplikację logiki, a następnie wybierz pozycję **Usuń**.

## <a name="next-steps"></a>Kolejne kroki

* [Tworzenie i trasy zdarzeń niestandardowych zdarzeń siatki](../event-grid/custom-event-quickstart.md)
