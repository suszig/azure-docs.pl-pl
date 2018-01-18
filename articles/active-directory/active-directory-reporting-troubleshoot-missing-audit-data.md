---
title: "Rozwiązywanie problemów: Brak danych w dzienniku aktywności usługi Azure Active Directory | Microsoft Docs"
description: "Lista raportów dostępnych w ramach usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 55587fb4ff6d8a2130eb838782a65788fb2dd2b3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>W dzienniku aktywności usługi Azure Active Directory nie można znaleźć niektórych wykonywanych akcji


## <a name="symptoms"></a>Objawy

W witrynie Azure Portal wykonano pewne akcje, które powinny zostać odzwierciedlone w dziennikach inspekcji w bloku `Activity logs > Audit Logs`, ale nie można ich znaleźć.

 ![Raportowanie](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Przyczyna

Akcje nie pojawiają się natychmiast w dzienniku inspekcji aktywności. Dzienniki inspekcji pojawiają się w portalu dopiero po pewnym czasie (od 15 minut do godziny) od momentu wykonania operacji.

## <a name="resolution"></a>Rozwiązanie

Poczekaj od 15 minut do godziny i sprawdź, czy akcje pojawią się w dzienniku. Jeśli nadal nie są one wyświetlane, zgłoś do nas bilet pomocy technicznej, abyśmy mogli przeanalizować tę sytuację.


## <a name="next-steps"></a>Następne kroki
Zobacz temat[Azure Active Directory reporting FAQ](active-directory-reporting-faq.md) (Często zadawane pytania dotyczące raportowania usługi Azure Active Directory).

