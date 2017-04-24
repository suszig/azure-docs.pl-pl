---
title: "Azure AD: resetowanie hasła | Microsoft Docs"
description: "Odzyskaj dostęp do swojego konta, samodzielnie resetując hasło"
services: active-directory
keywords: Active directory password management, password management, Azure AD self service password reset, SSPR
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: bb62d6973dff00932e72ddb4b2c344f070537828
ms.lasthandoff: 04/14/2017


---
# <a name="help-i-forgot-my-password"></a>Pomocy, nie pamiętam hasła

Krok 1... nie panikuj

Jeśli dotyczą Cię następujące scenariusze, możemy Ci pomóc

* Nie masz pewności, jak uzyskać dostęp do konta i nie pamiętasz hasła
* Hasło nie zostało przypisane i administrator skierował Cię tutaj

## <a name="unlock-my-account"></a>Odblokowywanie mojego konta

Jeśli chcesz odblokować konto, wykonaj poniższe kroki. Po wyświetleniu okna **Wybierz nowe hasło** w kroku 6 poniżej możesz odblokować konto lub zmienić hasło i je odblokować.

## <a name="reset-my-password"></a>Resetowanie hasła

Aby ponownie uzyskać dostęp do swojego konta, wykonaj poniższe kroki.
1. Na dowolnej stronie logowania do konta służbowego kliknij link **Nie możesz uzyskać dostępu do konta?**, a następnie **Konto służbowe** lub przejdź bezpośrednio do [strony resetowania hasła](https://passwordreset.microsoftonline.com/)

    ![Nie możesz uzyskać dostępu do konta?][Login]

2. Wprowadź służbowy **Identyfikator użytkownika** i udowodnij, że nie jesteś robotem, przechodząc weryfikację CAPTCHA przez wprowadzenie wyświetlonego tekstu, a następnie kliknij przycisk **Dalej**

   > [!NOTE]
   > Jeśli administrator nie włączył tej funkcji, w tym momencie zostanie wyświetlony link „Skontaktuj się z administratorem”, aby administrator pomógł w tej sprawie za pośrednictwem poczty e-mail lub własnego portalu sieci Web.
   >

3. W zależności od konfiguracji przeprowadzonej przez administratora zostanie wyświetlona co najmniej jedna z poniższych opcji
    * **Wyślij e-mail na alternatywny adres e-mail** — powoduje wysłanie wiadomości e-mail z 6-cyfrowym kodem na Twój alternatywny adres e-mail lub adres e-mail uwierzytelniania (do wyboru).
    * **Wyślij wiadomość SMS na mój telefon komórkowy** — powoduje wysłanie wiadomości SMS z 6-cyfrowym kodem na numer telefonu komórkowego lub numer telefonu uwierzytelniania (do wyboru).
    * **Zadzwoń na mój telefon komórkowy** — powoduje nawiązanie połączenia z numerem telefonu komórkowego lub numerem telefonu uwierzytelniania (do wyboru). Naciśnij klawisz #, aby zweryfikować połączenie.
    * **Zadzwoń na mój telefon biurowy** — powoduje nawiązanie połączenia z numerem telefonu biurowego. Naciśnij klawisz #, aby zweryfikować połączenie.
    * **Odpowiedz na moje pytania zabezpieczeń** — powoduje wyświetlenie wcześniej zarejestrowanych pytań zabezpieczających, na które musisz odpowiedzieć.
4. Wypełnij wymagane pola odpowiednimi informacjami i kliknij przycisk **Dalej**

    ![Sprawdzanie danych uwierzytelniania][Verification]

5. Administrator może wymagać dodatkowego kroku weryfikacji i może być konieczne powtórzenie kroku 4 z użyciem innej opcji
6. Na stronie **Wybierz nowe hasło** wprowadź nowe hasło spełniające wymagania organizacji, potwierdź je, a następnie kliknij przycisk **Zakończ**

    ![Zmiana hasła][Change]

7. Po wyświetleniu komunikatu **Hasło zostało zresetowane** możesz zalogować się przy użyciu nowego hasła.

    ![Hasło zostało zresetowane][Complete]

Po użyciu tej metody do odblokowania konta lub zresetowania hasła możesz otrzymać wiadomość e-mail z potwierdzeniem ukończenia tego procesu pochodzącą z konta takiego jak „Firma Microsoft w imieniu firmy nazwa_Twojej_organizacji”. Jeśli otrzymasz taką wiadomość e-mail, ale samoobsługowe resetowanie hasła nie było używane w celu odzyskania dostępu do konta, skontaktuj się z administratorem.

## <a name="change-my-password"></a>Zmiana hasła

Jeśli znasz swoje hasło i chcesz je zmienić, wykonaj poniższe kroki

### <a name="change-your-password-from-the-office-365-portal"></a>Zmiana hasła w portalu usługi Office 365

1. Kliknij swój profil w prawym górnym rogu i kliknij pozycję **Wyświetl konto**
2. **Zabezpieczenia i prywatność**
3. **Hasło**
4. Wprowadź stare hasło, a następnie ustaw i potwierdź nowe hasło
5. **Prześlij**

### <a name="change-your-password-from-the-azure-access-panel"></a>Zmiana hasła w panelu dostępu platformy Azure

1. Zaloguj się do [portalu dostępu platformy Azure](https://myapps.microsoft.com/) przy użyciu istniejącego hasła
2. Kliknij swój profil w prawym górnym rogu i kliknij pozycję **Profil**
3. **Zmień hasło**
4. Wprowadź stare hasło, a następnie ustaw i potwierdź nowe hasło
5. **Prześlij**

## <a name="next-steps"></a>Następne kroki

* [Jak zarejestrować się do używania samoobsługowego resetowania hasła](active-directory-passwords-reset-register.md)
* [Strona rejestracji w celu resetowania hasła](http://aka.ms/ssprsetup)
* [Portal resetowania hasła](https://passwordreset.microsoftonline.com/)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Strona logowania — Nie możesz uzyskać dostępu do konta?"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Weryfikowanie danych uwierzytelniania"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Zmiana hasła"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Hasło zostało zresetowane"

