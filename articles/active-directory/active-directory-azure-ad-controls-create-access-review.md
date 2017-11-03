---
title: "Utwórz Przegląd dostępu dla członków grupy lub użytkownicy z dostępem do aplikacji z usługą Azure AD | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć Przegląd dostępu dla członków grupy lub użytkownicy z dostępem do aplikacji."
services: active-directory
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: ae926e8d3a856c9b488d46b481a1c7b5b581d532
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Tworzenie przeglądu dostępu do członków grupy lub dostęp do aplikacji z usługą Azure AD

Przydziały dostępu nieodświeżone "", gdy użytkownicy mają dostęp, których nie potrzebują więcej. W celu zmniejszenia ryzyka związanego z przypisania starych dostępu, Administratorzy mogą używać usługi Azure Active Directory (Azure AD) do utworzenia Przegląd dostępu dla członków grupy lub użytkowników przypisanych do aplikacji. Aby uzyskać więcej informacji na temat tych scenariuszy, zobacz [zarządzanie dostępem użytkowników](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) i [zarządzanie dostępem gościa](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Utwórz Przegląd dostępu

1. Jako administrator globalny, przejdź do [dostępu monitoruje strony](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)i wybierz **programy**.

2. Wybierz program, który zawiera formant przeglądu dostępu, który chcesz utworzyć. **Domyślny Program** występuje zawsze, lub można utworzyć inny program. Na przykład można mieć jeden program dla każdego inicjatywy zgodności lub celach biznesowych.

3. W programie, wybierz **formanty**, a następnie wybierz **Dodaj** można dodać formantu.

4. Nazwa każdego przeglądu dostępu. Opcjonalnie Nadaj każdej Przejrzyj opis. Nazwa jest wyświetlana do recenzentów.

5. Ustaw daty początkową i końcową. Domyślnie dostęp Przejrzyj rozpoczyna się tego samego dnia, w którym zostały utworzone, a kończy się w ciągu miesiąca. Zmienisz rozpoczęcia i daty zakończenia dostępu Przejrzyj start w przyszłości i ostatnio jednak ma wiele dni.

6. Przeglądy dostępu może być członkami grupy lub użytkowników, którzy są przypisani do aplikacji. Dostęp można dodatkowo zakresu przeglądu do przejrzenia tylko użytkownicy gościa który są elementami członkowskimi (lub przypisane do aplikacji), zamiast przegląd wszystkich użytkowników, którzy są członkami lub mają dostęp do aplikacji.

7. Wybierz co najmniej jeden osoby, aby przejrzeć wszystkich użytkowników w zakresie. Lub możesz wybrać członków, Przejrzyj swoje własne dostępu. Jeśli zasób jest grupą, możesz poprosić właścicieli grupy, aby przejrzeć. Możesz również wymagać czy recenzentów udzielają dostępu, podać przyczynę.

8. Na koniec wybierz **Start**.


## <a name="manage-the-access-review"></a>Zarządzanie Przegląd dostępu

Domyślnie usługi Azure AD wysyła wiadomość e-mail do osób dokonujących przeglądu, wkrótce po uruchomieniu przeglądu. Jeśli wybierzesz nie usługi Azure AD, Wyślij wiadomość e-mail, należy poinformować recenzentów, które Przegląd dostępu oczekuje na ich zakończenie. Można je wyświetlić instrukcje dotyczące sposobu [sprawdzaj dostęp](active-directory-azure-ad-controls-perform-access-review.md). Jeśli dla gości przejrzeć ich dostęp do zapoznania się z nimi, je wyświetlić instrukcje dotyczące sposobu [Przejrzyj własne dostępu](active-directory-azure-ad-controls-perform-access-review.md).

Jeśli niektórzy z recenzentów gości, gości są powiadamiane za pośrednictwem poczty e-mail tylko wtedy, gdy już zaakceptowane ich zaproszenia.


Postęp można śledzić w recenzentów zakończenia ich przeglądami na pulpicie nawigacyjnym usługi Azure AD w **przegląda dostępu** sekcji. Nie prawa dostępu są zmieniane w katalogu do [zakończeniu przeglądu](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Następne kroki

Gdy Przegląd dostępu została uruchomiona, usługi Azure AD automatycznie wysyła recenzentów wiadomości e-mail, która żąda sprawdzaj dostęp. Jeśli użytkownik nie otrzymasz wiadomość e-mail, możesz wysłać je z instrukcjami dotyczącymi sposobu [sprawdzaj dostęp](active-directory-azure-ad-controls-perform-access-review.md). 

Po okresu przeglądu dostępu lub administrator zatrzymuje Przegląd dostępu, postępuj zgodnie z instrukcjami [zakończenia przeglądu dostępu](active-directory-azure-ad-controls-complete-access-review.md) aby zobaczyć i zastosować wyniki.


