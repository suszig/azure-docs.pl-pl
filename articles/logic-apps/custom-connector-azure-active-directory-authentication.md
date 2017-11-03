---
title: "Zabezpieczanie łączników niestandardowych za pomocą usługi Azure AD - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dodawanie zabezpieczeń i uwierzytelniania do interfejsu API i łącznika z usługą Azure Active Directory (Azure AD)"
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
ms.openlocfilehash: bcee842cb880002daaa3ae9d9110ee1734941b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Dodawanie zabezpieczeń do interfejsu API i łącznika z usługą Azure Active Directory (Azure AD)

Do zabezpieczenia połączenia między interfejsu API i łącznik niestandardowy, należy dodać uwierzytelniania usługi Azure AD do interfejsu API sieci Web i z łącznika. W tym scenariuszu Utwórz dwie aplikacje usługi Azure Active Directory (Azure AD) — jedną aplikację usługi Azure AD zabezpiecza interfejsu API sieci Web, natomiast na inną aplikację usługi Azure AD zabezpiecza rejestracji łącznika i dodaje delegowany dostęp. 

Ten samouczek używa interfejsu API Menedżera zasobów Azure jako przykład. Usługa Azure Resource Manager ułatwia zarządzanie składniki rozwiązania, które zostały zbudowane na platformie Azure, takich jak bazy danych, maszyn wirtualnych i aplikacji sieci web. Niestandardowego łącznika usługi Azure Resource Manager mogą być przydatne, gdy użytkownik chce zarządzać zasobami Azure z przepływów pracy. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, można uruchomić z poziomu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestrować się w celu [subskrypcji płatność za rzeczywiste użycie](https://azure.microsoft.com/pricing/purchase-options/).

* W tym samouczku [przykładowy plik OpenAPI dla usługi Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Przykładowy plik OpenAPI nie definiuje wszystkie operacje usługi Azure Resource Manager i obecnie zawiera tylko operacja dla [listy wszystkie subskrypcje](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Możesz edytować ten OpenAPI lub Utwórz innego pliku OpenAPI przy użyciu [edytora online OpenAPI](http://editor.swagger.io/).
  >
  > W tym samouczku można stosować do jakiegokolwiek interfejsu API RESTful, której chcesz użyć uwierzytelniania usługi Azure AD.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Tworzenie pierwszej aplikacji usługi Azure AD dla zabezpieczanie interfejsu API sieci Web

Pierwszej aplikacji usługi Azure AD przeprowadza uwierzytelnianie, gdy łącznik niestandardowy wywołania interfejsu API, który jest interfejs API Menedżera zasobów Azure, w tym przykładzie.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Jeśli masz więcej niż jednej dzierżawy usługi Azure Active Directory, upewnij się, że zalogowano Cię w poprawnym katalogu sprawdzając katalogu, w obszarze nazwy użytkownika. 

   ![Potwierdź katalogu](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Aby zmienić katalogów, wybierz nazwę użytkownika, a następnie wybierz katalog, który ma.

2. W menu głównym Azure wybierz **usługi Azure Active Directory** , aby wyświetlić bieżący katalog.

   ![Wybierz polecenie "Azure Active Directory"](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Jeśli nie wyświetla menu głównego Azure **usługi Azure Active Directory**, wybierz **więcej usług**. W **filtru** polu, wpisz "Azure Active Directory" jako filtr, a następnie wybierz **usługi Azure Active Directory**.

3. W menu katalogu w obszarze **Zarządzaj**, wybierz **rejestracji aplikacji**. Na liście zarejestrowanych aplikacji wybierz **+ nowej rejestracji aplikacji**.

   ![Wybierz pozycję "Rejestracji aplikacji", "+ nowej rejestracji aplikacji"](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. W obszarze **Utwórz**, podaj szczegóły dla aplikacji usługi Azure AD, zgodnie z opisem w tabeli, a następnie wybierz **Utwórz**. 

   ![Tworzenie aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *sieci Web-api-app-name* | Nazwa aplikacji usługi Azure AD Twojego API sieci Web | 
   | **Typ aplikacji** | **Aplikacja sieci Web / interfejs API** | Typ aplikacji | 
   | **Adres URL logowania** | `https://login.windows.net` | | 
   |||| 

5. Po powrocie do swojego katalogu **rejestracji aplikacji** listy, wybierz aplikację usługi Azure AD.

   ![Wybierz z listy aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Po wyświetleniu strony szczegółów aplikacji, upewnij się, że możesz **skopiuj i Zapisz aplikacji *identyfikator aplikacji* gdzieś bezpieczne**. Ten identyfikator jest konieczne do późniejszego użycia.

   ![Zapisz "Identyfikator aplikacji" do późniejszego użycia](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Teraz należy wprowadzić adres URL odpowiedzi dla aplikacji usługi Azure AD. W aplikacji **ustawienia** menu, wybierz **adresy URL odpowiedzi**. Wprowadź adres URL, a następnie wybierz pozycję **zapisać**.

   ![Adresy URL odpowiedzi](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Adresy URL odpowiedzi** | Dla własnego interfejsu API wprowadź ten adres URL: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Delegowane uprawnienia** | {nie jest konieczna} | | 
   | **Klucz klienta** | {nie jest konieczna} | | 
   |||| 

   > [!TIP]
   > Jeśli **ustawienia** wcześniej pojawił się w menu, wybierz **ustawienia** tutaj:
   >
   > ![Wybierz pozycję "Ustawienia"](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Utwórz aplikację usługi Azure AD drugi dla łącznik niestandardowy

Drugi aplikacji usługi Azure AD zabezpiecza rejestracji niestandardowego łącznika i dodaje delegowany dostęp do interfejsu API sieci Web chronionych przez pierwszej aplikacji usługi Azure AD. 

> [!IMPORTANT]
> Upewnij się, że zarówno aplikacji usługi Azure AD istnieje w tym samym katalogu.

1. Wróć do **rejestracji aplikacji** listy, a następnie wybierz pozycję **+ nowej rejestracji aplikacji** ponownie.

   ![Wybierz pozycję "Rejestracji aplikacji", "+ nowej rejestracji aplikacji"](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. W obszarze **Utwórz**, podaj szczegóły drugi aplikacji usługi Azure AD, zgodnie z opisem w tabeli, a następnie wybierz **Utwórz**. 

   ![Tworzenie aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Nazwa** | *Twoja nazwa łącznika* | Nazwa aplikacji usługi Azure AD Twojego łącznika | 
   | **Typ aplikacji** | **Aplikacja sieci Web / interfejs API** | Typ aplikacji | 
   | **Adres URL logowania** | `https://login.windows.net` | | 
   |||| 

3. Po powrocie do swojego katalogu **rejestracji aplikacji** Wybierz drugi aplikacji usługi Azure AD.

   ![Wybierz z listy drugi aplikacji usługi Azure AD](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Po wyświetleniu strony szczegółów aplikacji, upewnij się, że możesz również **skopiuj i Zapisz tę aplikację *identyfikator aplikacji* gdzieś bezpieczne zbyt**. Ten identyfikator jest konieczne do późniejszego użycia.

   ![Zapisz "Identyfikator aplikacji" do późniejszego użycia](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Teraz należy wprowadzić adres URL odpowiedzi dla aplikacji usługi Azure AD. W aplikacji **ustawienia** menu, wybierz **adresy URL odpowiedzi**. Wprowadź adres URL, a następnie wybierz pozycję **zapisać**.

   | Ustawienie | Sugerowana wartość | 
   | ------- | --------------- | 
   | **Adresy URL odpowiedzi** | Łącznik niestandardowych usługi Azure Resource Manager należy wprowadzić ten adres URL:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Jeśli **ustawienia** wcześniej pojawił się w menu, wybierz **ustawienia** tutaj:
   >
   > ![Wybierz pozycję "Ustawienia"](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. W **ustawienia** menu, wybierz **wymagane uprawnienia** > **Dodaj**.

   ![Wymagane uprawnienia > Dodaj](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Gdy **dostępu Dodaj interfejsu API** zostanie otwarte menu, wybierz **wybierz interfejs API** > 
**interfejs API zarządzania usługami Windows Azure** > **wybierz **.

   ![Wybierz interfejs API](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Na **dostępu Dodaj interfejsu API** menu, wybierz **wybierz uprawnienia**. W obszarze **delegowane uprawnienia**, wybierz **dostęp do zarządzania usługą Azure jako organizacja użytkowników** > **wybierz**.

   ![Wybierz polecenie "Delegowani uprawnienia" > "dostęp do zarządzania usług platformy Azure jako organizacja użytkowników"](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   W przeciwnym razie dla innych opcji:

   | Opcja | Sugerowana wartość | Opis | 
   | ------ | --------------- | ----------- | 
   | **Delegowane uprawnienia** | | Wybierz uprawnienia delegowane dostępu do interfejsu API sieci Web | 
   |||| 

9. Obecnie **dostępu Dodaj interfejsu API** menu, wybierz **gotowe**.

   ![Menu "Dodaj dostępu do interfejsu API" > "Gotowe"](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Teraz Generowanie *klucz klienta*, lub "tajne dla aplikacji usługi Azure AD Twojego łącznika". 

    1. Ponownie **ustawienia** menu, wybierz **klucze**. 
    Podaj nazwę klucza z 16 znaków, wybierz okres ważności, a następnie wybierz **zapisać**.

       ![Utwórz klucz klienta](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Po wyświetleniu klucz wygenerowany **upewnij się, skopiować, a następnie zapisz ten klucz innym bezpiecznym** przed opuszczeniem **klucze** strony.
    
       ![Ręcznie skopiuj i Zapisz klucz](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Po zapisaniu klucza, możesz bezpiecznie zamknąć **ustawienia** menu.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Dodawanie uwierzytelniania usługi Azure AD do aplikacji interfejsu API sieci Web

Teraz należy włączyć uwierzytelnianie dla interfejsu API sieci Web z pierwszej aplikacji usługi Azure AD. Aby uzyskać więcej informacji, Dowiedz się [sposobu konfigurowania aplikacji usługi aplikacji do użycia usługi Azure Active Directory logowania](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. W [portalu Azure](https://portal.azure.com), Znajdź aplikację interfejsu API sieci Web, który wcześniej publikowane w [Tworzenie niestandardowych łączniki z interfejsów API sieci Web](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. W obszarze **ustawienia**, wybierz **uwierzytelniania / autoryzacji**. 

   1. W obszarze **aplikacji uwierzytelniania usługi**, wybierz **na**.
   2. Aby uzyskać **działania należy podjąć w przypadku nieuwierzytelnionego żądania**, wybierz pozycję **Zaloguj się za pomocą usługi Azure Active Directory**.
   3. Aby uzyskać **dostawców uwierzytelniania**, wybierz pozycję **usługi Azure Active Directory**. 

   ![Wybierz opcję "uwierzytelnianie / autoryzacja"](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Na **ustawień usługi Azure Active Directory** strony:

   1. W obszarze **tryb zarządzania**, wybierz **Express**.

   2. Teraz wybierz aplikację usługi Azure AD, która do uwierzytelniania używa aplikacja interfejsu API sieci Web. 
   Wybierz **wybierz istniejącą aplikację usługi AD** > **aplikacji usługi Azure AD**.

   3. W obszarze **aplikacji usługi Azure AD**, wybierz aplikację usługi Azure AD, która wcześniej utworzony dla twojej aplikacji interfejsu API sieci Web. 
   
   4. Wybierz **OK** aby powrócić do **uwierzytelniania / autoryzacji** strony.

   Na przykład:

   ![Wybierz aplikację usługi Azure AD, która reprezentuje aplikacji interfejsu API sieci Web](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Na **uwierzytelniania / autoryzacji** wybierz pozycję **zapisać**.

   ![Zapisz ustawienia uwierzytelniania](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Aplikacja interfejsu API sieci Web można teraz używać usługi Azure AD do uwierzytelniania.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Konfigurowanie uwierzytelniania usługi Azure AD w pliku OpenAPI API sieci Web

Otwórz plik OpenAPI dla aplikacji interfejsu API sieci Web, tak aby można było dodać `securityDefintions` obiektu i uwierzytelniania usługi Azure AD w obszarze `host` właściwości. Oto przykład pokazujący `host` właściwości i `securityDefinitions` obiektu:

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

## <a name="next-steps"></a>Następne kroki

* [Rejestrowanie łącznika](../logic-apps/logic-apps-custom-connector-register.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)
