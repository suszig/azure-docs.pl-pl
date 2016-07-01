<properties
    pageTitle="Tworzenie aplikacji logiki | Microsoft Azure"
    description="Dowiedz się, jak utworzyć aplikację logiki łączącą usługi SaaS"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/16/2016"
    ms.author="stepsic"/>

# Tworzenie nowej aplikacji logiki łączącej usługi SaaS

| Krótki przewodnik |
| --------------- |
| [Język definicji usługi Logic Apps](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Dokumentacja dotycząca łączników usługi Logic Apps](../connectors/apis-list.md) |
| [Forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

W tym temacie pokazano, jak w ciągu zaledwie kilku minut rozpocząć korzystanie z usługi [Logic Apps w usłudze App Service](app-service-logic-what-are-logic-apps.md). Zostanie omówiony przepływ pracy, który pozwala dostarczyć zestaw określonych tweetów do skrzynki pocztowej.

Aby użyć tego scenariusza, potrzebne są:

- Subskrypcja platformy Azure
- Konto w serwisie Twitter
- Konto usługi Office 365

## Tworzenie nowej aplikacji logiki umożliwiającej wysyłanie tweetów pocztą e-mail

1. Na pulpicie nawigacyjnym portalu Azure wybierz pozycję **Marketplace**. 
2. Poszukaj pozycji „aplikacje logiki” w obszarze Wszystko, a następnie wybierz pozycję **Aplikacja logiki (wersja Preview)**. Możesz również wybrać kolejno pozycje **Nowy**, **Sieci Web i mobilność** i **Aplikacja logiki (wersja Preview)**. 
3. Wpisz nazwę aplikacji logiki, wybierz plan Usługi aplikacji i wybierz pozycję **Utwórz**.  
    W tym kroku zakłada się, że masz plan Usługi aplikacji i dysponujesz wiedzą na temat wymaganych właściwości. Jeśli tak nie jest, nie martw się — możesz rozpocząć pracę od zapoznania się z tematem [Azure App Service plans in-depth overview](azure-web-sites-web-hosting-plans-in-depth-overview.md) (Szczegółowe omówienie planów usługi Azure App Service). 

4. Przy pierwszym uruchomieniu aplikacji logiki będzie potrzebny wyzwalacz. Wyszukaj tekst **twitter** w polu wyszukiwania wyzwalacza i wybierz pozycję Twitter.

7. Teraz wpisz słowo kluczowe, które ma służyć do wyszukiwania tekstu w serwisie Twitter. 
    ![Wyszukiwanie w serwisie Twitter](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. Wybierz znak plus, a następnie wybierz pozycję **Dodaj akcję** lub **Dodaj warunek**:  
    ![Znak plus](./media/app-service-logic-create-a-logic-app/plus.png)
6. Po wybraniu pozycji **Dodaj akcję** pojawi się lista wszystkich łączników oraz dostępnych akcji. Za jej pomocą możesz wybrać łączniki i akcje, które chcesz dodać do aplikacji logiki. Możesz na przykład wybrać akcję **Usługa Office 365 — wyślij wiadomość e-mail** oraz inne akcje usługi Office 365:  
    ![Akcje](./media/app-service-logic-create-a-logic-app/actions.png)

7. Teraz musisz podać parametry określające wiadomość e-mail:  ![Parametry](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Na koniec wybierz pozycję **Zapisz**, aby uruchomić aplikację logiki.

## Zarządzanie utworzoną aplikacją logiki

Twoja aplikacja logiki już działa. Przy każdym uruchomieniu zaplanowanego przepływu pracy są wyszukiwane tweety z określonym hasztagiem, które następnie są umieszczane w usłudze Dropbox. Na koniec będzie można wyłączyć aplikację lub sprawdzić jej działanie.

1. Kliknij pozycję **Przeglądaj** w lewej części ekranu i wybierz pozycję **Logic Apps**.

2. Kliknij utworzoną aplikację logiki, aby wyświetlić jej bieżący stan i ogólne informacje.

3. Aby edytować nową aplikację logiki, kliknij pozycję **Wyzwalacze i akcje**.

5. Aby wyłączyć aplikację, kliknij pozycję **Wyłącz** na pasku poleceń.

W czasie krótszym niż 5 minut udało się skonfigurować prostą aplikację logiki działającą w chmurze. Aby dowiedzieć się więcej o korzystaniu z funkcji Logic Apps, zobacz [Korzystanie z funkcji Logic Apps]. Aby zapoznać się z definicjami Aplikacji logiki, zobacz [Author Logic App definitions](app-service-logic-author-definitions.md) (Tworzenie definicji aplikacji logiki).

<!-- Shared links -->
[Portal Azure]: https://portal.azure.com
[Korzystanie z funkcji Logic Apps]: app-service-logic-create-a-logic-app.md



<!--HONumber=Jun16_HO2-->


