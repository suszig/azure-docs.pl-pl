---
title: "Tworzenie pierwszego automatycznego przepływu pracy między systemami a usługami w chmurze — Azure Logic Apps | Microsoft Docs"
description: "Automatyzacja procesów biznesowych i przepływów pracy na potrzeby scenariuszy integracji systemów i usługi Enterprise Application Integration (EAI) przez tworzenie i uruchamianie aplikacji logiki"
author: ecfan
manager: anneta
editor: 
services: logic-apps
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: LADocs; estfan
ms.openlocfilehash: d62255ba6e3d5bdfbd792a47f3a92d4c88876742
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-logic-app-for-automating-workflows-and-processes-through-the-azure-portal"></a>Tworzenie pierwszej aplikacji logiki w celu automatyzacji przepływów pracy i procesów za pośrednictwem witryny Azure Portal

Bez pisania kodu możesz zintegrować systemy i usługi, tworząc i uruchamiając automatyczne przepływy pracy za pomocą usługi [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). Aby pokazać, jak łatwo możesz zautomatyzować zadania za pomocą przepływu pracy, w tym samouczku zostanie utworzona podstawowa aplikacja logiki, która sprawdza źródło danych RSS pod kątem nowej zawartości w witrynie internetowej i wysyła wiadomość e-mail dla każdego nowego elementu w źródle danych. 

