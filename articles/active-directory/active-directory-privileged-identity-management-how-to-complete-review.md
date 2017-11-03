---
title: "Jak przeprowadzić przegląd dostępu | Dokumentacja firmy Microsoft"
description: "Po rozpoczęciu Przegląd dostępu w usłudze Azure AD Privileged Identity Management Dowiedz się, jak zakończyć je i wyświetlić wyniki"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: ca2a1c7c287e4cf6b1b50cfb0068861b6f525596
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak przeprowadzić przegląd dostępu w usłudze Azure AD Privileged Identity Management
Administratorzy ról uprzywilejowanych można przejrzeć uprzywilejowanego dostępu raz [przeglądu zabezpieczeń została uruchomiona](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM) będzie automatycznie wysyłać wiadomości e-mail monitowania użytkowników, aby przejrzeć jego uprawnienia dostępu. Jeśli użytkownik nie pobrały wiadomości e-mail, możesz wysłać je zgodnie z instrukcjami [jak przeprowadzić przegląd zabezpieczeń](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Po umieszczeniu okresu przeglądu zabezpieczeń lub wszystkich użytkowników mają Zakończono ich własnym Przejrzyj, wykonaj kroki opisane w tym artykule, aby zarządzać przeglądu i wyświetlić wyniki.

## <a name="manage-security-reviews"></a>Zarządzanie inspekcje zabezpieczeń
1. Przejdź do [portalu Azure](https://portal.azure.com/) i wybierz **Azure AD Privileged Identity Management** aplikacji na pulpicie nawigacyjnym.
2. Wybierz **dostępu przeglądami** pulpitu nawigacyjnego.
3. Wybierz Przegląd dostępu, które mają być zarządzane.

W bloku szczegółów przejrzyj dostępu istnieją liczba opcji zarządzania przeglądu.

![Przyciski przeglądu dostępu PIM — zrzut ekranu][1]

### <a name="remind"></a>Przypomnij
Jeśli Przegląd dostępu jest skonfigurowana tak, aby użytkownicy Przejrzyj, **Przypomnij** przycisk wysyła powiadomienia. 

### <a name="stop"></a>Stop
Wszystkie przeglądy dostępu mają datę końcową, ale można użyć **zatrzymać** przycisk, aby zakończyć wcześniej. Jeśli jeszcze nie zostały sprawdzone wszyscy użytkownicy tego czasu, będą mogli po zatrzymaniu przeglądu. Nie można ponownie uruchomić przeglądu, po jest została zatrzymana.

### <a name="apply"></a>Składanie wniosku o przyjęcie do programu
Po zakończeniu Przegląd dostępu, ponieważ osiągnięto datę zakończenia lub zatrzymana go ręcznie, **Zastosuj** przycisk implementuje wyniku przeglądu. Jeśli w przeglądzie nastąpiła odmowa dostępu użytkownika, to krok, który spowoduje usunięcie przypisania roli.  

### <a name="export"></a>Eksportowanie
Jeśli chcesz ręcznie zastosować wyników przeglądu zabezpieczeń, możesz wyeksportować przeglądu. **Wyeksportować** przycisk rozpocznie się pobieranie pliku CSV. Wyniki w programie Excel lub inne programy, które mają otwarte pliki CSV można zarządzać.

### <a name="delete"></a>Usuwanie
Jeśli nie jest konieczne w żadnych późniejszych przeglądu, usuń go. **Usunąć** przycisk usuwa aplikacji PIM przeglądu.

> [!IMPORTANT]
> Nie zostanie wyświetlone ostrzeżenie, zanim nastąpi jego usunięcia, dlatego upewnij się, że chcesz usunąć tego przeglądu. 

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
