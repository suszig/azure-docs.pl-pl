<properties
    pageTitle="Tworzenie aplikacji logiki | Microsoft Azure"
    description="Dowiedz się, jak utworzyć aplikację logiki łączącą usługi SaaS"
    authors="jeffhollan"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/16/2016"
    ms.author="jehollan"/>

# Tworzenie nowej aplikacji logiki łączącej usługi SaaS

W tym temacie pokazano, jak w ciągu zaledwie kilku minut rozpocząć korzystanie z usługi [Azure Logic Apps](app-service-logic-what-are-logic-apps.md). Zawiera on omówienie prostego przepływu pracy, który pozwala wysyłać interesujące tweety na adres e-mail.

Aby użyć tego scenariusza, potrzebne są:

- Subskrypcja platformy Azure
- Konto w serwisie Twitter
- Skrzynka pocztowa Outlook.com lub hostowana skrzynka pocztowa Office 365

## Tworzenie nowej aplikacji logiki umożliwiającej wysyłanie tweetów pocztą e-mail

1. Na [pulpicie nawigacyjnym witryny Azure Portal](https://portal.azure.com) wybierz pozycję **Nowy**. 
2. Za pomocą paska wyszukiwania wyszukaj frazę „aplikacja logiki”, a następnie wybierz pozycję **Aplikacja logiki**. Możesz również wybrać kolejno pozycje **Nowy**, **Sieci Web i mobilność** oraz **Aplikacja logiki**. 
3. Wprowadź nazwę aplikacji logiki, wybierz lokalizację i grupę zasobów, a następnie wybierz pozycję **Utwórz**.  Jeśli wybierzesz pozycję **Przypnij do pulpitu nawigacyjnego**, aplikacja logiki zostanie automatycznie otwarta po jej wdrożeniu.  
4. Po pierwszym otwarciu aplikacji logiki będzie można wybrać szablon, aby rozpocząć.  Teraz kliknij pozycję **Pusta aplikacja logiki**, aby rozpocząć tworzenie aplikacji od podstaw. 
1. Pierwszy element, który należy utworzyć, to wyzwalacz.  Jest to zdarzenie, które uruchomi aplikację logiki.  Wyszukaj tekst **twitter** w polu wyszukiwania wyzwalacza i wybierz pozycję Twitter.
7. Następnie wpisz w polu wyszukiwania termin wyszukiwania, który spowoduje uruchomienie wyzwalacza.  Pola **Częstotliwość** i **Interwał** umożliwiają określenie, jak często aplikacja logiki sprawdza, czy istnieją nowe tweety (i zwraca wszystkie tweety z tego okresu).
    ![Wyszukiwanie w serwisie Twitter](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. Wybierz przycisk **Nowy krok**, a następnie wybierz pozycję **Dodaj akcję** lub **Dodaj warunek**
6. Po wybraniu pozycji **Dodaj akcję** możesz wyszukać [dostępne łączniki](../connectors/apis-list.md) w celu wybrania akcji. Na przykład możesz wybrać pozycję **Outlook.com — wysłanie wiadomości e-mail**, aby wysłać wiadomość e-mail z adresu outlook.com:  
    ![Akcje](./media/app-service-logic-create-a-logic-app/actions.png)

7. Teraz musisz podać parametry określające wiadomość e-mail:  ![Parametry](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Na koniec wybierz pozycję **Zapisz**, aby uruchomić aplikację logiki.

## Zarządzanie utworzoną aplikacją logiki

Twoja aplikacja logiki już działa. Będzie okresowo sprawdzać tweety z wprowadzonym terminem wyszukiwania. Po znalezieniu pasującego tweeta wyśle Ci wiadomość e-mail. Na koniec będzie można wyłączyć aplikację lub sprawdzić jej działanie.

1. Przejdź do witryny [Azure Portal](https://portal.azure.com)

1. Kliknij pozycję **Przeglądaj** w lewej części ekranu i wybierz pozycję **Logic Apps**.

2. Kliknij utworzoną aplikację logiki, aby wyświetlić jej bieżący stan i ogólne informacje.

3. Aby edytować nową aplikację logiki, kliknij pozycję **Edytuj**.

5. Aby wyłączyć aplikację, kliknij pozycję **Wyłącz** na pasku poleceń.

1. Wyświetl historię uruchamiania i wyzwalania, gdy aplikacja logiki działa.  Możesz kliknąć przycisk **Odśwież**, aby zobaczyć najnowsze dane.

W czasie krótszym niż 5 minut udało się skonfigurować prostą aplikację logiki działającą w chmurze. Aby dowiedzieć się więcej o korzystaniu z funkcji Logic Apps, zobacz [Korzystanie z funkcji Logic Apps]. Aby zapoznać się z definicjami Aplikacji logiki, zobacz [Author Logic App definitions](app-service-logic-author-definitions.md) (Tworzenie definicji aplikacji logiki).

<!-- Shared links -->
[Azure Portal]: https://portal.azure.com
[Korzystanie z funkcji Logic Apps]: app-service-logic-create-a-logic-app.md



<!--HONumber=sep16_HO1-->


