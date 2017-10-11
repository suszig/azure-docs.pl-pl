---
title: "Usługa Azure Active Directory B2C: Samoobsługowego resetowania hasła | Dokumentacja firmy Microsoft"
description: "Temat pokazująca, jak skonfigurować samoobsługowego resetowania haseł dla użytkowników w usłudze Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Usługa Azure Active Directory B2C: Konfigurowanie samoobsługowego resetowania haseł dla użytkowników
Dzięki funkcji resetowania hasła samoobsługi użytkowników (którzy utworzyli konto dla kont lokalnych) można zresetować hasła na ich własnych. Pozwala to znacznie ograniczyć obciążenie działu pomocy technicznej, zwłaszcza, jeśli aplikacja ma miliony użytkowników przy użyciu go na bieżąco. Aktualnie obsługiwany jest tylko metoda odzyskiwania przy użyciu ze zweryfikowanym adresem e-mail. Teraz dodamy metody dodatkowe odzyskiwania (numer telefonu zweryfikowane, pytań zabezpieczających, itp.) w przyszłości.

> [!NOTE]
> Ten artykuł dotyczy hasła Samoobsługowe Resetowanie używane w kontekście zasad logowania. Jeśli potrzebujesz zasady resetowania hasła można swobodnie dostosowywać wywoływane z aplikacji, zobacz [w tym artykule](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Domyślnie katalogu nie będzie miał hasła Samoobsługowe Resetowanie włączona. Aby włączyć tę funkcję, wykonaj następujące kroki:

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator subskrypcji. To jest tej samej pracy lub konta służbowego lub tego samego konta Microsoft, który został użyty do utworzenia katalogu.
2. Przejdź do rozszerzenia usługi Active Directory na pasku nawigacyjnym po lewej stronie.
3. Znajdź katalogu, w obszarze **katalogu** i kliknij ją.
4. Kliknij kartę **Konfiguracja**.
5. Przewiń w dół do **zasady resetowania hasła użytkownika** sekcji, a następnie **użytkownicy włączeni do resetowania hasła** opcji w celu **tak**. Zwróć uwagę, że **alternatywny adres E-mail** zaznaczono opcję; pozostaw, ponieważ jest on.
   
    ![Samoobsługowe resetowanie haseł](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Kliknij przycisk **Zapisz** w dolnej części strony. Gotowe!

Aby przetestować, funkcja "Uruchom teraz" na żadnych zasad logowania z kontami lokalnymi funkcję dostawcy tożsamości. Na logowanie lokalne konto strony (gdzie należy wprowadzić adres e-mail i hasło lub nazwę użytkownika i hasło), kliknij przycisk **nie może uzyskać dostępu do konta?** można zweryfikować środowiska użytkownika.

> [!NOTE]
> Strony resetowania hasła samoobsługi można dostosować za pomocą [firmowe funkcji](../active-directory/active-directory-add-company-branding.md).
> 
> 

