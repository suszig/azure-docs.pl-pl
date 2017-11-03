---
title: "Test stacji aplikacji sieci web usługi Azure AD B2C | Dokumentacja firmy Microsoft"
description: "Testowanie Zaloguj, zarejestruj się edytowanie profilu i resetowania hasła użytkownika podróże przy użyciu środowiska testowego usługi Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 2a51f15fd38a901548dcf4c56922f9715c328463
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-web-application-configured-with-azure-ad-b2c"></a>Przetestuj aplikację sieci web skonfigurowany w usłudze Azure AD B2C

Usługa Azure Active Directory B2C umożliwia zarządzanie tożsamości chmury do zachowania aplikacji, firmy oraz klientów chronionych.  Aby zademonstrować tego przewodnika Szybki Start korzysta z przykładową aplikację listy zadań do wykonania:

* Przy użyciu **Utwórz konto lub zaloguj** zasad, aby utworzyć lub zaloguj się przy użyciu dostawcy tożsamości społecznościowych lub lokalnego konta przy użyciu adresu e-mail. 
* Wywołanie interfejsu API zabezpieczonej przez usługi Azure AD B2C do tworzenia i edytowania elementów do wykonania.

## <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
    - **Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych**

* Konto społecznościowych z usługi Facebook, Google, Microsoft lub Twitter. Jeśli nie masz konta społecznościowych, wymagany jest prawidłowy adres e-mail.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobrać lub sklonować przykładowej aplikacji](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) z usługi GitHub.

## <a name="run-the-app-in-visual-studio"></a>Uruchom aplikację w programie Visual Studio

W folderze projektu aplikacji przykładowej Otwórz `B2C-WebAPI-DotNet.sln` rozwiązania w programie Visual Studio. 

Rozwiązania zawiera dwa projekty:

* **TaskWebApp** — aplikacji sieci web platformy ASP.NET MVC, której użytkownik można zarządzać ich elementy listy zadań do wykonania.  
* **TaskService** — wykonać wewnętrznej bazy danych interfejsu API sieci Web platformy ASP.NET, która zarządza wszystkich operacji CRUD na elementy listy zadań do wykonania przez użytkownika. Aplikacja sieci web wywołuje ten interfejs API i wyświetla wyniki.

Dla tego przewodnika Szybki Start, musisz uruchomić oba `TaskWebApp` i `TaskService` projekty w tym samym czasie. 

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie i wybierz **Ustaw projekty startowe...** . 
2. Wybierz **wiele projektów startowych** przycisk radiowy.
3. Zmień **akcji** dla obu projektów do **Start**. Kliknij przycisk **OK**.

