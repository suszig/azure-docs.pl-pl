---
title: "Jak skonfigurować uwierzytelnianie w usłudze Azure Active Directory dla aplikacji usługi aplikacji"
description: "Dowiedz się, jak skonfigurować uwierzytelnianie w usłudze Azure Active Directory dla aplikacji usługi aplikacji."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 25f0578a9e273c30ecc98af5b66c6dd43305aa03
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Jak skonfigurować aplikację App Service na potrzeby logowania do usługi Azure Active Directory
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie przedstawiono sposób konfigurowania usługi aplikacji Azure do użycia usługi Azure Active Directory jako dostawcy uwierzytelniania.

## <a name="express"></a>Konfigurowanie usługi Azure Active Directory przy użyciu ustawień ekspresowych
1. W [portalu Azure], przejdź do aplikacji. Kliknij przycisk **ustawienia**, a następnie **uwierzytelniania/autoryzacji**.
2. Jeśli uwierzytelnianie / autoryzacja funkcja nie jest włączona, włącz przełącznik do **na**.
3. Kliknij przycisk **usługi Azure Active Directory**, a następnie kliknij przycisk **Express** w obszarze **tryb zarządzania**.
4. Kliknij przycisk **OK** zarejestrować aplikację w usłudze Azure Active Directory. Spowoduje to utworzenie nowej rejestracji. Jeśli chcesz zamiast tego wybrać rejestracją istniejącą, kliknij przycisk **wybierz istniejącą aplikację** , a następnie wyszukaj nazwę wcześniej utworzonej rejestracji w ramach dzierżawy.
   Kliknij przycisk rejestracji, aby go zaznaczyć, a następnie kliknij przycisk **OK**. Następnie kliknij przycisk **OK** w bloku ustawień usługi Azure Active Directory.
   Domyślnie usługi App Service zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanego dostępu do interfejsów API i zawartości witryny. Musisz zezwolić użytkownikom w kodzie aplikacji.
5. (Opcjonalnie) Aby ograniczyć dostęp do witryny użytkownikom tylko uwierzytelniony przez usługę Azure Active Directory, należy ustawić **działania należy podjąć w przypadku nieuwierzytelnionego żądania** do **Zaloguj się za pomocą usługi Azure Active Directory**. Wymaga to, że wszystkie żądania uwierzytelnienia, a wszystkie nieuwierzytelnione żądania są przekierowywane do usługi Azure Active Directory do uwierzytelniania.
6. Kliknij pozycję **Zapisz**.

Teraz można przystąpić do użycia usługi Azure Active Directory do uwierzytelniania w aplikacji.

## <a name="advanced"></a>(Metoda alternatywna) ręcznie skonfigurować usługi Azure Active Directory z ustawieniami zaawansowanymi
Można również podać ustawienia konfiguracji ręcznie. Jest to preferowane rozwiązanie, jeśli dzierżawę usługi AAD, którego chcesz użyć różni się od dzierżawcy, z którym należy zalogować się do platformy Azure. W celu ukończenia konfiguracji, należy najpierw utworzyć rejestracji w usłudze Azure Active Directory, a następnie podaj niektórych szczegółów rejestracji usługi aplikacji.

### <a name="register"></a>Zarejestrować aplikację w usłudze Azure Active Directory
1. Zaloguj się do [portalu Azure], a następnie przejdź do aplikacji. Kopiowanie aplikacji **adres URL**. Umożliwia to skonfigurować aplikację usługi Azure Active Directory.
2. Przejdź do **usługi Active Directory**, a następnie wybierz pozycję **rejestracji aplikacji**, następnie kliknij przycisk **nowej rejestracji aplikacji** u góry, aby rozpocząć nowej rejestracji aplikacji. 
3. W oknie dialogowym Tworzenie rejestracji aplikacji, wprowadź **nazwa** dla aplikacji, wybierz **interfejsu API aplikacji sieci Web** wpisz w **adres URL logowania** pole Wklej adres URL aplikacji (z Krok 1). Kliknij, aby **Utwórz**.
4. W ciągu kilku sekund powinna zostać wyświetlona nowej rejestracji aplikacji utworzony są wyświetlane.
5. Po dodaniu aplikacji, kliknij na nazwie rejestracji aplikacji i kliknij pozycję **ustawienia** u góry, następnie kliknij polecenie **właściwości** 
6. W **identyfikator URI aplikacji** Wklej adres URL aplikacji (z kroku 1), również w **adres URL strony głównej** Wklej adres URL aplikacji (z kroku 1) oraz, następnie kliknij przycisk **Zapisz**
7. Teraz kliknij **adresy URL odpowiedzi**, Edytuj **adres URL odpowiedzi**, wklej adres URL aplikacji (z kroku 1), zmodyfikuj protokół upewnij się, że masz **https://** protokołu (nie http://) następnie jest dołączany na końcu adresu URL, */.auth/login/aad/callback* . (Na przykład `https://contoso.azurewebsites.net/.auth/login/aad/callback`.) Kliknij pozycję **Zapisz**.   
8.  W tym momencie Skopiuj **identyfikator aplikacji** dla aplikacji. Zachowaj to do późniejszego użycia. Będzie on potrzebny do konfigurowania aplikacji sieci web.
9. Zamknij blok szczegółów rejestracji aplikacji. Należy wrócić do podsumowania rejestracji aplikacji Azure Active Directory kliknij **punkty końcowe** przycisk u góry, skopiuj **dokument metadanych usług federacyjnych** adresu URL. 
10. Otwórz nowe okno przeglądarki i przejdź do adresu URL wklejanie i przechodzenia do pliku XML. W górnej części dokumentu będzie **EntityDescriptor** element, należy **entityID** atrybut w postaci `https://sts.windows.net/` następuje GUID przeznaczonych dla Twojej dzierżawy (nazywanych "identyfikator dzierżawy"). Skopiuj tę wartość — będzie służyć jako sieci **adres URL wystawcy**. Skonfiguruj aplikację, aby użyć tego później.

