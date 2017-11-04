---
title: "Usługi Azure AD: Rejestracji SSPR | Dokumentacja firmy Microsoft"
description: "Zarejestruj dane uwierzytelniania dla usługi Azure AD hasła samoobsługi resetowania"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: b884f8bc3a20052fa0cb40772deef591b69d7b10
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="register-for-self-service-password-reset"></a>Rejestrowanie na potrzeby samoobsługowego resetowania hasła

> [!IMPORTANT]
> **Jesteś tutaj, ponieważ masz problemy z logowaniem?** Jeśli tak, [w tym miejscu opisano, jak zmienić i zresetować własne hasło](active-directory-passwords-update-your-own-password.md).

Jako użytkownik końcowy może zresetować hasło lub odblokować konto bez konieczności mowy do osoby za pomocą Samoobsługowego resetowania hasła (SSPR). Przed użyciem tej funkcji musisz zarejestrować metody uwierzytelniania lub potwierdzić wstępnie zdefiniowane metody uwierzytelniania podane przez administratora.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Rejestrowanie lub potwierdzanie danych uwierzytelniania na potrzeby samoobsługowego resetowania hasła

1. Otwórz przeglądarkę sieci Web na urządzeniu i przejdź do [strony rejestracji resetowania hasła](http://aka.ms/ssprsetup)
2. Wprowadź nazwę użytkownika i hasło podane przez administratora
3. W zależności od tego, jak działu IT skonfigurowano rzeczy są dostępne umożliwiające skonfigurowanie i zweryfikowanie co najmniej jeden z następujących opcji. Administrator może wypełnienie pól to dla Ciebie, gdy mają Twojej zgody na korzystanie z informacji.
    * Telefon biurowy może tylko określonych przez administratora
    * Numer telefonu uwierzytelniania należy wybrać opcję inny numer telefonu, czy masz dostęp do takich jak telefon komórkowy, który może odbierać SMS lub połączenie.
    * Adres E-mail uwierzytelniania należy wybrać opcję alternatywny adres e-mail dostępnym bez hasła, które należy zresetować.
    * Pytania zabezpieczające przedstawiono listę administratora została zatwierdzona dla Ciebie odpowiedzi na pytania. Nie możesz użyć tego samego pytania lub odpowiedzi na więcej niż raz.
4. Podaj i sprawdź informacje wymagane przez administratora. Jeśli więcej niż jedna opcja jest dostępna, zalecamy, aby zarejestrować wiele metod zapewnienie elastyczności, gdy inna metoda jest niedostępny (przykład: podróży i nie można uzyskać dostępu do Twojego telefonu biurowego)

    ![Rejestrowanie metod uwierzytelniania i klikanie przycisku Zakończ][Register]

5. Po wykonaniu kroku 4 wybierz pozycję **Zakończ**. Od teraz możesz w razie potrzeby używać samoobsługowego resetowania hasła.

Dane wprowadzone w polu Telefon uwierzytelniania lub Adres e-mail uwierzytelniania nie są widoczne w katalogu globalnym. Jedynie Ty i Twoi administratorzy mogą wyświetlać te dane. Odpowiedzi na pytania zabezpieczające są widoczne tylko dla Ciebie.

Administratorzy mogą wymagać potwierdzenia metod uwierzytelniania po pewnym czasie, aby upewnić się, że nadal masz zarejestrowane właściwe metody.

## <a name="common-problems-and-their-solutions"></a>Typowe problemy i ich rozwiązania

 Poniżej przedstawiono niektóre typowe przypadki błąd i ich rozwiązania:

| Przypadek błędu| Jakie błąd znaleźć?| Rozwiązanie |
| --- | --- | --- |
| Pojawia się Strona "Skontaktuj się z administratorem" po wprowadzeniu mój identyfikator użytkownika | Skontaktuj się z administratorem <br> <br> Wykryliśmy, że hasło konta użytkownika nie jest zarządzany przez firmę Microsoft. W związku z tym nie możemy automatycznie zresetować hasła. <br> <br> Należy się skontaktować z działu IT w celu dalszej pomocy. | Ten komunikat jest wyświetlany, ponieważ działu IT zarządza hasła w środowisku lokalnym i nie pozwala na zresetowanie hasła z nie może uzyskać dostępu połączenie z kontem. <br> <br> Aby zresetować hasło, bezpośrednio uzyskać pomoc, skontaktuj się z pracownikami działu IT i go poinformować, że chcesz zresetować hasło, więc ich włączenia tej funkcji można.|
| Pojawia się komunikat o błędzie "Twoje konto nie ma uprawnień do resetowania hasła" po wprowadzeniu mój identyfikator użytkownika | Twoje konto nie ma uprawnień do resetowania hasła <br> <br> Przepraszamy, ale pracownicy działu informatycznego nie skonfigurował tego konta do korzystania z tej usługi. <br> <br> Jeśli chcesz, możemy skontaktować się z administratorem w Twojej organizacji, aby zresetował Twoje hasło. | Ten komunikat jest wyświetlany, ponieważ pracownicy działu informatycznego nie włączył resetowania haseł dla organizacji z nie ma dostępu do konta łącze lub nie ma licencji można korzystać z funkcji. <br> <br> Aby zresetować hasło, kliknij kontakt łącze administratora, aby wysłać wiadomość e-mail do firmy przez personel działu informatycznego i daj znać, aby zresetować hasło, więc ich włączenia tej funkcji można. |
| Pojawia się komunikat o błędzie "nie można zweryfikować konto" po wprowadzeniu mój identyfikator użytkownika | Nie można zweryfikować konta <br> <br> Jeśli chcesz, możemy skontaktować się z administratorem w Twojej organizacji, aby zresetował Twoje hasło. | Ten komunikat jest wyświetlany, ponieważ są włączone do resetowania hasła, ale nie została zarejestrowana do korzystania z usługi. Aby zarejestrować do resetowania hasła, należy przejść do http://aka.ms/ssprsetup po ma odzyskać dostęp do Twojego konta. <br> <br> Aby zresetować hasło, kliknij kontakt łącze administratora, aby wysłać wiadomość e-mail do firmy personel działu informatycznego. |

## <a name="next-steps"></a>Następne kroki

* [Jak zmienić hasło przy użyciu samoobsługowego resetowania hasła](active-directory-passwords-update-your-own-password.md)
* [Strona rejestracji w celu resetowania hasła](http://aka.ms/ssprsetup)
* [Portal resetowania hasła](https://passwordreset.microsoftonline.com/)
* [Nie można zalogować się do konta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Strona rejestrowania na potrzeby resetowania hasła, na której widoczne są zarejestrowane metody i przycisk Zakończ"
