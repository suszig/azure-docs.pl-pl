---
title: "Połączone wdrożenia usługi Azure decyzje dotyczące stosu Azure zintegrowanych systemów | Dokumentacja firmy Microsoft"
description: "Określ decyzje dotyczące wdrożeniach z wieloma węzłami połączone Azure Azure stosu dotyczących planowania wdrażania."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: c1a3b2107abdc3ef19a314616518c494687d81bf
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Połączone wdrożenia usługi Azure planowania decyzje dotyczące stosu Azure zintegrowanych systemów
Po ustaleniu [jak zintegruje stosu Azure do środowiska chmury hybrydowej](azure-stack-deployment-decisions.md), można następnie zakończyć swoje decyzje dotyczące wdrażania usługi Azure stosu.

Wdrażanie stosu Azure połączony Azure oznacza, że dla magazynu tożsamości może mieć usługi Azure Active Directory (Azure AD) lub usługi Active Directory Federation Services (AD FS). Możesz również z obu modelu rozliczeń: płatności jako — użytkownik użytku lub na podstawie pojemności. Połączone wdrożenia jest domyślną opcją, ponieważ pozwala uzyskać największą wartość spoza stosu Azure, szczególnie w scenariuszach chmur hybrydowych, obejmujących zarówno Azure i stosu Azure. 

## <a name="choose-an-identity-store"></a>Wybierz magazyn tożsamości
Z wdrożeniem połączenia można wybrać usługi Azure AD lub AD FS dla magazynu tożsamości. Rozłączona wdrożenia, bez łączności z Internetem, można używać tylko usługi AD FS.

Wybór magazynu tożsamości nie ma żadnego wpływu na maszynach wirtualnych dzierżawy (VM). Maszyny wirtualne dzierżawy mogą wybrać magazynu tożsamości, które chcą Połącz w zależności od sposobu ich konfiguracji: usługi Azure AD, przyłączonych do domeny usługi Active Directory systemu Windows Server w grupie roboczej, itd. To jest związana z decyzji Azure stosu dostawcy tożsamości. 

Na przykład jeśli wdrożenia IaaS dzierżawione maszyny wirtualne na szczycie stosu Azure i chcesz, aby dołączyć firmowej domeny usługi Active Directory i używać kont z niego, można nadal w tym. Nie należy używać magazynu tożsamości usługi Azure AD, tutaj dla tych kont.

### <a name="azure-ad-identity-store"></a>Magazyn tożsamości w usłudze Azure AD
Jeśli używasz usługi Azure AD Twojego magazynu tożsamości wymaga dwóch kont usługi Azure AD: konto administratora globalnego i konta rozliczeniowego. Te konta można kont w tym samym lub różnych kont. Przy użyciu tego samego konta użytkownika mogą być łatwiejsze i przydatne w przypadku ograniczonej liczby kont platformy Azure, może sugerować potrzeb firmy, za pomocą dwóch kont:

1. **Konto administratora globalnego** (wymagane tylko w przypadku wdrożeń połączonych). To jest konto platformy Azure, który służy do tworzenia aplikacji i nazwy główne usług dla usługi infrastruktury platformy Azure stosu w usłudze Azure Active Directory. To konto musi mieć uprawnienia administratora katalogu do katalogu systemu Azure stosu zostanie wdrożona w obszarze. Stanie się "operatorowi chmury" Administrator globalny dla usługi Azure AD dzierżawy i będą używane: 
    - Aby udostępnić, a następnie delegatem aplikacji i nazwy główne usług dla wszystkich usług Azure stosu, które muszą wchodzić w interakcje z usługą Azure Active Directory i interfejsu API programu Graph. 
    - Za pomocą konta administratora usługi. To jest właścicielem subskrypcji dostawcy domyślnego (które można później zmienić). Można zalogować się do portalu administratora stosu Azure z tym kontem i służy do tworzenia oferty i planów, ustawiać przydziały i wykonywania innych zadań administracyjnych w stosie Azure.
2. **Konto rozliczeniowe** (wymagane w przypadku obu podłączone i odłączone wdrożenia). To konto Azure jest używane do ustanawiania rozliczeń relacji między systemu Azure stosu zintegrowany i Azure commerce wewnętrznej bazy danych. To konto, które będą naliczane za opłaty stosu Azure. To konto będzie również zespolonego marketplace i innych scenariuszy hybrydowych. 

