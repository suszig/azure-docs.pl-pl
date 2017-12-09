---
title: "Wprowadzenie do raportowania interfejsu API usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę z usługą Active Directory Azure, interfejsu API raportowania"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5feaaa64a130e86f2016bcb6c1d01853a9a406a7
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Wprowadzenie do korzystania z usługi Azure Active Directory raportowania interfejsu API

Usługa Azure Active Directory oferuje szerokiej gamy raportów. Dane z tych raportów mogą być bardzo przydatne w aplikacjach, takich jak systemy SIEM oraz narzędzia do inspekcji i analizy biznesowej. Interfejsy API raportów usługi Azure AD umożliwiają dostęp programowy do danych za pomocą zestawu interfejsów API opartych na architekturze REST. Te interfejsy API można wywoływać przy użyciu różnych języków i narzędzi do programowania.

Ten artykuł zawiera informacje potrzebne do Rozpoczynanie pracy z usługą Azure AD, na zgłoszenie interfejsów API.
W następnej sekcji możesz znaleźć szczegółowe informacje o użyciu inspekcji i logowania interfejsów API. 

Często zadawane pytania, przeczytaj nasze [— często zadawane pytania](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-faq). W przypadku problemów skontaktuj się z [pliku biletu pomocy technicznej](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

## <a name="learning-map"></a>Mapa uczenia się
1. **Przygotowanie** — przed próbek interfejsu API można przetestować, należy wykonać [wymagania wstępne dotyczące raportowania interfejsu API usługi Azure AD dostęp](active-directory-reporting-api-prerequisites-azure-portal.md).
2. **Eksploruj** -uzyskać pierwszy wrażenie raportowania interfejsów API:
   
   * [Korzystanie z przykładów dla inspekcji interfejsu API](active-directory-reporting-api-audit-samples.md) 
   * [Korzystanie z przykładów dla raport aktywności logowania interfejsu API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Dostosowywanie** — tworzenie własnych rozwiązań: 
   
   * [Przy użyciu audytu dokumentacja interfejsu API](active-directory-reporting-api-audit-reference.md) 
   * [Przy użyciu działań logowania odwołania raportu interfejsu API](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Następne kroki
Jeśli zastanawiasz się wyświetlić wszystkie dostępne punkty końcowe interfejsu API usługi Azure AD Graph, użyj tego linku: [https://graph.windows.net/tenant-name/activities/$ metadanych? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).

