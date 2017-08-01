---
title: "Tworzenie pierwszego przepływu pracy między aplikacjami w chmurze a usługami w chmurze — Azure Logic Apps | Microsoft Docs"
description: "Automatyzacja procesów biznesowych na potrzeby scenariuszy integracji systemów i usługi Enterprise Application Integration (EAI) przez tworzenie i uruchamianie przepływów pracy w usłudze Azure Logic Apps"
author: jeffhollan
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
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.contentlocale: pl-pl
ms.lasthandoff: 05/17/2017

---

# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a>Tworzenie pierwszego przepływu pracy aplikacji logiki w celu automatyzacji procesów między aplikacjami w chmurze a usługami w chmurze

Procesy biznesowe możesz zautomatyzować bez konieczności pisania kodu łatwiej i szybciej, jeśli utworzysz i uruchomisz przepływy pracy za pomocą usługi [Azure Logic Apps](logic-apps-what-are-logic-apps.md). W pierwszym przykładzie przedstawiono tworzenie przepływu pracy podstawowej aplikacji logiki, który sprawdza kanał informacyjny RSS w poszukiwaniu nowej zawartości w witrynie sieci Web. Gdy w kanale informacyjnym witryny sieci Web pojawi się nowy element, aplikacja logiki wysyła wiadomość e-mail z konta programu Outlook lub usługi Gmail.