### <a name="ad-fs-identity-store"></a>MAGAZYN TOŻSAMOŚCI USŁUGI AD FS
Wybierz tę opcję, jeśli chcesz użyć własnych magazynu tożsamości, takie jak sieci firmowej usługi Active Directory, dla kont administratora usługi.  

## <a name="choose-a-billing-model"></a>Wybierz modelu rozliczeń
Można wybrać **płatności jako — użytkownik użycia** lub **pojemności** modelu rozliczeń. Płatności jako — użytkownik użycia rozliczeń modelu wdrożenia musi mieć możliwość użycia raportów za pośrednictwem połączenia z platformą Azure co najmniej raz na 30 dni, w związku z tym, jeśli połączenie zostanie nie jest dostępna, modelu rozliczeń pojemności jest jedyną opcją. 

### <a name="pay-as-you-use"></a>Płatności jako — użytkownik użycia
Z modelu rozliczeń płatności jako — użytkownik użycia użycia jest pobierana z subskrypcją platformy Azure. Płacisz tylko podczas korzystania z usług Azure stosu. Jeśli zdecydujesz się na modelu, będziesz potrzebować subskrypcji platformy Azure i Identyfikatora konta skojarzone z danej subskrypcji (na przykład serviceadmin@contoso.onmicrosoft.com). Umowa EA, dostawca usług Kryptograficznych i CSL subskrypcje są obsługiwane. Raportowanie użycia jest ustawiany podczas [rejestracji stosu Azure](azure-stack-registration.md).

> [!NOTE]
> W większości przypadków klienci korporacyjni użyje EA subskrypcje, a usługodawców będzie używać dostawcy usług Kryptograficznych lub CSL subskrypcji.

Jeśli zamierzasz używać subskrypcji dostawcy usług Kryptograficznych, przejrzeć tabelę poniżej, aby zidentyfikować subskrypcję dostawcy usług Kryptograficznych, jako poprawne podejście jest zależna od dokładnej scenariusz dostawcy usług Kryptograficznych:

|Scenariusz|Opcje domeny i subskrypcji|
|-----|-----|
|Bezpośrednie lub pośrednie partnera dostawcy usług Kryptograficznych i będzie działać na stosie Azure|Użyj subskrypcji CSL (wspólnej warstwy usług).|
|Bezpośrednie lub pośrednie partnera dostawcy usług Kryptograficznych i będzie działać na stosie Azure|W Centrum partnerskiego Utwórz dzierżawę usługi Azure AD z opisową nazwę, na przykład <your organization>CSPAdmin i subskrypcji platformy Azure dostawcy usług Kryptograficznych skojarzonych z nim.|
|Jesteś pośrednim sprzedawcą dostawcy usług Kryptograficznych i będzie działać na stosie Azure|Poproś pośrednie dostawcy CSP do tworzenia, przy użyciu Centrum partnerskiego, dzierżawa usługi Azure AD dla Twojej organizacji i subskrypcji platformy Azure dostawcy usług Kryptograficznych skojarzonych z nim.|

### <a name="capacity-based-billing"></a>Pojemności na podstawie rozliczeń
Jeśli zdecydujesz się używać modelu rozliczeń pojemności, należy zakupić Azure stosu pojemności planowanie jednostki SKU na podstawie wydajności systemu. Należy znać liczba fizycznych rdzeni w stosie Azure poprawną ilość zakupu. 

Enterprise Agreement (EA) wymaga pojemności rozliczeń subskrypcji platformy Azure dla rejestracji. Przyczyną jest to, że rejestracja konfiguruje zespolonego, co wymaga subskrypcji platformy Azure. Subskrypcja nie jest używany przez wykorzystanie stosu Azure.

## <a name="learn-more"></a>Dowiedz się więcej
- Informacje o przypadki użycia, zakupu partnerami i dostawcami sprzętu OEM, zobacz [stosu Azure](https://azure.microsoft.com/overview/azure-stack/) stronę produktu.
- Informacji o plan i dostępności geograficznie stosu Azure zintegrowanych systemów, zobacz oficjalny dokument: [stosu Azure: rozszerzenie Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Aby dowiedzieć się więcej o Microsoft Azure stosu pakowania i cenach [Pobierz PDF](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 