![Ustaw strony początkowej w Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Wybierz **Debuguj > Rozpocznij debugowanie** Aby skompilować i uruchomić obydwu aplikacji. Każda aplikacja otworzy się w osobnej karcie przeglądarki:

* `https://localhost:44316/`-Ta strona jest aplikacja sieci web ASP.NET. Możesz bezpośrednią interakcję z tej aplikacji z opcją szybkiego startu.
* `https://localhost:44332/`-Ta strona jest interfejs API, który jest wywoływany przez aplikacje sieci web ASP.NET w sieci web.

## <a name="create-an-account"></a>Tworzenie konta usługi

Kliknij przycisk **Zarejestruj / Zaloguj** łącze w aplikacji sieci web ASP.NET, aby uruchomić **Utwórz konto lub zaloguj** przepływu pracy. Podczas tworzenia konta, można użyć istniejącego konta dostawcy tożsamości społecznościowych lub konto e-mail.

![Przykładową aplikację sieci web ASP.NET](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaloguj przy użyciu dostawcy tożsamości społecznościowych

Aby utworzyć konto przy użyciu dostawcy tożsamości społecznościowych, kliknij przycisk dostawcy tożsamości, który ma być używany. Jeśli wolisz używać adresu e-mail, należy przejść do [zaloguj przy użyciu adresu e-mail](#sign-up-using-an-email-address) sekcji.

![Dostawca logowania lub Utwórz konto](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Musisz uwierzytelnić (logowania) za pomocą konta społecznościowych poświadczenia i zezwolić aplikacji na odczytywanie informacji z Twojego konta społecznościowych. Udzielenie dostępu, aplikacja może pobrać informacji o profilu konta społecznościowych, takie jak nazwa i Miasto. 

![Uwierzytelniania i autoryzacji przy użyciu konta społecznościowych](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Zakończ proces logowania dla dostawcy tożsamości. Na przykład kliknij pozycję **Zaloguj** przycisk Twitter.

Szczegóły nowego profilu konta są wstępnie wypełnione informacjami z konta społecznościowych.

![Nowe konto Szczegóły profilu rejestracji](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Zaktualizuj pola Nazwa wyświetlana, stanowisko i Miasto, a następnie kliknij przycisk **Kontynuuj**.  Wartości, które należy wprowadzić służą do profilu konta użytkownika usługi Azure AD B2C.

Pomyślnie utworzono nowe konto użytkownika usługi Azure AD B2C, która używa dostawcy tożsamości. 

Następny krok: [skok do wyświetlenia z oświadczeń](#view-your-claims) sekcji.

### <a name="sign-up-using-an-email-address"></a>Zaloguj przy użyciu adresu e-mail

Jeśli wybierzesz opcję rezygnacji z używania kont społecznościowych do zapewnienia uwierzytelniania, można utworzyć konta użytkownika usługi Azure AD B2C przy użyciu prawidłowy adres e-mail. Konto użytkownika lokalnego usługi Azure AD B2C używa usługi Azure Active Directory jako dostawcy tożsamości. Aby użyć adresu e-mail, kliknij przycisk **nie masz konta? Zarejestruj się teraz** łącza.

![Zaloguj się lub zaloguj się za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-email-web.png)

Wprowadź prawidłowy adres e-mail i kliknij przycisk **wysłać kod weryfikacyjny**. Prawidłowy adres e-mail jest wymagany, aby uzyskać kod weryfikacyjny z usługi Azure AD B2C. 

Wprowadź kod weryfikacyjny wyświetlany w wiadomości e-mail i kliknij przycisk **Sprawdź kod**.

Dodawanie informacji o Twoim profilu, a następnie kliknij przycisk **Utwórz**.

![Logowanie się przy nowe konto, za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-web-app/sign-up-new-account-profile-email-web.png)

Pomyślnie utworzono nowe konto użytkownika lokalnego dla usługi Azure AD B2C.

## <a name="reset-your-password"></a>Resetowanie hasła

Jeśli utworzono przy użyciu adresu e-mail konta usługi Azure AD B2C ma funkcję, aby umożliwić użytkownikom resetowania hasła. Aby edytować to profil utworzony, kliknij swoją nazwę profilu na pasku menu, a następnie wybierz **Resetuj hasło**.

Zweryfikuj swój adres e-mail, wprowadzając ją i klikając **wysłać kod weryfikacyjny**. Kod weryfikacyjny są wysyłane na adres e-mail.

Wprowadź kod weryfikacyjny odebrany w wiadomości e-mail, a następnie kliknij przycisk **Sprawdź kod**.

Po zweryfikowaniu swój adres e-mail, kliknij przycisk **Kontynuuj**.

Wprowadź nowe hasło, a następnie kliknij przycisk **Kontynuuj**.

## <a name="view-your-claims"></a>Wyświetl oświadczenia użytkownika

Kliknij przycisk **oświadczeń** na pasku menu aplikacji sieci web, aby wyświetlić oświadczeń skojarzone z ostatnią akcję. 

![Logowanie się przy nowe konto, za pomocą poczty e-mail](media/active-directory-b2c-quickstarts-web-app/view-claims-sign-up-web.png)

W tym przykładzie, ostatnia akcja została dla *zalogowania się lub zarejestrować się* wystąpić. Powiadomienie **typ oświadczenia** `http://schemas.microsoft.com/claims/authnclassreference` jest `b2c_1_susi` wskazujący ostatnia akcja została rejestracji lub logowania. Jeśli ostatnia akcja została resetowania hasła **typ oświadczenia** będzie `b2c_1_reset`.

## <a name="edit-your-profile"></a>Edytowanie profilu

Usługa Azure Active Directory B2C zawiera funkcję, aby umożliwić użytkownikom aktualizowanie ich profile. Na pasku menu aplikacji sieci web, kliknij swoją nazwę profilu, a następnie wybierz **edytowanie profilu** Aby edytować to profil utworzony.

![Edytowanie profilu](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Zmień Twoje **Nazwa wyświetlana** i **miasta**.  Kliknij przycisk **Kontynuuj** można zaktualizować Twojego profilu.

![Aktualizuj profil](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Zwróć uwagę aktualizacje nazwę wyświetlaną w prawej górnej części strony, po zmianie nazwy. 

Kliknij przycisk **oświadczeń**. Zmian wprowadzanych w **Nazwa wyświetlana** i **miasta** są uwzględniane w oświadczeniach.

![Widok oświadczeń](media/active-directory-b2c-quickstarts-web-app/view-claims-update-web.png)

 Powiadomienie **typ oświadczenia** `http://schemas.microsoft.com/claims/authnclassreference` został zaktualizowany do `b2c_1_edit_profile` wskazujący ostatnia akcja wykonywana edycji profilu. Należy również zauważyć, nazwa i Miasto są nowe wartości *S. Ewa* i *Seattle*.

## <a name="access-a-resource"></a>Dostęp do zasobu

Kliknij przycisk **listy zadań do wykonania** o wprowadzenie i zmodyfikuj elementy listy zadań do wykonania. Aplikacja sieci web ASP.NET zawiera token dostępu w żądania sieci Web interfejsu API zasobów żądania uprawnień do wykonywania operacji na elementy listy zadań do wykonania przez użytkownika. 

Wprowadź tekst w **nowy element** pola tekstowego. Kliknij przycisk **Dodaj** do wywołania usługi Azure AD B2C zabezpieczonej interfejsu API sieci web, który dodaje element do listy zadań do wykonania.

![Dodawanie elementu listy zadań do wykonania](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

## <a name="other-scenarios"></a>Inne scenariusze

Przetestuj inne scenariusze są następujące:

* Wylogować się z aplikacji i kliknij przycisk **listy zadań do wykonania**. Zwróć uwagę, jak pojawieniu się zalogować i elementy listy są zachowywane. 
* Utwórz nowe konto, przy użyciu innego typu konta. Jeśli utworzony wcześniej przy użyciu adresu e-mail konta, na przykład użyć dostawcy tożsamości społecznościowych.

## <a name="next-steps"></a>Następne kroki

Następnym krokiem jest tworzenie dzierżawy usługi Azure AD B2C i konfigurowanie przykładowych przy użyciu dzierżawy. 

> [!div class="nextstepaction"]
> [Tworzenie dzierżawy usługi Azure Active Directory B2C w portalu Azure](active-directory-b2c-get-started.md)