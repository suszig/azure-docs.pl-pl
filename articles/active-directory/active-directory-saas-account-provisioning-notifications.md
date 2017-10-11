---
title: "Inicjowanie obsługi administracyjnej powiadomienia konta | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak i upewnij się, że użytkownik jest powiadamiany o problemy związane z Inicjowanie obsługi użytkowników, które wymagają Twojej uwagi, umożliwiając Inicjowanie obsługi administracyjnej powiadomienia konta."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: b99037fc28eca1a3ebffefb9e99991e74f52c9a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="account-provisioning-notifications"></a>Powiadomienia związane z aprowizacją kont
Obsługa administracyjna użytkownika, można zautomatyzować proces zarządzania użytkowników w innej aplikacji SaaS. <br>
Jest to zautomatyzowany proces, interakcję z tym procesem od czasu do czasu jest wymagana. <br>
Dotyczy, na przykład, gdy hasło konta, które zostały skonfigurowane w celu wymiany danych z innej strony wygasła aplikacji SaaS. 

Przez włączenie inicjowania obsługi powiadomień konta, możesz upewnij się, że użytkownik jest powiadamiany o problemy związane z Inicjowanie obsługi użytkowników, które wymagają Twojej uwagi.

Aktywować lub dezaktywować inicjowania obsługi powiadomienia użytkownika inicjowania obsługi konfiguracji dla innej aplikacji SaaS w ramach konta.

![Inicjowanie obsługi użytkowników][1] 

Aby aktywować konto inicjowania obsługi powiadomień, zaznacz pole wyboru powiązane na **potwierdzenie** strony okna dialogowego, a następnie wpisz alias e-mail odbiorcy.

![Powiadomienia związane z aprowizacją kont][2]

Możesz wprowadzić listę dystrybucyjną jako recipient; jest jednak należy pamiętać, że powiadomienie e-mail zawiera linki do raportów dostępnych tylko przez administratorów usługi Azure AD.

Jeśli masz konto inicjowania obsługi powiadomień włączone, będziesz otrzymywać wiadomości e-mail o krytyczne problemy, które są powiązane z Inicjowanie obsługi użytkowników. Jednak aby uniknąć przeciążenia poczty e-mail, otrzymasz tylko jednego powiadomienia e-mail dziennie dla każdej aplikacji SaaS powiadomienia e-mail jest włączone dla.

## <a name="related-articles"></a>Pokrewne artykuły
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Automatyzowanie użytkownika udostępniania/anulowania obsługi do aplikacji SaaS](active-directory-saas-app-provisioning.md)
* [Dostosowywanie mapowań atrybutów do inicjowania obsługi użytkowników](active-directory-saas-customizing-attribute-mappings.md)
* [Tworzenie wyrażeń na potrzeby mapowań atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry zakresu dla Inicjowanie obsługi użytkowników](active-directory-saas-scoping-filters.md)
* [Włączanie automatycznej aprowizacji użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
