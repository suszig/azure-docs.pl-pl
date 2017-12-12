---
title: "Usługa Azure Active Directory B2C: Google + konfiguracji | Dokumentacja firmy Microsoft"
description: "Podaj rejestracji i logowania klientom korzystającym z konta Google + w aplikacjach, które są zabezpieczone przez usługi Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 93589352094fdd556811ba906ee27e7b8ac1d8b5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Usługa Azure Active Directory B2C: Umożliwiają rejestracji i logowania użytkowników z konta Google +
## <a name="create-a-google-application"></a>Tworzenie aplikacji Google +
Aby użyć Google + jako dostawca tożsamości w usłudze Azure Active Directory (Azure AD) B2C, należy do utworzenia aplikacji Google + i dostarczyć prawo parametrów. Musisz mieć konto Google + w tym celu. Jeśli nie masz, możesz pobrać go w [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/) i zaloguj się przy użyciu poświadczeń konta Google +.
2. Kliknij przycisk **Tworzenie projektu**, wprowadź **Nazwa projektu**, a następnie kliknij przycisk **Utwórz**.
   
    ![Google + — wprowadzenie](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + — nowy projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Kliknij przycisk **Menedżer interfejsu API** , a następnie kliknij przycisk **poświadczenia** na lewym pasku nawigacyjnym.
4. Kliknij przycisk **ekranu zgoda OAuth** u góry.
   
    ![Google + - poświadczeń](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Wybierz lub określ prawidłowe **adres E-mail**, podaj **nazwa produktu**i kliknij przycisk **zapisać**.
   
    ![Google + - ekranu zgoda OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Kliknij przycisk **nowe poświadczenia** , a następnie wybierz **identyfikator klienta OAuth**.
   
    ![Google + - ekranu zgoda OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. W obszarze **typu aplikacji**, wybierz pozycję **aplikacji sieci Web**.
   
    ![Google + - ekranu zgoda OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Podaj **nazwa** dla aplikacji, wprowadź `https://login.microsoftonline.com` w **źródeł autoryzowany JavaScript** pola i `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w **autoryzowanych przekierowania URI** pola. Zastąp **{dzierżawa}** nazwą Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com). **{Dzierżawa}** wartość jest rozróżniana wielkość liter. Kliknij przycisk **Utwórz**.
   
    ![Google + - Utwórz identyfikator klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Skopiuj wartości z **identyfikator klienta** i **klucz tajny klienta**. Należy ich skonfigurowanie Google + funkcję dostawcy tożsamości w dzierżawie. **Klucz tajny klienta** jest ważne poświadczenie zabezpieczeń.
   
    ![Google + - klucz tajny klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Skonfiguruj Google + funkcję dostawcy tożsamości w Twojej dzierżawie
1. Wykonaj następujące kroki, aby [przejdź do bloku funkcji B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) w portalu Azure.
2. W bloku funkcji B2C, kliknij polecenie **dostawców tożsamości**.
3. Kliknij pozycję **+Dodaj** w górnej części bloku.
4. Podaj przyjazną nazwę w polu **nazwa** konfiguracji dostawcy tożsamości. Na przykład wprowadź "G +".
5. Kliknij przycisk **typ dostawcy tożsamości**, wybierz pozycję **Google**i kliknij przycisk **OK**.
6. Kliknij przycisk **skonfigurować ten dostawca tożsamości** , a następnie wprowadź identyfikator klienta i klucz tajny klienta aplikacji Google + utworzony wcześniej.
7. Kliknij przycisk **OK** , a następnie kliknij przycisk **Utwórz** Aby zapisać konfigurację Google +.