### <a name="secrets"></a>Dodaj Azure informacji usługi Active Directory do aplikacji
1. W [portalu Azure], przejdź do aplikacji. Kliknij przycisk **uwierzytelniania/autoryzacji**. Jeśli nie włączono funkcji uwierzytelniania/autoryzacji, włącz przełącznik do **na**. Polecenie **usługi Azure Active Directory**, w obszarze dostawcy uwierzytelniania, aby skonfigurować aplikację. (Opcjonalnie) Domyślnie usługi App Service zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanego dostępu do interfejsów API i zawartości witryny. Musisz zezwolić użytkownikom w kodzie aplikacji. Wybierz **działania należy podjąć w przypadku nieuwierzytelnionego żądania**, ustaw tę wartość na **Zaloguj się za pomocą usługi Azure Active Directory**. Wymaga to, że wszystkie żądania uwierzytelnienia, a wszystkie nieuwierzytelnione żądania są przekierowywane do usługi Azure Active Directory do uwierzytelniania.
2. w konfiguracji uwierzytelniania w usłudze Active Directory, kliknij przycisk **zaawansowane** w obszarze **tryb zarządzania**. Wklej identyfikator aplikacji w polu Identyfikatora klienta (z kroku 8) i Wklej entityId (z kroku 10) na wartość adres URL wystawcy. Następnie kliknij przycisk **OK**.
3. W bloku konfiguracji uwierzytelniania w usłudze Active Directory, kliknij przycisk **zapisać**.

Teraz można przystąpić do użycia usługi Azure Active Directory do uwierzytelniania w aplikacji.

## <a name="optional-configure-a-native-client-application"></a>(Opcjonalnie) Skonfiguruj aplikację native client
Usługa Azure Active Directory umożliwia również zarejestrować klientach natywnych, co zapewnia większą kontrolę nad uprawnieniami mapowania. To konieczne, jeśli chcesz wykonać logowania przy użyciu biblioteki, takich jak **biblioteki uwierzytelniania usługi Active Directory**.

1. Przejdź do **usługi Active Directory** w [klasycznego portalu Azure].
2. Wybierz katalog, a następnie wybierz **aplikacji** u góry. Kliknij przycisk **dodać** na dole, aby utworzyć nowej rejestracji aplikacji.
3. Kliknij przycisk **Dodaj aplikację moją organizację**.
4. W Kreatorze dodawania aplikacji wprowadź **nazwa** dla aplikacji i kliknij przycisk **natywną aplikację kliencką** typu. Następnie kliknij, aby kontynuować.
5. W **identyfikator URI przekierowania** wprowadź witryny */.auth/login/done* punktu końcowego, za pomocą schematu HTTPS. Ta wartość powinna być podobna do *https://contoso.azurewebsites.net/.auth/login/done*. W przypadku tworzenia aplikacji systemu Windows, zamiast tego użyj [identyfikatora SID pakietu](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) jako identyfikator URI.
6. Po dodaniu aplikacji natywnej kliknij **Konfiguruj** kartę. Znajdź **identyfikator klienta** i zanotuj tę wartość.
7. Przewiń w dół do strony **uprawnień dotyczących innych aplikacji** sekcji, a następnie kliknij przycisk **Dodaj aplikację**.
8. Wyszukaj aplikacji sieci web, który został wcześniej zarejestrowany, a następnie kliknij ikonę znaku plus. Następnie kliknij przycisk wyboru, aby zamknąć okno dialogowe. Jeśli aplikacja sieci web nie można odnaleźć, przejdź do rejestracji i dodać nowy adres URL odpowiedzi (np. HTTP wersja bieżącą adres URL), kliknij przycisk Zapisz, a następnie powtórz te kroki — powinny być widoczne aplikacji na liście.
9. Na nowy wpis dodaną, otwórz **delegowane uprawnienia** listy rozwijanej i wybierz **dostępu (appName)**. Następnie kliknij przycisk **Save** (Zapisz).

Aplikację native client, który ma dostęp do aplikacji usługi aplikacji został skonfigurowany.

## <a name="related-content"></a>Związane z zawartością
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[portalu Azure]: https://portal.azure.com/
[klasycznego portalu Azure]: https://manage.windowsazure.com/
[alternative method]:#advanced