Do utworzenia i uruchomienia aplikacji logiki potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [rozpocząć pracę z bezpłatnym kontem platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie możesz [skorzystać z subskrypcji z płatnością zgodnie z rzeczywistym użyciem](https://azure.microsoft.com/pricing/purchase-options/).

  Subskrypcja platformy Azure jest używana do rozliczania użycia aplikacji logiki. Dowiedz się, jak działają [pomiary użycia](../logic-apps/logic-apps-pricing.md) i jak wyglądają [ceny](https://azure.microsoft.com/pricing/details/logic-apps) w usłudze Azure Logic Apps.

Przykład wymaga także następujących elementów:

* Konto usługi Outlook.com, Office 365 Outlook lub Gmail

    > [!TIP]
    > Jeśli masz osobiste [konto Microsoft](https://account.microsoft.com/account), masz także konto usługi Outlook.com. W przeciwnym razie, jeśli masz służbowe konto platformy Azure, masz konto usługi **Office 365 Outlook**.

* Link do kanału informacyjnego RSS witryny sieci Web. W tym przykładzie użyto [kanału informacyjnego RSS z najciekawszymi wiadomościami z witryny internetowej CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Dodawanie wyzwalacza uruchamiającego przepływ pracy

[*Wyzwalacz*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) jest zdarzeniem uruchamiającym przepływ pracy aplikacji logiki i jest pierwszym elementem, którego potrzebuje aplikacja logiki.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal").

2. Z menu po lewej stronie wybierz kolejno pozycje **Nowy** > **Integracja w przedsiębiorstwie** > **Aplikacja logiki**, jak pokazano tutaj:

     ![Azure Portal, Nowy, Integracja w przedsiębiorstwie, Aplikacja logiki](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Możesz również wybrać pozycję **Nowy**, w polu wyszukiwania wpisać `logic app` i nacisnąć klawisz Enter. Następnie wybierz kolejno pozycje **Aplikacja logiki** > **Utwórz**.

3. Nadaj nazwę aplikacji logiki i wybierz subskrypcję platformy Azure. Teraz utwórz lub wybierz grupę zasobów platformy Azure, która ułatwi organizowanie powiązanych zasobów platformy Azure i zarządzanie nimi. Na koniec wybierz lokalizację centrum danych do hostowania aplikacji logiki. Gdy wszystko będzie gotowe, wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie pozycję **Utwórz**.

     ![Szczegóły aplikacji logiki](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Gdy wybierzesz pozycję **Przypnij do pulpitu nawigacyjnego**, aplikacja logiki zostanie wyświetlona na pulpicie nawigacyjnym platformy Azure po wdrożeniu i automatycznie otwarta. Jeśli aplikacja logiki nie zostanie wyświetlona na pulpicie nawigacyjnym, na kafelku **Wszystkie zasoby** wybierz pozycję **Zobacz więcej**, a następnie wybierz swoją aplikację logiki. Alternatywnie w menu po lewej stronie wybierz pozycję **Więcej usług**. W obszarze **Integracja w przedsiębiorstwie** wybierz pozycję **Aplikacje logiki** i wybierz swoją aplikację logiki.

4. Po pierwszym uruchomieniu aplikacji logiki Projektant aplikacji logiki wyświetli szablony, przy użyciu których można rozpocząć pracę. Na razie wybierz pozycję **Pusta aplikacja logiki**, aby rozpocząć tworzenie aplikacji logiki od podstaw.

    Projektant aplikacji logiki otworzy i wyświetli dostępne usługi i *wyzwalacze*, których można używać w aplikacji logiki.

5. W polu wyszukiwania wpisz `RSS`, a następnie wybierz następujący wyzwalacz: **RSS — Gdy element kanału informacyjnego jest publikowany** 

    ![Wyzwalacz kanału informacyjnego RSS](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Wprowadź link kanału informacyjnego RSS witryny sieci Web, który chcesz śledzić. 

     Dodatkowo możesz zmienić **Częstotliwość** i **Interwał**. 
     Te ustawienia umożliwiają określenie, jak często aplikacja logiki ma sprawdzać istnienie nowych elementów i zwracać wszystkie elementy z tego okresu.

     W tym przykładzie będziemy każdego dnia sprawdzać najciekawsze artykuły opublikowane w witrynie internetowej CNN.

     ![Konfigurowanie kanału informacyjnego RSS, częstotliwości i interwału dla wyzwalacza](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Zapisz swoją pracę. (Na pasku poleceń projektanta wybierz pozycję **Zapisz**).

   ![Zapisywanie aplikacji logiki](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Po zapisaniu aplikacja zostanie udostępniona, ale obecnie aplikacja logiki sprawdza istnienie nowych elementów tylko w określonym kanale informacyjnym RSS. 
   Aby ten przykład był bardziej użyteczny, dodamy akcję, którą aplikacja logiki wykona po uruchomieniu wyzwalacza.

## <a name="add-an-action-that-responds-to-your-trigger"></a>Dodawanie akcji reagującej na wyzwalacz

[*Akcja*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) to zadanie wykonywane przez przepływ pracy aplikacji logiki. Po dodaniu wyzwalacza do aplikacji logiki możesz dodać akcję, aby wykonywać operacje na danych generowanych przez ten wyzwalacz. W tym przykładzie dodamy akcję wysyłającą wiadomość e-mail, gdy w kanale informacyjnym RSS witryny sieci Web pojawi się nowy element.

1. W projektancie pod wyzwalaczem wybierz pozycję **Nowy krok** > **Dodaj akcję**, jak pokazano tutaj:

   ![Dodawanie akcji](media/logic-apps-create-a-logic-app/add-new-action.png)

   Projektant wyświetli [dostępne łączniki](../connectors/apis-list.md), aby umożliwić wybranie akcji, która ma zostać wykonana w momencie uruchomienia wyzwalacza.

2. W zależności od posiadanego konta e-mail wykonaj czynności dla programu Outlook lub usługi Gmail.

   * Aby wysłać wiadomość e-mail z konta programu Outlook, w polu wyszukiwania wpisz `outlook`. W obszarze **Usługi** wybierz pozycję **Outlook.com** w przypadku osobistych kont Microsoft lub pozycję **Office 365 Outlook** w przypadku kont służbowych platformy Azure. 
   W obszarze **Akcje** wybierz pozycję **Wyślij wiadomość e-mail**.

       ![Wybieranie akcji programu Outlook „Wyślij wiadomość e-mail”](media/logic-apps-create-a-logic-app/actions.png)

   * Aby wysłać wiadomość e-mail z konta usługi Gmail, w polu wyszukiwania wpisz `gmail`. 
   W obszarze **Akcje** wybierz pozycję **Wyślij wiadomość e-mail**.

       ![Wybieranie pozycji „Gmail — Wyślij wiadomość e-mail”](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Gdy zostanie wyświetlony monit o podanie poświadczeń, zaloguj się przy użyciu nazwy użytkownika i hasła swojego konta e-mail. 

4. Podaj szczegóły dla tej akcji, takie jak docelowy adres e-mail, i wybierz parametry danych, które mają być uwzględnione w wiadomości, na przykład:

   ![Wybieranie danych, które mają być uwzględnione w wiadomości e-mail](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Jeśli wybrano program Outlook, aplikacja logiki może wyglądać jak w tym przykładzie:

    ![Ukończona aplikacja logiki](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.    Zapisz zmiany. (Na pasku poleceń projektanta wybierz pozycję **Zapisz**).

6. Teraz możesz ręcznie uruchomić aplikację logiki na potrzeby testowania. Na pasku poleceń projektanta wybierz pozycję **Uruchom**. W innym przypadku możesz zezwolić aplikacji logiki na sprawdzanie określonego kanału informacyjnego RSS na podstawie skonfigurowanego harmonogramu.

   Jeśli aplikacja logiki znajdzie nowe elementy, wyśle wiadomość e-mail zawierającą wybrane dane. 
   Jeśli nie zostaną znalezione żadne nowe elementy, aplikacja logiki pominie akcję wysyłania wiadomości e-mail.

7. Aby monitorować i sprawdzać historię wyzwalania i uruchamiania aplikacji logiki, w menu aplikacji logiki wybierz pozycję **Przegląd**.

   ![Monitorowanie i wyświetlanie historii wyzwalania oraz uruchamiania aplikacji logiki](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Jeśli nie widzisz danych, których oczekujesz, spróbuj wybrać pozycję **Odśwież** na pasku poleceń.

   Aby dowiedzieć się więcej na temat stanu aplikacji logiki bądź historii wyzwalania i uruchamiania lub aby zdiagnozować aplikację logiki, zobacz [Rozwiązywanie problemów z aplikacją logiki](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Aplikacja logiki będzie działać do momentu jej wyłączenia. Aby na razie wyłączyć aplikację, w menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku poleceń wybierz pozycję **Wyłącz**.

Gratulacje, udało Ci się skonfigurować i uruchomić Twoją pierwszą podstawową aplikację logiki. Wiesz już, jak łatwe jest tworzenie przepływów pracy automatyzujących procesy oraz integrowanie aplikacji w chmurze i usług w chmurze — a wszystko to bez konieczności pisania kodu.

## <a name="manage-your-logic-app"></a>Zarządzanie aplikacją logiki

Aby zarządzać aplikacją logiki, możesz wykonywać zadania takie jak sprawdzanie stanu, edytowanie, wyświetlanie historii, wyłączanie lub usuwanie aplikacji logiki.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal").

2. W menu po lewej stronie wybierz pozycję **Więcej usług**. W obszarze **Integracja w przedsiębiorstwie** wybierz pozycję **Logic Apps**. Wybierz aplikację logiki. 

   W menu aplikacji logiki są dostępne następujące zadania zarządzania:

   |Zadanie|Kroki| 
   |:---|:---| 
   | Wyświetlenie stanu aplikacji, historii wykonywania i informacji ogólnych| Wybierz pozycję **Przegląd**.| 
   | Edytowanie aplikacji | Wybierz pozycję **Projektant aplikacji logiki**. | 
   | Wyświetlenie definicji kodu JSON przepływu pracy aplikacji | Wybierz pozycję **Widok kodu aplikacji logiki**. | 
   | Wyświetlenie operacji wykonywanych względem aplikacji logiki | Wybierz pozycję **Dziennik aktywności**. | 
   | Wyświetlenie wcześniejszych wersji aplikacji logiki | Wybierz pozycję **Wersje**. | 
   | Tymczasowe wyłączenie aplikacji | Wybierz pozycję **Przegląd**, a następnie na pasku poleceń wybierz pozycję **Wyłącz**. | 
   | Usunięcie aplikacji | Wybierz pozycję **Przegląd**, a następnie na pasku poleceń wybierz pozycję **Usuń**. Wprowadź nazwę aplikacji logiki, a następnie wybierz pozycję **Usuń**. | 

## <a name="get-help"></a>Uzyskiwanie pomocy

Aby zadawać pytania, odpowiadać na nie i patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Aby pomóc w ulepszaniu usługi Azure Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Następne kroki

*  [Dodawanie warunków i uruchamianie przepływów pracy](../logic-apps/logic-apps-use-logic-app-features.md)
*     [Szablony aplikacji logiki](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Tworzenie aplikacji logiki z szablonów usługi Azure Resource Manager](../logic-apps/logic-apps-arm-provision.md)

