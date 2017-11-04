---
title: "Test stacji pulpitu aplikacji usługi Azure AD B2C | Dokumentacja firmy Microsoft"
description: "Testowanie Zaloguj, zarejestruj się edytowanie profilu i resetowania hasła użytkownika podróże przy użyciu środowiska testowego usługi Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 9653d86dd5635016512bf6e6fafbf7195145ed07
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a>Testowanie aplikacji pulpitu skonfigurowane w usłudze Azure AD B2C

Usługa Azure Active Directory B2C umożliwia zarządzanie tożsamości chmury do zachowania aplikacji, firmy oraz klientów chronionych.  Ta opcja szybkiego startu używa przykładową aplikację pulpitu systemu Windows Presentation Foundation (WPF) aby zademonstrować:

* Przy użyciu **Utwórz konto lub zaloguj** zasad, aby utworzyć lub zaloguj się przy użyciu dostawcy tożsamości społecznościowych lub lokalnego konta przy użyciu adresu e-mail. 
* **Wywołanie interfejsu API** można pobrać nazwy wyświetlanej z usługi Azure AD B2C zabezpieczonej zasobów.

## <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
    - **.NET — rozwój pulpitu**

* Konto społecznościowych z usługi Facebook, Google, Microsoft lub Twitter. Jeśli nie masz konta społecznościowych, wymagany jest prawidłowy adres e-mail.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobrać lub sklonować przykładowej aplikacji](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) z usługi GitHub.

## <a name="run-the-app-in-visual-studio"></a>Uruchom aplikację w programie Visual Studio

W folderze projektu aplikacji przykładowej Otwórz `active-directory-b2c-wpf.sln` rozwiązania w programie Visual Studio. 

Wybierz **Debuguj > Rozpocznij debugowanie** Aby skompilować i uruchomić aplikację. 

## <a name="create-an-account"></a>Tworzenie konta usługi

Kliknij przycisk **Zaloguj** uruchomić **Utwórz konto lub zaloguj** przepływu pracy. Podczas tworzenia konta, można użyć istniejącego konta dostawcy tożsamości społecznościowych lub konto e-mail.

![Przykładowa aplikacja](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaloguj przy użyciu dostawcy tożsamości społecznościowych

Aby utworzyć konto przy użyciu dostawcy tożsamości społecznościowych, kliknij przycisk dostawcy tożsamości, który ma być używany. Jeśli wolisz używać adresu e-mail, należy przejść do [zaloguj przy użyciu adresu e-mail](#sign-up-using-an-email-address) sekcji.

![Dostawca logowania lub Utwórz konto](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

Musisz uwierzytelnić (logowania) za pomocą konta społecznościowych poświadczenia i zezwolić aplikacji na odczytywanie informacji z Twojego konta społecznościowych. Udzielenie dostępu, aplikacja może pobrać informacji o profilu konta społecznościowych, takie jak nazwa i Miasto. 

![Uwierzytelniania i autoryzacji przy użyciu konta społecznościowych](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

Szczegóły nowego profilu konta są wstępnie wypełnione informacjami z konta społecznościowych. Zmodyfikuj szczegóły, jeśli chcesz i kliknij przycisk **Kontynuuj**.

![Nowe konto Szczegóły profilu rejestracji](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

Pomyślnie utworzono nowe konto użytkownika usługi Azure AD B2C, która używa dostawcy tożsamości. Po zalogowaniu, token dostępu jest wyświetlany w obszarze *Token informacji* pola tekstowego. Token dostępu jest używany podczas uzyskiwania dostępu do zasobu interfejsu API.

![Token autoryzacji](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

Następny krok: [przeskoczyć do edycji profilu](#edit-your-profile) sekcji.

### <a name="sign-up-using-an-email-address"></a>Zaloguj przy użyciu adresu e-mail

Jeśli wybierzesz opcję rezygnacji z używania kont społecznościowych do zapewnienia uwierzytelniania, można utworzyć konta użytkownika usługi Azure AD B2C przy użyciu prawidłowy adres e-mail. Konto użytkownika lokalnego usługi Azure AD B2C używa usługi Azure Active Directory jako dostawcy tożsamości. Aby użyć adresu e-mail, kliknij przycisk **nie masz konta? Zarejestruj się teraz** łącza.

![Zaloguj się lub zaloguj się za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

Wprowadź prawidłowy adres e-mail i kliknij przycisk **wysłać kod weryfikacyjny**. Prawidłowy adres e-mail jest wymagany, aby uzyskać kod weryfikacyjny z usługi Azure AD B2C.

Wprowadź kod weryfikacyjny wyświetlany w wiadomości e-mail i kliknij przycisk **Sprawdź kod**.

Dodawanie informacji o Twoim profilu, a następnie kliknij przycisk **Utwórz**.

![Logowanie się przy nowe konto, za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

Pomyślnie utworzono nowe konto użytkownika lokalnego dla usługi Azure AD B2C. Po zalogowaniu, token dostępu jest wyświetlany w obszarze *Token informacji* pola tekstowego. Token dostępu jest używany podczas uzyskiwania dostępu do zasobu interfejsu API.

![Token autoryzacji](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a>Edytowanie profilu

Usługa Azure Active Directory B2C zawiera funkcję, aby umożliwić użytkownikom aktualizowanie ich profile. Kliknij przycisk **edytowanie profilu** Aby edytować to profil utworzony.

![Edytowanie profilu](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

Wybierz dostawcę tożsamości, skojarzone z utworzonego konta. Na przykład jeśli używasz usługi Twitter jako dostawca tożsamości, podczas tworzenia konta, wybierz pozycję Twitter, aby zmodyfikować szczegóły skojarzony profil.

![Wybierz dostawcę skojarzone z profilem do edycji](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

Zmień Twoje **Nazwa wyświetlana** lub **miasta**. 

![Aktualizuj profil](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

Nowy token dostępu jest wyświetlany w *Token informacji* pola tekstowego. Jeśli chcesz zweryfikować zmiany do swojego profilu, skopiuj i Wklej token dostępu do https://jwt.ms dekodera tokenów.

![Token autoryzacji](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a>Dostęp do zasobu

Kliknij przycisk **wywołania interfejsu API** Wyślij żądanie do usługi Azure AD B2C zabezpieczonej https://fabrikamb2chello.azurewebsites.net/hello zasobów. 

![Wywołanie interfejsu API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Aplikacja zawiera token dostępu wyświetlane w *Token informacji* pole tekstowe w żądaniu. Interfejs API odsyła nazwę wyświetlaną zawartych w tokenie dostępu.

## <a name="next-steps"></a>Następne kroki

Następnym krokiem jest tworzenie dzierżawy usługi Azure AD B2C i konfigurowanie przykładowych przy użyciu dzierżawy. 

> [!div class="nextstepaction"]
> [Tworzenie dzierżawy usługi Azure Active Directory B2C w portalu Azure](active-directory-b2c-get-started.md)
