---
title: Wyłączyć weryfikację wiadomości e-mail podczas tworzenia konta - konsumentów Azure Active Directory B2C
description: Temat pokazująca, jak wyłączyć Weryfikacja adresu e-mail, podczas tworzenia konta w usłudze Azure Active Directory B2C konsumenta
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 2/06/2017
ms.author: davidmu
ms.openlocfilehash: 4f48df553d35386fb2b0448972dc01f9c6427ebd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Usługa Azure Active Directory B2C: Weryfikacja adresu e-mail Wyłącz podczas tworzenia konta użytkownika
Po włączeniu usługi Azure Active Directory (Azure AD) B2C umożliwia konsumenta zalogowania się do aplikacji, zapewniając adres e-mail i tworzenia konta lokalnego. Usługa Azure AD B2C zapewnia prawidłowe adresy e-mail, wymagając od użytkowników w celu weryfikacji w procesie rejestracji. Również uniemożliwia złośliwego zautomatyzowany proces generowania fałszywych kont dla aplikacji.

Niektórzy deweloperzy aplikacji preferowane może pominąć weryfikacji wiadomości e-mail w procesie rejestracji, a zamiast tego konsumentów później zweryfikować adres e-mail. Aby to obsłużyć, można skonfigurować usługi Azure AD B2C wyłączyć Weryfikacja adresu e-mail. Należy utworzyć płynniejszy procesu tworzenia konta i zapewnia elastyczność do odróżnienia konsumentów, które sprawdzeniu swój adres e-mail z tych klientów, które nie mają.

Zasady rejestracji mają domyślnie włączona Weryfikacja adresu e-mail. Aby ją wyłączyć, wykonaj następujące kroki:

1. [Wykonaj następujące kroki, aby przejść do bloku funkcji B2C w portalu Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Kliknij przycisk **zasady rejestracji** lub **zasad rejestracji i logowania** w zależności od konfiguracji dla rejestracji.
3. Kliknij zasady (na przykład "B2C_1_SiUp"), aby go otworzyć. Kliknij przycisk **Edytuj** w górnej części bloku.
4. Kliknij przycisk **dostosowywania interfejsu użytkownika strony**.
5. Kliknij przycisk **stronę tworzenia konta lokalnego konta**.
6. Kliknij przycisk **adres E-mail** w **nazwa** kolumnie **atrybuty rejestracji** sekcji.
7. Przełącz **Żądaj weryfikacji** opcji w celu **nr**.
8. Kliknij przycisk **OK** u dołu, aż dojdziesz **edytować zasady** bloku.
9. Kliknij przycisk **zapisać** w górnej części bloku. Gotowe!

> [!NOTE]
> Wyłączanie Weryfikacja adresu e-mail w procesie rejestracji może prowadzić do wysyłania spamu. Jeśli wyłączysz domyślny, zaleca się dodawania systemu weryfikacji.
> 
> 

Firma Microsoft zawsze są otwarte na opinie i sugestie! Jeśli masz trudności w tym temacie lub mają zalecenia dotyczące poprawy tej zawartości, prosimy o wyrażenie opinii na dole strony. Funkcja żądań, dodaj je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
