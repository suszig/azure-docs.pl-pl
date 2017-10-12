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
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4cfc1652377f0cfd059e336aec6994b40d32c559
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="roll-out-password-reset-for-users"></a>Wdrażanie resetowania haseł dla użytkowników

Większość klientów wykonuje poniższe kroki, aby zapewnić sobie bezproblemowe wdrażanie funkcji samoobsługowego resetowania haseł.

1. [Włączanie resetowania haseł w katalogu](active-directory-passwords-getting-started.md)
2. [Konfigurowanie lokalnych uprawnień usługi AD do zapisywania zwrotnego haseł](active-directory-passwords-how-it-works.md#active-directory-permissions)
3. [Konfigurowanie zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#configuring-password-writeback) w celu zapisywania haseł z usługi Azure AD z powrotem w katalogu lokalnym
4. [Przypisywanie i weryfikowanie wymaganych licencji](active-directory-passwords-licensing.md)
5. Jeśli chcesz przeprowadzić wdrożenie stopniowe, możesz opcjonalnie ograniczyć możliwość resetowania haseł do grupy użytkowników, aby powoli wdrażać funkcję w miarę upływu czasu. W tym celu zmień ustawienie przełącznika **Funkcja samoobsługowego resetowania hasła jest włączona** z opcji **Wszyscy** na opcję **Grupa** i wybierz grupę zabezpieczeń, dla której ma być włączone resetowanie haseł. Wszyscy członkowie tej grupy muszą mieć przypisaną licencję. Jest to doskonały sposób na włączenie [licencjonowania bazującego na grupach](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing).
6. Wypełnij minimalny zestaw [danych uwierzytelniania](active-directory-passwords-data.md) zgodnie z używanymi zasadami.
7. Naucz użytkowników korzystać z samoobsługowego resetowania haseł, wysyłając im instrukcje rejestrowania i resetowania.
    > [!NOTE]
    > Przetestuj samoobsługowe resetowanie haseł na koncie użytkownika. Nie rób tego na koncie administratora, ponieważ firma Microsoft narzuca silne wymagania w zakresie uwierzytelniania dla kont administratorów platformy Azure. Aby uzyskać więcej informacji na temat zasad haseł administratorów, zobacz nasz [szczegółowy artykuł](active-directory-passwords-how-it-works.md).

8. Możesz zdecydować się na wymuszanie rejestracji w dowolnym punkcie i wymagać od użytkowników ponownego potwierdzania ich informacji uwierzytelniających po określonym czasie. Jeśli nie chcesz, aby Twoi użytkownicy musieli się rejestrować, możesz [wdrożyć resetowanie haseł bez wymagania rejestracji użytkowników końcowych](active-directory-passwords-data.md).
9. Co jakiś czas przeglądaj użytkowników rejestrujących się i korzystających z tej funkcji, wyświetlając [raporty udostępniane przez usługę Azure AD](active-directory-passwords-reporting.md).

## <a name="email-based-rollout"></a>Wdrożenie bazujące na poczcie e-mail

Wielu klientów uważa, że kampania e-mail z prostymi do zastosowania instrukcjami to najłatwiejsza metoda zachęcenia użytkowników do korzystania z samoobsługowego resetowania haseł. [Przygotowaliśmy trzy proste wiadomości e-mail, których możesz użyć jako szablonów w swoim wdrożeniu.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* Szablon wiadomości e-mail **Dostępne wkrótce** do zastosowania w tygodniach lub dniach poprzedzających wdrażanie w celu powiadomienia użytkowników o tym, że mają coś zrobić.
* Szablon wiadomości e-mail **Już dostępne** do zastosowania w dzień uruchomienia, aby zachęcić użytkowników do rejestracji i potwierdzenia swoich danych uwierzytelniania, co pozwoli im skorzystać z samoobsługowego resetowania haseł, gdy będą tego potrzebować.
* Szablon wiadomości e-mail **Przypomnienie o rejestracji** do zastosowania w ciągu kilku dni lub tygodni po wdrożeniu w celu przypomnienia użytkownikom, aby zarejestrowali się i potwierdzili swoje dane uwierzytelniania.

## <a name="creating-your-own-password-portal"></a>Tworzenie własnego portalu haseł

Wielu naszych większych klientów decyduje się na hostowanie strony sieci Web i utworzenie głównego wpisu w usłudze DNS, takiego jak https://passwords.contoso.com. Umieszczają na tej stronie linki do funkcji resetowania haseł usługi Azure AD, rejestracji w funkcji resetowaniu haseł, portali do zmiany haseł i innych informacji specyficznych dla organizacji. Możliwe jest wtedy umieszczanie w dowolnej komunikacji e-mail i rozsyłanych materiałach marketingowych firmowych, łatwych do zapamiętania adresów URL, które użytkownicy mogą odwiedzić, gdy będą potrzebować użyć tych usług.

* Portal resetowania haseł — https://passwordreset.microsoftonline.com/
* Portal rejestracji w funkcji resetowania haseł — http://aka.ms/ssprsetup
* Portal zmiany haseł — https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Korzystanie z rejestracji wymuszonej

Jeśli chcesz, aby użytkownicy rejestrowali się w celu zresetowania hasła, możesz wymusić na nich rejestrowanie podczas logowania się przy użyciu usługi Azure AD. Opcję tę możesz włączyć w bloku **Resetowanie hasła** swojego katalogu, włączając opcję **Czy wymagać od użytkowników rejestrowania się podczas logowania?** na karcie **Rejestracja**.

Administratorzy mogą wymagać od użytkowników ponownego zarejestrowania się po upływie określonego czasu, ustawiając opcję **Liczba dni, zanim użytkownicy zostaną poproszeni o ponowne potwierdzenie swoich informacji uwierzytelniania** na wartość z zakresu od 0 do 730 dni.

Po włączeniu tej opcji logujący się użytkownicy zobaczą komunikat informujący, że administrator wymaga od nich zweryfikowania ich informacji uwierzytelniania.

## <a name="populate-authentication-data"></a>Wypełnianie danych uwierzytelniania

Jeśli [wypełnisz dane uwierzytelniania dla swoich użytkowników](active-directory-passwords-data.md), nie będą oni musieli rejestrować się w funkcji resetowania haseł, aby skorzystać z samoobsługowego resetowania haseł. Tak długo, jak użytkownicy będą mieli zdefiniowane dane uwierzytelniania spełniające zdefiniowane przez Ciebie zasady resetowania haseł, będą mogli resetować swoje hasła.

## <a name="disabling-self-service-password-reset"></a>Wyłączanie samoobsługowego resetowania haseł

Wyłączanie samoobsługowego resetowania haseł sprowadza się do otwarcia dzierżawy usługi Azure AD, wybrania pozycji **Resetowanie hasła** i **Właściwości**, a następnie wybrania pozycji **Nikt** w sekcji **Funkcja samoobsługowego resetowania hasła jest włączona**

## <a name="next-steps"></a>Następne kroki

Poniższe linki dają dostęp do dodatkowych informacji dotyczących resetowania haseł za pomocą usługi Azure AD

* [**Szybki start**](active-directory-passwords-getting-started.md) — Przygotowywanie do pracy samoobsługowego zarządzania hasłami w usłudze Azure AD 
* [**Licencjonowanie**](active-directory-passwords-licensing.md) — Konfigurowanie licencjonowania w usłudze Azure AD
* [**Dane**](active-directory-passwords-data.md) — Omówienie wymaganych danych i sposobu ich wykorzystania w zarządzaniu hasłami
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dostosowywanie wyglądu i działania środowiska samoobsługowego resetowania haseł dla firmy
* [**Zasady**](active-directory-passwords-policy.md) — Omówienie zasad haseł usługi Azure AD i ich ustawianie
* [**Zapisywanie zwrotne haseł**](active-directory-passwords-writeback.md) — Jak zapisywanie zwrotne haseł współpracuje z katalogiem lokalnym
* [**Raportowanie**](active-directory-passwords-reporting.md) — Czy, kiedy i gdzie użytkownicy uzyskują dostęp do funkcji samoobsługowego resetowania haseł
* [**Szczegóły techniczne**](active-directory-passwords-how-it-works.md) — Informacje o tym, co dzieje się za kulisami tej funkcji i jak ona działa
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — Jak? Dlaczego? Co? Gdzie? Kto? Kiedy? — Odpowiedzi na wszystkie nurtujące Cię pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Informacje o tym, jak rozwiązywać typowe problemy z samoobsługowym resetowaniem haseł