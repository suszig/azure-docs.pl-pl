---
title: "Azure AD: Rejestrowanie w celu samoobsługowego resetowania hasła | Microsoft Docs"
description: "Zarejestruj dane uwierzytelniania na potrzeby samoobsługowego resetowania hasła"
services: active-directory
keywords: Active directory password management, password management, Azure AD self service password reset, SSPR
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/16/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0f10ad4904b10a9554e949e77f0192edcb8f624f
ms.lasthandoff: 04/12/2017


---
# <a name="register-for-self-service-password-reset"></a>Rejestrowanie na potrzeby samoobsługowego resetowania hasła

Jeśli administrator włączył tę funkcję, użytkownicy końcowi mogą zresetować swoje hasło lub odblokować konto, korzystając z samoobsługowego resetowania hasła. Nie muszą z nikim rozmawiać. Przed użyciem tej funkcji musisz zarejestrować metody uwierzytelniania lub potwierdzić wstępnie zdefiniowane metody uwierzytelniania podane przez administratora.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Rejestrowanie lub potwierdzanie danych uwierzytelniania na potrzeby samoobsługowego resetowania hasła

1. Otwórz przeglądarkę sieci Web na urządzeniu i przejdź do [strony rejestracji resetowania hasła](http://aka.ms/ssprsetup)
2. Wprowadź nazwę użytkownika i hasło podane przez administratora
3. W zależności od opcji zatwierdzonych przez administratora zobaczysz co najmniej jedną z następujących pozycji, które można skonfigurować lub zweryfikować do użycia w razie potrzeby zresetowania hasła
    * Telefon biurowy — tę opcję może ustawić jedynie administrator
    * Telefon uwierzytelniania — tę opcję należy ustawić na numer innego telefonu, do którego będziesz mieć dostęp i który umożliwia odbieranie wiadomości SMS lub połączeń, np. telefonu komórkowego (administrator może wypełnić to pole Twoim numerem telefonu komórkowego za Ciebie, jeśli otrzymał zgodę na użycie tych informacji)
    * Adres e-mail uwierzytelniania — tę opcję należy ustawić na alternatywny adres e-mail, do którego można uzyskać dostęp bez konieczności podawania resetowanego hasła
    * Pytania zabezpieczające — ta opcja powoduje wyświetlenie listy pytań zatwierdzonych przez administratora, na które musisz odpowiedzieć. Nie można podać tej samej odpowiedzi na więcej niż jedno pytanie.
4. Podaj i sprawdź informacje wymagane przez administratora. Jeśli jest dostępna więcej niż jedna opcja zalecamy zarejestrowanie wielu metod. Zapewnia to elastyczność w razie niedostępności jednej z metod (na przykład wtedy, gdy jesteś w podróży i nie masz dostępu do telefonu biurowego)

    ![Rejestrowanie metod uwierzytelniania i klikanie przycisku Zakończ][Register]

5. Po wykonaniu kroku 4 wybierz pozycję **Zakończ**. Od teraz możesz w razie potrzeby używać samoobsługowego resetowania hasła.

Dane wprowadzone w polu Telefon uwierzytelniania lub Adres e-mail uwierzytelniania nie są widoczne w katalogu globalnym. Jedynie Ty i Twoi administratorzy mogą wyświetlać te dane. Odpowiedzi na pytania zabezpieczające są widoczne tylko dla Ciebie.

Administratorzy mogą wymagać potwierdzenia metod uwierzytelniania po pewnym czasie, aby upewnić się, że nadal masz zarejestrowane właściwe metody.

## <a name="next-steps"></a>Następne kroki

* [Jak zmienić hasło przy użyciu samoobsługowego resetowania hasła](active-directory-passwords-update-your-own-password.md)
* [Strona rejestracji w celu resetowania hasła](http://aka.ms/ssprsetup)
* [Portal resetowania hasła](https://passwordreset.microsoftonline.com/)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Strona rejestrowania na potrzeby resetowania hasła, na której widoczne są zarejestrowane metody i przycisk Zakończ"

