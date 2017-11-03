---
title: "Opisz niestandardowych interfejsów API i łączniki z Postman - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie kolekcji Postman do opisu, grupy i organizowania niestandardowych interfejsów API i łączniki"
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
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 072d544e5d29c4abb3d69651e53cfb33d5e0c9e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="describe-custom-apis-and-custom-connectors-with-postman"></a>Opis niestandardowych interfejsów API i łączniki niestandardowych z Postman

Tworzenie [niestandardowych interfejsów API](../logic-apps/logic-apps-create-api-app.md) i [łączników niestandardowych](../logic-apps/custom-connector-overview.md) szybciej i łatwiej, można utworzyć [Postman](https://www.getpostman.com/) kolekcji, a nie dokumentów OpenAPI, opisu sieci Interfejsy API i łączniki. Postman kolekcji pomocy organizować i grupować powiązane żądania interfejsu API. Na przykład można użyć Postman, podczas tworzenia łączników dla usługi Azure Logic Apps, Flow firmy Microsoft lub PowerApps firmy Microsoft. 

W tym samouczku przedstawiono sposób tworzenia [kolekcji Postman](https://www.getpostman.com/docs/postman/collections/creating_collections) za pomocą [wykryć interfejs API języka](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) w [kognitywnych Analiza tekstu usług Azure](https://azure.microsoft.com/services/cognitive-services/text-analytics/) co w przykładzie. Ten interfejs API identyfikuje języka, wskaźniki nastrojów klientów i klucza fraz w tekście, który jest przekazywany do interfejsu API.

## <a name="prerequisites"></a>Wymagania wstępne

* Jeśli jesteś nowym użytkownikiem Postman, [zainstalowania aplikacji Postman](https://www.getpostman.com/apps).

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, można uruchomić z poziomu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestrować się w celu [subskrypcji płatność za rzeczywiste użycie](https://azure.microsoft.com/pricing/purchase-options/).

* Jeśli masz subskrypcję platformy Azure, załóż interfejsów API Analytics tekstu, wykonując [zadaniu 1](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-signup.md). 

## <a name="create-a-postman-collection"></a>Utwórz kolekcję Postman

Aby można było utworzyć kolekcję, utworzyć żądania HTTP dla punktu końcowego interfejsu API. 

### <a name="create-an-http-request-for-your-api"></a>Tworzenie żądania HTTP do interfejsu API

1. Otwórz aplikację Postman, dzięki czemu można tworzyć [żądania HTTP](https://www.getpostman.com/docs/postman/sending_api_requests/requests) dla punktu końcowego interfejsu API. Aby uzyskać więcej informacji, zobacz Postman [żądań dokumentacji](https://www.getpostman.com/docs/postman/sending_api_requests/requests).

   1. Na **konstruktora** karcie, wybierz metodę HTTP, wprowadź adres URL żądania dla punktu końcowego interfejsu API i wybierz protokół autoryzacji, jeśli istnieje. 
   Gdy wszystko jest gotowe, wybierz pozycję **Params**.

      W tym samouczku tych ustawień można użyć w tym przykładzie:

      ![Utwórz żądanie: "Metoda HTTP", "URL żądania", "Autoryzacji"](./media/custom-connector-api-postman-collection/01-create-api-http-request.png)

      | Parametr | Sugerowana wartość | 
      | --------- | --------------- | 
      | **Metoda HTTP** | POST | 
      | **Adres URL żądania** | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` | | 
      | **Autoryzacja** | "Nie autoryzacji" | | 
      ||| 

   2. Teraz można wprowadzić pary klucz wartość, które będą używane jako parametry zapytania lub ścieżki w adresie URL żądania. Postman scala te elementy jednocześnie w ciągu zapytania.
   Gdy wszystko będzie gotowe, wybierz pozycję **nagłówki**.

      ![Żądanie nadal: "Parametry"](./media/custom-connector-api-postman-collection/02-create-api-http-request-params.png)

      | Parametr | Sugerowana wartość | 
      | --------- | --------------- | 
      | **Parametry** | **Klucz**: "numberOfLanguagesToDetect" </br>**Wartość**: "1" | 
      ||| 

   3. Wprowadź pary klucz wartość dla nagłówka żądania. 
   Wprowadź dowolny ciąg, który ma nazwę nagłówka. Dla typowych nagłówków HTTP można wybrać z listy rozwijanej. Gdy wszystko będzie gotowe, wybierz pozycję **treści**. 
   
      ![Żądanie nadal: "Nagłówki"](./media/custom-connector-api-postman-collection/03-create-api-http-request-header.png)

      | Parametr | Wartość | 
      | --------- | ----- | 
      | **Nagłówki** | **Klucz**: "Ocp-Apim subskrypcji — klucz" </br>**Wartość**: *-API-subskrypcji — klucz*, która znajduje się na koncie usługi kognitywnych <p>**Klucz**: "Content-Type" </br> **Wartość**: "application/json" | 
      ||| 

   4. Wprowadź zawartość, która ma być wysyłane w treści żądania. 
   Aby sprawdzić żądanie polega otrzyma odpowiedź ponownie, wybierz **wysyłania**. 
   
      ![Żądanie nadal: "Treść"](./media/custom-connector-api-postman-collection/04-create-api-http-request-body.png)

      | Parametr | Sugerowana wartość | 
      | --------- | --------------- |    
      | **Treść** | ```{"documents": [{ "id": "1", "text": "Hello World"}]}``` | 
      ||| 

      Pole odpowiedzi zawiera całą odpowiedź z interfejsu API, w tym wynik lub wystąpił błąd, jeśli istnieje.

      ![Uzyskaj odpowiedzi na żądanie](./media/custom-connector-api-postman-collection/05-create-api-http-request-response.png)

2. Po sprawdzeniu czy działa Twoje żądanie, Zapisz żądanie do kolekcji Postman. 

   1. Wybierz pozycję **Zapisz**. 
      
      ![Wybierz opcję "Zapisz"](./media/custom-connector-api-postman-collection/06a-save-request.png)
 
   2. W obszarze **Zapisz żądanie**, podaj **Nazwa żądania** i, opcjonalnie, **żądania opis**. 

      > [!NOTE]
      > Łącznik niestandardowy później używa tych wartości. Tak upewnij się, musisz zapewnić tej samej wartości, które później użyć niestandardowego interfejsu API operacji podsumowania i opis.

   3. Wybierz **+ Utwórz kolekcję** i podaj nazwę kolekcji. 
   Wybierz znacznik wyboru, która tworzy folder kolekcji, a następnie wybierz **zapisać *your Postman kolekcji — nazwa-***.

      ![Zapisywanie żądania](./media/custom-connector-api-postman-collection/06b-save-request.png)

### <a name="save-the-request-response"></a>Zapisz żądanie odpowiedź

Po zapisaniu tego żądania można zapisać odpowiedzi. W ten sposób odpowiedzi pojawi się na przykład podczas ładowania żądanie później.

1. Nad oknem odpowiedzi wybierz **zapisać odpowiedzi**. 

   ![Zapisz odpowiedzi](./media/custom-connector-api-postman-collection/07-create-api-http-request-save-response.png)

   Niestandardowe łączniki obsługuje tylko jedna odpowiedź na żądanie. 
   Jeśli zapiszesz wiele odpowiedzi na żądanie jest używana tylko pierwsza z nich.

2. Podaj nazwę, na przykład sieci, a następnie wybierz pozycję **Zapisz przykładowy**.

3. Kontynuować tworzenie kolekcji Postman żadnych dodatkowych żądań i odpowiedzi.

### <a name="export-your-postman-collection"></a>Eksportuj kolekcji Postman

1. Gdy wszystko będzie gotowe, należy wyeksportować kolekcji do pliku JSON.

   ![Kolekcja eksportu](./media/custom-connector-api-postman-collection/08-export-http-request.png)

2. Wybierz **v1 kolekcji** format eksportu, a następnie przejdź do lokalizacji, w której chcesz zapisać plik JSON.

   > [!NOTE]
   > Obecnie tylko V1 działa dla łączników niestandardowych.

   ![Wybierz format eksportowania: "V1 kolekcji"](./media/custom-connector-api-postman-collection/09-export-format.png)
   
Ta kolekcja Postman można teraz używać do tworzenia niestandardowych interfejsów API i łączniki. Po wyeksportowaniu kolekcji można zaimportować pliku JSON do aplikacji logiki, przepływ lub rozwiązaniu PowerApps.

> [!IMPORTANT]
> W przypadku utworzenia niestandardowego łącznika z kolekcji Postman, upewnij się, należy usunąć `Content-type` nagłówka z działań i wyzwalaczy. Usługi docelowej, na przykład przepływu, automatycznie dodaje tego nagłówka. Ponadto usunąć nagłówki uwierzytelniania w `securityDefintions` sekcji z działań i wyzwalaczy.

## <a name="next-steps"></a>Następne kroki

* [Logic Apps: Rejestrowanie niestandardowe łączników](../logic-apps/logic-apps-custom-connector-register.md)
* [Przepływ: Zarejestrować Twojego łącznika](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [Rozwiązanie PowerApps: Zarejestrować Twojego łącznika](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)
