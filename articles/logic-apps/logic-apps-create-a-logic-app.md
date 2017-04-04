---
title: "Tworzenie przepływów pracy przy użyciu pierwszej aplikacji Azure Logic App | Microsoft Docs"
description: "Wprowadzenie do łączenia aplikacji i usług SaaS z pierwszą aplikacją logiki"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 221f1b9f0985bbaf0553f6ca01f0f048b9976315
ms.lasthandoff: 03/28/2017


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Tworzenie nowej aplikacji logiki łączącej usługi SaaS
W tym temacie pokazano, jak w ciągu zaledwie kilku minut rozpocząć korzystanie z usługi [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Zawiera on omówienie prostego przepływu pracy, który pozwala wysyłać interesujące tweety na adres e-mail.

Aby użyć tego scenariusza, potrzebne są:

* Subskrypcja platformy Azure
* Konto w serwisie Twitter
* Konto usługi Outlook.com lub Office 365 Outlook

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Tworzenie nowej aplikacji logiki umożliwiającej wysyłanie tweetów pocztą e-mail

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). 

2. Z menu po lewej wybierz kolejno pozycje **Nowy** > **Integracja w przedsiębiorstwie** > **Aplikacja logiki**.

    Możesz również wybrać pozycję **Nowy**, w polu wyszukiwania wpisać `logic app` i nacisnąć klawisz Enter. Wybierz kolejno pozycje **Aplikacja logiki** > **Utwórz**.

3. Wprowadź nazwę aplikacji logiki, wybierz subskrypcję platformy Azure, utwórz lub wybierz grupę zasobów platformy Azure, wybierz lokalizację i wybierz pozycję **Utwórz**.

    Jeśli wybierzesz pozycję **Przypnij do pulpitu nawigacyjnego**, aplikacja logiki będzie automatycznie otwierana po wdrożeniu.

4. Po pierwszym otwarciu aplikacji logiki będzie można wybrać szablon, aby rozpocząć.
Teraz kliknij pozycję **Pusta aplikacja logiki**, aby rozpocząć tworzenie aplikacji od podstaw. 

5. Pierwszy element, który należy utworzyć, to wyzwalacz. Jest to zdarzenie, które uruchamia aplikację logiki. Wyszukaj tekst **twitter** w polu wyszukiwania i wybierz pozycję **Po wysłaniu nowego tweetu**. Zaloguj się przy użyciu nazwy użytkownika i hasła do konta w usłudze Twitter.

6. Następnie wpisz termin wyszukiwania, który spowoduje wyzwolenie aplikacji logiki.

   ![Wyszukiwanie w serwisie Twitter](media/logic-apps-create-a-logic-app/twittersearch.png)

    Wartości **Częstotliwość** i **Interwał** umożliwiają określenie, jak często aplikacja logiki ma sprawdzać istnienie nowych tweetów (i zwracać wszystkie tweety z tego okresu).

7. Wybierz pozycję **Nowy krok** i wybierz pozycję **Dodaj akcję** lub **Dodaj warunek**.

    Po wybraniu pozycji **Dodaj akcję** możesz wyszukać [dostępne łączniki](../connectors/apis-list.md) w celu wybrania akcji. 

8. Wyszukaj nazwę **outlook** w polu wyszukiwania i wybierz pozycję **Wyślij wiadomość e-mail**, aby wysłać wiadomość e-mail ze swojego konta programu Outlook na dowolnie określony adres e-mail.

   ![Akcje](media/logic-apps-create-a-logic-app/actions.png)

9. Teraz musisz podać parametry określające wiadomość e-mail:

   ![Parametry](media/logic-apps-create-a-logic-app/parameters.png)

10. Na koniec wybierz pozycję **Zapisz**, aby uruchomić aplikację logiki.

## <a name="manage-your-logic-app-after-creation"></a>Zarządzanie utworzoną aplikacją logiki

Twoja aplikacja logiki już działa. Będzie okresowo sprawdzać tweety z wprowadzonym terminem wyszukiwania. Po znalezieniu pasującego tweeta wyśle Ci wiadomość e-mail. Na koniec będzie można wyłączyć aplikację lub sprawdzić jej działanie.

1. Przejdź do witryny [Azure Portal](https://portal.azure.com).

2. W menu po lewej kliknij pozycję **Więcej usług**. W obszarze **Integracja w przedsiębiorstwie** wybierz pozycję **Logic Apps**. Wybierz aplikację logiki.

    *    Aby wyświetlić stan aplikacji, historię wykonywania i informacje ogólne, w menu aplikacji logiki wybierz pozycję **Przegląd**. Jeśli nie widzisz danych, których oczekujesz, na pasku poleceń kliknij pozycję **Odśwież**.

    *    Aby edytować aplikację, w menu aplikacji logiki wybierz pozycję **Projektant aplikacji logiki**.

    *    Aby tymczasowo wyłączyć aplikację, w menu aplikacji logiki wybierz pozycję **Przegląd**. Na pasku poleceń wybierz pozycję **Wyłącz**.

    *    Aby usunąć aplikację, z menu aplikacji logiki wybierz pozycję **Przegląd**. 
    Na pasku poleceń wybierz pozycję **Usuń**. Wprowadź nazwę aplikacji logiki, a następnie wybierz pozycję **Usuń**.

W czasie krótszym niż 5 minut udało się skonfigurować prostą aplikację logiki działającą w chmurze. Aby dowiedzieć się więcej o korzystaniu z funkcji Logic Apps, zobacz [Korzystanie z funkcji Logic Apps]. Aby zapoznać się z definicjami Aplikacji logiki, zobacz [Author Logic App definitions](../logic-apps/logic-apps-author-definitions.md) (Tworzenie definicji aplikacji logiki).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Korzystanie z funkcji Logic Apps]: logic-apps-create-a-logic-app.md
