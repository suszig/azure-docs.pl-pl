---
title: "Tworzy funkcję, która integruje się z usługą Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzy funkcję, która integruje się z usługi Azure Logic Apps i kognitywnych usług Azure, aby kategoryzować tweet wskaźniki nastrojów klientów i wysyłania powiadomień, gdy wskaźniki nastrojów klientów jest niska."
services: functions, logic-apps, cognitive-services
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 910077645b521d4cd303d39f543cf155161a31c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Tworzy funkcję, która integruje się z usługą Azure Logic Apps

Azure Functions można integrować z usługi Azure Logic Apps w Projektancie aplikacji logiki. Integracja ta umożliwia używanie mocy obliczeniowej funkcje w orchestrations z innymi usługami innych firm i Azure. 

Ten samouczek pokazuje, jak za pomocą funkcji Logic Apps i kognitywnych usług Microsoft Azure i analizować wskaźniki nastrojów klientów z wpisów Twitter. Funkcja HTTP wyzwalane kategoryzuje tweetów jako zielone, żółte lub czerwone oparte na wynik wskaźniki nastrojów klientów. Zostanie wysłana wiadomość e-mail, po wykryciu niską wskaźniki nastrojów klientów. 

![pierwsze dwa kroki obrazu aplikacji w Projektancie aplikacji logiki](media/functions-twitter-email/designer1.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto usługi kognitywnych.
> * Utwórz funkcję, który kategoryzuje tweet wskaźniki nastrojów klientów.
> * Tworzenie aplikacji logiki łączącej się z serwisem Twitter.
> * Dodaj wykrywania wskaźniki nastrojów klientów do aplikacji logiki. 
> * Łączenie aplikacji logiki do funkcji.
> * Wyślij wiadomość e-mail na podstawie na odpowiedź od funkcji.

## <a name="prerequisites"></a>Wymagania wstępne

+ Aktywny [Twitter](https://twitter.com/) konta. 
+ [Outlook.com](https://outlook.com/) konta (w przypadku wysyłania powiadomień).
+ Do wykonania czynności przedstawionych w tym temacie są wymagane zasoby utworzone w temacie [Tworzenie pierwszej funkcji w witrynie Azure Portal](functions-create-first-azure-function.md).  
Jeśli nie zostało to jeszcze zrobione, należy wykonać następujące kroki teraz, aby utworzyć aplikację funkcji.

## <a name="create-a-cognitive-services-account"></a>Utwórz konto usługi kognitywnych

Konta usług kognitywnych jest wymaganego do wykrycia wskaźniki nastrojów klientów tweetów monitorowane.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

2. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

3. Kliknij przycisk **dane i analiza** > **kognitywnych usług**. Następnie należy użyć ustawienia określone w tabeli, zaakceptuj warunki i sprawdź **Przypnij do pulpitu nawigacyjnego**.

    ![Tworzenie konta kognitywnych strony](media/functions-twitter-email/cog_svcs_account.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                                        |
    | --- | --- | --- |
    | **Nazwa** | MyCognitiveServicesAccnt | Wybierz unikatową nazwę konta. |
    | **Typ interfejsu API** | Interfejs API analizy tekstu | Interfejs API używany do analizowania tekstu.  |
    | **Lokalizacja** | Zachodnie stany USA | Obecnie tylko **zachodnie stany USA** jest dostępna dla Analiza tekstu. |
    | **Warstwa cenowa** | F0 | Rozpocznij od najniższej warstwy. Po uruchomieniu poza wywołania skalować do wyższego poziomu.|
    | **Grupa zasobów** | myResourceGroup | Użyj tej samej grupy zasobów dla wszystkich usług, w tym samouczku.|

4. Kliknij przycisk **Utwórz** do utworzenia konta. Po utworzeniu konta kliknij nowych usług kognitywnych konta przypięta do pulpitu nawigacyjnego. 

5. W ramach konta, kliknij przycisk **klucze**, a następnie skopiuj wartość **klucz 1** i zapisz go. Ten klucz umożliwia łączenie aplikacji logiki do swojego konta usługi kognitywnych. 
 
    ![Klucze](media/functions-twitter-email/keys.png)

## <a name="create-the-function"></a>Tworzenie funkcji

Funkcje zapewnia to dobry sposób na rozwiązanie odciążania zadań przetwarzania w przepływie pracy aplikacji logiki. W tym samouczku korzysta z funkcji HTTP wyzwalane przetwarzanie wyników wskaźniki nastrojów klientów tweet z kognitywnych usług i zwracać wartość kategorii.  

1. Rozwiń funkcji aplikacji, kliknij przycisk  **+**  znajdujący się obok **funkcje**, kliknij przycisk **HTTPTrigger** szablonu. Typ `CategorizeSentiment` dla funkcji **nazwa** i kliknij przycisk **Utwórz**.

    ![Funkcja blok aplikacje, funkcje +](media/functions-twitter-email/add_fun.png)

2. Zastąp zawartość pliku run.csx następującym kodem, a następnie kliknij przycisk **zapisać**:

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Ten kod funkcja zwraca kategorii koloru oparte na wynik wskaźniki nastrojów klientów została odebrana w żądaniu. 

3. Aby przetestować funkcji, kliknij przycisk **testu** na prawym końcu do zwiększenia karcie testów. Wpisz wartość `0.2` dla **treść żądania**, a następnie kliknij przycisk **Uruchom**. Wartość **czerwony** jest zwracany w treści odpowiedzi. 

    ![Testowanie funkcji w portalu Azure](./media/functions-twitter-email/test.png)

Zostanie zainstalowana funkcja, który kategoryzuje wyniki wskaźniki nastrojów klientów. Następnie należy utworzyć aplikację logiki, która integruje się funkcji z konta usługi kognitywnych i Twitter. 

## <a name="create-a-logic-app"></a>Tworzenie aplikacji logiki   

1. W portalu Azure kliknij **nowy** znaleziono przycisku w lewym górnym rogu portalu Azure.

2. Kliknij przycisk **integracji przedsiębiorstwa** > **aplikacji logiki**. Następnie należy użyć ustawienia określone w tabeli wyboru **Przypnij do pulpitu nawigacyjnego**i kliknij przycisk **Utwórz**.
 
4. Następnie wpisz **nazwa** jak `TweetSentiment`, użyj ustawień określonych w tabeli, zaakceptuj warunki i sprawdź **Przypnij do pulpitu nawigacyjnego**.

    ![Tworzenie aplikacji logiki w portalu Azure](./media/functions-twitter-email/new_logicApp.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                                        |
    | ----------------- | ------------ | ------------- |
    | **Nazwa** | TweetSentiment | Wybierz odpowiednią nazwę aplikacji. |
    | **Grupa zasobów** | myResourceGroup | Wybierz istniejącą grupę zasobów jako przed. |
    | **Lokalizacja** | Wschodnie stany USA | Wybierz bliską lokalizację. |    

4. Wybierz **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz** do tworzenia aplikacji logiki. 

5. Po utworzeniu aplikacji kliknij aplikację logiki przypięty do pulpitu nawigacyjnego. Następnie w Projektancie aplikacji logiki, przewiń w dół i kliknij przycisk **pustą aplikację logiki** szablonu. 

    ![Pusty szablon aplikacji logiki](media/functions-twitter-email/blank.png)

Projektant aplikacji logiki umożliwia teraz dodać do aplikacji usługi i wyzwalacze.

## <a name="connect-to-twitter"></a>Połączenia z serwisem Twitter

Najpierw należy utworzyć połączenie do swojego konta w serwisie Twitter. Aplikację logiki sonduje wyszukiwane tweety, które wyzwolenia uruchamiania aplikacji.

1. W projektancie, kliknij **Twitter** usługi, a następnie kliknij przycisk **po jest przesyłana z nowego tweet** wyzwalacza. Zaloguj się do konta w usłudze Twitter i zezwolić aplikacji logiki, aby korzystać z Twojego konta.

2. Użyj ustawień wyzwalacza Twitter określoną w tabeli. 

    ![Ustawienia łącznika usługi Twitter](media/functions-twitter-email/azure_tweet.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                                        |
    | ----------------- | ------------ | ------------- |
    | **Wyszukiwanie tekstu** | #Azure | Użyj hasztagiem, będącą popularnych do generowania nowych tweetów w wybranym interwałem. Po użyciu warstwę bezpłatna i z hasztagiem zbyt popularne, możesz szybko zużywać transakcji na koncie usługi kognitywnych. |
    | **Częstotliwość** | Minuta | Częstotliwość Jednostka używana do sondowania Twitter.  |
    | **Interwał** | 15 | Czas między żądania usługi Twitter, w jednostkach częstotliwości. |

3.  Kliknij przycisk **zapisać** do łączenia się z kontem usługi Twitter. 

Teraz aplikacji jest połączony z usługą Twitter. Następnie połączenie z analizy tekstu do wykrywania wskaźniki nastrojów klientów tweetów zebranych.

## <a name="add-sentiment-detection"></a>Dodaj wykrywania wskaźniki nastrojów klientów

1. Kliknij przycisk **nowy krok**, a następnie **Dodaj akcję**.

    ![Nowy krok, a następnie dodaj akcję](media/functions-twitter-email/new_step.png)

2. W **wybierz akcję**, kliknij przycisk **Analiza tekstu**, a następnie kliknij przycisk **wykrywa wskaźniki nastrojów klientów** akcji.

    ![Wykrywa wskaźniki nastrojów klientów](media/functions-twitter-email/detect_sent.png)

3. Wpisz nazwę połączenia, takich jak `MyCognitiveServicesConnection`, Wklej klucz dla usług kognitywnych konta zapisane i kliknij przycisk **Utwórz**.  

4. Kliknij przycisk **tekstu do analizowania** > **Tweetować tekst**, a następnie kliknij przycisk **zapisać**.  

    ![Wykrywa wskaźniki nastrojów klientów](media/functions-twitter-email/ds_tta.png)

Teraz, gdy skonfigurowano wykrywania wskaźniki nastrojów klientów, można dodać połączenie z funkcji, które korzysta z danych wyjściowych wynik wskaźniki nastrojów klientów.

## <a name="connect-sentiment-output-to-your-function"></a>Połącz dane wyjściowe wskaźniki nastrojów klientów do funkcji

1. W Projektancie aplikacji logiki, kliknij **nowy krok** > **Dodaj akcję**, a następnie kliknij przycisk **usługi Azure Functions**. 

2. Kliknij przycisk **należy wybrać funkcję Azure**, wybierz pozycję **CategorizeSentiment** funkcja utworzony wcześniej.  

    ![Azure funkcja okno Wybierz funkcję platformy Azure](media/functions-twitter-email/choose_fun.png)

3. W **treść żądania**, kliknij przycisk **wynik** , a następnie **zapisać**.

    ![Wynik](media/functions-twitter-email/trigger_score.png)

Teraz funkcja jest wyzwalane, gdy wynik wskaźniki nastrojów klientów są wysyłane z aplikacji logiki. Oznaczone kolorami kategorii jest zwracana do aplikacji logiki przez funkcję. Następnie dodaj powiadomienie e-mail, który jest wysyłany, gdy wartość wskaźniki nastrojów klientów **czerwony** zostanie zwrócona przez funkcję. 

## <a name="add-email-notifications"></a>Dodawanie powiadomień e-mail

Ostatnia część przepływu pracy jest do wyzwolenia wiadomość e-mail, gdy wskaźniki nastrojów klientów są oceniane jako _czerwony_. W tym temacie używa łącznika usługi Outlook.com. Można wykonać podobne kroki do używania łącznika usługi Gmail lub Office 365 Outlook.   

1. W Projektancie aplikacji logiki, kliknij **nowy krok** > **Dodaj warunek**. 

2. Kliknij przycisk **wybierz wartość**, następnie kliknij przycisk **treści**. Wybierz **jest równa**, kliknij przycisk **wybierz wartość** i typ `RED`i kliknij przycisk **zapisać**. 

    ![Dodaj warunek do aplikacji logiki.](media/functions-twitter-email/condition.png)

3. W **Jeśli tak, nic nie RÓB**, kliknij przycisk **Dodaj akcję**, wyszukaj `outlook.com`, kliknij przycisk **wysłać wiadomość e-mail**i zaloguj się do swojego konta w usłudze Outlook.com.
    
    ![Wybierz akcję dla warunku.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Jeśli nie masz konta usługi Outlook.com, można wybrać innego łącznika, takich jak usługi Gmail lub Office 365 Outlook

4. W **wysłać wiadomość e-mail** akcji, użyj ustawienia poczty e-mail jako określony w tabeli. 

    ![Konfigurowanie poczty e-mail do wysyłania poczty e-mail akcji.](media/functions-twitter-email/sendEmail.png)

    | Ustawienie      |  Sugerowana wartość   | Opis  |
    | ----------------- | ------------ | ------------- |
    | **Do** | Wpisz swój adres e-mail | Adres e-mail, która odbiera powiadomienia. |
    | **Temat** | Ujemna tweet wykryto wskaźniki nastrojów klientów  | Wiersz tematu powiadomienia pocztą e-mail.  |
    | **Treść** | Tekst tweet, lokalizacji | Kliknij przycisk **Tweetować tekst** i **lokalizacji** parametrów. |

5.  Kliknij pozycję **Zapisz**.

Teraz, przepływ pracy zostanie zakończone, można włączyć aplikacji logiki i zobacz opis funkcji w miejscu pracy.

## <a name="test-the-workflow"></a>Testowanie przepływu pracy

1. W Projektancie aplikacji logiki, kliknij przycisk **Uruchom** do uruchomienia aplikacji.

2. W lewej kolumnie kliknij **omówienie** aby zobaczyć stan aplikacji logiki. 
 
    ![Stan wykonania aplikacji logiki](media/functions-twitter-email/over1.png)

3. (Opcjonalnie) Kliknij jeden z działa, aby wyświetlić szczegóły wykonywania.

4. Przejdź do funkcji, sprawdź dzienniki i sprawdź, czy wskaźniki nastrojów klientów wartości były odbierane i przetwarzane.
 
    ![Wyświetl dzienniki — funkcja](media/functions-twitter-email/sent.png)

5. Po wykryciu potencjalnie ujemna wskaźniki nastrojów klientów, otrzymasz wiadomość e-mail. Jeśli nie otrzymasz wiadomość e-mail, można zmienić kod funkcji do zwrócenia czerwony za każdym razem, gdy:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Po zweryfikowaniu powiadomienia e-mail, należy zmienić wstecz do oryginalnego kodu:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Po ukończeniu tego samouczka, należy wyłączyć aplikację logiki. Wyłączenie aplikacji, można uniknąć obciążona do wykonania i wykorzystuje się transakcji na koncie usługi kognitywnych.

Teraz już wspomniano, jak łatwo jest integrację funkcji przepływu pracy Logic Apps.

## <a name="disable-the-logic-app"></a>Wyłączanie aplikacji logiki

Aby wyłączyć aplikację logiki, kliknij przycisk **omówienie** , a następnie kliknij przycisk **wyłączyć** w górnej części ekranu. Powoduje to zatrzymanie aplikacji logiki z systemem i naliczania opłat bez usuwania aplikacji. 

![Dzienniki funkcji](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto usługi kognitywnych.
> * Utwórz funkcję, który kategoryzuje tweet wskaźniki nastrojów klientów.
> * Tworzenie aplikacji logiki łączącej się z serwisem Twitter.
> * Dodaj wykrywania wskaźniki nastrojów klientów do aplikacji logiki. 
> * Łączenie aplikacji logiki do funkcji.
> * Wyślij wiadomość e-mail na podstawie na odpowiedź od funkcji.

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak utworzyć interfejs API niekorzystającą dla funkcji.

> [!div class="nextstepaction"] 
> [Tworzenie bezserwerowego interfejsu API za pomocą usługi Azure Functions](functions-create-serverless-api.md)

Aby dowiedzieć się więcej na temat aplikacji logiki, zobacz [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

