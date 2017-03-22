---
title: "Rozwiązywanie problemów: Brak danych w pobranych dziennikach aktywności usługi Azure Active Directory — wersja zapoznawcza | Microsoft Docs"
description: "Zawiera rozwiązanie problemu dotyczącego braku danych w pobranych dziennikach aktywności usługi Azure Active Directory w wersji zapoznawczej."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/09/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e0d65edcb7c14114565402038b0958c3a2ffb477
ms.lasthandoff: 03/09/2017


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


