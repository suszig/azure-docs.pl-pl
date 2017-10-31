---
title: "Jak dodać istniejącą subskrypcję platformy Azure do katalogu usługi Azure AD | Microsoft Docs"
description: "Jak dodać istniejącą subskrypcję do katalogu usługi Azure AD"
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
ms.date: 10/17/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: abf207a3ceec708a828170936f7dc7948ccf34a9
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-add-an-azure-subscription-to-azure-active-directory"></a>Jak dodać subskrypcję platformy Azure do katalogu usługi Azure Active Directory
W tym artykule opisano relację między subskrypcją platformy Azure i usługą Azure Active Directory (Azure AD) oraz sposób dodawania istniejącej subskrypcji do katalogu usługi Azure AD. Twoja subskrypcja platformy Azure ma relację zaufania z usługą Azure AD, co oznacza, że subskrypcja ufa katalogowi podczas uwierzytelniania użytkowników, usług i urządzeń. Wiele subskrypcji może ufać temu samemu katalogowi, ale każda subskrypcja może ufać tylko jednemu katalogowi. 

Relacja zaufania między subskrypcją a katalogiem różni się od relacji z innymi zasobami na platformie Azure (witrynami internetowymi, bazami danych itd.). Jeśli subskrypcja wygaśnie, dostęp do innych zasobów skojarzonych z tą subskrypcją również nie będzie możliwy. Katalog usługi Azure AD pozostanie jednak na platformie Azure i będzie można skojarzyć z nim inną subskrypcję oraz zarządzać katalogiem przy użyciu nowej subskrypcji.

Wszyscy użytkownicy mają jeden katalog macierzysty, który ich uwierzytelnia, ale mogą również być gośćmi w innych katalogach. W usłudze Azure AD możesz zobaczyć katalogi, w których Twoje konto użytkownika jest elementem członkowskim lub gościem.

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>Aby dodać istniejącą subskrypcję do katalogu usługi Azure AD
Musisz zalogować się przy użyciu konta, które istnieje w bieżącym katalogu, z którym jest skojarzona subskrypcja, oraz w katalogu, do którego chcesz ją dodać. 

1. Zaloguj się do [Centrum konta platformy Azure](https://account.azure.com/Subscriptions) przy użyciu konta należącego do administratora konta dla subskrypcji, której własność chcesz przenieść.
2. Upewnij się, że użytkownik, który ma być właścicielem subskrypcji, znajduje się w katalogu docelowym.
3. Kliknij pozycję **Przenieś subskrypcję**.
4. Określ adresata. Adresat automatycznie otrzymuje wiadomość e-mail z linkiem umożliwiającym akceptację.
5. Adresat klika link i postępuje zgodnie z instrukcjami, co obejmuje również wprowadzenie informacji dotyczących płatności. Jeśli czynności wykonywane przez adresata zakończą się pomyślnie, subskrypcja zostanie przeniesiona. 
6. Domyślny katalog subskrypcji zostanie zmieniony na katalog, w którym znajduje się użytkownik.

Aby uzyskać więcej informacji, zobacz [Transfer Azure subscription ownership to another account (Przenoszenie własności subskrypcji platformy Azure na inne konto)](../billing/billing-subscription-transfer.md).

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej o zmianie administratorów subskrypcji platformy Azure, zobacz [Transfer ownership of an Azure subscription to another account](../billing/billing-subscription-transfer.md) (Przenoszenie własności subskrypcji platformy Azure na inne konto)
* Aby dowiedzieć się więcej o kontrolowaniu dostępu do zasobów na platformie Microsoft Azure, zobacz [Understanding resource access in Azure](active-directory-understanding-resource-access.md) (Opis dostępu do zasobów na platformie Azure).
* Aby uzyskać więcej informacji na temat sposobu przypisywania ról w usłudze Azure AD, zobacz [Przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
