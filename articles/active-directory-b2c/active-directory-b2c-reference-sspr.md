---
title: 'Usługa Azure Active Directory B2C: Samoobsługowego resetowania hasła | Dokumentacja firmy Microsoft'
description: Temat pokazująca, jak skonfigurować samoobsługowego resetowania haseł dla użytkowników w usłudze Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: f38473989f90bfe6d35bffb17a02a892ad08cf5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Usługa Azure Active Directory B2C: Konfigurowanie samoobsługowego resetowania haseł dla użytkowników
Dzięki funkcji resetowania hasła samoobsługi użytkowników (którzy utworzyli konto dla kont lokalnych) można zresetować hasła na ich własnych. Pozwala to znacznie ograniczyć obciążenie działu pomocy technicznej, zwłaszcza, jeśli aplikacja ma miliony użytkowników przy użyciu go na bieżąco. Aktualnie obsługiwany jest tylko metoda odzyskiwania przy użyciu ze zweryfikowanym adresem e-mail. Teraz dodamy metody dodatkowe odzyskiwania (numer telefonu zweryfikowane, pytań zabezpieczających, itp.) w przyszłości.

> [!NOTE]
> Ten artykuł dotyczy hasła Samoobsługowe Resetowanie używane w kontekście zasad logowania. Jeśli potrzebujesz zasady resetowania hasła można swobodnie dostosowywać wywoływane z aplikacji, zobacz [w tym artykule](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Domyślnie katalogu nie będzie miał hasła Samoobsługowe Resetowanie włączona. Aby włączyć tę funkcję, wykonaj następujące kroki:

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) jako Administrator subskrypcji. To jest tej samej pracy lub konta służbowego lub tego samego konta Microsoft, który został użyty do utworzenia katalogu.
2. Otwórz usługi Active Directory (na pasku nawigacyjnym po lewej stronie).
3. Wybierz **właściwości**.
4. Przewiń w dół do **włączyć samoobsługowe Resetowanie hasła** sekcji i przełączyć go do **wszystkich**. 
5. Kliknij przycisk **zapisać** w górnej części strony. Gotowe!

Aby przetestować, funkcja "Uruchom teraz" na żadnych zasad logowania z kontami lokalnymi funkcję dostawcy tożsamości. Na logowanie lokalne konto strony (gdzie należy wprowadzić adres e-mail i hasło lub nazwę użytkownika i hasło), kliknij przycisk **nie może uzyskać dostępu do konta?** można zweryfikować środowiska użytkownika.

> [!NOTE]
> Strony resetowania hasła samoobsługi można dostosować za pomocą [firmowe funkcji](../active-directory/customize-branding.md).
> 
> 

