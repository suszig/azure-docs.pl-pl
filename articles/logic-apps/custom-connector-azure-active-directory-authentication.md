---
title: "Zabezpieczanie łączników niestandardowych za pomocą usługi Azure AD - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dodawanie zabezpieczeń i uwierzytelniania do interfejsu API i łącznika przy użyciu usługi Azure Active Directory (Azure AD)"
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
ms.openlocfilehash: 5fd59677fc6abf80666caf90aa3da7e553648bca
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Dodawanie zabezpieczeń do interfejsu API i łącznika z usługą Azure Active Directory (Azure AD)

Aby zabezpieczyć połączenia między interfejsem API i łącznikiem niestandardowym, należy dodać uwierzytelnianie usługi Azure AD do internetowego interfejsu API i łącznika. W tym scenariuszu tworzysz dwie aplikacje usługi Azure Active Directory (Azure AD) — jedna aplikacja usługi Azure AD zabezpiecza internetowy interfejs API, a druga aplikacja usługi Azure AD zabezpiecza rejestrację łącznika i dodaje delegowany dostęp. 

Ten samouczek używa jako przykładu interfejsu API usługi Azure Resource Manager. Usługa Azure Resource Manager ułatwia zarządzanie składnikami rozwiązania skompilowanego na platformie Azure, takimi jak bazy danych, maszyny wirtualne i aplikacje internetowe. Łącznik niestandardowy usługi Azure Resource Manager może być przydatny, jeśli chcesz zarządzać zasobami platformy Azure z poziomu przepływów pracy. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz rozpocząć pracę z [bezpłatnym kontem platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestruj się, aby [skorzystać z subskrypcji z płatnością zgodnie z rzeczywistym użyciem](https://azure.microsoft.com/pricing/purchase-options/).

* W tym samouczku [przykładowy plik OpenAPI dla usługi Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Przykładowy plik OpenAPI nie definiuje wszystkich operacji usługi Azure Resource Manager i obecnie zawiera tylko operację [tworzenia listy wszystkich subskrypcji](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Możesz edytować ten plik OpenAPI lub utworzyć inny plik OpenAPI przy użyciu [edytora plików OpenAPI w trybie online](http://editor.swagger.io/).
  >
  > Ten samouczek można zastosować do dowolnego interfejsu API RESTful, w którym chcesz użyć uwierzytelniania usługi Azure AD.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Tworzenie pierwszej aplikacji usługi Azure AD na potrzeby zabezpieczania internetowego interfejsu API

Pierwsza aplikacja usługi Azure AD przeprowadza uwierzytelnianie, gdy łącznik niestandardowy wywołuje interfejs API — w tym przykładzie interfejs API usługi Azure Resource Manager.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Jeśli masz więcej niż jedną dzierżawę usługi Azure Active Directory, upewnij się, że nastąpiło zalogowanie do prawidłowego katalogu, sprawdzając katalogi powiązane z Twoją nazwą użytkownika. 

   ![Potwierdzanie katalogu](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Aby zmienić katalogi, wybierz swoją nazwę użytkownika, a następnie wybierz nowy katalog.

2. W menu głównym platformy Azure wybierz pozycję **Azure Active Directory**, aby wyświetlić bieżący katalog.

   ![Wybieranie pozycji „Azure Active Directory”](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Jeśli nie wyświetla menu głównego Azure **usługi Azure Active Directory**, wybierz **wszystkie usługi**. W polu **Filtr** wpisz ciąg „Azure Active Directory” jako filtr, a następnie wybierz pozycję **Azure Active Directory**.

3. W menu katalogu w obszarze **Zarządzanie** wybierz pozycję **Rejestracje aplikacji**. Na liście zarejestrowanych aplikacji wybierz pozycję **+ Rejestrowanie nowej aplikacji**.

   ![Wybieranie pozycji „Rejestracje aplikacji” i „+ Rejestrowanie nowej aplikacji”](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. W obszarze **Tworzenie** podaj szczegóły aplikacji usługi Azure AD zgodnie z opisem w tabeli, a następnie wybierz pozycję **Utwórz**. 

   ![Tworzenie aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *nazwa-aplikacji-internetowego-interfejsu-api* | Nazwa aplikacji usługi Azure AD dla internetowego interfejsu API | 
   | **Typ aplikacji** | **Aplikacja internetowa/interfejs API** | Typ Twojej aplikacji | 
   | **Adres URL logowania** | `https://login.windows.net` | | 
   |||| 

5. Po powrocie do listy **Rejestracje aplikacji** w swoim katalogu wybierz aplikację usługi Azure AD.

   ![Wybieranie aplikacji usługi Azure AD z listy](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Po wyświetleniu strony szczegółów aplikacji upewnij się, że możesz **skopiować i zapisać *identyfikator aplikacji* w bezpiecznym miejscu**. Ten identyfikator będzie potrzebny później.

   ![Zapisywanie wartości „Identyfikator aplikacji” do późniejszego użycia](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Teraz podaj adres URL odpowiedzi dla aplikacji usługi Azure AD. W menu **Ustawienia** aplikacji wybierz pozycję **Adresy URL odpowiedzi**. Wprowadź ten adres URL, a następnie wybierz pozycję **Zapisz**.

   ![Adresy URL odpowiedzi](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Adresy URL odpowiedzi** | Dla własnego interfejsu API wprowadź ten adres URL: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Uprawnienia delegowane** | {niewymagane} | | 
   | **Klucz klienta** | {niewymagane} | | 
   |||| 

   > [!TIP]
   > Jeśli menu **Ustawienia** nie było wcześniej wyświetlane, wybierz w tym miejscu pozycję **Ustawienia**:
   >
   > ![Wybieranie pozycji „Ustawienia”](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Tworzenie drugiej aplikacji usługi Azure AD dla łącznika niestandardowego

Druga aplikacja usługi Azure AD zabezpiecza rejestrację łącznika niestandardowego i dodaje delegowany dostęp do internetowego interfejsu API chronionego przez pierwszą aplikację usługi Azure AD. 

> [!IMPORTANT]
> Upewnij się, że obydwie aplikacje usługi Azure AD znajdują się w tym samym katalogu.

1. Wróć do listy **Rejestracje aplikacji**, a następnie ponownie wybierz pozycję **+ Rejestrowanie nowej aplikacji**.

   ![Wybieranie pozycji „Rejestracje aplikacji” i „+ Rejestrowanie nowej aplikacji”](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. W obszarze **Tworzenie** podaj szczegóły drugiej aplikacji usługi Azure AD zgodnie z opisem w tabeli, a następnie wybierz pozycję **Utwórz**. 

   ![Tworzenie aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *nazwa-łącznika* | Nazwa aplikacji usługi Azure AD łącznika | 
   | **Typ aplikacji** | **Aplikacja internetowa/interfejs API** | Typ Twojej aplikacji | 
   | **Adres URL logowania** | `https://login.windows.net` | | 
   |||| 

3. Po powrocie do listy **Rejestracje aplikacji** w swoim katalogu wybierz drugą aplikację usługi Azure AD.

   ![Wybieranie drugiej aplikacji usługi Azure AD z listy](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Po wyświetleniu strony szczegółów aplikacji upewnij się, że możesz również **skopiować i zapisać *identyfikator tej aplikacji* w bezpiecznym miejscu**. Ten identyfikator będzie potrzebny później.

   ![Zapisywanie wartości „Identyfikator aplikacji” do późniejszego użycia](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Teraz podaj adres URL odpowiedzi dla aplikacji usługi Azure AD. W menu **Ustawienia** aplikacji wybierz pozycję **Adresy URL odpowiedzi**. Wprowadź ten adres URL, a następnie wybierz pozycję **Zapisz**.

   | Ustawienie | Sugerowana wartość | 
   | ------- | --------------- | 
   | **Adresy URL odpowiedzi** | W przypadku łącznika niestandardowego usługi Azure Resource Manager wprowadź ten adres URL: `https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Jeśli menu **Ustawienia** nie było wcześniej wyświetlane, wybierz w tym miejscu pozycję **Ustawienia**:
   >
   > ![Wybieranie pozycji „Ustawienia”](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. W menu **Ustawienia** wybierz kolejno pozycje **Wymagane uprawnienia** > **Dodaj**.

   ![Wymagane uprawnienia > Dodaj](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Po otwarciu menu **Dodaj dostęp do interfejsu API** wybierz kolejno pozycje **Wybierz interfejs API** > 
**Interfejs API zarządzania usługą Windows Azure** > **Wybierz**.

   ![Wybieranie interfejsu API](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. W menu **Dodaj dostęp do interfejsu API** wybierz pozycję **Wybierz uprawnienia**. W obszarze **Uprawnienia delegowane** wybierz pozycję **Dostęp do usługi Azure Service Management jako użytkownicy organizacji** > **Wybierz**.

   ![Wybieranie pozycji „Uprawnienia delegowane” > „Dostęp do usługi Azure Service Management jako użytkownicy organizacji”](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   W przeciwnym razie w przypadku innych opcji:

   | Opcja | Sugerowana wartość | Opis | 
   | ------ | --------------- | ----------- | 
   | **Uprawnienia delegowane** | | Wybierz uprawnienia delegowanego dostępu do internetowego interfejsu API. | 
   |||| 

9. Teraz w menu **Dodaj dostęp do interfejsu API** wybierz pozycję **Gotowe**.

   ![Menu „Dodaj dostęp do interfejsu API” > „Gotowe”](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Teraz wygeneruj *klucz klienta* lub „wpis tajny” dla aplikacji usługi Azure AD łącznika. 

    1. Po powrocie do menu **Ustawienia** wybierz pozycję **Klucze**. 
    Podaj nazwę klucza składającą się z maksymalnie 16 znaków, wybierz okres ważności, a następnie wybierz pozycję **Zapisz**.

       ![Tworzenie klucza klienta](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Po wyświetleniu wygenerowanego klucza **upewnij się, że klucz został skopiowany i zapisany w bezpiecznym miejscu** przed opuszczeniem strony **Klucze**.
    
       ![Ręczne kopiowanie i zapisywanie klucza](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Po zapisaniu klucza możesz bezpiecznie zamknąć menu **Ustawienia**.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Dodawanie uwierzytelniania usługi Azure AD do aplikacji internetowej interfejsu API

Teraz włącz uwierzytelnianie internetowego interfejsu API przy użyciu pierwszej aplikacji usługi Azure AD. Aby uzyskać więcej informacji, Dowiedz się [sposobu konfigurowania aplikacji usługi aplikacji do użycia usługi Azure Active Directory logowania](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. W [portalu Azure](https://portal.azure.com), Znajdź aplikację interfejsu API sieci Web, który wcześniej publikowane w [Tworzenie niestandardowych łączniki z interfejsów API sieci Web](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. W obszarze **Ustawienia** wybierz pozycję **Uwierzytelnianie/autoryzacja**. 

   1. W obszarze **Uwierzytelnianie usługi App Service** wybierz pozycję **Włączone**.
   2. Z listy **Akcja do wykonania w przypadku nieuwierzytelnionego żądania** wybierz pozycję **Zaloguj się za pomocą usługi Azure Active Directory**.
   3. W obszarze **Dostawcy uwierzytelniania** wybierz pozycję **Azure Active Directory**. 

   ![Wybieranie pozycji „Uwierzytelnianie/autoryzacja”](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Na stronie **Ustawienia usługi Azure Active Directory**:

   1. W obszarze **Tryb zarządzania** wybierz pozycję **Ekspresowy**.

   2. Teraz wybierz aplikację usługi Azure AD używaną na potrzeby uwierzytelniania przez aplikację internetowego interfejsu API. 
   Wybierz pozycję **Wybierz istniejącą aplikację usługi AD** > **Aplikacja usługi Azure AD**.

   3. W obszarze **Aplikacje usługi Azure AD** wybierz aplikację usługi Azure AD, która została wcześniej utworzona dla Twojej aplikacji internetowego interfejsu API. 
   
   4. Wybieraj przycisk **OK** do momentu powrotu na stronę **Uwierzytelnianie/autoryzacja**.

   Na przykład:

   ![Wybieranie aplikacji usługi Azure AD, która reprezentuje aplikację internetowego interfejsu API](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Na stronie **Uwierzytelnianie/autoryzacja** wybierz pozycję **Zapisz**.

   ![Zapisywanie ustawień uwierzytelniania](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Teraz aplikacja internetowego interfejsu API może używać usługi Azure AD do uwierzytelniania.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Konfigurowanie uwierzytelniania usługi Azure AD w pliku OpenAPI internetowego interfejsu API

Otwórz plik OpenAPI dla aplikacji internetowego interfejsu API, tak aby można było dodać obiekt `securityDefintions` i uwierzytelnianie usługi Azure AD w obszarze właściwości `host`. Oto przykład pokazujący właściwość `host` i obiekt `securityDefinitions`:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
Teraz możesz utworzyć i zarejestrować łącznik niestandardowy.

## <a name="next-steps"></a>Kolejne kroki

* [Rejestrowanie łącznika](../logic-apps/logic-apps-custom-connector-register.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)
