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
ms.openlocfilehash: 9e3d88abe751b37700590cc68c458f208d5868d2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Zarejestruj łączników niestandardowych w aplikacjach logiki platformy Azure

Po utworzeniu interfejsu API REST, konfigurowanie uwierzytelniania i uzyskać plik definicji OpenAPI lub kolekcji Postman, możesz zarejestrować Twojego łącznika. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby zarejestrować łącznik niestandardowy, potrzebne są następujące elementy:

* Szczegóły rejestracji Twojego łącznika na platformie Azure, takie jak nazwa, subskrypcji platformy Azure, grupy zasobów platformy Azure i lokalizacji, w której chcesz użyć

* Plik OpenAPI (Swagger) lub kolekcji Postman, który opisuje interfejs API

  W tym samouczku, można użyć [przykładowy plik OpenAPI Menedżera zasobów Azure](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Ikona, który reprezentuje łącznik programu

* Krótki opis programu łącznika

* Lokalizacja hosta do interfejsu API

## <a name="1-create-your-connector"></a>1. Utwórz łącznik programu

1. W portalu Azure, w menu głównym Azure wybierz **nowy**. W polu wyszukiwania wprowadź "łącznik aplikacje logiki" jako filtr, a następnie naciśnij klawisz Enter.

   ![Nowe Wyszukaj "łącznik aplikacje logiki"](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Na liście wyników wybierz **łącznik aplikacji logiki** > **Utwórz**.

   ![Tworzenie łącznika Logic Apps](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Podaj szczegóły rejestrowaniu Twojego łącznika zgodnie z opisem w tabeli. Gdy wszystko będzie gotowe, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Szczegóły łącznika niestandardowych aplikacji logiki](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Właściwość | Sugerowana wartość | Opis | 
   | -------- | --------------- | ----------- | 
   | **Nazwa** | *Nazwa niestandardowego łącznika* | Podaj nazwę dla Twojego łącznika. | 
   | **Subskrypcja** | *Nazwa w przypadku subskrypcji platformy Azure* | Wybierz subskrypcję platformy Azure. | 
   | **Grupa zasobów** | *Azure-resource grupy name* | Utwórz lub wybierz grupę Azure służący do organizowania zasobów platformy Azure. | 
   | **Lokalizacja** | *region wdrożenia* | Wybierz region wdrożenia dla Twojego łącznika. | 
   |||| 

   Po Azure wdraża z łącznika, automatycznie zostanie otwarte menu łącznik niestandardowy. 
   W przeciwnym razie wybierz łącznik niestandardowy z pulpitu nawigacyjnego platformy Azure.

## <a name="2-define-your-connector"></a>2. Zdefiniuj Twojego łącznika

Teraz określić plik OpenAPI lub kolekcji Postman tworzenia z łącznika, uwierzytelniania, który korzysta z łącznika, akcje i wyzwalacze, które zawiera łącznik niestandardowy i parametrów, które można używać działań i wyzwalaczy.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. Określ plik OpenAPI lub kolekcji Postman dla Twojego łącznika

1. W menu z łącznika, jeśli jeszcze nie jest wybrana, wybierz **łącznik aplikacji logiki**. Na pasku narzędzi wybierz **Edytuj**.

   ![Edytuj łącznik niestandardowy](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Wybierz **ogólne** tak, aby uzyskać szczegółowe informacje w poniższych tabelach zapewniają do tworzenia, zabezpieczanie i definiowanie działań i wyzwalacze łącznik niestandardowy.

   1. Dla **łączniki niestandardowe**, wybierz opcję umożliwiającą dostarczanie pliku OpenAPI (Swagger), który opisuje interfejs API.

      ![Określ plik OpenAPI do interfejsu API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Opcja | Format |Opis | 
      | ------ | ------ | ----------- | 
      | **Przekaż plik OpenAPI** | *OpenAPI (Swagger) - json — plik* | Przejdź do lokalizacji pliku OpenAPI, a następnie wybierz tego pliku. | 
      | **Użyj adresu URL OpenAPI** | http://*ścieżka struktury swagger-json — plik* | Podaj adres URL dla pliku OpenAPI Twój interfejs API. | 
      | **Przekaż kolekcji Postman V1** | *wyeksportowane Postman kolekcji V1-pliku* | Wskaż lokalizację dla eksportowanego kolekcji Postman w formacie w wersji 1. | 
      |||| 

   2. Aby uzyskać **ogólne informacje**, Przekaż ikonę dla Twojego łącznika. 
   Zazwyczaj **opis**, **hosta**, i **podstawowego adresu URL** pola zostaną automatycznie wypełnione z pliku OpenAPI. 
   Ale jeśli nie, Dodaj te informacje, zgodnie z opisem w tabeli i wybierz polecenie **Kontynuuj**. 

      ![Szczegóły łącznika](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Opcja lub ustawienia | Format | Opis | 
      | ----------------- | ------ | ----------- | 
      | **Przekaż ikonę** | *PNG-or-jpg-File-under-1-MB* | Ikona, który reprezentuje łącznik programu <p>Kolor: Najlepiej białe logo względem koloru tła. <p>Wymiary: Logo ~ 160 pikseli wewnątrz kwadratu 230 pikseli | 
      | **Kolor tła ikon** | *Ikona marki — kolor — szesnastkowy — kod* | <p>Kolor tła z ikonę, która odpowiada kolor tła w pliku ikony. <p>Format: szesnastkową. Na przykład #007ee5 reprezentuje kolor niebieski. | 
      | **Opis** | *Opis łącznika* | Podaj krótki opis Twojego łącznika. | 
      | **Host** | *Łącznik hosta* | Podaj domeny hosta dla interfejsu API sieci Web. | 
      | **Podstawowy adres URL** | *Łącznik podstawowego adresu URL* | Podaj podstawowy adres URL dla interfejsu API sieci Web. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Opis uwierzytelniania, który korzysta z łącznika

1. Teraz wybierz **zabezpieczeń** , możesz przejrzeć lub opisano uwierzytelniania, który korzysta z łącznika. Uwierzytelnianie zapewnia odpowiednio przepływu tożsamości użytkowników między usługą i wszystkich klientów.

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

2. Aby zapisać Twoje łącznika po wprowadzeniu informacji o zabezpieczeniach, w górnej części strony, wybierz polecenie **zaktualizować łącznika**, a następnie wybierz **Kontynuuj**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Przeglądanie, zaktualizować lub zdefiniuj akcje i Wyzwalacze dla Twojego łącznika

1. Teraz wybierz **definicji** , można przejrzeć, edytować lub zdefiniuj nowe akcje i wyzwalacze, które użytkownicy mogą dodawać do ich przepływów pracy.

   Akcje i wyzwalacze są oparte na operacje zdefiniowane w pliku OpenAPI lub Postman kolekcji, które automatycznie wypełnić **definicji** obejmują żądania i odpowiedzi wartości i strony. Tak Jeśli wymagane operacje już występować w tym miejscu, można przejść do następnego kroku w procesie rejestracji bez wprowadzania zmian na tej stronie.

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
   Wybierz **Dodaj domyślny**.

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
   Wybierz **Dodaj domyślny**.

   2. Na **importu z próbki** strony, Wklej przykładowa odpowiedź, a następnie wybierz **importu**.

   3. W obszarze **konfiguracji wyzwalacza**, określ wartość do przekazania dla parametru parametru zapytania i *wskazówka wyzwalacza* , który określa prawidłowego interwał sondowania dla następnego żądania.

5. Ponadto w obszarze **weryfikacji**, przejrzyj i rozwiąż wszelkie potencjalne problemy zidentyfikowane dla wyzwalacza.

#### <a name="review-reference-definitions-for-your-connector"></a>Przejrzyj definicje odwołanie łącznika programu

**Odwołania** sekcji automatycznie wypełnia z opisu interfejsu API oraz opis pola żadnych parametrów, które akcje i wyzwalaczy może się odwoływać. 

1. W obszarze **odwołania**, wybierz numer definicji odwołania, który chcesz przejrzeć.

2. W obszarze **nazwa**, przejrzyj lub zaktualizuj nazwę definicji odwołania.

3. W obszarze **weryfikacji**, przejrzyj i rozwiąż wszelkie potencjalne problemy zidentyfikowane dla definicji odwołania.

## <a name="3-finish-creating-your-connector"></a>3. Zakończ tworzenie łącznika programu

Gdy wszystko jest gotowe, wybierz pozycję **Utwórz** aby umożliwić wdrażanie Twojego łącznika. Jeśli aktualizujesz istniejący łącznik, wybierz **zaktualizować łącznika**.

Gratulacje! Teraz podczas tworzenia aplikacji logiki można znaleźć z łącznika w Projektancie aplikacji logiki i Dodaj ten łącznik do aplikacji logiki.

![W Projektancie aplikacji logiki znajdź łącznik programu](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Udostępnianie Twojego łącznika innym użytkownikom Logic Apps

Zarejestrowane, ale Niepoświadczone łączników niestandardowych działają podobnie łączniki zarządzany przez firmę Microsoft, ale są widoczne i dostępne *tylko* autora łącznika i użytkowników, którzy mają tej samej dzierżawy usługi Azure Active Directory i subskrypcji platformy Azure w przypadku aplikacji logiki w regionie, w których te aplikacje są wdrożone. Mimo że udostępnianie jest opcjonalny, konieczne może być scenariuszy, w którym chcesz udostępnić innym użytkownikom łączników. 

> [!IMPORTANT]
> Jeśli łącznik zostanie udostępniona, inne rozpoczęcie zależą od tego łącznika. 
> ***Usunięcie łącznika programu powoduje usunięcie wszystkich połączeń z tego łącznika.***
 
Udostępniać użytkownikom Logic Apps, przepływu i rozwiązanie PowerApps użytkowników zewnętrznych poza te granice, na przykład z łącznika [przesłać Twojego łącznika do certyfikacji Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Często zadawane pytania

**Pytanie:** istnieją żadnych limitów dla łączników niestandardowych? </br>
**Odpowiedź:** tak, zobacz [łącznik niestandardowy ogranicza tutaj](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Obsługa tworzenia i dołączania lub do żądania funkcji, które nie są dostępne w kreatorze Rejestracja, skontaktuj się z [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Firma Microsoft monitoruje tego konta na problemów i pytania deweloperów i kieruje je do odpowiednich zespołu.

* Aby zadać lub odpowiedzi na pytania, lub zobacz, co robią innych użytkowników usługi Azure Logic Apps, odwiedź stronę [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Aby pomóc w udoskonalaniu Logic Apps, Zagłosuj lub Prześlij pomysłów na [witrynę opinii użytkowników Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Następne kroki

* [Opcjonalnie: Certyfikować Twojego łącznika](../logic-apps/custom-connector-submit-certification.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)