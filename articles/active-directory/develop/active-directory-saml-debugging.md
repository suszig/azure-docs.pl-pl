---
title: "Debugowanie na języku SAML logowanie jednokrotne do aplikacji w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można debugować na języku SAML logowanie jednokrotne do aplikacji w usłudze Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 31447d597296bac57481dc2acb4a95ee3a104161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Debugowanie na języku SAML logowanie jednokrotne do aplikacji w usłudze Azure Active Directory
Podczas debugowania integracji aplikacji opartej na SAML, warto często za pomocą narzędzia, takie jak [Fiddler](http://www.telerik.com/fiddler) żądania SAML, odpowiedzi SAML i rzeczywistego tokenu SAML wystawiony do aplikacji. Po sprawdzeniu tokenu SAML zapewni, że wszystkie wymagane atrybuty, nazwę użytkownika w podmiotu SAML i wystawcy identyfikatora URI pochodzą za pośrednictwem zgodnie z oczekiwaniami.

![][1]

Odpowiedź z usługi Azure AD, który zawiera SAML token jest zwykle występuje po przekierowania HTTP 302 z https://login.windows.net, która jest wysyłana do skonfigurowanych **adres URL odpowiedzi** aplikacji. 

SAML token można wyświetlić tego wiersza, a następnie zaznaczając **inspektorzy > WebForms** kartę na prawym panelu. Z tego miejsca, kliknij prawym przyciskiem myszy **SAMLResponse** wartość, a następnie wybierz **przesyłają TextWizard**. Następnie wybierz **Base64 z** z **przekształcenie** menu do zdekodowania token i wyświetlić jego zawartość.

**Uwaga**: Aby wyświetlić zawartość tego żądania HTTP, narzędzia Fiddler może zostać wyświetlony komunikat można konfigurować odszyfrowywania ruchu HTTPS, które należy wykonać.

## <a name="related-articles"></a>Pokrewne artykuły
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurowanie logowania jednokrotnego do aplikacji, które nie znajdują się w galerii aplikacji Azure Active Directory](../active-directory-saas-custom-apps.md)
* [Dostosowywanie oświadczeń wydanych w tokenie SAML dla wstępnie zintegrowanych aplikacji](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png