---
title: "Usługa Azure Active Directory B2C: Twitter konfiguracji | Dokumentacja firmy Microsoft"
description: "Umożliwiają tworzenie kont i logowania użytkowników z kontami usługi Twitter w aplikacjach, które są zabezpieczone przez usługi Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: dde17c89c49223c75842f328f455c6637aa1915b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a>Azure Active Directory B2C: Umożliwiają tworzenie kont i logowania użytkowników z kontami usługi Twitter

> [!NOTE]
> Ta funkcja jest dostępna w wersji zapoznawczej.
> 

## <a name="create-a-twitter-application"></a>Utwórz aplikację usługi Twitter
Aby użyć usługi Twitter jako dostawca tożsamości w usłudze Azure Active Directory (Azure AD) B2C, należy utworzyć aplikację usługi Twitter i dostarczyć prawo parametrów. Musisz mieć konto dewelopera usługi Twitter, w tym celu. Jeśli nie masz, możesz pobrać go w [https://dev.twitter.com/](https://dev.twitter.com/).

1. Przejdź do [dewelopera witryny sieci Web w usłudze Twitter](https://dev.twitter.com/) i zaloguj się przy użyciu poświadczeń.
2. Kliknij przycisk **Moje aplikacje** w obszarze **narzędzia i pomoc techniczna** , a następnie kliknij przycisk **Utwórz nową aplikację**. 
3. W formularzu, należy podać wartość parametru **nazwa**, **opis**, i **witryny sieci Web**.
4. Aby uzyskać **wywołania zwrotnego adresu URL**, wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Upewnij się zastąpić **{dzierżawa}** nazwą Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com).
5. Pole wyboru, aby zaakceptować **umowy Developer** i kliknij przycisk **tworzenie aplikacji Twitter**.
6. Po utworzeniu aplikacji, kliknij przycisk **kluczy i tokenów dostępu**.
7. Skopiuj wartość **konsumenta** i **klucz tajny klienta**. Należy obie z nich, aby skonfigurować funkcję dostawcy tożsamości w dzierżawie usługi Twitter.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Skonfiguruj funkcję dostawcy tożsamości w dzierżawie usługi Twitter
1. Wykonaj następujące kroki, aby [przejdź do bloku funkcji B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) w portalu Azure.
2. W bloku funkcji B2C, kliknij polecenie **dostawców tożsamości**.
3. Kliknij pozycję **+Dodaj** w górnej części bloku.
4. Podaj przyjazną nazwę w polu **nazwa** konfiguracji dostawcy tożsamości. Wprowadź na przykład "Twitter".
5. Kliknij przycisk **typ dostawcy tożsamości**, wybierz pozycję **Twitter**i kliknij przycisk **OK**.
6. Kliknij przycisk **skonfigurować ten dostawca tożsamości** , a następnie wprowadź Twitter **konsumenta** dla **identyfikator klienta** i Twitter **klucz tajny klienta** dla **klucz tajny klienta**.
7. Kliknij przycisk **OK**, a następnie kliknij przycisk **Utwórz** Aby zapisać konfigurację usługi Twitter.

