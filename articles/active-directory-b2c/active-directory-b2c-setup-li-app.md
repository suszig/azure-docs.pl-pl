---
title: 'Usługa Azure Active Directory B2C: LinkedIn konfiguracji | Dokumentacja firmy Microsoft'
description: Podaj rejestracji i logowania klientom korzystającym z kontami LinkedIn w aplikacjach, które są zabezpieczone przez usługi Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: 91970b3ec41c645cd1cd3e203cc96ad655dafd7e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Umożliwiają tworzenie kont i logowania użytkowników z kontami LinkedIn
## <a name="create-a-linkedin-application"></a>Tworzenie aplikacji LinkedIn
Aby użyć LinkedIn jako dostawca tożsamości w usłudze Azure Active Directory (Azure AD) B2C, należy utworzyć aplikację LinkedIn i dostarczyć prawo parametrów. Należy to zrobić przy użyciu konta LinkedIn. Jeśli nie masz, możesz pobrać go w [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Przejdź do [deweloperzy LinkedIn witryny sieci Web](https://www.developer.linkedin.com/) i zaloguj się przy użyciu poświadczeń konta LinkedIn.
2. Kliknij przycisk **Moje aplikacje** paska menu u góry, a następnie kliknij polecenie **tworzenie aplikacji**.
   
    ![LinkedIn — nowej aplikacji](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. W **Utwórz nową aplikację** tworzą, wprowadź odpowiednie informacje (**nazwa firmy**, **nazwa**, **opis**, **Adres URL Logo aplikacji**, **stosowania**, **adres URL witryny sieci Web**, **służbowy adres E-mail** i **Telefon służbowy**).
4. Zgadzam się **LinkedIn interfejsu API warunki użytkowania** i kliknij przycisk **przesyłania**.
   
    ![LinkedIn - rejestrowania aplikacji](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Skopiuj wartości z **identyfikator klienta** i **klucz tajny klienta**. (Można znaleźć je w obszarze **klucze uwierzytelniania**.) Należy ich skonfigurowanie LinkedIn funkcję dostawcy tożsamości w dzierżawie.
   
   > [!NOTE]
   > **Klucz tajny klienta** jest ważne poświadczenie zabezpieczeń.
   > 
   > 
6. Wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w **autoryzacji adresów URL przekierowań** pola (w obszarze **OAuth 2.0**). Zastąp **{dzierżawa}** nazwą Twojej dzierżawy (np. contoso.onmicrosoft.com). Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **aktualizacji**. **{Dzierżawa}** wartość jest rozróżniana wielkość liter.
   
    ![LinkedIn — ustawienia aplikacji](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Skonfiguruj LinkedIn funkcję dostawcy tożsamości w Twojej dzierżawie
1. Wykonaj następujące kroki, aby [przejdź do bloku funkcji B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) w portalu Azure.
2. W bloku funkcji B2C, kliknij polecenie **dostawców tożsamości**.
3. Kliknij pozycję **+Dodaj** w górnej części bloku.
4. Podaj przyjazną nazwę w polu **nazwa** konfiguracji dostawcy tożsamości. Wprowadź na przykład "LI".
5. Kliknij przycisk **typ dostawcy tożsamości**, wybierz pozycję **LinkedIn**i kliknij przycisk **OK**.
6. Kliknij przycisk **skonfigurować ten dostawca tożsamości** , a następnie wprowadź identyfikator klienta i klucz tajny klienta aplikacji LinkedIn, który został utworzony wcześniej.
7. Kliknij przycisk **OK** , a następnie kliknij przycisk **Utwórz** Aby zapisać konfigurację LinkedIn.

