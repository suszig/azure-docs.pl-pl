---
title: "Dostosowywanie mapowań atrybutów usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jakie mapowań atrybutów dla aplikacji SaaS w usłudze Azure Active Directory są, jak można dostosować je do adresów potrzeb biznesowych."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6921ca86efeea9d1255bb2d1773f55daa48b9b4a
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Dostosowywanie użytkownika udostępniania mapowań atrybutów dla aplikacji SaaS w usłudze Azure Active Directory
Inicjowanie obsługi użytkowników do aplikacji SaaS innych firm, takie jak Salesforce, Google Apps i innych użytkowników usługi Microsoft Azure AD zapewnia obsługę. Jeśli masz użytkownika Inicjowanie obsługi administracyjnej dla aplikacji SaaS innych firm włączone portalu zarządzania Azure określa jego wartości atrybutów w postaci konfiguracji o nazwie "mapowanie atrybutu".

Brak zestawu wstępnie skonfigurowanych atrybutu mapowania między obiektami użytkownika usługi Azure AD i każda aplikacja SaaS użytkownika. Niektóre aplikacje zarządzania innych typów obiektów, takich jak grup ani kontaktów. <br> 
 Mapowanie atrybutów domyślne można dostosować zgodnie z potrzebami firmy. Oznacza to, że możesz zmienić lub usunąć istniejące mapowania atrybutu lub utworzyć nowe mapowanie atrybutów.

W portalu usługi Azure AD, możesz korzystać z tej funkcji, klikając **mapowania** Konfiguracja **inicjowania obsługi administracyjnej** w **Zarządzaj** części  **Aplikacja całościowa**.


![SalesForce][5] 

Kliknięcie przycisku **mapowania** konfiguracji, otwiera pokrewny **mapowanie atrybutu** bloku.  
Brak mapowań atrybutów, które są wymagane przez aplikacji SaaS, aby mógł działać poprawnie. Dla wymaganych atrybutów **usunąć** funkcja jest niedostępna.


![SalesForce][6]  

W powyższym przykładzie można stwierdzić, że **Username** atrybutu zarządzanego obiektu w usłudze Salesforce jest wypełniana **userPrincipalName** wartość połączonego Azure obiektu usługi Active Directory.

Można dostosować istniejący **mapowań atrybutów** klikając mapowania. Spowoduje to otwarcie **atrybutu Edytuj** bloku.

![SalesForce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Opis atrybutu mapowania typów
Z mapowań atrybutów możesz kontrolować sposób atrybuty są wypełnione w aplikacji SaaS innych firm. Istnieją cztery typy innego mapowania obsługiwane:

* **Bezpośrednie** — atrybut docelowy jest wypełniane przy użyciu wartości atrybutu połączonego obiektu w usłudze Azure AD.
* **Stała** — atrybut docelowy jest wypełniana określony ciąg został określony.
* **Wyrażenie** — atrybut docelowy jest wypełniana na podstawie wyniku wyrażenia przypominającej skryptu. 
  Aby uzyskać więcej informacji, zobacz [pisać wyrażenia potrzeby mapowań atrybutów w usłudze Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Brak** — atrybut docelowy zostanie pozostawiony bez modyfikacji. Jednak jeśli atrybut docelowy jest kiedykolwiek pusta, jest wypełnione wartością domyślną, który określisz.

Oprócz tych cztery typy mapowanie atrybutu podstawowego mapowań atrybutów niestandardowych obsługuje pojęcie opcjonalny **domyślne** przypisanie wartości. Przypisanie wartości domyślne gwarantuje, że atrybut docelowy jest wypełniana wartości, które nie istnieje żadna wartość w usłudze Azure AD, ani na obiekcie docelowym. Najbardziej typowych konfiguracji jest to pole pozostanie puste.


## <a name="understanding-attribute-mapping-properties"></a>Opis właściwości Mapowanie atrybutów

W poprzedniej sekcji możesz już zostały wprowadzone do właściwości typu atrybutu mapowania.
Oprócz tej właściwości mapowań atrybutów obsługują następujące atrybuty:

- **Atrybut źródłowy** — atrybut użytkownika z systemu źródłowego (np.: Azure Active Directory).
- **Atrybut TARGET** — atrybut użytkownika w systemie docelowym (np.: ServiceNow).
- **Zgodne obiektów przy użyciu tego atrybutu** — czy to mapowanie powinien być używany do jednoznacznego identyfikowania użytkowników między systemami źródłowym i docelowym. Jest to zazwyczaj ustawiana na atrybut userPrincipalName lub poczty w usłudze Azure AD, która zwykle jest mapowana na pole nazwy użytkownika w aplikacji docelowej.
- **Dopasowywanie pierwszeństwo** — wiele pasujących atrybuty można ustawić. Jeśli dostępnych jest wiele, są oceniane pod w kolejności wynika z tego pola. Jak dopasowania zostanie znaleziony, żadne dodatkowe dopasowania atrybuty są oceniane.
- **Zastosuj to mapowanie**
    - **Zawsze** — Zastosuj to mapowanie na obu Tworzenie użytkownika i aktualizowanie akcji
    - **Tylko podczas tworzenia** -Zastosuj to mapowanie tylko akcje creation użytkownika


## <a name="what-you-should-know"></a>Co należy wiedzieć

Microsoft Azure AD zapewnia wydajne wykonania procesu synchronizacji. W środowisku zainicjowana tylko obiekty wymagające aktualizacji są przetwarzane podczas cyklu synchronizacji. Aktualizowanie mapowań atrybutów ma wpływ na wydajność cyklu synchronizacji. Aktualizacja konfiguracji mapowania atrybut wymaga wszystkich obiektów zarządzanych go obliczyć ponownie. Jest zalecanym najlepszym rozwiązaniem aby utrzymać liczbę kolejnych zmian do mapowania atrybutu co najmniej.

## <a name="next-steps"></a>Następne kroki

* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Automatyzowanie użytkownika udostępniania/anulowania obsługi do aplikacji SaaS](active-directory-saas-app-provisioning.md)
* [Tworzenie wyrażeń na potrzeby mapowań atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry zakresu dla Inicjowanie obsługi użytkowników](active-directory-saas-scoping-filters.md)
* [Włączanie automatycznej aprowizacji użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
* [Powiadomienia aprowizacji kont](active-directory-saas-account-provisioning-notifications.md)
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

