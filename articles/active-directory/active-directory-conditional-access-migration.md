---
title: Migracja zasad klasycznego w portalu Azure | Dokumentacja firmy Microsoft
description: Migracja zasad klasycznego w portalu Azure.
services: active-directory
keywords: "dostęp warunkowy do aplikacji, dostęp warunkowy przy użyciu usługi Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c584eddb5542c2c49d08d35bcaf8e7acb5c5b83a
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migracja zasad klasycznego w portalu Azure 


[Dostęp warunkowy](active-directory-conditional-access-azure-portal.md) jest możliwość Azure Active Directory (Azure AD), który umożliwia kontrolowanie sposobu autoryzacji dostępu użytkowników aplikacji w chmurze. Podczas celem jest taka sama, wersji portalu Azure ma również wprowadzono znaczne ulepszenia w sposobie działania dostępu warunkowego. Zasady dostępu warunkowego, które zostały skonfigurowane poza portalem Azure mogą współistnieć z nowych zasad tworzonych w portalu Azure. Tak długo, jak nie są wyłączenie lub usunięcie ich, nadal są stosowane w danym środowisku. Firma Microsoft zaleca jednak migracja zasad klasycznego do nowej usługi Azure AD warunkowe zasady dostępu, ponieważ:

- Nowe zasady umożliwiają scenariuszy adres, który nie może obsłużyć przy użyciu klasycznego zasad.

- Pozwala zmniejszyć liczbę zasad, które trzeba zarządzać konsolidując je.   

Ten temat pomaga w migracji istniejących zasad klasycznego do nowego nowej usługi Azure AD, zasady dostępu warunkowego.


## <a name="classic-policies"></a>Zasady klasycznego

Zasady dostępu warunkowego dla usługi Azure AD i usługi Intune nie zostały utworzone w portalu Azure są również znane jako **klasycznego zasady**. Aby migrować klasyczny zasad, nie trzeba mieć dostęp do klasycznego portalu Azure. Azure portal udostępnia [ **zasady klasyczny (wersja zapoznawcza)** widoku](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) umożliwiająca Przejrzyj zasady klasycznego.

![Usługa Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


### <a name="open-a-classic-policy"></a>Otwórz zasadę klasycznego

**Aby otworzyć klasycznego zasad:**

1. W [portalu Azure](https://portal.azure.com), na lewy pasek nawigacyjny, kliknij przycisk **usługi Azure Active Directory**.

    ![Usługa Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Na **usługi Azure Active Directory** strony w **Zarządzaj** kliknij **dostępu warunkowego**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/02.png)
 
2. Na **dostępu warunkowego — zasady** strony w **Zarządzaj** kliknij **zasady klasyczny (wersja zapoznawcza)**.

3. Z listy klasycznych zasady wybierz zasady, które są dla Ciebie ważne.   

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/34.png)



## <a name="azure-ad-conditional-access-policies"></a>Zasady dostępu warunkowego w usłudze Azure AD

Ten temat zawiera zasady z szczegółowy opis kroków umożliwiających migracji zasad klasycznego bez zapoznać się z dostępu warunkowego dla usługi Azure AD. Jednak trwa zapoznać się z terminologią i podstawowe pojęcia dotyczące usługi Azure AD pozwala udoskonalić migracji dostępu warunkowego.

Zobacz:

- [Dostęp warunkowy w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md) informacje na temat podstawowych pojęć i terminologii

- [Rozpoczynanie pracy z dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) zapoznać się z interfejsu użytkownika w portalu Azure


 





## <a name="multi-factor-authentication-policy"></a>Zasady uwierzytelniania wieloskładnikowego 

W tym przykładzie pokazano, jak przeprowadzić migrację klasycznego zasadę, która wymaga usługi Multi-Factor authentication ** dla aplikacji w chmurze. 

![Usługa Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


**Aby przeprowadzić migrację klasycznego zasad:**

1. [Otwórz klasyczny zasad](#open-a-classic-policy) można pobrać ustawień konfiguracji.
2. Utwórz nowe zasady dostępu warunkowego usługi Azure AD zastąpić klasycznego zasad. 


### <a name="create-a-new-conditional-access-policy"></a>Utwórz nowe zasady dostępu warunkowego


1. W [portalu Azure](https://portal.azure.com), na lewy pasek nawigacyjny, kliknij przycisk **usługi Azure Active Directory**.

    ![Usługa Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Na **usługi Azure Active Directory** strony w **Zarządzaj** kliknij **dostępu warunkowego**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/02.png)



3. Na **dostępu warunkowego** strony, aby otworzyć **nowy** na pasku narzędzi u góry, kliknij przycisk **Dodaj**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/03.png)

4. Na **nowy** strony w **nazwa** tekstowym, wpisz nazwę zasady.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/29.png)

5. W **przypisania** kliknij **użytkowników i grup**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/05.png)

    a. Jeśli wszyscy użytkownicy wybrane w zasadach klasycznym kliknij **wszyscy użytkownicy**. 

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/35.png)

    b. Jeśli wybrana w klasycznym zasad grupy, kliknij przycisk **Wybierz użytkowników i grupy**, a następnie wybierz wymaganych użytkowników i grup.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/36.png)

    c. Jeśli masz wykluczone grupy, kliknij przycisk **wykluczyć** , a następnie wybierz wymaganych użytkowników i grup. 

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/37.png)

6. Na **nowy** strony, aby otworzyć **aplikacji w chmurze** strony w **przypisania** kliknij **aplikacji w chmurze**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Na **aplikacji w chmurze** wykonaj następujące czynności:

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/08.png)

    a. Kliknij przycisk **Wybierz aplikacje**.

    b. Kliknij pozycję **Wybierz**.

    c. Na **wybierz** wybierz aplikacji w chmurze, a następnie kliknij przycisk **wybierz**.

    d. Na **aplikacji w chmurze** kliknij przycisk **gotowe**.



9. Jeśli masz **wymusić uwierzytelnianie wieloskładnikowe** wybranych:

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/26.png)

    a. W **dostęp do formantów** kliknij **Grant**.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/27.png)

    b. Na **Grant** kliknij przycisk **udzielić dostępu**, a następnie kliknij przycisk **wymusić uwierzytelnianie wieloskładnikowe**.

    c. Kliknij pozycję **Wybierz**.


10. Kliknij przycisk **na** Aby włączyć zasady.

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/30.png)

11. Wyłącz zasady klasycznego. 

    ![Dostęp warunkowy](./media/active-directory-conditional-access-migration/38.png)



 


## <a name="next-steps"></a>Następne kroki

- Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego dla danego środowiska, zobacz [najlepszych rozwiązań dotyczących dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-best-practices.md). 
