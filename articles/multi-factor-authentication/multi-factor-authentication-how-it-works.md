---
title: "Uwierzytelnianie wieloskładnikowe platformy Azure — jak działa"
description: "Usługa Azure Multi-Factor Authentication zabezpiecza dostęp do danych i aplikacji, a jednocześnie spełnia wymagania użytkowników dotyczące prostoty procesu logowania. Zawiera dodatkowe zabezpieczenia, gdyż drugiej formy uwierzytelniania i zapewnia silne uwierzytelnianie za pomocą różnych opcji weryfikacji łatwe."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Jak działa usługa Azure Multi-Factor Authentication
Zabezpieczenia weryfikacji dwuetapowej znajduje się w jego warstwowego podejścia. Naruszenie wiele czynników uwierzytelniania przedstawiono istotne wyzwanie osobom atakującym. Nawet jeśli osoba atakująca zarządza się hasło użytkownika, jest bezużyteczny bez również dostęp do zaufanego urządzenia. 

![Biurowego](./media/multi-factor-authentication-how-it-works/howitworks.png)

Usługa Azure Multi-Factor Authentication zabezpiecza dostęp do danych i aplikacji, a jednocześnie spełnia wymagania użytkowników dotyczące prostoty procesu logowania.  Zawiera dodatkowe zabezpieczenia, gdyż drugiej formy uwierzytelniania i zapewnia silne uwierzytelnianie za pomocą różnych opcji weryfikacji łatwe.


## <a name="methods-available-for-two-step-verification"></a>Dostępne metody na potrzeby weryfikacji dwuetapowej
Gdy użytkownik się zaloguje, dodatkowej weryfikacji są wysyłane do użytkownika.  Poniżej przedstawiono listę metod, które mogą być używane dla drugiego weryfikacji.

| Metoda weryfikacji | Opis |
| --- | --- |
| Połączenie telefoniczne |Wywołanie jest umieszczany zarejestrowanego telefonem użytkownika. Użytkownik wprowadza numeru PIN, jeśli to konieczne następnie naciska klawisz #. |
| Wiadomość SMS |Wiadomość SMS są wysyłane na telefon komórkowy użytkownika z sześciocyfrowy kod. Na stronie logowania, użytkownik wprowadza tego kodu. |
| Powiadomienie aplikacji mobilnej |Żądanie weryfikacji jest wysyłane do smartfon użytkownika. Użytkownik wprowadza numeru PIN, jeśli to konieczne, a następnie wybiera **Sprawdź** za pomocą aplikacji mobilnej. |
| Kod weryfikacyjny aplikacji mobilnej |Aplikacji mobilnej, w którym jest uruchomiony przez użytkownika smartfon, wyświetla kod weryfikacyjny, który zmienia co 30 sekund. Najnowsze kodu i wstawia go na stronie logowania użytkownika. |
| Tokeny OATH innej firmy | Azure aplikacji serwer Multi-Factor Authentication mogą być skonfigurowane do akceptowania metody weryfikacji innych firm. |

Uwierzytelnianie wieloskładnikowe platformy Azure udostępnia metody weryfikacji selectable dla chmurą i z serwera. Można wybrać, które metody są dostępne dla użytkowników: połączenie telefoniczne, tekst, powiadomienie aplikacji lub kody aplikacji. Aby uzyskać więcej informacji, zobacz [metody weryfikacji počítačů, které](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Następne kroki

- Przeczytaj informacje o różnych [wersji i metody zużycia uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-versions-plans.md)

- Określ, czy do wdrożenia usługi Azure MFA [w chmurze lub lokalnie](multi-factor-authentication-get-started.md)

- Odczyt odpowiada [— często zadawane pytania](multi-factor-authentication-faq.md)