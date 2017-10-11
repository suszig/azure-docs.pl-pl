---
title: "Rozwiązywanie problemów: Brak danych w dzienniku aktywności usługi Azure Active Directory | Microsoft Docs"
description: "Lista raportów dostępnych w ramach usługi Azure Active Directory"
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
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 47617f8f727027de113a0f503308c8accc58859e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
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

