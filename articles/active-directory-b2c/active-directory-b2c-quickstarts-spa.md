---
title: "Test stacji aplikacji jednostronicowej usługi Azure AD B2C | Dokumentacja firmy Microsoft"
description: "Testowanie Zaloguj, zarejestruj się edytowanie profilu i resetowania hasła użytkownika podróże przy użyciu środowiska testowego usługi Azure AD B2C"
services: active-directory-b2c
documentationcenter: 
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 5a8a46af-28bb-4b70-a7f0-01a5240d0255
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 22da1ae317ba685d32f93d3331cf794b568891ec
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-single-page-application-configured-with-azure-ad-b2c"></a>Testowanie aplikacji jednostronicowej, skonfigurowane w usłudze Azure AD B2C

## <a name="about-this-sample"></a>Informacje o tym przykładzie

Usługa Azure Active Directory B2C umożliwia zarządzanie tożsamości chmury do zachowania aplikacji, firmy oraz klientów chronionych.  Tego szybkiego startu używana przykładowej aplikacji jednej strony, aby zademonstrować:

* Przy użyciu **Utwórz konto lub zaloguj** zasad, aby utworzyć lub zaloguj się przy użyciu dostawcy tożsamości społecznościowych lub lokalnego konta przy użyciu adresu e-mail. 
* **Wywołanie interfejsu API** można pobrać nazwy wyświetlanej z usługi Azure AD B2C zabezpieczonej zasobów.

## <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
    - **Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych**

* Zainstalować środowisko [Node.js](https://nodejs.org/en/download/).

* Konto społecznościowych z usługi Facebook, Google, Microsoft lub Twitter. Jeśli nie masz konta społecznościowych, wymagany jest prawidłowy adres e-mail.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobrać lub sklonować przykładowej aplikacji](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) z usługi GitHub.

## <a name="run-the-sample-application"></a>Uruchom przykładową aplikację

Aby uruchomić ten przykład z poziomu wiersza polecenia Node.js: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

W oknie konsoli Pokazuje numer portu dla aplikacji sieci web uruchomionych na danym komputerze.

```
Listening on port 6420...
```

Otwórz `http://localhost:6420` w przeglądarce sieci web, aby uzyskać dostęp do aplikacji sieci web.


![Przykładową aplikację w przeglądarce](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Tworzenie konta usługi

Kliknij przycisk **logowania** przycisk, aby uruchomić usługi Azure AD B2C **Utwórz konto lub zaloguj** przepływu pracy. Podczas tworzenia konta, można użyć istniejącego konta dostawcy tożsamości społecznościowych lub konto e-mail.

### <a name="sign-up-using-a-social-identity-provider"></a>Zaloguj przy użyciu dostawcy tożsamości społecznościowych

Aby utworzyć konto przy użyciu dostawcy tożsamości społecznościowych, kliknij przycisk dostawcy tożsamości, który ma być używany. Jeśli wolisz używać adresu e-mail, należy przejść do [zaloguj przy użyciu adresu e-mail](#sign-up-using-an-email-address) sekcji.

![Dostawca logowania lub Utwórz konto](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

Musisz uwierzytelnić (logowania) za pomocą konta społecznościowych poświadczenia i zezwolić aplikacji na odczytywanie informacji z Twojego konta społecznościowych. Udzielenie dostępu, aplikacja może pobrać informacji o profilu konta społecznościowych, takie jak nazwa i Miasto. 

![Uwierzytelniania i autoryzacji przy użyciu konta społecznościowych](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

Szczegóły nowego profilu konta są wstępnie wypełnione informacjami z konta społecznościowych. 

![Nowe konto Szczegóły profilu rejestracji](media/active-directory-b2c-quickstarts-spa/new-account-sign-up-profile-details-spa.png)

Zaktualizuj pola Nazwa wyświetlana, stanowisko i Miasto, a następnie kliknij przycisk **Kontynuuj**.  Wartości, które należy wprowadzić służą do profilu konta użytkownika usługi Azure AD B2C.

Pomyślnie utworzono nowe konto użytkownika usługi Azure AD B2C, która używa dostawcy tożsamości. 

Następny krok: [wywołać zasobu](#call-a-resource) sekcji.

### <a name="sign-up-using-an-email-address"></a>Zaloguj przy użyciu adresu e-mail

Jeśli wybierzesz opcję rezygnacji z używania kont społecznościowych do zapewnienia uwierzytelniania, można utworzyć konta użytkownika usługi Azure AD B2C przy użyciu prawidłowy adres e-mail. Konto użytkownika lokalnego usługi Azure AD B2C używa usługi Azure Active Directory jako dostawcy tożsamości. Aby użyć adresu e-mail, kliknij przycisk **nie masz konta? Zarejestruj się teraz** łącza.

![Zaloguj się lub zaloguj się za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-email-spa.png)

Wprowadź prawidłowy adres e-mail i kliknij przycisk **wysłać kod weryfikacyjny**. Prawidłowy adres e-mail jest wymagany, aby uzyskać kod weryfikacyjny z usługi Azure AD B2C. 

Wprowadź kod weryfikacyjny wyświetlany w wiadomości e-mail i kliknij przycisk **Sprawdź kod**.

Dodawanie informacji o Twoim profilu, a następnie kliknij przycisk **Utwórz**.

![Logowanie się przy nowe konto, za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-spa/sign-up-new-account-profile-email-web.png)

Pomyślnie utworzono nowe konto użytkownika lokalnego dla usługi Azure AD B2C.

## <a name="call-a-resource"></a>Wywołaj zasobu

Po zalogowaniu możesz kliknąć **wywołania interfejsu API sieci Web** przycisk, aby mieć nazwy wyświetlanej zwracana z wywołania interfejsu API sieci Web jako obiekt JSON. 

![Interfejs API sieci Web odpowiedzi](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

## <a name="next-steps"></a>Następne kroki

Następnym krokiem jest tworzenie dzierżawy usługi Azure AD B2C i konfigurowanie przykładowych przy użyciu dzierżawy. 

> [!div class="nextstepaction"]
> [Tworzenie dzierżawy usługi Azure Active Directory B2C w portalu Azure](active-directory-b2c-get-started.md)