![Omówienie — przykład pierwszej aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-overview.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenia pustej aplikacji logiki.
> * Dodawania wyzwalacza uruchamiającego Twoją aplikację logiki, gdy zostanie opublikowany element źródła danych RSS.
> * Dodawania akcji wysyłania wiadomości e-mail ze szczegółowymi informacjami o elemencie źródła danych RSS.
> * Uruchamiania i testowania aplikacji logiki.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [rozpocząć pracę z bezpłatnym kontem platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie możesz [skorzystać z subskrypcji z płatnością zgodnie z rzeczywistym użyciem](https://azure.microsoft.com/pricing/purchase-options/).

* Konto e-mail od [dowolnego dostawcy poczty e-mail obsługiwanego przez usługę Azure Logic Apps](../connectors/apis-list.md) do wysyłania powiadomień. Na przykład możesz użyć usługi Office 365 Outlook, usługi Outlook.com, usługi Gmail lub innego obsługiwanego dostawcy. W tym samouczku jest używana usługa Office 365 Outlook.

  > [!TIP]
  > Jeśli masz osobiste [konto Microsoft](https://account.microsoft.com/account), masz także konto usługi Outlook.com. W przeciwnym razie, jeśli masz służbowe konto platformy Azure, masz konto usługi Office 365 Outlook.

* Link do źródła danych RSS witryny internetowej. W tym przykładzie użyto [źródła danych RSS z najciekawszymi wiadomościami z witryny internetowej CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="1-create-a-blank-logic-app"></a>1. Tworzenie pustej aplikacji logiki 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal").

2. Z głównego menu platformy Azure wybierz kolejno pozycje **Nowy** > **Integracja dla przedsiębiorstw** > **Aplikacja logiki**.

   ![Azure Portal, Nowy, Integracja w przedsiębiorstwie, Aplikacja logiki](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Utwórz własną aplikację logiki z ustawieniami określonymi w tabeli.

   ![Podawanie szczegółów aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *Twoja nazwa aplikacji logiki* | Podaj unikatową nazwę aplikacji logiki. | 
   | **Subskrypcja** | *Twoja subskrypcja platformy Azure* | Wybierz subskrypcję platformy Azure, której chcesz użyć. | 
   | **Grupa zasobów** | *Twoja grupa zasobów platformy Azure* | Utwórz lub wybierz grupę zasobów platformy Azure, która ułatwi organizowanie powiązanych zasobów platformy Azure i zarządzanie nimi. | 
   | **Lokalizacja** | *Twój region świadczenia usługi Azure* | Wybierz region centrum danych do wdrażania swojej aplikacji logiki. | 
   |||| 

4. Gdy wszystko będzie gotowe, wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie pozycję **Utwórz**.

   Teraz został utworzony zasób platformy Azure dla Twojej aplikacji logiki. 
   Po wdrożeniu Twojej aplikacji logiki na platformie Azure projektant aplikacji usługi Logic Apps pokaże szablony dla typowych wzorców, więc możesz szybciej rozpocząć pracę.

   > [!NOTE] 
   > Gdy wybierzesz pozycję **Przypnij do pulpitu nawigacyjnego**, Twoja aplikacja logiki pojawi się na pulpicie nawigacyjnym platformy Azure po wdrożeniu i automatycznie otworzy się w projektancie aplikacji usługi Logic Apps. Jeśli jej nie wybierzesz, możesz ręcznie znaleźć i otworzyć swoją aplikację logiki.

5. Na razie w obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**, aby rozpocząć tworzenie swojej aplikacji logiki od podstaw.

   ![Wybieranie szablonu aplikacji logiki](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Projektant aplikacji logiki wyświetla teraz dostępne [*łączniki*](../connectors/apis-list.md) i ich [*wyzwalacze*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), których używasz do uruchamiania przepływu pracy aplikacji logiki.

   ![Wyzwalacze aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="2-add-a-trigger-for-starting-the-workflow"></a>2. Dodawanie wyzwalacza do uruchamiania przepływu pracy

Każda aplikacja logiki musi rozpoczynać się od [*wyzwalacza*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Wyzwalacz jest aktywowany po wystąpieniu określonego zdarzenia lub gdy nowe dane spełnią ustawiony warunek. Następnie aparat usługi Logic Apps tworzy wystąpienie aplikacji logiki do uruchamiania Twojego przepływu pracy. Po każdym wyzwoleniu wyzwalacza aparat tworzy inne, oddzielne wystąpienie, które uruchamia przepływ pracy Twojej aplikacji logiki.

1. W polu wyszukiwania wpisz „rss” jako filtr. Wybierz następujący wyzwalacz: **RSS — Gdy element kanału informacyjnego jest publikowany** 

   ![Wybieranie wyzwalacza: „RSS — Gdy element kanału informacyjnego jest publikowany”](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Podaj link źródła danych RSS witryny internetowej, które chcesz śledzić, na przykład `http://rss.cnn.com/rss/cnn_topstories.rss`. Ustaw odstęp czasu i częstotliwość cyklu. W tym przykładzie ustaw właściwości tak, aby codziennie sprawdzać źródło danych. 

   ![Konfigurowanie źródła danych RSS, częstotliwości i interwału dla wyzwalacza](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

3. Zapisz swoją pracę. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.
Aby zwinąć i ukryć szczegóły wyzwalacza, wybierz pasek tytułu tego wyzwalacza.

   ![Zapisywanie aplikacji logiki](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Twoja aplikacja logiki teraz działa, ale nie robi nic innego oprócz sprawdzania nowych elementów w źródle danych RSS do chwili dodania akcji do przepływu pracy. 

## <a name="3-add-an-action-that-responds-to-the-trigger"></a>3. Dodawanie akcji reagującej na wyzwalacz

Teraz dodaj [*akcję*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), która jest zadaniem wykonywanym przez przepływ pracy Twojej aplikacji logiki. W tym przykładzie dodaj akcję, która wysyła wiadomość e-mail, gdy pojawi się nowy element w źródle danych RSS.

1. W projektancie aplikacji logiki, w obszarze wyzwalacza, wybierz pozycje **+ Nowy krok** > **Dodaj akcję**.

   ![Dodawanie akcji](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Projektant wyświetli [dostępne łączniki](../connectors/apis-list.md), aby umożliwić wybranie akcji, która ma zostać wykonana w momencie uruchomienia wyzwalacza.

   ![Wybieranie z listy akcji](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. W polu wyszukiwania wprowadź „wyślij wiadomość e-mail” jako filtr. W oparciu o Twojego dostawcę poczty e-mail znajdź i wybierz zgodny łącznik. Następnie wybierz akcję „Wyślij wiadomość e-mail” dla Twojego łącznika. Na przykład: 

   * Dla konta służbowego platformy Azure wybierz łącznik usługi Office 365 Outlook. 
   * Dla osobistych kont Microsoft wybierz łącznik usługi Outlook.com. 
   * Dla kont usługi Gmail wybierz łącznik usługi Gmail. 

   Będziemy kontynuować z użyciem łącznika usługi Office 365 Outlook. 
   Jeśli używasz innego dostawcy, kroki pozostają takie same, ale Twój interfejs użytkownika może wyglądać inaczej. 

   ![Wybieranie akcji: „Office 365 Outlook — Wyślij wiadomość e-mail”](./media/logic-apps-create-a-logic-app/actions.png)

3. Gdy zostanie wyświetlony monit o podanie poświadczeń, zaloguj się przy użyciu nazwy użytkownika i hasła swojego konta e-mail. 

4. Podaj szczegóły określone w tabeli, a następnie wybierz pola, które chcesz włączyć do wiadomości e-mail.

   | Do | Kroki | 
   | -- | ----- | 
   | Wybierz pola dostępne dla Twojego przepływu pracy. | Kliknij wewnątrz pola edycji tak, aby została otwarta lista **Zawartość dynamiczna**, lub wybierz pozycję **Dodaj zawartość dynamiczną**. | 
   | Wyświetl inne dostępne pola. | Z listy **Zawartość dynamiczna** wybierz pozycję **Zobacz więcej** dla każdej sekcji.  | 
   | Dodaj puste wiersze w polu edycji. | Naciśnij klawisze Shift + Enter. | 
   | Zamknij listę **Zawartość dynamiczna**. | Ponownie wybierz pozycję **Dodaj zawartość dynamiczną**. | 
   ||| 

   ![Wybieranie danych, które mają być uwzględnione w wiadomości e-mail](./media/logic-apps-create-a-logic-app/rss-action-setup.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Do** | *adres e-mail adresata* | Wprowadź adres e-mail adresata. Do celów testowych możesz użyć własnego adresu e-mail. | 
   | **Temat** | Nowy wpis CNN: **Tytuł źródła danych** | Wprowadź zawartość w polu tematu wiadomości e-mail. <p>W tym samouczku wprowadź sugerowany tekst, a następnie wybierz pole wyzwalacza **Tytuł źródła danych**, które wyświetla tytuł elementu źródła danych. | 
   | **Treść** | Tytuł: **Tytuł źródła danych** <p>Data opublikowania: **Podstawowy link źródła danych** <p>Link: **Link podstawowego źródła danych** | Wprowadź zawartość w polu treści wiadomości e-mail. <p>W tym samouczku wprowadź sugerowany tekst, a następnie wybierz następujące pola wyzwalacza: <p>- **Tytuł źródła danych**, które ponownie wyświetla tytuł elementu źródła danych </br>- **Data opublikowania źródła danych**, które wyświetla datę i godzinę opublikowania elementu </br>- **Link podstawowego źródła danych**, które wyświetla adres URL elementu źródła danych | 
   |||| 

   > [!NOTE] 
   > Jeśli wybierzesz pole przechowujące tablicę, projektant automatycznie dodaje pętlę „Dla każdego” wokół akcji, która odwołuje się do tablicy. W ten sposób Twoja aplikacja logiki wykonuje tę akcję dla każdego elementu tablicy.

5. Gdy wszystko będzie gotowe, zapisz zmiany. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   ![Ukończona aplikacja logiki](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

   Aby teraz przetestować Twoją aplikację logiki, przejdź do następnej sekcji.

## <a name="4-run-and-test-your-workflow"></a>4. Uruchamianie i testowanie aplikacji logiki

1. Aby ręcznie uruchomić swoją aplikację logiki w celu przetestowania, na pasku narzędzi projektanta wybierz pozycję **Uruchom**. Możesz też zezwolić aplikacji logiki na sprawdzanie określonego źródła danych RSS na podstawie skonfigurowanego przez siebie harmonogramu.

   ![Uruchamianie aplikacji logiki](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

   Jeśli Twoja aplikacja logiki znajdzie nowe elementy, wyśle wiadomość e-mail zawierającą wybrane przez Ciebie dane, na przykład:

   ![Wysyłanie wiadomości e-mail dla nowego elementu źródła danych RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

   Jeśli Twoja aplikacja logiki nie odnajdzie żadnych nowych elementów, aplikacja logiki pomija akcję, która wysyła wiadomość e-mail, i czeka na następny odstęp czasu przed ponownym sprawdzeniem. 

2. Aby sprawdzać historię wyzwalania i uruchamiania Twojej aplikacji logiki, w menu aplikacji logiki wybierz pozycję **Przegląd**.
Aby wyświetlić więcej szczegółów na temat uruchomienia, wybierz wiersz dla tego uruchomienia.

   ![Monitorowanie i wyświetlanie historii wyzwalania oraz uruchamiania aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Jeśli nie widzisz danych, których oczekujesz, spróbuj wybrać pozycję **Odśwież** na pasku narzędzi.

   Bez względu na to, czy uruchomienie zakończyło się powodzeniem, czy też niepowodzeniem, widok szczegółów uruchomienia przedstawia kroki, które się udały lub nie udały. 

   ![Wyświetlanie szczegółów uruchomienia aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-run-details.png)

   Aby dowiedzieć się więcej na temat stanu swojej aplikacji logiki, historii uruchamiania i historii wyzwalania lub aby zdiagnozować aplikację logiki, zobacz [Rozwiązywanie problemów z aplikacją logiki](../logic-apps/logic-apps-diagnosing-failures.md).

3. Aby wyświetlić dane wejściowe i wyjściowe dla każdego kroku, rozwiń krok, który chcesz przejrzeć. Te informacje ułatwiają diagnozowanie i debugowanie problemów w aplikacji logiki. Na przykład:

   ![Wyświetlanie szczegółów kroku](./media/logic-apps-create-a-logic-app/logic-app-run-details-expanded.png)

   Aby uzyskać więcej informacji, zobacz [Monitorowanie aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gratulacje, udało Ci się teraz utworzyć i uruchomić swoją pierwszą podstawową aplikację logiki. Ten przykład pokazuje, jak łatwo możesz tworzyć przepływy pracy automatyzujące procesy integrowania systemów i usług — a wszystko to bez konieczności pisania kodu.

> [!NOTE]
> Aplikacja logiki będzie działać do momentu jej wyłączenia. Aby tymczasowo wyłączyć swoją aplikację, należy przejść do następnej sekcji.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Ten samouczek używa zasobów i wykonuje akcje, które mogą spowodować naliczenie opłat w ramach Twojej subskrypcji platformy Azure. Gdy ukończysz pracę z samouczkiem i testowanie, upewnij się, że zostały wyłączone lub usunięte wszelkie zasoby, dla których nie chcesz naliczenia opłat.

Możesz zapobiec uruchamianiu aplikacji logiki i wysyłaniu przez nią wiadomości e-mail bez usuwania aplikacji. W menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku narzędzi wybierz pozycję **Wyłącz**.

![Wyłączanie aplikacji logiki](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Często zadawane pytania

**P:** Jakie inne możliwości oferuje moja aplikacja logiki? </br>
**O:** Istnieją inne zadania, które możesz wykonać, na przykład edycja, wyświetlanie definicji JSON, przeglądanie dziennika aktywności lub usuwanie własnej aplikacji logiki.

Aby znaleźć inne zadania zarządzania aplikacją logiki, przejrzyj te polecenia w menu aplikacji logiki:

| Zadanie | Kroki | 
| ---- | ----- | 
| Wyświetlenie stanu aplikacji, historii uruchomień i wyzwalania oraz informacji ogólnych | Wybierz pozycję **Przegląd**. | 
| Edytowanie aplikacji | Wybierz pozycję **Projektant aplikacji logiki**. | 
| Wyświetlenie definicji kodu JSON przepływu pracy aplikacji | Wybierz pozycję **Widok kodu aplikacji logiki**. | 
| Wyświetlenie operacji wykonywanych względem aplikacji logiki | Wybierz pozycję **Dziennik aktywności**. | 
| Wyświetlenie wcześniejszych wersji aplikacji logiki | Wybierz pozycję **Wersje**. | 
| Tymczasowe wyłączenie aplikacji | Wybierz pozycję **Przegląd**, a następnie na pasku narzędzi wybierz pozycję **Wyłącz**. | 
| Usunięcie aplikacji | Wybierz pozycję **Przegląd**, a następnie na pasku narzędzi wybierz pozycję **Usuń**. Wprowadź nazwę aplikacji logiki, a następnie wybierz pozycję **Usuń**. | 
||| 

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania dotyczące usługi Azure Logic Apps, odwiedź stronę [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Czy chcesz pomóc ulepszyć usługę Azure Logic Apps i łączniki? Zagłosuj lub prześlij pomysły do [witryny usługi Azure Logic Apps User Voice](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Następne kroki

* [Tworzenie aplikacji logiki za pomocą programu Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Dodawanie warunków i uruchamianie przepływów pracy](../logic-apps/logic-apps-use-logic-app-features.md)
*   [Szablony aplikacji logiki](../logic-apps/logic-apps-use-logic-app-templates.md)
* [Tworzenie aplikacji logiki z szablonów usługi Azure Resource Manager](../logic-apps/logic-apps-arm-provision.md)
* [Pomiar użycia usługi Logic Apps](../logic-apps/logic-apps-pricing.md) 
* [Ceny usługi Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps)
