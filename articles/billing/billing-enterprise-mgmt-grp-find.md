---
title: "Znajdź Azure grupy zarządzania lub subskrypcji - Azure | Dokumentacja firmy Microsoft"
description: "Jak przechodzić między wiele katalogów, aby zobaczyć swoje grupy zarządzania i subskrypcji"
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: df4d85d0556a62311c112f24431b54d042333c7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="find-an-azure-subscription-or-management-group"></a>Znajdź Azure subskrypcji lub zarządzania grupy

Jeśli masz problem ze znalezieniem subskrypcji lub grupy zarządzania na platformie Azure, być może szukasz w niewłaściwego katalogu. Taka sytuacja może się zdarzyć, gdy Twoje konto istnieje w wielu Azure Active Directory. Każdy [usługi active directory jest niezależna](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-directory-independence) i dostępu nie jest dziedziczona przez katalogi.      

![Menu katalogu przełącznika](media/billing-enterprise-mgmt-groups/mgempty.png)



## <a name="switch-directories"></a>Przełącz katalogi 
Można łatwo przełączać katalogów w portalu Azure.
1.  Zaloguj się do [Azure Portal](https://portal.azure.com).
2.  Wybierz nazwę w górnym rogu ekranu. 
3.  U dołu menu wyświetla listę wszystkich katalogów, których masz dostęp do.
4.  Wybierz nazwę katalogu, aby uzyskać dostęp do. 

![Menu katalogu przełącznika](media/billing-enterprise-mgmt-groups/switch-directory.png)

## <a name="asset-is-unavailable"></a>Zasobów jest niedostępny? 
Jeśli wyświetlany jest komunikat o błędzie "ten zasób jest niedostępny" podczas próby uzyskania dostępu do grupy zarządzania lub subskrypcji, możesz nie ma odpowiednią rolę, aby wyświetlić ten element.  

![zasobów nie można odnaleźć](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Skontaktuj się z Administruj grup subskrypcji lub zarządzania, aby uzyskać dostęp.  
* Subskrypcji, odwołanie [based kontroli dostępu (RBAC)](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) dokumentu, aby uzyskać pomoc, na którym rola jest wymagana.
* Dla grupy zarządzania RBAC dostępu nie jest dostępny i będzie dostępna wkrótce. Skontaktuj się z portalem enterprise administrowania przypisany dostęp.   

## <a name="improve-your-experience-with-management-groups-and-subscriptions-in-the-same-directory"></a>Udoskonalanie z grupy zarządzania i subskrypcje, w tym samym katalogu 
W katalogu, który możesz wybrać, aby wszystkie elementy istnieje w tym samym miejscu, można przenieść subskrypcji lub grup zarządzania.  Konsolidacja subskrypcji i grup zarządzania, w tym samym katalogu pomaga ograniczyć potrzebę Przełącz katalogi i Zezwalaj na zasady być dziedziczone.  


### <a name="transfer-your-subscriptions"></a>Transfer subskrypcji 
Można przenieść subskrypcję do katalogu skojarzone z grupami zarządzania. To przeniesienie można osiągnąć dzięki użyciu administratorem rejestracji przeniesienia subskrypcji do konta w katalogu docelowym. Aby dowiedzieć się więcej, zaloguj się na stronie [Enterprise portal](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription).


 






