---
title: "Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory"
description: "Jak wyświetlić listę aplikacji, która obsługuje logowanie jednokrotne w galerii Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 0c324829469b9babe6608480204bd46691f84228
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory
Aby wyświetlić listę aplikacji, która obsługuje logowanie jednokrotne z usługą Azure Active Directory w [galerii Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikacja musi najpierw implementować jeden z następujących trybów integracji:

* **OpenID Connect** -bezpośrednia Integracja z usługą Azure AD przy użyciu protokołu OpenID Connect do uwierzytelniania i Azure AD zgody interfejsu API dla konfiguracji. Jeśli aplikacja nie obsługuje SAML integracji jest uruchamiana, to tryb zaleca się.
* **SAML** — aplikacja już ma możliwość konfigurowania przy użyciu protokołu SAML dostawcy tożsamości innych firm.

Listę wymagań dotyczących każdego trybu są poniżej.

## <a name="openid-connect-integration"></a>OpenID Connect integracji
Integracja aplikacji z usługą Azure AD po [instrukcje developer](active-directory-authentication-scenarios.md). Następnie wypełnij poniższe pytania i wysłać do waadpartners@microsoft.com.

* Podaj poświadczenia dla dzierżawy testowej lub konto z aplikacji, która może służyć przez zespół usługi Azure AD do testowania integracji.  
* Zawierają informacje o tym jak zespół usługi Azure AD zalogowanie się i nawiązać połączenia z wystąpieniem usługi Azure AD przy użyciu aplikacji [framework zgody usługi Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Podaj wszelkie dodatkowe informacje wymagane przez zespół usługi Azure AD do testowania rejestracji jednokrotnej z aplikacją. 
* Podaj poniższe informacje:

> Nazwa firmy:
> 
> Witryna sieci Web firmy:
> 
> Nazwa aplikacji:
> 
> Opis aplikacji (limit 200 znaków):
> 
> Aplikacji witryny sieci Web (informacyjnych):
> 
> Witryny sieci Web aplikacji Technical pomocy technicznej lub informacje o kontaktach:
> 
> Identyfikator aplikacji, jak pokazano w sekcji szczegółów aplikacji w https://portal.azure.com:
> 
> Gdy klienci przejdź do zarejestrowania się w adres URL zapisywania się do aplikacji i notebooka zakupu aplikacji:
> 
> Należy wybrać maksymalnie trzy kategorie aplikacji do wyświetlenia na liście (Aby uzyskać dostępne kategorie, zobacz Azure Active Directory Marketplace):
> 
> Dołącz małych ikon aplikacji (plik PNG, 45px przez 45px, pełny kolor tła):
> 
> Dołącz dużych ikon aplikacji (plik PNG, 215px przez 215px, pełny kolor tła):
> 
> Dołącz Logo aplikacji (plik PNG, 150px przez 122px, przezroczystego tła):
> 
> 

## <a name="saml-integration"></a>Integrację SAML
Dowolną aplikację, który obsługuje SAML 2.0 może być bezpośrednio integrowany z dzierżawy usługi Azure AD przy użyciu [te instrukcje, aby dodać niestandardową aplikację](../application-config-sso-how-to-configure-federated-sso-non-gallery.md). Po przetestowaniu się, że integracją aplikacji współpracuje z usługą Azure AD, należy wysłać następujące informacje, aby < mailto:waadpartners@microsoft.com >.

* Podaj poświadczenia dla dzierżawy testowej lub konto z aplikacji, która może służyć przez zespół usługi Azure AD do testowania integracji.  
* Podaj SAML adres URL logowania, adres URL wystawcy (identyfikator jednostki) i adres URL odpowiedzi (usługa konsumenta potwierdzenia) wartości dla aplikacji, zgodnie z opisem [tutaj](../application-config-sso-how-to-configure-federated-sso-non-gallery.md). Jeśli podasz zwykle te wartości jako część pliku metadanych SAML, można wysyłać również.
* Podaj krótki opis sposobu konfigurowania usługi Azure AD jako dostawcy tożsamości w aplikacji przy użyciu SAML 2.0. Jeśli aplikacja obsługuje konfigurowanie usługi Azure AD jako dostawcy tożsamości za pośrednictwem portalu samoobsługowego administracyjne, następnie upewnij się, że poświadczenia podane powyżej obejmują możliwość tej konfiguracji.
* Podaj poniższe informacje:

> Nazwa firmy:
> 
> Witryna sieci Web firmy:
> 
> Nazwa aplikacji:
> 
> Opis aplikacji (limit 200 znaków):
> 
> Aplikacji witryny sieci Web (informacyjnych):
> 
> Witryny sieci Web aplikacji Technical pomocy technicznej lub informacje o kontaktach:
> 
> Gdy klienci przejdź do zarejestrowania się w adres URL zapisywania się do aplikacji i notebooka zakupu aplikacji:
> 
> Wybrać maksymalnie trzy kategorie aplikacji do wyświetlenia na liście (dostępne kategorie można znaleźć [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Dołącz małych ikon aplikacji (plik PNG, 45px przez 45px, pełny kolor tła):
> 
> Dołącz dużych ikon aplikacji (plik PNG, 215px przez 215px, pełny kolor tła):
> 
> Dołącz Logo aplikacji (plik PNG, 150px przez 122px, przezroczystego tła):
> 
> 

