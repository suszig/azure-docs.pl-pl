---
title: "Raport użycia licencji | Dokumentacja firmy Microsoft"
description: "Pomaga raport użycia licencji zidentyfikować nielicencjonowani użytkownicy, którzy korzystają z płatnej funkcje usługi Azure AD."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.openlocfilehash: 91b48098cc8ba2bb230b0536a9bcd121db79c533
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="unlicensed-usage-report"></a>Raport użycia licencji
Pomaga raport użycia licencji zidentyfikować nielicencjonowani użytkownicy, którzy korzystają z płatnej funkcje usługi Azure AD. Dzięki temu można ulepszyć użycie licencji, które zostały zakupione i do identyfikowania wiadomo, kiedy może być konieczne dodatkowe licencje. 

Ten raport prezentuje active użycie płatnych funkcji w ciągu ostatnich 30 dni. 

## <a name="report-structure"></a>Struktura raportu
| Nazwa kolumny | Opis |
|:--- |:--- |
| Bez licencji użytkownika |Nazwa użytkownika |
| Funkcja |Nazwa funkcji. Na przykład: dostępu warunkowego |
| Dostęp do aplikacji |Nazwa aplikacji, która jest uzyskiwany z funkcją. Na przykład: usługi Office 365 SharePoint Online |

> [!NOTE]
> Jeśli konto użytkownika zostało usunięte zostaną wypełnione kolumny "Bez licencji użytkownika" z Identyfikatorem, takich jak 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Funkcja dostępu warunkowego
Nielicencjonowani użytkownicy zostaną oflagowane przy uzyskiwaniu dostępu do usługi, która ma zasady dostępu warunkowego zastosowane, jeśli nie mają licencji usługi Azure AD Premium. 

Dotyczy to usługi MFA / zasady lokalizacji, a także urządzenia zasady korzystające z usługi Intune.

## <a name="see-also"></a>Zobacz też
* [Za pomocą dostępu warunkowego z usługą Office 365 i inne usługi Azure Active Directory podłączonego aplikacji](active-directory-conditional-access-azure-portal.md)
* [Wprowadzenie do korzystania z dostępu warunkowego do usługi Azure AD](active-directory-conditional-access-azure-portal-get-started.md) 

