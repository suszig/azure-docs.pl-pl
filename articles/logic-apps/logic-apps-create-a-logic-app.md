---
title: "Automatyzowanie przepływów pracy między systemami i usługami w chmurze — Azure Logic Apps | Microsoft Docs"
description: "Tworzenie aplikacji logiki w celu zautomatyzowania przepływów pracy na potrzeby scenariuszy integracji systemów i usługi Enterprise Application Integration (EAI)"
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
ms.date: 10/20/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 5906605192f9b03f612e6ca3a445434a23713d7f
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2017
---
# <a name="automate-your-first-workflow-to-process-data-with-a-logic-app"></a>Automatyzowanie pierwszego przepływu pracy do przetwarzania danych za pomocą aplikacji logiki

Aby szybciej zintegrować systemy i usługi w organizacji, możesz zautomatyzować przepływy pracy i procesy biznesowe za pomocą usługi [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md). W tym przewodniku Szybki start pokazano, jak łatwo można utworzyć i uruchomić zautomatyzowany przepływ pracy, tworząc aplikację logiki. Przykładowa aplikacja demonstruje, jak zautomatyzować przepływ pracy sprawdzający źródło danych RSS pod kątem nowych elementów i wysyłający wiadomość e-mail dla każdego z elementów.

Ta przykładowa aplikacja logiki wysyła wiadomość e-mail taką jak w tym przykładzie:

