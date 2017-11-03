---
title: "Grupy w usłudze Azure Active Directory w wersji dedykowanej | Dokumentacja firmy Microsoft"
description: "Przegląd grup jak dedykowanych działają w usłudze Azure Active Directory oraz sposób ich tworzenia."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;oldportal
ms.openlocfilehash: 992f4563064d7a292cf4fdd90a9a3c84cdec91c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Grupy dedykowane w usłudze Azure Active Directory
W usłudze Azure Active Directory (Azure AD) funkcja grupy dedykowane automatycznie tworzy i wypełnia członkostwa w grupach usługi Azure AD, wstępnie zdefiniowane. Członkowie grupy dedykowane nie można dodać lub usunąć przy użyciu klasycznego portalu Azure, poleceń cmdlet programu Windows PowerShell, lub programowo.

> [!NOTE]
> Grupy dedykowane wymagają przypisania do licencji Azure AD Premium
>
> * Administratora zarządzającego regułą grupy
> * Wszyscy użytkownicy, którzy są wybrane przez regułę jako członek grupy
>
>

**Aby włączyć grupy dedykowane**

1. W [portalu Azure](https://portal.azure.com), wybierz pozycję **usługi Active Directory**, a następnie otwórz katalogu organizacji.
2. Wybierz **grup** karcie, a następnie otwórz grupę, którą chcesz edytować.
3. Wybierz **Konfiguruj** karcie, a następnie ustaw **Włącz grupy dedykowane** do **tak**.

Po ustawieniu przełącznika włączyć grupy dedykowane **tak**, dodatkowo można włączyć w katalogu, aby automatycznie utworzyć dedykowane grupie Wszyscy użytkownicy, ustawiając **włączyć "Wszyscy użytkownicy" grupy** przełączyć się do  **Tak**. Następnie można również edytować nazwę tej grupy dedykowane wpisując ją **"Wszyscy użytkownicy" Nazwa wyświetlana grupy** pola.

Grupy Wszyscy użytkownicy, można przypisać te same uprawnienia do wszystkich użytkowników w katalogu. Na przykład można przyznać wszystkim użytkownikom w katalogu dostęp do aplikacji SaaS, przypisując dostępu dla grupy dedykowane wszystkich użytkowników w tej aplikacji.

Dedykowany grupy Wszyscy użytkownicy należą wszyscy użytkownicy w katalogu, w tym gości i użytkowników zewnętrznych. Jeśli potrzebujesz grupy który wyklucza użytkowników zewnętrznych, a następnie można to zrobić, tworząc grupy z opartych na atrybutach dynamiczne reguły, takie jak następujące:

                (user.userPrincipalName -notContains "#EXT#@")

Grupy, która nie obejmuje wszystkich gości należy użyć reguły podobne do poniższych:

                (user.userType -ne "Guest")

Aby dowiedzieć się, jak tworzyć reguły *zaawansowane* (czyli takie, które mogą zawierać więcej niż jedno porównanie) na potrzeby dynamicznego zarządzania członkostwem w grupach, zobacz [Tworzenie reguł zaawansowanych za pomocą atrybutów](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Następne kroki
Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Co to jest usługa Azure Active Directory?](active-directory-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)
