---
title: "Aplikacje obsługujące oświadczenia — serwera Proxy aplikacji usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Jak opublikować lokalnej aplikacji ASP.NET, które akceptują oświadczenia usług AD FS dla bezpieczny dostęp zdalny przez użytkowników."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 4546f194e570282d331dde98cab98dd2f0981b72
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Praca z aplikacji obsługujących oświadczenia w serwer Proxy aplikacji
[Aplikacje obsługujące oświadczenia](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) wykonania przekierowania do zabezpieczenia usługi tokenów (STS). Usługi STS żąda poświadczeń użytkownika w zamian tokenu, a następnie przekierowuje użytkownika do aplikacji. Istnieje kilka sposobów, aby włączyć serwer Proxy aplikacji do pracy z tymi przekierowania. W tym artykule umożliwiają konfigurowanie wdrożenia dla aplikacji obsługujących oświadczenia. 

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że usługa tokenu Zabezpieczającego, które przekierowuje aplikacji obsługującej oświadczenia jest dostępny poza siecią lokalną. Można udostępnić usługi STS ujawnienie go za pośrednictwem serwera proxy lub zezwala na zewnętrzne połączenia. 

## <a name="publish-your-application"></a>Publikowanie aplikacji

1. Publikowanie aplikacji zgodnie z instrukcjami opisanego w [publikowania aplikacji za pomocą serwera Proxy aplikacji](application-proxy-publish-azure-portal.md).
2. Przejdź do strony aplikacji w portalu i wybierz **logowanie jednokrotne**.
3. Jeśli została wybrana opcja **usługi Azure Active Directory** jako sieci **metoda uwierzytelniania wstępnego**, wybierz pozycję **usługi Azure AD rejestracji jednokrotnej wyłączone** jako sieci **wewnętrzne Metoda uwierzytelniania**. Jeśli została wybrana opcja **Passthrough** jako sieci **metoda uwierzytelniania wstępnego**, nie jest konieczne wprowadzanie zmian.

## <a name="configure-adfs"></a>Konfigurowanie usług AD FS

Usługi AD FS można skonfigurować dla aplikacji obsługujących oświadczenia w jeden z dwóch sposobów. Pierwsza to przy użyciu domen niestandardowych. Drugim jest z protokołu WS-Federation. 

### <a name="option-1-custom-domains"></a>Opcja 1: Domen niestandardowych

Jeśli wszystkie wewnętrzne adresy URL dla aplikacji są w pełni kwalifikowanej nazwy domeny (FQDN), a następnie można skonfigurować [domen niestandardowych](active-directory-application-proxy-custom-domains.md) dla aplikacji. Użyj domeny niestandardowe, aby utworzyć zewnętrzne adresy URL, które są takie same jak wewnętrzne adresy URL. W przypadku sieci zewnętrzne adresy URL zgodne z wewnętrznych adresów URL, przekierowań STS pracować, czy użytkownicy są lokalnego lub zdalnego. 

### <a name="option-2-ws-federation"></a>Opcja 2: WS-Federation

1. Otwórz przystawkę Zarządzanie usługi AD FS.
2. Przejdź do **zaufania jednostek uzależnionych**, kliknij prawym przyciskiem myszy w aplikacji są publikowanie za pomocą serwera Proxy aplikacji i wybierz **właściwości**.  

   ![Zaufania jednostek uzależnionych kliknij prawym przyciskiem myszy nazwę aplikacji — zrzut ekranu](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. Na **punkty końcowe** , w obszarze **typ punktu końcowego**, wybierz pozycję **WS-Federation**.
4. W obszarze **URL zaufanych**, wprowadź adres URL wprowadzony w serwer Proxy aplikacji w obszarze **zewnętrzny adres URL** i kliknij przycisk **OK**.  

   ![Dodawanie punktu końcowego — ustaw wartość adres URL z zaufanych — zrzut ekranu](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Następne kroki
* [Włącz jednokrotnego](application-proxy-sso-overview.md) dla aplikacji, które nie są obsługujących oświadczenia
* [Włącz aplikacji klienta natywnego wchodzić w interakcje z serwera proxy aplikacji](active-directory-application-proxy-native-client.md)