![Wysyłanie wiadomości e-mail dla nowego elementu źródła danych RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

A oto przepływ pracy wysokiego poziomu w tworzonej aplikacji logiki:

![Omówienie — przykład aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-simple-overview.png)

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenia pustej aplikacji logiki.
> * Dodaj wyzwalacz, który będzie uruchamiać przepływ pracy po pojawieniu się nowego elementu w źródle danych RSS.
> * Dodaj akcję wysyłania wiadomości e-mail z informacjami o elemencie źródła danych RSS.
> * Uruchom przepływ pracy aplikacji logiki.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [zarejestruj się w celu założenia bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

* Konto e-mail od dowolnego dostawcy poczty e-mail obsługiwanego przez usługę Azure Logic Apps do wysyłania powiadomień. Na przykład możesz użyć usługi Office 365 Outlook, Outlook.com lub Gmail. Inne obsługiwane łączniki poczty e-mail znajdziesz [na liście łączników](https://docs.microsoft.com/connectors/). Ten przewodnik Szybki start korzysta z usługi Office 365 Outlook.

  > [!TIP]
  > Jeśli masz osobiste [konto Microsoft](https://account.microsoft.com/account), masz także konto usługi Outlook.com. W przeciwnym razie, jeśli masz służbowe konto platformy Azure, masz konto usługi Office 365 Outlook.

* Link do kanału informacyjnego RSS witryny sieci Web. W tym przykładzie użyto [źródła danych RSS z najciekawszymi wiadomościami z witryny internetowej Reuters](http://feeds.reuters.com/reuters/topNews): `http://feeds.reuters.com/reuters/topNews`

Ten przewodnik Szybki start nie wymaga pisania kodu, ale usługa Logic Apps obsługuje inne scenariusze z kodem, na przykład uruchamianie własnego kodu z aplikacji logiki za pomocą usługi [Azure Functions](../azure-functions/functions-overview.md).

## <a name="create-a-blank-logic-app"></a>Tworzenie pustej aplikacji logiki 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal"). 

2. Z głównego menu platformy Azure wybierz kolejno pozycje **Nowy** > **Integracja dla przedsiębiorstw** > **Aplikacja logiki**.

   ![Azure Portal, Nowy, Integracja w przedsiębiorstwie, Aplikacja logiki](./media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Utwórz własną aplikację logiki z ustawieniami podanymi w tabeli pod tą ilustracją:

   ![Podawanie szczegółów aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *Twoja nazwa aplikacji logiki* | Podaj unikatową nazwę aplikacji logiki. | 
   | **Subskrypcja** | *nazwa_Twojej_susbkrypcji_Azure* | Wybierz subskrypcję platformy Azure, której chcesz użyć. | 
   | **Grupa zasobów** | *nazwa_Twojej_grupy_zasobów_Azure* | Utwórz [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) dla tej aplikacji logiki i zorganizuj wszystkie zasoby skojarzone z tą aplikacją. | 
   | **Lokalizacja** | *region_Twojego_centrum_danych_Azure* | Wybierz region centrum danych do wdrożenia swojej aplikacji logiki, na przykład Zachodnie stany USA. | 
   | **Log Analytics** | Wyłączone | Włącz rejestrowania diagnostyczne dla aplikacji logiki, ale dla tego przewodnika Szybki start zostaw ustawienie **Wyłączone**. | 
   |||| 

4. Gdy wszystko będzie gotowe, wybierz pozycję **Przypnij do pulpitu nawigacyjnego**. Dzięki temu aplikacja logiki automatycznie pojawi się na pulpicie nawigacyjnym platformy Azure i zostanie otwarta po wdrożeniu. Wybierz pozycję **Utwórz**.

   > [!NOTE]
   > Jeśli nie chcesz przypinać aplikacji logiki, to aby można było kontynuować, musisz ją ręcznie znaleźć i otworzyć po wdrożeniu.

   Gdy platforma Azure wdroży aplikację logiki, zostanie otwarty Projektant aplikacji usługi Logic Apps wyświetlający stronę z wprowadzającym wideo. 
   Pod wideo znajdziesz szablony typowych wzorców aplikacji logiki. 
   W tym przewodniku Szybki start aplikacja logiki zostanie utworzona od zera. 

5. Przewiń ekran za wprowadzające wideo i typowe wyzwalacze. W obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**.

   ![Wybieranie szablonu pustej aplikacji logiki](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Projektant aplikacji logiki wyświetli dostępne łączniki i ich wyzwalacze, które służą do uruchamiania przepływów pracy aplikacji logiki.

   ![Wyzwalacze aplikacji logiki](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="add-a-trigger-to-detect-new-items"></a>Dodawanie wyzwalacza do wykrywania nowych elementów

Każdy przepływ pracy aplikacji logiki jest uruchamiany przez [wyzwalacz](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Wyzwalacz jest aktywowany po wystąpieniu określonego zdarzenia lub gdy nowe dane spełnią ustawiony warunek. Po każdym aktywowaniu wyzwalacza aparat usługi Logic Apps tworzy wystąpienie aplikacji logiki uruchamiającej i wykonującej przepływ pracy.

1. W polu wyszukiwania jako filtr wprowadź „rss”. Wybierz następujący wyzwalacz: **RSS — Gdy element kanału informacyjnego jest publikowany** 

   ![Wybieranie wyzwalacza: „RSS — Gdy element kanału informacyjnego jest publikowany”](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Podaj link do źródła danych RSS, które chcesz monitorować, na przykład `http://feeds.reuters.com/reuters/topNews`. Ustaw odstęp czasu i częstotliwość cyklu. W tym przykładzie źródło danych jest sprawdzane co pięć minut.

   ![Konfigurowanie kanału informacyjnego RSS, częstotliwości i interwału dla wyzwalacza](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

   Usługa Logic Apps utworzy połączenie ze źródłem danych RSS.

   > [!TIP]
   > Aby uprościć widok w projektancie, możesz zwinąć i ukryć szczegóły kształtu — wystarczy kliknąć wewnątrz paska tytułu kształtu.

3. Zapisz pracę. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**. 

   ![Zapisywanie aplikacji logiki](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Twoja aplikacja logiki jest już aktywna, ale nie robi niczego poza sprawdzaniem źródła danych RSS. Dodajmy akcję reagującą na aktywowanie wyzwalacza.

## <a name="add-an-action-to-send-email"></a>Dodawanie akcji wysyłającej wiadomość e-mail

Teraz, po utworzeniu wyzwalacza, dodaj [akcję](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) wysyłającą wiadomość e-mail po pojawieniu się nowego elementu w źródle danych RSS. Przepływ pracy wykonuje tę akcję po aktywowaniu wyzwalacza.

1. W projektancie aplikacji logiki, w obszarze wyzwalacza, wybierz pozycje **+ Nowy krok** > **Dodaj akcję**.

   ![Dodawanie akcji](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Projektant pokazuje akcje, które aplikacja logiki może wykonać po aktywowaniu wyzwalacza.

   ![Wybieranie z listy akcji](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. W polu wyszukiwania wprowadź „wyślij wiadomość e-mail” jako filtr. Znajdź i wybierz łącznik poczty e-mail, którego chcesz użyć. Następnie wybierz akcję „Wyślij wiadomość e-mail” dla tego łącznika. Na przykład: 

   * Dla kont służbowych platformy Azure wybierz pozycję Office 365 Outlook. 
   * Dla osobistych kont Microsoft wybierz pozycję Outlook.com. 
   * Dla kont usługi Gmail wybierz pozycję Gmail. 

   Ten przewodnik Szybki start korzysta z usługi Office 365 Outlook. 
   Jeśli używasz innego dostawcy poczty e-mail, kroki pozostają takie same, ale Twój interfejs użytkownika może wyglądać inaczej. 

   ![Wybieranie akcji: „Office 365 Outlook — Wyślij wiadomość e-mail”](./media/logic-apps-create-a-logic-app/actions.png)

3. Gdy zostanie wyświetlony monit o podanie poświadczeń, zaloguj się przy użyciu nazwy użytkownika i hasła swojego konta e-mail. 

   Usługa Logic Apps utworzy połączenie z kontem e-mail.

4. Teraz określ dane, które chcesz uwzględnić w wiadomości e-mail. 

   1. W polu **Do** wprowadź adres e-mail adresata. 
   Do celów testowych możesz użyć własnego adresu e-mail.

   2. W polu **Temat** wprowadź temat wiadomości e-mail. 
   Dla tego przykładu możesz wprowadzić „New RSS item:”, tak jak pokazano:

      ![Wprowadzanie tematu wiadomości e-mail](./media/logic-apps-create-a-logic-app/logic-app-add-subject.png)

      Po kliknięciu wewnątrz pola edycji zostanie otwarta **lista Dodaj zawartość dynamiczną**, z której można wybrać dostępne pola danych do uwzględnienia w akcji. 
      Jeśli lista zawartości dynamicznej nie zostanie otwarta, pod odpowiednim polem edycji wybierz pozycję **Dodaj zawartość dynamiczną**.

   3. Z listy **Dodaj zawartość dynamiczną** wybierz pozycję **Tytuł źródła danych**, która obejmuje tytuł elementu w wiadomości e-mail.

      ![Wprowadzanie tematu wiadomości e-mail](./media/logic-apps-create-a-logic-app/logic-app-select-field.png)

      Gdy wszystko będzie gotowe, temat wiadomości e-mail będzie wyglądać następująco:

      ![Dodany tytuł źródła danych](./media/logic-apps-create-a-logic-app/added-feed-title.png)

      > [!NOTE] 
      > Jeśli wybierzesz pole zawierające tablicę, taką jak **categories-item**, projektant automatycznie dodaje pętlę „For each” wokół akcji, która odwołuje się do tego pola. Dzięki temu Twoja aplikacja logiki może wykonać tę akcję dla każdego elementu tablicy. 
      > 
      > Aby usunąć pętlę, wybierz wielokropek (**...** ) na pasku tytułu pętli, a następnie wybierz pozycję **Usuń**.

   4. W polu **Treść** wprowadź treść wiadomości e-mail. 
   Dla tego przykładu wprowadź następujący tekst i wybierz te pola:

      ![Dodawanie treści wiadomości e-mail](./media/logic-apps-create-a-logic-app/logic-app-complete.png)

      | Pole | Opis | 
      | ----- | ----------- | 
      | **Tytuł źródła danych** | Pokazuje tytuł elementu. | 
      | **Data opublikowania kanału informacyjnego** | Pokazuje datę i godzinę opublikowania elementu. | 
      | **Link podstawowego źródła danych** | Pokazuje adres URL elementu. | 
      ||| 

      > [!TIP]
      > Aby dodać puste wiersze w polu edycji, naciśnij klawisze Shift + Enter. 
      
5. Zapisz pracę. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

   ![Ukończona aplikacja logiki](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

## <a name="run-your-logic-app-workflow"></a>Uruchamianie przepływu pracy aplikacji logiki.

Aby ręcznie uruchomić swoją aplikację logiki, na pasku narzędzi projektanta wybierz pozycję **Uruchom**. Zamiast tego możesz też poczekać na uruchomienie aplikacji logiki zgodnie ze skonfigurowanym harmonogramem.

![Uruchamianie aplikacji logiki](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

Jeśli źródło danych RSS będzie zawierać nowe elementy, aplikacja logiki wyśle wiadomość e-mail dla każdego nowego elementu. Oto przykładowa wiadomość e-mail w usłudze Outlook wysłana przez tę aplikację logiki:

![Wysyłanie wiadomości e-mail dla nowego elementu źródła danych RSS](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Jeśli źródło danych nie będzie zawierać żadnych nowych elementów, aplikacja logiki pominie krok wysyłania wiadomości e-mail i przeczeka kolejny odstęp czasu przed ponownym sprawdzeniem. 

> [!TIP]
> Jeśli nie dostaniesz żadnych wiadomości e-mail, sprawdź folder wiadomości-śmieci. W przeciwnym razie, jeśli nie masz pewności, czy aplikacja logiki została poprawnie uruchomiona, zobacz [Troubleshoot your logic app (Rozwiązywania problemów z aplikacją logiki)](../logic-apps/logic-apps-diagnosing-failures.md).

Gratulacje, udało Ci się utworzyć i uruchomić swoją pierwszą aplikację logiki. W tym przewodniku Szybki start pokazano, jak łatwo i szybko można tworzyć zautomatyzowane przepływy pracy do integrowania systemów i usług.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aplikacja logiki będzie nadal działać i potencjalnie powodować naliczanie opłat w ramach subskrypcji platformy Azure, dopóki nie zostanie wyłączona lub usunięta. Dodatkowo połączenia utworzone dla aplikacji logiki pozostają, nawet gdy aplikacja logiki zostanie usunięta. 

Gdy ukończysz pracę, upewnij się, że zostały wyłączone lub usunięte wszelkie zasoby, za które nie chcesz ponosić opłat i których nie chcesz zachować. Aby usunąć wszystkie zasoby utworzone dla tego przewodnika Szybki start, usuń grupę zasobów platformy Azure, która została utworzona dla tej aplikacji logiki. 

### <a name="delete-resource-group"></a>Usuwanie grupy zasobów

Jeśli nie chcesz zachować niczego związanego z aplikacją logiki, usuń grupę zasobów utworzoną dla tego przewodnika Szybki start i wszystkie powiązane zasoby. Dowiedz się więcej o tym, [jak zarządzać grupami zasobów platformy Azure](../azure-resource-manager/resource-group-portal.md#manage-resources).

1. Z menu Azure wybierz pozycję **Grupy zasobów**.

2. Wybierz grupę zasobów, którą chcesz usunąć. Z menu grupy zasobów wybierz pozycję **Przegląd**, jeśli jeszcze nie została wybrana. 

3. Przejrzyj wszystkie zasoby w grupie, którą chcesz usunąć. Gdy skończysz, wybierz pozycję **Usuń grupę zasobów** na pasku narzędzi grupy zasobów.

### <a name="turn-off-logic-app"></a>Wyłączanie aplikacji logiki

Aby zatrzymać aplikację logiki bez usuwania swojej pracy, wyłącz ją. 

W menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku narzędzi wybierz pozycję **Wyłącz**.

  ![Wyłączanie aplikacji logiki](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Jeśli nie widzisz menu aplikacji logiki, spróbuj wrócić do pulpitu nawigacyjnego platformy Azure i ponownie otworzyć aplikację logiki.

### <a name="delete-logic-app"></a>Usuwanie aplikacji logiki

Możesz usunąć samą aplikację logiki, zachowując wszystkie inne powiązane zasoby, takie jak utworzone połączenia.

1. Z menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku narzędzi wybierz pozycję **Usuń**. 

   ![Usuwanie aplikacji logiki](./media/logic-apps-create-a-logic-app/delete-logic-app.png)

   > [!TIP]
   > Jeśli nie widzisz menu aplikacji logiki, spróbuj wrócić do pulpitu nawigacyjnego platformy Azure i ponownie otworzyć aplikację logiki.

2. Potwierdź, że chcesz usunąć aplikację logiki, a następnie wybierz pozycję **Usuń**.

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać pomysły dotyczące funkcji lub zagłosować na nie, odwiedź [witrynę opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie przepływów pracy aplikacji logiki na podstawie wbudowanych szablonów](../logic-apps/logic-apps-create-logic-apps-from-templates.md)