---
title: "Oferty usług w stosie Azure | Dokumentacja firmy Microsoft"
description: "Operator chmury mają możliwość oferowania usługi dla użytkowników."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 5b117a9b174f5d2419ff596cc90436e4b9d21645
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Przegląd oferty usług Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

[Microsoft Azure stosu](azure-stack-poc.md) platforma chmury hybrydowej, która umożliwia świadczenie usług z centrum danych. Dostawcy usług mają możliwość oferowania usług dzierżawcom. W firmie lub agencji rządowych Stanów Zjednoczonych możesz zaoferować lokalnych usług pracownikom. Usługi, które można dostarczać obejmują, ale nie są ograniczone do:

- Usługi aplikacji, aplikacje interfejsu API, funkcje interfejsu API, SQL, MySQL, takich jak platforma jako usługa (PaaS) usługi.

Można nawet łączyć usług integracji do tworzenia złożonych rozwiązań dla różnych użytkowników.

Aby dostarczyć te usługi dla użytkowników, należy utworzyć [planów, ofertami i przydziały](azure-stack-plan-offer-quota-overview.md). Użytkownicy mogą następnie subskrybować oferty korzystać z usług.

## <a name="plan-your-service-offers"></a>Planowanie oferty usługi

Podczas planujesz Twojej oferty, należy uwzględnić następujące kwestie:

**Próbnych**: próbnych umożliwia przyciągania nowych użytkowników, które następnie można uaktualnić do dodatkowych usług. Wersja próbna, utworzyć małą [planu podstawowego](azure-stack-plan-offer-quota-overview.md#base-plan) z większego planu dodatek opcjonalne.

**Planowanie pojemności**: może być użytkowników, które pobrania dużych ilości zasobów i zapychaniem systemu dla wszystkich użytkowników. Aby ułatwić wydajności, można [skonfigurować przydziały planów](azure-stack-plan-offer-quota-overview.md#plans) z użyciem centralnych zasad dostępu.

**Delegowane dostawców**: można przyznać innym możliwość tworzenia oferty w danym środowisku. Na przykład jeśli jesteś dostawcą usług, możesz [delegować](azure-stack-delegated-provider.md) możliwość Twojej odsprzedawców. Lub, w przypadku organizacji, możesz delegować do innych działów/zależnych.

## <a name="next-steps"></a>Kolejne kroki
[Utwórz ofertę w stosie Azure](azure-stack-create-offer.md)

