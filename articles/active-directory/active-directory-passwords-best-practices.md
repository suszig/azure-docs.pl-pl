---
title: "Wdrażanie: Samoobsługowe resetowanie haseł w usłudze Azure AD | Microsoft Docs"
description: "Porady pomagające w pomyślnym wdrażaniu samoobsługowego resetowania haseł w usłudze Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0be1616f5df915e566dc73c15dbea2e53177aa1c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="how-to-successfully-rollout-self-service-password-reset"></a>Jak pomyślnie wdrożyć funkcję samoobsługowego resetowania haseł

Większość klientów wykonuje następujące kroki, aby zapewnić sobie bezproblemowe wdrażanie funkcji samoobsługowego resetowania haseł.

1. [Włącz resetowanie haseł w katalogu](active-directory-passwords-getting-started.md).
2. [Skonfiguruj lokalne uprawnienia usługi AD do zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#active-directory-permissions).
3. [Konfigurowanie zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#configuring-password-writeback) w celu zapisywania haseł z usługi Azure AD z powrotem w katalogu lokalnym.
4. [Przypisz i zweryfikuj wymagane licencje](active-directory-passwords-licensing.md).
5. Jeśli chcesz przeprowadzić stopniowe wdrażanie funkcji samoobsługowego resetowania haseł, możesz ograniczyć dostęp do jednej grupy użytkowników i przeprowadzić dla tej grupy pilotażowe wdrażanie. W tym celu zmień ustawienie przełącznika **Funkcja samoobsługowego resetowania hasła jest włączona** na opcję **Wybrano** i wybierz grupę zabezpieczeń, dla której ma być włączone resetowanie haseł. 
6. Wypełnij [dane uwierzytelniania](active-directory-passwords-data.md) użytkowników, takie jak numer telefonu służbowego, numer telefonu komórkowego i alternatywny adres e-mail.
7. [Dostosuj środowisko logowania do usługi Azure AD, aby uwzględnić oznaczenie marką firmy](active-directory-passwords-customize.md).
8. Naucz użytkowników korzystać z samoobsługowego resetowania haseł, wysyłając im instrukcje rejestrowania i resetowania.
9. Możesz zdecydować się na wymuszanie rejestracji w dowolnym punkcie i wymagać od użytkowników ponownego potwierdzania ich informacji uwierzytelniających po określonym czasie.
10. Co jakiś czas przeglądaj użytkowników rejestrujących się i korzystających z tej funkcji, wyświetlając [raporty udostępniane przez usługę Azure AD](active-directory-passwords-reporting.md).
11. Gdy wszystko będzie gotowe, włącz resetowanie haseł dla wszystkich użytkowników, ustawiając przełącznik **Funkcja samoobsługowego resetowania hasła jest włączona** na wartość **Wszystkie**. 

   > [!IMPORTANT]
   > Przetestuj samoobsługowe resetowanie haseł na koncie użytkownika. Nie rób tego na koncie administratora, ponieważ firma Microsoft narzuca silne wymagania w zakresie uwierzytelniania dla kont administratorów platformy Azure. Aby uzyskać więcej informacji na temat zasad haseł administratorów, zobacz nasz [artykuł na temat zasad haseł](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>Wdrożenie bazujące na poczcie e-mail

Wielu klientów uważa, że kampania e-mail z prostymi do zastosowania instrukcjami to najłatwiejsza metoda zachęcenia użytkowników do korzystania z samoobsługowego resetowania haseł. [Przygotowaliśmy trzy proste wiadomości e-mail, których możesz użyć jako szablonów w swoim wdrożeniu.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* Szablon wiadomości e-mail **Dostępne wkrótce** do zastosowania w tygodniach lub dniach poprzedzających wdrażanie w celu powiadomienia użytkowników o tym, że mają coś zrobić.
* Szablon wiadomości e-mail **Już dostępne** do zastosowania w dzień uruchomienia, aby zachęcić użytkowników do rejestracji i potwierdzenia swoich danych uwierzytelniania, co pozwoli im skorzystać z samoobsługowego resetowania haseł, gdy będą tego potrzebować.
* Szablon wiadomości e-mail **Przypomnienie o rejestracji** do zastosowania w ciągu kilku dni lub tygodni po wdrożeniu w celu przypomnienia użytkownikom, aby zarejestrowali się i potwierdzili swoje dane uwierzytelniania.

![Wiadomość e-mail][Email]

## <a name="creating-your-own-password-portal"></a>Tworzenie własnego portalu haseł

Wielu naszych większych klientów decyduje się na hostowanie strony sieci Web i utworzenie głównego wpisu w usłudze DNS, takiego jak https://passwords.contoso.com. Umieszczają na tej stronie linki do funkcji resetowania haseł usługi Azure AD, rejestracji w funkcji resetowaniu haseł, portali do zmiany haseł i innych informacji specyficznych dla organizacji. Możliwe jest wtedy umieszczanie w dowolnej komunikacji e-mail i rozsyłanych materiałach marketingowych firmowych, łatwych do zapamiętania adresów URL, które użytkownicy mogą odwiedzić, gdy będą potrzebować użyć tych usług.

* Portal resetowania haseł — https://aka.ms/sspr
* Portal rejestracji w funkcji resetowania haseł — http://aka.ms/ssprsetup
* Portal zmiany haseł — https://account.activedirectory.windowsazure.com/ChangePassword.aspx

Pomocną może się okazać utworzona przykładowa strona, której można użyć po ewentualnym jej dostosowaniu do potrzeb organizacji. Można ją pobrać z witryny [GitHub](https://github.com/ajamess/password-reset-page).

## <a name="using-enforced-registration"></a>Korzystanie z rejestracji wymuszonej

Jeśli chcesz, aby użytkownicy rejestrowali się w celu zresetowania hasła, możesz wymusić na nich rejestrowanie podczas logowania się przy użyciu usługi Azure AD. Opcję tę możesz włączyć w bloku **Resetowanie hasła** swojego katalogu, włączając opcję **Czy wymagać od użytkowników rejestrowania się podczas logowania?** na karcie **Rejestracja**.

Administratorzy mogą wymagać od użytkowników ponownego zarejestrowania się po upływie określonego czasu, ustawiając opcję **Liczba dni, zanim użytkownicy zostaną poproszeni o ponowne potwierdzenie swoich informacji uwierzytelniania** na wartość z zakresu od 0 do 730 dni.

Po włączeniu tej opcji logujący się użytkownicy zobaczą komunikat informujący, że administrator wymaga od nich zweryfikowania ich informacji uwierzytelniania.

## <a name="populate-authentication-data"></a>Wypełnianie danych uwierzytelniania

Jeśli [wypełnisz dane uwierzytelniania dla swoich użytkowników](active-directory-passwords-data.md), nie będą oni musieli rejestrować się w funkcji resetowania haseł, aby skorzystać z samoobsługowego resetowania haseł. Tak długo, jak użytkownicy będą mieli zdefiniowane dane uwierzytelniania spełniające zdefiniowane przez Ciebie zasady resetowania haseł, będą mogli resetować swoje hasła.

## <a name="disabling-self-service-password-reset"></a>Wyłączanie samoobsługowego resetowania haseł

Wyłączanie samoobsługowego resetowania haseł sprowadza się do otwarcia dzierżawy usługi Azure AD, wybrania pozycji **Resetowanie hasła** i **Właściwości**, a następnie wybrania pozycji **Nikt** w sekcji **Funkcja samoobsługowego resetowania hasła jest włączona**

## <a name="next-steps"></a>Następne kroki

* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie na potrzeby samoobsługowego resetowania haseł](active-directory-passwords-reset-register.md).
* [Czy masz pytanie dotyczące licencjonowania?](active-directory-passwords-licensing.md)
* [Jakie dane są używane przez funkcję samoobsługowego resetowania haseł i jakie dane powinny zostać wypełnione dla użytkowników?](active-directory-passwords-data.md)
* [Jakie są opcje zasad dla funkcji samoobsługowego resetowania haseł?](active-directory-passwords-policy.md)
* [Co to jest funkcja zapisywania zwrotnego haseł i dlaczego jest ona tak ważna?](active-directory-passwords-writeback.md)
* [Jak zgłosić działanie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-reporting.md)
* [Jakie są dostępne opcje funkcji samoobsługowego resetowania haseł i do czego one służą?](active-directory-passwords-how-it-works.md)
* [Myślę, że coś działa niewłaściwie. Jak rozwiązywać problemy z funkcją samoobsługowego resetowania haseł?](active-directory-passwords-troubleshoot.md)
* [Mam pytanie, na które nie mogę znaleźć odpowiedzi](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Dostosuj te szablony wiadomości e-mail do wymagań swojej organizacji"