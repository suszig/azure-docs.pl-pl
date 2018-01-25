---
title: "Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory"
description: "Jak wyświetlić listę aplikacji, która obsługuje logowanie jednokrotne w galerii Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory


##  <a name="what-is-azure-ad-app-gallery"></a>Co to jest galerii aplikacji Azure AD?

Usługi Azure AD to usługa, tożsamości opartej na chmurze. [Galerii aplikacji usługi Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) wspólnej magazynu, gdzie wszystkie łączniki aplikacji są publikowane w celu logowania jednokrotnego i Inicjowanie obsługi użytkowników. Szukaj naszych wzajemne klientów, którzy używają usługi Azure AD jako dostawcy tożsamości dla różnych łączników aplikacji SaaS, które są publikowane w tym miejscu. IT administrator dodaje łącznika z galerii aplikacji i konfiguruje i użyć jej do logowania jednokrotnego i inicjowania obsługi administracyjnej. Usługi Azure AD obsługuje wszystkie protokoły głównych federacyjnych jak SAML 2.0, OpenID Connect, OAuth i WS-Fed rejestracji jednokrotnej. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Jakie są zalety listę aplikacji w galerii?

*  Zapewnić najlepsze możliwe pojedynczego logowania jednokrotnego środowisko z klientami.

*  Proste i minimalnej konfiguracji aplikacji.

*  Klienci mogą wyszukiwania aplikacji i go znaleźć w galerii. 

*  Każdy klient można użyć tej integracji niezależnie od wolne jednostki SKU programu Azure AD, podstawowa lub Premium.

*  Krok samouczka konfiguracji krok wzajemne klientów.

*  Włącz inicjowanie obsługi użytkowników dla tej samej aplikacji, jeśli są przy użyciu SCIM.


##  <a name="what-are-the-pre-requisites"></a>Jakie są wymagania wstępne?

Aby wyświetlić listę aplikacji w galerii Azure AD, aplikacja najpierw musi implementować jeden z federacyjnego protokoły obsługiwane przez usługę Azure AD. Przeczytaj warunki i postanowienia galerii aplikacji Azure AD, w tym miejscu. Jeśli używasz: 

*   **OpenID Connect** — tworzenie wielodostępnych aplikacji w usłudze Azure AD i zaimplementować [framework zgody usługi Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) aplikacji. Wyślij żądanie logowania do wspólnego punktu końcowego tak, aby każdy klient może zapewnić zgody do aplikacji. Można kontrolować dostęp użytkownika klienta na podstawie Identyfikatora dzierżawy i nazwy UPN użytkownika odebrane w tokenie. Integracja aplikacji z usługą Azure AD, możesz wykonać [instrukcje developer](active-directory-authentication-scenarios.md).

*   **SAML 2.0 i WS-Fed** — aplikacji powinien mieć możliwość czy integrację SAML/WS-Fed logowania jednokrotnego w trybie SP lub dostawców tożsamości. Dowolną aplikację, który obsługuje SAML 2.0, może być bezpośrednio integrowany z dzierżawa usługi Azure AD przy użyciu [instrukcje dotyczące dodawania aplikacji niestandardowej](../active-directory-saas-custom-apps.md).

*   **Hasło logowania jednokrotnego** — tworzenie aplikacji sieci web strony logowania HTML do skonfigurowania [opartego na hasłach rejestracji jednokrotnej](../active-directory-appssoaccess-whatis.md). Logowanie Jednokrotne opartego na hasłach, także określane jako hasło vaulting, umożliwia zarządzanie dostępem użytkowników i hasła do aplikacji sieci web, które nie obsługują federacji tożsamości. Jest również przydatne w przypadku scenariuszy, w którym musi kilku użytkowników do udostępnienia jednego konta, takich jak do kont aplikacji mediów społecznościowych w organizacji. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Proces przesyłania żądania w portalu

Po przetestowaniu się, że integracją aplikacji współpracuje z usługą Azure AD, należy przesłać żądanie dostępu w naszym [Portal sieci aplikacji](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Jeśli masz konto usługi Office 365, można użyć które w przeciwnym razie zalogować się do tego portalu, należy użyć Identyfikatora Microsoft (Live ID, Outlook, Hotmail itp.) do logowania. Zobacz następujące strony na żądanie dostępu. Podaj biznesowego wyjaśnienia, w polu tekstowym i kliknij **żądania dostępu**. Nasz zespół będzie zapoznaj się ze szczegółami i umożliwiają dostęp odpowiednio. Po wykonaniu tej możesz zalogować się do portalu i przesłać żądanie szczegółowe dla aplikacji.

Jeśli przed wszelkie problemy dotyczące dostępu, skontaktuj się z [zespołu integracji usługi Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Żądanie dostępu w portalu programu SharePoint](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Osi czasu
    
*   Proces listę SAML 2.0 i WS-Fed aplikacji do galerii - **7 10 dni**

   ![Wyświetlanie listy aplikacji saml do galerii osi czasu](./media/active-directory-app-gallery-listing/timeline.png)

*   Proces wyświetlania OpenID Connect aplikacji do galerii - **2-5 dni roboczych**

   ![Wyświetlanie listy aplikacji saml do galerii osi czasu](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Eskalacji

Odpowiedzi na wszystkie pytania techniczne, Porzuć wiadomość e-mail na adres [zespołu integracji usługi Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) i możemy zająć się JNW.

