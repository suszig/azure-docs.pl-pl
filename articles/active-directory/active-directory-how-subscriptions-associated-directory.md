---
title: "Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory | Microsoft Docs"
description: "Logowanie do platformy Microsoft Azure i powiązane zagadnienia, takie jak relacja między subskrypcją platformy Azure i usługą Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: 081cf2bde44a0b55508cc7f0197fa7f8e378189b
ms.contentlocale: pl-pl
ms.lasthandoff: 09/22/2017

---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory
W tym artykule opisano relację między subskrypcją platformy Azure i usługą Azure Active Directory (Azure AD) oraz sposób dodawania istniejącej subskrypcji do katalogu usługi Azure AD.

## <a name="your-azure-subscriptions-relationship-to-azure-ad"></a>Relacja subskrypcji platformy Azure i usługi Azure AD
Twoja subskrypcja platformy Azure ma relację zaufania z usługą Azure AD, co oznacza, że subskrypcja ufa katalogowi podczas uwierzytelniania użytkowników, usług i urządzeń. Wiele subskrypcji może ufać temu samemu katalogowi, ale każda subskrypcja może ufać tylko jednemu katalogowi. 

Relacja zaufania między subskrypcją a katalogiem różni się od relacji z innymi zasobami na platformie Azure (witrynami internetowymi, bazami danych itd.). Jeśli subskrypcja wygaśnie, dostęp do innych zasobów skojarzonych z tą subskrypcją również nie będzie możliwy. Katalog usługi Azure AD pozostanie jednak na platformie Azure i będzie można skojarzyć z nim inną subskrypcję oraz zarządzać katalogiem przy użyciu nowej subskrypcji.

![diagram przedstawiający sposób skojarzenia subskrypcji](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Wszyscy użytkownicy mają jeden katalog macierzysty, który ich uwierzytelnia, ale mogą również być gośćmi w innych katalogach. W usłudze Azure AD możesz zobaczyć katalogi, w których Twoje konto użytkownika jest elementem członkowskim lub gościem.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Usługa Azure AD i subskrypcje usług w chmurze
Usługa Azure AD zapewnia podstawowe możliwości zarządzania katalogiem i tożsamościami, na których bazuje większość usług w chmurze firmy Microsoft, w tym:

* Azure
* Usługa Microsoft Office 365
* Usługa Microsoft Dynamics CRM Online
* Microsoft Intune

Usługę Azure AD uzyskujesz bezpłatnie, gdy rejestrujesz się w ramach dowolnej z tych usług firmy Microsoft w chmurze. Jeśli chcesz dodać dodatkową subskrypcję platformy Azure do katalogu usługi Azure AD, musisz zalogować się za pomocą konta Microsoft. W przypadku logowania się do platformy Azure przy użyciu konta służbowego nie możesz dodać subskrypcji platformy Azure do istniejącego katalogu, ponieważ Twojego konta służbowego nie można uwierzytelnić bezpośrednio przez platformę Azure. 

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>Aby dodać istniejącą subskrypcję do katalogu usługi Azure AD
Musisz zalogować się przy użyciu konta, które istnieje w bieżącym katalogu, z którym jest skojarzona subskrypcja, oraz w katalogu, do którego chcesz ją dodać. 

1. Zaloguj się do [Centrum konta platformy Azure](https://account.azure.com/Subscriptions) przy użyciu konta należącego do administratora konta dla subskrypcji, której własność chcesz przenieść.
2. Upewnij się, że użytkownik, który ma być właścicielem subskrypcji, znajduje się w katalogu docelowym.
3. Kliknij pozycję **Przenieś subskrypcję**.
4. Określ adresata. Adresat automatycznie otrzymuje wiadomość e-mail z linkiem umożliwiającym akceptację.
5. Adresat klika link i postępuje zgodnie z instrukcjami, co obejmuje również wprowadzenie informacji dotyczących płatności. Jeśli czynności wykonywane przez adresata zakończą się pomyślnie, subskrypcja zostanie przeniesiona. 
6. Domyślny katalog subskrypcji zostanie zmieniony na katalog, w którym znajduje się użytkownik.

Aby dowiedzieć się więcej, zobacz [Transfer Azure subscription ownership to another account (Przenoszenie własności subskrypcji platformy Azure na inne konto)](../billing/billing-subscription-transfer.md).

## <a name="suggestions-to-manage-both-a-subscription-and-a-directory"></a>Sugestie dotyczące zarządzania zarówno subskrypcją, jak i katalogiem
Role administracyjne dla subskrypcji platformy Azure zarządzają zasobami powiązanymi z subskrypcją platformy Azure. Ta sekcja zawiera wyjaśnienie różnic między administratorami subskrypcji platformy Azure i administratorami katalogu usługi Azure AD. Role administracyjne i inne sugestie dotyczące ich używania do zarządzania własną subskrypcją są opisane w artykule [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md) (Przypisywanie ról administratorów w usłudze Azure Active Directory).

Domyślnie po utworzeniu konta do użytkownika jest przypisywana rola administratora usługi. Jeśli inni użytkownicy potrzebują logować się i uzyskiwać dostęp do usług za pomocą tej samej subskrypcji, możesz dodać ich jako współadministratorów. 

Usługa Azure AD ma inny zestaw ról administracyjnych na potrzeby zarządzania katalogiem i funkcjami dotyczącymi tożsamości. Na przykład administrator globalny katalogu może dodać użytkowników i grupy do katalogu lub wymusić stosowanie uwierzytelniania wieloskładnikowego przez użytkowników. Do użytkownika, który utworzył katalog, jest przypisywana rola administratora globalnego. Może on przypisać role administracyjne do innych użytkowników. Role administracyjne usługi Azure AD są również używane przez inne usługi, takie jak Microsoft Intune i Office 365. 

Administratorzy subskrypcji platformy Azure i administratorzy katalogu usługi Azure AD to dwie oddzielne role. 
* Administratorzy subskrypcji platformy Azure mogą zarządzać zasobami na platformie Azure i mogą używać usługi Azure AD w witrynie Azure Portal (ponieważ sama witryna Azure Portal jest zasobem platformy Azure). 
* Administratorzy katalogu mogą zarządzać właściwościami w katalogu usługi Azure AD.

Osoba może pełnić obie te role, lecz nie jest to wymagane. Administrator globalny katalogu nie musi być administratorem usługi ani współadministratorem subskrypcji platformy Azure lub odwrotnie. Nie będąc administratorem subskrypcji, użytkownik może zalogować się do witryny Azure Portal, ale nie może zarządzać katalogami dla tej subskrypcji w portalu. Ten użytkownik może jednak zarządzać katalogami przy użyciu innych narzędzi, takich jak program Azure AD PowerShell lub Centrum administracyjne usługi Office 365.

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej o zmianie administratorów subskrypcji platformy Azure, zobacz [Transfer ownership of an Azure subscription to another account](../billing/billing-subscription-transfer.md) (Przenoszenie własności subskrypcji platformy Azure na inne konto)
* Aby dowiedzieć się więcej o kontrolowaniu dostępu do zasobów na platformie Microsoft Azure, zobacz [Understanding resource access in Azure](active-directory-understanding-resource-access.md) (Opis dostępu do zasobów na platformie Azure).
* Aby uzyskać więcej informacji na temat sposobu przypisywania ról w usłudze Azure AD, zobacz [Przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

