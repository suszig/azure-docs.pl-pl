---
title: "Zarejestruj łączników niestandardowych - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Konfigurowanie niestandardowych łączniki do użycia w aplikacjach logiki platformy Azure"
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
ms.openlocfilehash: 722b82aabe796367d906e9338355b83a1a8b1e1c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/26/2018
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Zarejestruj łączników niestandardowych w aplikacjach logiki platformy Azure

Po utworzeniu interfejsu API REST, konfigurowanie uwierzytelniania i uzyskać plik definicji OpenAPI lub kolekcji Postman, możesz zarejestrować Twojego łącznika. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby zarejestrować łącznik niestandardowy, potrzebne są następujące elementy:

* Szczegóły rejestracji Twojego łącznika na platformie Azure, takie jak nazwa, subskrypcji platformy Azure, grupy zasobów platformy Azure i lokalizacji, w której chcesz użyć

* Plik OpenAPI (Swagger) lub kolekcji Postman, który opisuje interfejs API

  W tym samouczku, można użyć [przykładowy plik OpenAPI Menedżera zasobów Azure](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Ikona reprezentująca łącznik

* Krótki opis programu łącznika

* Lokalizacja hosta do interfejsu API

## <a name="1-create-your-connector"></a>1. Tworzenie łącznika

1. W portalu Azure, w menu głównym Azure wybierz **Utwórz zasób**. W polu wyszukiwania wpisz „logic apps connector” jako filtr i naciśnij klawisz Enter.

   ![Nowy, wyszukiwanie ciągu „logic apps connector”](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Z listy wyników wybierz pozycję **Logic Apps Connector** > **Utwórz**.

   ![Tworzenie łącznika usługi Logic Apps](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Podaj szczegóły potrzebne do zarejestrowania łącznika, jak opisano w tabeli. Gdy skończysz, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Szczegóły łącznika niestandardowego usługi Logic Apps](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Właściwość | Sugerowana wartość | Opis | 
   | -------- | --------------- | ----------- | 
   | **Nazwa** | *nazwa-łącznika-niestandardowego* | Podaj nazwę łącznika. | 
   | **Subskrypcja** | *nazwa-subskrypcji-platformy-Azure* | Wybierz swoją subskrypcję platformy Azure. | 
   | **Grupa zasobów** | *nazwa-grupy-zasobów-platformy-Azure* | Utwórz lub wybierz grupę platformy Azure na potrzeby organizowania zasobów platformy Azure. | 
   | **Lokalizacja** | *region-wdrożenia* | Wybierz region wdrożenia łącznika. | 
   |||| 

   Po wdrożeniu łącznika na platformie Azure zostanie automatycznie otworzone menu łącznika niestandardowego. 
   Jeśli tak się nie stanie, wybierz swój łącznik niestandardowy na pulpicie platformy Azure.

## <a name="2-define-your-connector"></a>2. Definiowanie łącznika

Teraz określić plik OpenAPI lub kolekcji Postman tworzenia z łącznika, uwierzytelniania, który korzysta z łącznika, akcje i wyzwalacze, które zawiera łącznik niestandardowy i parametrów, które można używać działań i wyzwalaczy.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. Określ plik OpenAPI lub kolekcji Postman dla Twojego łącznika

1. Z menu łącznika wybierz pozycję **Łącznik usługi Logic Apps**, jeśli nie została jeszcze wybrana. Na pasku narzędzi wybierz pozycję **Edytuj**.

   ![Edytuj łącznik niestandardowy](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Wybierz **ogólne** tak, aby uzyskać szczegółowe informacje w poniższych tabelach zapewniają do tworzenia, zabezpieczanie i definiowanie działań i wyzwalacze łącznik niestandardowy.

   1. Dla **łączniki niestandardowe**, wybierz opcję umożliwiającą dostarczanie pliku OpenAPI (Swagger), który opisuje interfejs API.

      ![Określ plik OpenAPI do interfejsu API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Opcja | Format |Opis | 
      | ------ | ------ | ----------- | 
      | **Przekaż plik OpenAPI** | *OpenAPI (Swagger)-json-file* | Przejdź do lokalizacji pliku OpenAPI, a następnie wybierz tego pliku. | 
      | **Użyj adresu URL OpenAPI** | http://*path-to-swagger-json-file* | Podaj adres URL dla pliku OpenAPI Twój interfejs API. | 
      | **Przekaż kolekcji Postman V1** | *exported-Postman-collection-V1-file* | Wskaż lokalizację dla eksportowanego kolekcji Postman w formacie w wersji 1. | 
      |||| 

   2. W obszarze **Ogólne informacje** przekaż ikonę łącznika. 
   Zazwyczaj **opis**, **hosta**, i **podstawowego adresu URL** pola zostaną automatycznie wypełnione z pliku OpenAPI. 
   W przeciwnym razie dodaj te informacje zgodnie z opisem w tabeli i wybierz pozycję **Kontynuuj**. 

      ![Szczegóły łącznika](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Opcja lub ustawienie | Format | Opis | 
      | ----------------- | ------ | ----------- | 
      | **Przekaż ikonę** | *plik-png-lub-jpeg-mniejszy-niż-1-MB* | Ikona reprezentująca łącznik <p>Kolor: najlepiej białe logo na kolorowym tle. <p>Wymiary: logo ~160 pikseli wewnątrz kwadratu 230 pikseli | 
      | **Kolor tła ikony** | *kod-szesnastkowy-koloru-marki-ikony* | <p>Kolor tła za ikoną, który odpowiada kolorowi tła w pliku ikony. <p>Format: szesnastkowy. Na przykład kod #007ee5 reprezentuje kolor niebieski. | 
      | **Opis** | *opis-łącznika* | Podaj krótki opis łącznika. | 
      | **Host** | *host-łącznika* | Podaj domeny hosta dla interfejsu API sieci Web. | 
      | **Podstawowy adres URL** | *łącznik-podstawowego-adresu-URL* | Podaj podstawowy adres URL dla interfejsu API sieci Web. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Podawanie opisu uwierzytelniania używanego przez łącznik

1. Teraz wybierz pozycję **Zabezpieczenia**, aby przejrzeć lub opisać uwierzytelnianie używane przez łącznik. Dzięki uwierzytelnianiu masz pewność, że przepływ tożsamości użytkowników między usługą i dowolnymi klientami jest prawidłowy.

   ![Typ uwierzytelniania](./media/logic-apps-custom-connector-register/security.png)

   * Jeśli możesz przekazać plik OpenAPI, Kreator rejestracji automatycznie wykrywa typ uwierzytelniania, która korzysta z interfejsu API sieci Web, i automatycznie wypełni **zabezpieczeń** sekcji w kreatorze na podstawie `securityDefinitions` obiektu w tym plik. Na przykład w tym miejscu jest sekcja, która określa przy użyciu OAuth2.0:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * Jeśli plik OpenAPI nie wypełnić typ uwierzytelniania i właściwości, wybierz **Edytuj** dzięki czemu można dodać te informacje. 
   
     Na przykład jeśli był wcześniej [Konfigurowanie uwierzytelniania usługi Azure AD w tym samouczku](../logic-apps/custom-connector-azure-active-directory-authentication.md), tworzone aplikacje zabezpieczanie interfejsu API sieci Web i z łącznika usługi Azure AD. 
     Dlatego teraz można podać klucz aplikacji identyfikator i klienta, wcześniej zapisany:
    
     | Ustawienie | Sugerowana wartość | Opis | 
     | ------- | --------------- | ----------- | 
     | **Typ uwierzytelniania** | Uwierzytelnianie OAuth 2.0 | | 
     | **Dostawca tożsamości** | Usługa Azure Active Directory | | 
     | **Identyfikator klienta** | *application-ID-for-connector-Azure-AD-app* | Identyfikator aplikacji, zapisany dla aplikacji usługi Azure AD Twojego łącznika | 
     | **Klucz tajny klienta** | *client-key-for-connector-Azure-AD-app* | Klucz klienta dla aplikacji usługi Azure AD Twojego łącznika | 
     | **Adres URL logowania** | `https://login.windows.net` | | 
     | **Adres URL zasobu** | `https://management.core.windows.net/` | Upewnij się, że Dodaj adres URL dokładnie w określony sposób, w tym ukośnika. | 
     |||| 

   * Kolekcja Postman, automatycznie generuje plik OpenAPI dla Ciebie, automatycznie wypełni typ uwierzytelniania *tylko* korzystając typy obsługiwanych uwierzytelniania, takich jak OAuth 2.0 lub Basic.

2. Aby zapisać łącznik po wprowadzeniu informacji o zabezpieczeniach, w górnej części strony wybierz pozycję **Zaktualizuj łącznik**, a następnie wybierz pozycję **Kontynuuj**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Przeglądanie, aktualizowanie lub definiowanie akcji i wyzwalaczy łącznika

1. Teraz wybierz pozycję **Definicja**, aby przeglądać, edytować lub definiować nowe akcje i wyzwalacze, które użytkownicy mogą dodawać do swoich przepływów pracy.

   Akcje i wyzwalacze są oparte na operacje zdefiniowane w pliku OpenAPI lub Postman kolekcji, które automatycznie wypełnić **definicji** obejmują żądania i odpowiedzi wartości i strony. Dlatego jeśli wymagane operacje są już widoczne w tym miejscu, możesz przejść do następnego kroku w procesie rejestracji bez wprowadzania zmian na tej stronie.

   ![Definicja łącznika](./media/logic-apps-custom-connector-register/definition.png)

2. Opcjonalnie Jeśli chcesz edytować istniejące działania i jest wyzwalane, dodać nowe lub kontynuować następujące kroki.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Edytuj lub dodać akcje dla Twojego łącznika

1. Aby edytować istniejącą akcję, wybierz numer dla tej akcji. Aby dodać akcję, która nie istnieje w pliku OpenAPI lub Postman kolekcji, w obszarze **akcje**, wybierz **nowa akcja**.

2. W obszarze **ogólne**, podaj lub edytować te informacje dotyczące akcji:
   
   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Podsumowanie** | *Nazwa operacji* | Tytuł dla tego działania | 
   | **Opis** | *Opis operacji* | Opis dla tej akcji. <p>**Porada**: Upewnij się, że opis kończy się kropką. |
   | **Identyfikator operacji** | *Identyfikator operacji* | Unikatowa nazwa identyfikowania tej akcji. Użyj formatu camelcase, na przykład: "GetPullRequest" | 
   |**Widoczność**| **Brak**, **zaawansowane**, **wewnętrzny**, lub **ważne** | Widoczność uwzględniającym działania użytkownika dla tej akcji. Aby uzyskać więcej informacji, zobacz [OpenAPI rozszerzenia](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Teraz można zdefiniować żądanie dla akcji.
  
   1. W **żądania** wybierz **importu z próbki**. 

   2. Na **importu z próbki** strony, Wklej przykładowe żądanie. 

      Zazwyczaj są dostępne w dokumentacji interfejsu API, gdzie można uzyskać informacje dotyczące żądania próbki **zlecenie**, **adres URL**, **nagłówki**, i **treści**pola. Na przykład, zobacz [dokumentacji interfejsu API z analizy tekstu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Importuj przykładowe żądanie](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > W przypadku utworzenia łącznika z kolekcji Postman, upewnij się, że należy usunąć `Content-type` nagłówka z działań i wyzwalaczy. Logic Apps automatycznie dodaje tego nagłówka. Ponadto usunąć nagłówki uwierzytelniania, które zostały zdefiniowane w `Security` sekcji z działań i wyzwalaczy.

      Aby zaawansowane funkcje OpenAPI, zobacz [OpenAPI rozszerzenia niestandardowego łączników](../logic-apps/custom-connector-openapi-extensions.md).

   3. Aby zakończyć definicję żądania, wybierz **importu**.

4. Teraz można zdefiniować odpowiedzi dla akcji.

   1. W obszarze **odpowiedzi**, można określić odpowiedź domyślna. 
   Wybierz pozycję **Dodaj odpowiedź domyślną**.

   2. Na **importu z próbki** strony, Wklej przykładowa odpowiedź, a następnie wybierz **importu**.

5. Ponadto w obszarze **weryfikacji**, przejrzyj i rozwiąż wszelkie potencjalne problemy zidentyfikowane dla akcji.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Edytuj lub Dodaj Wyzwalacze dla Twojego łącznika

1. Aby dokonać edycji istniejącego wyzwalacza, wybierz numer dla tego wyzwalacza. Aby dodać wyzwalacz, który nie istnieje w pliku OpenAPI lub Postman kolekcji, w obszarze **wyzwalaczy**, wybierz **nowego wyzwalacza**.

2. W obszarze **ogólne**, podaj lub edytować te informacje dotyczące wyzwalacz:

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Podsumowanie** | *Nazwa operacji* | Tytuł dla tego wyzwalacza | 
   | **Opis** | *Opis operacji* | Opis dla tego wyzwalacza. <p>**Porada**: Upewnij się, że opis kończy się kropką. | 
   | **Identyfikator operacji** | *Identyfikator operacji* | Unikatowa nazwa identyfikowania tego wyzwalacza. Użyj formatu camelcase, na przykład: "TriggerOnGitHubPushEvent" | 
   |**Widoczność**| **Brak**, **zaawansowane**, **wewnętrzny**, lub **ważne** | Widoczność uwzględniającym działania użytkownika dla tego wyzwalacza. Aby uzyskać więcej informacji, zobacz [OpenAPI rozszerzenia](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Typ wyzwalacza** | **Element Webhook** lub **sondowania** | Typ dla tego wyzwalacza. Na przykład wyzwalacz elementu webhook czeka na określone zdarzenia przed uruchamiania. Wyzwalacz sondowania regularnie sprawdza punktu końcowego usługi na podstawie określonego interwału i częstotliwości. <p>Aby uzyskać więcej informacji o webhook i wzorce wyzwalacza sondowania, zobacz [Tworzenie niestandardowych interfejsów API](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. Teraz można zdefiniować żądania, która tworzy wyzwalacz. 

   1. W **żądania** wybierz **importu z próbki**.

   2. Na **importu z próbki** strony, Wklej przykładowe żądanie. 

      Zazwyczaj są dostępne w dokumentacji interfejsu API, gdzie można uzyskać informacje dotyczące żądania próbki **zlecenie**, **adres URL**, **nagłówki**, i **treści**pola. Na przykład, zobacz [dokumentacji interfejsu API z analizy tekstu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Importuj przykładowe żądanie](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > W przypadku utworzenia łącznika z kolekcji Postman, upewnij się, że należy usunąć `Content-type` nagłówka z działań i wyzwalaczy. Logic Apps automatycznie dodaje tego nagłówka. Ponadto usunąć nagłówki uwierzytelniania, które zostały zdefiniowane w `Security` sekcji z działań i wyzwalaczy.

      Aby zaawansowane funkcje OpenAPI, zobacz [OpenAPI rozszerzenia niestandardowego łączników](../logic-apps/custom-connector-openapi-extensions.md).

   3. Aby zakończyć definicję żądania, wybierz **importu**. 

4. Teraz można zdefiniować odpowiedzi tego wyzwalacza. W **odpowiedzi** sekcji, na podstawie typu wyzwalacza, wykonaj następujące kroki:

   **Wyzwalacz elementu Webhook**
   1. W **odpowiedzi elementu Webhook**, wybierz **importu z próbki**. 

   2. Na **importu z próbki** strony, Wklej przykładowa odpowiedź, a następnie wybierz **importu**. Aby wyświetlić przykład odpowiedzi, zobacz [dokumentacja interfejsu API GitHub do tworzenia elementu webhook](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. W obszarze **konfiguracji wyzwalacza**, wybierz parametr do użycia dla żądania tworzenia elementu webhook. Logic Apps używa wartości tego parametru podczas wypełniania adres URL wywołania zwrotnego używane przez usługę do komunikowania się z wyzwalacza.

   **Wyzwalacz sondowania**
   1. W obszarze **odpowiedzi**, można określić odpowiedź domyślna. 
   Wybierz pozycję **Dodaj odpowiedź domyślną**.

   2. Na **importu z próbki** strony, Wklej przykładowa odpowiedź, a następnie wybierz **importu**.

   3. W obszarze **konfiguracji wyzwalacza**, określ wartość do przekazania dla parametru parametru zapytania i *wskazówka wyzwalacza* , który określa prawidłowego interwał sondowania dla następnego żądania.

5. Ponadto w obszarze **weryfikacji**, przejrzyj i rozwiąż wszelkie potencjalne problemy zidentyfikowane dla wyzwalacza.

#### <a name="review-reference-definitions-for-your-connector"></a>Przejrzyj definicje odwołanie łącznika programu

**Odwołania** sekcji automatycznie wypełnia z opisu interfejsu API oraz opis pola żadnych parametrów, które akcje i wyzwalaczy może się odwoływać. 

1. W obszarze **odwołania**, wybierz numer definicji odwołania, który chcesz przejrzeć.

2. W obszarze **nazwa**, przejrzyj lub zaktualizuj nazwę definicji odwołania.

3. W obszarze **weryfikacji**, przejrzyj i rozwiąż wszelkie potencjalne problemy zidentyfikowane dla definicji odwołania.

## <a name="3-finish-creating-your-connector"></a>3. Kończenie tworzenia łącznika

Gdy wszystko jest gotowe, wybierz pozycję **Utwórz** aby umożliwić wdrażanie Twojego łącznika. Jeśli aktualizujesz istniejący łącznik, wybierz **zaktualizować łącznika**.

Gratulacje! Teraz w przypadku tworzenia aplikacji logiki możesz znaleźć łącznik w Projektancie aplikacji usługi Logic Apps i dodać go do aplikacji logiki.

![Wyszukiwanie łącznika w Projektancie aplikacji usługi Logic Apps](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Udostępnianie łącznika innym użytkownikom usługi Logic Apps

Zarejestrowane, ale niecertyfikowane łączniki działają tak jak łączniki zarządzane przez firmę Microsoft, ale są widoczne i dostępne *tylko* dla autora łącznika oraz użytkowników, którzy mają tę samą dzierżawę usługi Azure Active Directory i subskrypcję platformy Azure dla aplikacji logiki w regionie, w których te aplikacje są wdrażane. Udostępnianie jest opcjonalne, ale w niektórych scenariuszach udostępnienie łączników innym użytkownikom może być potrzebne. 

> [!IMPORTANT]
> Jeśli udostępnisz łącznik, praca innych osób może zacząć od niego zależeć. 
> ***Usunięcie łącznika powoduje usunięcie wszystkich połączeń z tym łącznikiem.***
 
Udostępniać użytkownikom Logic Apps, przepływu i rozwiązanie PowerApps użytkowników zewnętrznych poza te granice, na przykład z łącznika [przesłać Twojego łącznika do certyfikacji Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Często zadawane pytania

**Pyt.:** Czy istnieją limity łączników niestandardowych? </br>
**Odp.:** Tak, zobacz [Custom connector limits (Limity łączników niestandardowych)](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Aby uzyskać pomoc techniczną dotyczącą tworzenia i dołączania lub żądania funkcji, które nie są dostępne w kreatorze rejestracji, skontaktuj się z nami pod adresem [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Firma Microsoft monitoruje to konto pod kątem problemów i pytań deweloperów, a następnie kieruje je do odpowiednich zespołów.

* Aby zadawać pytania, odpowiadać na nie lub patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Aby pomóc w ulepszaniu usługi Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Kolejne kroki

* [Opcjonalnie: Certyfikować Twojego łącznika](../logic-apps/custom-connector-submit-certification.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)