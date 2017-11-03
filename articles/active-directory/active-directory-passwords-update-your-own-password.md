---
title: "Azure AD: resetowanie hasła | Microsoft Docs"
description: "Użyj samoobsługowego resetowania hasła, aby odzyskać dostęp do tego konta służbowego"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: gahug
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8f175f5711b9798153d209f6645aa8bae9f2a00b
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="help-i-forgot-my-azure-ad-password"></a>Pomoc, pamiętam hasła usługi Azure AD

Jeśli nie pamiętasz hasła, Twój dział IT nigdy Ci go nie przekazał, masz zablokowany dostęp do swojego konta lub chcesz zmienić hasło, możemy Ci pomóc. Jeśli znasz hasła i po prostu należy zmienić nadal w dół do sekcji [zmienić hasło](#change-my-password) poniżej.

   > [!NOTE]
   > Jeśli próbujesz wrócić na koncie osobistych, takich jak Xbox, hotmail.com lub outlook.com spróbuj [sugestie znaleziono w tym artykule](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)
   >

## <a name="reset-or-unlock-my-password-for-a-work-or-school-account"></a>Resetowanie lub odblokowywanie hasła do konta służbowego

Jeśli użytkownik nie może uzyskać dostępu do konta z jednego z następujących czynności:

* Hasło nie działa, a chcesz je zresetować.
* Znać hasło, ale Twoje konto jest zablokowane i chcesz odblokować konto.

Wykonaj kroki, które należy wykonać, aby dostęp do usługi Azure AD samoobsługowego resetowania hasła, SSPR, jak firma Microsoft, takich jak wywołać ją i wrócić do konta.

1. Na dowolnej stronie logowania do konta służbowego kliknij link **Nie możesz uzyskać dostępu do konta?**, a następnie kliknij pozycję **Konto służbowe** lub przejdź bezpośrednio do [strony resetowania hasła](https://passwordreset.microsoftonline.com/).

    ![Nie możesz uzyskać dostępu do konta?][Login]

2. Wprowadź **identyfikator użytkownika** swojego konta służbowego, wpisz znaki widoczne na ekranie, aby udowodnić, że nie jesteś robotem, a następnie kliknij przycisk **Dalej**.

   > [!NOTE]
   > Jeśli pracownicy działu IT nie włączyli tej funkcji, pojawi się link „skontaktuj się z administratorem”, aby pracownicy działu IT mogli udzielić pomocy za pośrednictwem poczty e-mail lub własnego portalu sieci Web.
   >
   > Jeśli potrzebujesz odblokować konto, w tym momencie wybierz opcję "Znam hasło, ale nadal nie możesz zarejestrować się".
   >

3. W zależności od tego, jak działu IT skonfigurował SSPR zobaczysz jednego lub więcej z następujących metod uwierzytelniania. Użytkownik lub pracownicy działu informatycznego wypełnił niektóre z tych informacji, aby można było używać tego artykułu [rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).

   * **Wyślij wiadomość e-mail na mój alternatywny adres e-mail**
   * **Wyślij wiadomość SMS na mój telefon komórkowy**
   * **Zadzwoń na mój telefon komórkowy**
   * **Zadzwoń na mój telefon biurowy**
   * **Odpowiedz na moje pytania zabezpieczeń**

   Wybierz jedną z opcji, podaj poprawne odpowiedzi, a następnie kliknij przycisk **Dalej**.

   ![Sprawdzanie danych uwierzytelniania][Verification]

4. Pracownicy działu IT mogą wymagać dodatkowej weryfikacji i może być konieczne powtórzenie kroku 3 z wyborem innej opcji.
5. Na stronie **Wybierz nowe hasło** wprowadź nowe hasło, potwierdź je, a następnie kliknij przycisk **Zakończ**. Sugerujemy, aby hasło miało długość od 8 do 16 znaków i zawierało wielkie i małe litery, cyfry oraz znaki specjalne.
6. Po wyświetleniu komunikatu **Hasło zostało zresetowane** możesz zalogować się przy użyciu nowego hasła.

    ![Hasło zostało zresetowane][Complete]

Teraz uzyskanie dostępu do konta powinno być możliwe. Jeśli nie, skontaktuj się ze swoim działem IT w celu uzyskania dalszej pomocy.

Otrzymasz wiadomość e-mail z potwierdzeniem pochodzi z konta, takich jak "Microsoft zastępczy \<organizacji >". Jeśli otrzymasz taką wiadomość e-mail, ale samoobsługowe resetowanie hasła nie było używane w celu odzyskania dostępu do konta, skontaktuj się z działem IT swojej organizacji.

## <a name="change-my-password"></a>Zmiana hasła

Jeśli znasz swoje hasło i chcesz je zmienić, wykonaj poniższe kroki w celu zmiany hasła.

### <a name="change-your-password-from-the-office-365-portal"></a>Zmiana hasła w portalu usługi Office 365

Skorzystaj z tej metody, jeśli zwykle uzyskujesz dostęp do aplikacji za pomocą portalu usługi Office.

1. Zaloguj się do Twojego [konto usługi Office 365](https://www.office.com) przy użyciu istniejącego hasła
2. Kliknij swój profil w prawym górnym rogu i kliknij pozycję **Wyświetl konto**
3. Kliknij pozycje **Zabezpieczenia i prywatność** > **Hasła**
4. Wprowadź stare hasło, ustaw i potwierdź nowe hasło, a następnie kliknij przycisk **Prześlij**

### <a name="change-your-password-from-the-azure-access-panel"></a>Zmiana hasła w panelu dostępu platformy Azure

Skorzystaj z tej metody, jeśli zwykle uzyskujesz dostęp do aplikacji za pomocą portalu dostępu platformy Azure.

1. Zaloguj się do [portalu dostępu platformy Azure](https://myapps.microsoft.com/) przy użyciu istniejącego hasła
2. Kliknij swój profil w prawym górnym rogu i kliknij pozycję **Profil**
3. Kliknij pozycję **Zmień hasło**
4. Wprowadź stare hasło, ustaw i potwierdź nowe hasło, a następnie kliknij przycisk **Prześlij**

## <a name="common-problems-and-their-solutions"></a>Typowe problemy i ich rozwiązania

 Poniżej przedstawiono niektóre typowe przypadki błąd i ich rozwiązania:

| Przypadek błędu| Jakie błąd znaleźć?| Rozwiązanie |
| --- | --- | --- |
| Pojawia się Strona "Skontaktuj się z administratorem" po wprowadzeniu mój identyfikator użytkownika | Skontaktuj się z administratorem <br> <br> Wykryliśmy, że hasło konta użytkownika nie jest zarządzany przez firmę Microsoft. W związku z tym nie możemy automatycznie zresetować hasła. <br> <br> Należy się skontaktować z działu IT w celu dalszej pomocy. | Ten komunikat jest wyświetlany, ponieważ działu IT zarządza hasła w środowisku lokalnym i nie pozwala na zresetowanie hasła z nie może uzyskać dostępu połączenie z kontem. <br> <br> Aby zresetować hasło, bezpośrednio uzyskać pomoc, skontaktuj się z pracownikami działu IT i go poinformować, że chcesz zresetować hasło, więc ich włączenia tej funkcji można.|
| Pojawia się komunikat o błędzie "Twoje konto nie ma uprawnień do resetowania hasła" po wprowadzeniu mój identyfikator użytkownika | Twoje konto nie ma uprawnień do resetowania hasła <br> <br> Przepraszamy, ale pracownicy działu informatycznego nie skonfigurował tego konta do korzystania z tej usługi. <br> <br> Jeśli chcesz, możemy skontaktować się z administratorem w Twojej organizacji, aby zresetował Twoje hasło. | Ten komunikat jest wyświetlany, ponieważ pracownicy działu informatycznego nie włączył resetowania haseł dla organizacji z nie ma dostępu do konta łącze lub nie ma licencji można korzystać z funkcji. <br> <br> Aby zresetować hasło, kliknij kontakt łącze administratora, aby wysłać wiadomość e-mail do firmy przez personel działu informatycznego i daj znać, aby zresetować hasło, więc ich włączenia tej funkcji można. |
| Pojawia się komunikat o błędzie "nie można zweryfikować konto" po wprowadzeniu mój identyfikator użytkownika | Nie można zweryfikować konta <br> <br> Jeśli chcesz, możemy skontaktować się z administratorem w Twojej organizacji, aby zresetował Twoje hasło. | Ten komunikat jest wyświetlany, ponieważ są włączone do resetowania hasła, ale nie została zarejestrowana do korzystania z usługi. Aby zarejestrować do resetowania hasła, należy przejść do http://aka.ms/ssprsetup po ma odzyskać dostęp do Twojego konta. <br> <br> Aby zresetować hasło, kliknij kontakt łącze administratora, aby wysłać wiadomość e-mail do firmy personel działu informatycznego. |

## <a name="next-steps"></a>Następne kroki

* [Jak zarejestrować się do używania samoobsługowego resetowania hasła](active-directory-passwords-reset-register.md)
* [Strona rejestracji w celu resetowania hasła](http://aka.ms/ssprsetup)
* [Portal resetowania hasła](https://passwordreset.microsoftonline.com/)
* [Nie można zalogować się do konta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Strona logowania — Nie możesz uzyskać dostępu do konta?"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Weryfikowanie danych uwierzytelniania"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Zmiana hasła"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Hasło zostało zresetowane"
