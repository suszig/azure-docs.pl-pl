---
title: "Zarządzanie dostępem do aplikacji SaaS przy użyciu grupy | Dokumentacja firmy Microsoft"
description: "Jak używać grup w usłudze Azure Active Directory — wersja Premium lub Basic udzielania dostępu do aplikacji SaaS, które są zintegrowane z usługą Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 4aee5b28fd42be98fac8f9c7d61538319e3efd9a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Używanie grupy do zarządzania dostępem do aplikacji SaaS
Za pomocą usługi Azure Active Directory (Azure AD) z licencją Azure AD Premium lub usługi Azure AD podstawowa, można użyć grupy do udzielania dostępu do aplikacji SaaS, który jest zintegrowany z usługą Azure AD. Na przykład, jeśli ma zostać przypisany dostęp dla działu marketingu do używania pięciu różnych aplikacji SaaS, można utworzyć grupę, która zawiera użytkowników w dziale marketingu i przypisz następujące pięć aplikacji SaaS, które są wymagane przez tę grupę Dział marketingu. W ten sposób można oszczędzić czas dzięki zarządzaniu członkostwa w dziale marketingu w jednym miejscu. Następnie przypisywania użytkowników do aplikacji podczas zostaną dodane jako członkowie grupy marketing, i ich przypisania usunięte z aplikacji w przypadku usunięcia ich z grupy marketing. Ta możliwość może służyć z setkami aplikacji, które można dodać z w galerii aplikacji usługi Azure AD.

> [!IMPORTANT]
> Ta funkcja służy tylko wtedy, gdy zakupu licencji Azure AD Premium lub usługi Azure AD podstawowa lub uruchomić wersji próbnej platformy Azure AD Premium. Przypisywanie na podstawie grupy jest obsługiwana tylko dla grup zabezpieczeń. Aktualnie w ramach przypisywania do aplikacji na podstawie grup nie jest obsługiwane członkostwo w grupach zagnieżdżonych.

**Aby przypisać dostępu dla użytkownika lub grupy do aplikacji SaaS**

1. W [Centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com), wybierz pozycję **aplikacje dla przedsiębiorstw**.
2. Wybierz aplikację dodano z galerii aplikacji, aby go otworzyć.
3. Wybierz **użytkowników i grup**, a następnie wybierz **Dodaj użytkownika**.
4. Na **Dodaj przydziału**, wybierz pozycję **użytkowników i grup** otworzyć **użytkowników i grup** listy wyboru.
6. Wybierz dowolną liczbę grup lub użytkowników, można dowolnie, następnie kliknij lub naciśnij polecenie **wybierz** dodanie ich do **Dodaj przydziału** listy. Na tym etapie można także przypisać rolę użytkownikowi.
7. Wybierz **przypisać** można przypisać użytkowników lub grup, do aplikacji wybranej przedsiębiorstwa.

### <a name="next-steps"></a>Kolejne kroki
Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Polecenia cmdlet usługi Azure Active Directory służące do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Co to jest usługa Azure Active Directory?](active-directory-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)
