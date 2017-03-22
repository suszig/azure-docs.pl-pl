---
title: "Rozwiązywanie problemów: Brak danych w dzienniku aktywności usługi Azure Active Directory — wersja zapoznawcza | Microsoft Docs"
description: "Zawiera rozwiązanie problemu dotyczącego listy różnych raportów dostępnych w ramach usługi Azure Active Directory w wersji zapoznawczej"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/09/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: ebc92bc23201dbcd01f90e0cc3d5055fb85820be
ms.lasthandoff: 03/09/2017


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


