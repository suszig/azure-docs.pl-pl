---
title: "Rozwiązywanie problemów: Brak danych w pobranych dziennikach aktywności usługi Azure Active Directory | Microsoft Docs"
description: "Zawiera rozwiązanie problemu dotyczącego braku danych w pobranych dziennikach aktywności usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d976e2476001ad4d23e55913681500f14e285014
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>Nie mogę znaleźć danych w pobranych dziennikach aktywności usługi Azure Active Directory


## <a name="symptoms"></a>Objawy

Pobrano dzienniki aktywności (inspekcji lub logowania), ale nie widać wszystkich rekordów dla wybranego czasu. Dlaczego? 

 ![Raportowanie](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Przyczyna

Dzienniki aktywności pobierane z witryny Azure Portal mają nałożony limit wynoszący 120 tysięcy rekordów posortowanych według najnowszych wpisów. 

## <a name="resolution"></a>Rozwiązanie

Można wykorzystać [interfejsy API raportowania usługi Azure AD](active-directory-reporting-api-getting-started.md), aby pobrać do miliona rekordów z dowolnego okresu. Zalecamy regularne uruchamianie skryptu wywołującego interfejsy API raportowania w celu pobrania rekordów w sposób przyrostowy w podanym okresie (np. codziennie lub co tydzień).

## <a name="next-steps"></a>Następne kroki
Zobacz temat[Azure Active Directory reporting FAQ](active-directory-reporting-faq.md) (Często zadawane pytania dotyczące raportowania usługi Azure Active Directory).

