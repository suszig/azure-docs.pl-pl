---
title: "Jak skonfigurować uwierzytelnianie Google aplikacji usługi aplikacji"
description: "Dowiedz się, jak skonfigurować uwierzytelnianie Google aplikacji usługi aplikacji."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d6c1707f67d986487e5a45e76ffc9a02ddf16eb1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Konfigurowanie aplikacji usługi aplikacji umożliwiająca użycie logowania Google
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie przedstawiono sposób konfigurowania usługi Azure App Service służące Google jako dostawcy uwierzytelniania.

Aby ukończyć tę procedurę w tym temacie, musi mieć konto Google ze zweryfikowanym adresem e-mail. Aby utworzyć nowe konto Google, przejdź do strony [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Zarejestrować aplikację w usłudze Google
1. Zaloguj się do [portalu Azure], a następnie przejdź do aplikacji. Kopia programu **adres URL**, którego można później skonfigurować aplikacji Google.
2. Przejdź do [interfejsy API Google](http://go.microsoft.com/fwlink/p/?LinkId=268303) kliknij witrynę sieci Web, zaloguj się przy użyciu poświadczeń konta Google **tworzenia projektu**, podaj **Nazwa projektu**, następnie kliknij przycisk  **Utwórz**.
3. W obszarze **społecznościowych interfejsów API** kliknij **interfejsu API Google +** , a następnie **włączyć**.
4. W obszarze nawigacji po lewej stronie **poświadczenia** > **ekranu zgoda OAuth**, a następnie wybierz pozycję z **adres E-mail**, wprowadź **nazwa produktu**i kliknij przycisk **zapisać**.
5. W **poświadczenia** , kliknij pozycję **Utwórz poświadczenia** > **identyfikator klienta OAuth**, a następnie wybierz pozycję **aplikacji sieci Web**.
6. Wklej usługi App Service **adres URL** zostanie wcześniej skopiowany do **autoryzowany źródeł JavaScript**, Wklej przekierowania z identyfikatora URI do **autoryzowany identyfikator URI przekierowania**. Przekierowanie URI jest adres URL aplikacji dołączany wraz ze ścieżką */.auth/login/google/callback*. Na przykład `https://contoso.azurewebsites.net/.auth/login/google/callback`. Upewnij się, że używasz schematu HTTPS. Następnie kliknij pozycję **Utwórz**.
7. Na następnym ekranie zanotuj wartości Identyfikatora klienta i klucz tajny klienta.

    > [!IMPORTANT]
    > Klucz tajny klienta jest ważne poświadczenie zabezpieczeń. Nie udostępniaj nikomu ten klucz tajny i rozpowszechnienie go w aplikacji klienta.


## <a name="secrets"></a>Google Dodawanie informacji do aplikacji
1. W [portalu Azure], przejdź do aplikacji. Kliknij przycisk **ustawienia**, a następnie **uwierzytelniania / autoryzacji**.
2. Jeśli uwierzytelnianie / autoryzacja funkcja nie jest włączona, włącz przełącznik do **na**.
3. Kliknij przycisk **Google**. Wklej w wartości Identyfikatora aplikacji i klucz tajny aplikacji, które uzyskany wcześniej i opcjonalnie włączyć wszystkie zakresy wymaganych przez aplikację. Następnie kliknij przycisk **OK**.
   
   ![][1]
   
   Domyślnie usługi App Service zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanego dostępu do interfejsów API i zawartości witryny. Musisz zezwolić użytkownikom w kodzie aplikacji.
4. (Opcjonalnie) Aby ograniczyć dostęp do witryny użytkownikom tylko uwierzytelniony przez firmę Google, ustaw **działania należy podjąć w przypadku nieuwierzytelnionego żądania** do **Google**. Wymaga to, że wszystkie żądania uwierzytelnienia, a wszystkie nieuwierzytelnione żądania są przekierowywane do Google do uwierzytelniania.
5. Kliknij pozycję **Zapisz**.

Teraz można przystąpić do uwierzytelniania w aplikacji Google.

## <a name="related-content"></a>Związane z zawartością
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[portalu Azure]: https://portal.azure.com/

