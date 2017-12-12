---
title: "Test stacji aplikacji sieci web platformy Azure AD B2C włączone | Dokumentacja firmy Microsoft"
description: "Testowanie Zaloguj, zarejestruj się edytowanie profilu i resetowania hasła użytkownika podróże przy użyciu środowiska testowego usługi Azure AD B2C"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: patricka
ms.openlocfilehash: bc56da695145f396a2899fb9dc7add3af9a549e8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="test-drive-an-azure-ad-b2c-enabled-web-app"></a>Testowanie usługi Azure AD B2C włączone aplikacji sieci web

Usługa Azure Active Directory B2C umożliwia zarządzanie tożsamości chmury do zachowania aplikacji, firmy oraz klientów chronionych. Aby zademonstrować tego przewodnika Szybki Start korzysta z przykładową aplikację listy zadań do wykonania:

> [!div class="checklist"]
> * Zaloguj się za pomocą strony logowania niestandardowych.
> * Zaloguj się przy użyciu dostawcy tożsamości społecznościowych.
> * Tworzenie i Zarządzanie profilem konta i użytkowników usługi Azure AD B2C.
> * Wywoływanie składnika web API zabezpieczonej przez usługi Azure AD B2C.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) z **ASP.NET i sieć web development** obciążenia. 
* Konto społecznościowych z usługi Facebook, Google, Microsoft lub Twitter.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobrać lub sklonować przykładowej aplikacji](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) z usługi GitHub.

## <a name="run-the-app-in-visual-studio"></a>Uruchom aplikację w programie Visual Studio

W folderze projektu aplikacji przykładowej Otwórz `B2C-WebAPI-DotNet.sln` rozwiązania w programie Visual Studio. 

Rozwiązanie to przykładowa aplikacja listy zadań do wykonania składające się z dwóch projektów:

* **TaskWebApp** — aplikacji sieci web platformy ASP.NET MVC, której użytkownik można zarządzać ich elementy listy zadań do wykonania.  
* **TaskService** — zaplecza interfejsu API sieci Web platformy ASP.NET, zarządzającej operacje wykonywane na elementy listy zadań do wykonania przez użytkownika. Aplikacja sieci web wywołuje interfejs API sieci web i wyświetla wyniki.

Dla tego przewodnika Szybki Start, musisz uruchomić oba `TaskWebApp` i `TaskService` projekty w tym samym czasie. 

1. W menu programu Visual Studio wybierz **projekty > Ustaw projekty startowe...** . 
2. Wybierz **wiele projektów startowych** przycisk radiowy.
3. Zmień **akcji** dla obu projektów do **Start**. Kliknij przycisk **OK**.

![Ustaw strony początkowej w Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Wybierz **Debuguj > Rozpocznij debugowanie** Aby skompilować i uruchomić obydwu aplikacji. Każda aplikacja otworzy się w osobnej karcie przeglądarki:

`https://localhost:44316/`-Ta strona jest aplikacja sieci web ASP.NET. Możesz bezpośrednią interakcję z tej aplikacji z opcją szybkiego startu.
`https://localhost:44332/`-Ta strona jest interfejs API, który jest wywoływany przez aplikacje sieci web ASP.NET w sieci web.

## <a name="create-an-account"></a>Tworzenie konta usługi

Kliknij przycisk **Zarejestruj / Zaloguj** łącze w aplikacji sieci web ASP.NET, aby uruchomić **Utwórz konto lub zaloguj** przepływu pracy. Podczas tworzenia konta, można użyć istniejącego konta dostawcy tożsamości społecznościowych lub konto e-mail. Dla tego przewodnika Szybki Start należy użyć konta dostawcy społecznościowych tożsamości z usługi Facebook, Google, Microsoft lub Twitter.

![Przykładową aplikację sieci web ASP.NET](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Zaloguj przy użyciu dostawcy tożsamości społecznościowych

Aby utworzyć konto przy użyciu dostawcy tożsamości społecznościowych, kliknij przycisk dostawcy tożsamości, który ma być używany. 

![Dostawca logowania lub Utwórz konto](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Musisz uwierzytelnić (logowania) za pomocą konta społecznościowych poświadczenia i zezwolić aplikacji na odczytywanie informacji z Twojego konta społecznościowych. Udzielenie dostępu, aplikacja może pobrać informacji o profilu konta społecznościowych, takie jak nazwa i Miasto. 

Zakończ proces logowania dla dostawcy tożsamości. Na przykład, jeśli została wybrana opcja Twitter, wprowadź swoje poświadczenia usługi Twitter i kliknij przycisk **Zaloguj**.

![Uwierzytelniania i autoryzacji przy użyciu konta społecznościowych](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Szczegóły nowego profilu konta usługi Azure AD B2C są wstępnie wypełnione informacjami z konta społecznościowych.

Zaktualizuj pola Nazwa wyświetlana, stanowisko i Miasto, a następnie kliknij przycisk **Kontynuuj**.  Wartości, które należy wprowadzić służą do profilu konta użytkownika usługi Azure AD B2C.

![Nowe konto Szczegóły profilu rejestracji](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Został pomyślnie:

> [!div class="checklist"]
> * Uwierzytelniony przy użyciu dostawcy tożsamości.
> * Utworzone konto użytkownika usługi Azure AD B2C. 

## <a name="edit-your-profile"></a>Edytowanie profilu

Usługa Azure Active Directory B2C zawiera funkcję, aby umożliwić użytkownikom aktualizowanie ich profile. Na pasku menu aplikacji sieci web, kliknij swoją nazwę profilu, a następnie wybierz **edytowanie profilu** Aby edytować to profil utworzony.

![Edytowanie profilu](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Zmień Twoje **Nazwa wyświetlana** i **miasta**.  Kliknij przycisk **Kontynuuj** można zaktualizować Twojego profilu.

![Aktualizowanie profilu](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Należy zauważyć, że aktualizacja nazwy zawiera nazwy wyświetlanej w prawej górnej części strony. 

## <a name="access-a-secured-web-api-resource"></a>Dostęp do zabezpieczonych zasobów interfejsu API sieci web

Kliknij przycisk **listy zadań do wykonania** o wprowadzenie i zmodyfikuj elementy listy zadań do wykonania. Aplikacja sieci web ASP.NET zawiera token dostępu w żądania sieci Web interfejsu API zasobów żądania uprawnień do wykonywania operacji na elementy listy zadań do wykonania przez użytkownika. 

Wprowadź tekst w **nowy element** pola tekstowego. Kliknij przycisk **Dodaj** do wywołania usługi Azure AD B2C zabezpieczonej interfejsu API sieci web, który dodaje element do listy zadań do wykonania.

![Dodawanie elementu listy zadań do wykonania](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

Pomyślnie użyto konta użytkownika usługi Azure AD B2C do wywoływania autoryzowanych usługi Azure AD B2C zabezpieczonej interfejsu API sieci web.

## <a name="next-steps"></a>Następne kroki

Przykładowe używane w tym szybkiego startu może służyć do spróbuj innych scenariuszy usługi Azure AD B2C, w tym:

* Tworzenie nowego konta lokalnego przy użyciu adresu e-mail.
* Resetowanie hasła konta lokalnego.

Jeśli wszystko jest gotowe do delve do procesu tworzenia dzierżawy usługi Azure AD B2C i konfigurowania przykładowych przy użyciu własnego dzierżawy, wypróbuj następujące samouczka.

> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci web platformy ASP.NET z usługi Azure Active Directory B2C profilu rejestracji, logowania, edycji i resetowania hasła](active-directory-b2c-devquickstarts-web-dotnet-susi.md)