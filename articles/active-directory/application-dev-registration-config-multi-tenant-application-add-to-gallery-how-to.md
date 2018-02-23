---
title: "Jak dodać aplikację wielodostępne do galerii aplikacji Azure AD | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak można wyświetlić listę własnej rozwinięte wielodostępnych aplikacji w galerii aplikacji usługi Azure AD"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Jak dodać aplikację wielodostępne do galerii aplikacji Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>Co to jest galerii aplikacji usługi Azure AD?

Usługi Azure AD to usługa, tożsamości opartej na chmurze. [Galerii aplikacji usługi Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) wspólnej magazynu, gdzie wszystkie łączniki aplikacji są publikowane w celu logowania jednokrotnego i Inicjowanie obsługi użytkowników. Szukaj naszych wzajemne klientów, którzy używają usługi Azure AD jako dostawcy tożsamości dla różnych łączników aplikacji SaaS, które są publikowane w tym miejscu. IT administrator dodaje łącznika z galerii aplikacji i konfiguruje i użyć jej do logowania jednokrotnego i inicjowania obsługi administracyjnej. Usługi Azure AD obsługuje wszystkie protokoły głównych federacyjnych jak SAML 2.0, OpenID Connect, OAuth i WS-Fed rejestracji jednokrotnej. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Jeśli aplikacja obsługuje SAML lub OpenIDConnect
Jeśli masz aplikację wielodostępne, który chcesz wyświetlić w galerii aplikacji usługi Azure AD, najpierw należy się upewnić, czy aplikacja obsługuje jeden z następujących pojedynczego logowania jednokrotnego technologii:

1. **OpenID Connect** — tworzenie wielodostępnych aplikacji w usłudze Azure AD i zaimplementować [framework zgody usługi Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) aplikacji. Wyślij żądanie logowania do wspólnego punktu końcowego tak, aby każdy klient może zapewnić zgody do aplikacji. Można kontrolować dostęp użytkownika klienta na podstawie Identyfikatora dzierżawy i nazwy UPN użytkownika odebrane w tokenie. Prześlij aplikacji, jak wspomniano w tym [artykułu](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

2. **SAML** — Jeśli aplikacja obsługuje SAML 2.0, a następnie można utworzyć listę aplikacji w galerii i przedstawiono instrukcje [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Jeśli aplikacja obsługuje jeden z tych pojedynczego trybów logowania jednokrotnego i chcesz wyświetlanie wielodostępnych aplikacji w galerii aplikacji usługi Azure AD, można wykonać czynności wymienionych w [to](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artykułu. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Jeśli aplikacja nie obsługuje SAML lub OpenIDConnect
Nawet wtedy, gdy aplikacja nie obsługuje jeden z trybów, firma Microsoft może nadal zintegrować ją z galerii przy użyciu technologii nasze hasło logowania jednokrotnego.

**Hasło logowania jednokrotnego** — tworzenie aplikacji sieci web strony logowania HTML do skonfigurowania [opartego na hasłach rejestracji jednokrotnej](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). Logowanie Jednokrotne opartego na hasłach, także określane jako hasło vaulting, umożliwia zarządzanie dostępem użytkowników i hasła do aplikacji sieci web, które nie obsługują federacji tożsamości. Jest również przydatne w przypadku scenariuszy, w którym musi kilku użytkowników do udostępnienia jednego konta, takich jak do kont aplikacji mediów społecznościowych w organizacji. 

Jeśli chcesz wyświetlić listę aplikacji z tej technologii następnie, Prześlij żądanie, zgodnie z opisem w [to](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artykułu.

## <a name="escalations"></a>Eskalacji

Odpowiedzi na wszystkie pytania techniczne, Porzuć wiadomość e-mail na adres [zespołu integracji usługi Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) i możemy zająć się JNW.

## <a name="next-steps"></a>Kolejne kroki
[Jak wyświetlić listę aplikacji w galerii aplikacji usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
