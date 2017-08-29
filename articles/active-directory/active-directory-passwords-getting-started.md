---
title: "Szybki start: Samoobsługowe resetowanie haseł w usłudze Azure AD | Microsoft Docs"
description: "Szybkie wdrażanie samoobsługowego resetowania haseł w usłudze Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 07c7f3ad066c735054cb339f6e09aa4d7d23f23a
ms.contentlocale: pl-pl
ms.lasthandoff: 08/11/2017

---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Szybki start: Samoobsługowe resetowanie haseł w usłudze Azure AD

> [!IMPORTANT]
> **Jesteś tutaj, ponieważ masz problemy z logowaniem?** Jeśli tak, [w tym miejscu opisano, jak zmienić i zresetować własne hasło](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Szybkie wdrażanie samoobsługowego resetowania haseł

Funkcja samoobsługowego resetowania haseł pozwala administratorom IT w prosty sposób dać użytkownikom możliwość resetowania lub odblokowywania ich haseł lub kont. System obejmuje szczegółowe raporty pozwalające śledzić, kiedy użytkownicy korzystają z systemu, oraz powiadomienia ostrzegające o jego nieprawidłowym wykorzystaniu.

W tym przewodniku założono, że masz już działającą dzierżawę usługi Azure AD, w wersji próbnej lub licencjonowanej. Jeśli potrzebujesz pomocy przy konfigurowaniu usługi Azure AD, zobacz artykuł [Getting Started with Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/) (Rozpoczynanie pracy z usługą Azure AD).

1. W istniejącej dzierżawie usługi Azure AD wybierz pozycję **„Resetowanie hasła”**

2. Na ekranie **„Właściwości”** pod opcją „Funkcja samoobsługowego resetowania hasła jest włączona” wybierz jedną z następujących pozycji:
    * Nikt — nikt nie może korzystać z funkcji samoobsługowego resetowania haseł
    * Grupa — z funkcji samoobsługowego resetowania haseł mogą korzystać tylko członkowie określonej, wybranej przez Ciebie grupy usługi Azure AD
    * Wszyscy — z funkcji samoobsługowego resetowania haseł mogą korzystać wszyscy użytkownicy z kontami w Twojej dzierżawie usługi Azure AD

3. Na ekranie **„Metody uwierzytelniania”** wybierz:
    * Liczba metod wymaganych do zresetowania — obsługujemy jedną lub dwie metody
    * Metody dostępne dla użytkowników — potrzebujemy co najmniej jednej metody, ale zawsze dobrze jest mieć wybór
        * **Wiadomość e-mail** — wiadomość e-mail z kodem jest wysyłana na adres e-mail uwierzytelniania skonfigurowany przez użytkownika
        * **Telefon komórkowy** — daje możliwość odebrania połączenia lub wiadomości SMS z kodem pod numerem telefonu komórkowego skonfigurowanym przez użytkownika
        * **Telefon biurowy** — na numer telefonu w biurze skonfigurowany przez użytkownika wykonywane jest połączenie w celu przekazania kodu
        * Opcja **Pytania zabezpieczające** wymaga wybrania następujących wartości:
            * Liczba pytań wymaganych do zarejestrowania się — jest to minimum potrzebne do pomyślnej rejestracji, co oznacza, że użytkownik może wybrać więcej pytań, aby utworzyć pulę pytań do wyboru. Tę opcję można ustawić na wartość z zakresu od 3 do 5 i musi ona być co najmniej równa liczbie pytań wymaganych do zresetowania.
                * Pytania niestandardowe można dodawać podczas wybierania pytań zabezpieczających, klikając przycisk „Niestandardowe”
            * Liczba pytań wymaganych do zresetowania — tę opcję można ustawić na wartość z zakresu od 3 do 5 pytań, na które musi zostać udzielona poprawna odpowiedź, aby użytkownicy mogli zresetować lub odblokować hasło.

4. ZALECANE: opcja **„Dostosowanie”** pozwala zmienić link „Skontaktuj się z administratorem” tak, aby wskazywał zdefiniowaną przez Ciebie stronę lub adres e-mail

5. OPCJONALNE: ekran **„Rejestracja”** zawiera następujące opcje dla administratorów:
    * Czy wymagać od użytkowników rejestrowania się podczas logowania?
    * Liczba dni, zanim użytkownicy zostaną poproszeni o ponowne potwierdzenie swoich informacji uwierzytelniania

6. OPCJONALNE: ekran **„Powiadomienie”** zawiera następujące opcje dla administratorów:
    * Czy powiadamiać użytkowników o resetowaniu hasła?
    * Czy powiadamiać wszystkich administratorów, gdy inni administratorzy zresetują swoje hasło?

**W tym momencie skonfigurowano samoobsługowe resetowanie haseł dla dzierżawy usługi Azure AD**. Możesz się zatrzymać w tym miejscu albo kontynuować w celu skonfigurowania synchronizacji haseł z lokalną domeną usługi AD.

> [!NOTE]
> Przetestuj samoobsługowe resetowanie haseł na koncie użytkownika. Nie rób tego na koncie administratora, ponieważ firma Microsoft narzuca silne wymagania w zakresie uwierzytelniania dla kont administratorów platformy Azure. Aby uzyskać więcej informacji na temat zasad haseł administratorów, zobacz nasz [artykuł na temat zasad haseł](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurowanie synchronizacji z istniejącym źródłem tożsamości

Aby włączyć synchronizację lokalnych tożsamości z usługą Azure AD, musisz zainstalować i skonfigurować aplikację [Azure AD Connect](./connect/active-directory-aadconnect.md) na serwerze w swojej organizacji. Ta aplikacja zajmuje się synchronizowaniem użytkowników oraz grup w istniejącym źródle tożsamości do dzierżawy usługi Azure AD.

* [Uaktualnienie z programu DirSync lub Azure AD Sync do programu Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Rozpoczynanie pracy z programem Azure AD Connect przy użyciu ustawień ekspresowych](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurowanie zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#configuring-password-writeback) w celu zapisywania haseł z usługi Azure AD z powrotem w katalogu lokalnym.

## <a name="disabling-self-service-password-reset"></a>Wyłączanie samoobsługowego resetowania haseł

Wyłączanie samoobsługowego resetowania haseł sprowadza się do otwarcia dzierżawy usługi Azure AD, przejścia do pozycji **Resetowanie hasła > Właściwości** i wybrania pozycji **Brak** w obszarze **Funkcja samoobsługowego resetowania hasła jest włączona**

### <a name="learn-more"></a>Dowiedz się więcej
Poniższe linki dają dostęp do dodatkowych informacji dotyczących resetowania haseł za pomocą usługi Azure AD

* [**Licencjonowanie**](active-directory-passwords-licensing.md) — Konfigurowanie licencjonowania w usłudze Azure AD
* [**Dane**](active-directory-passwords-data.md) — Omówienie wymaganych danych i sposobu ich wykorzystania w zarządzaniu hasłami
* [**Wdrażanie**](active-directory-passwords-best-practices.md) — Planowanie funkcji samoobsługowego resetowania haseł i jej wdrażanie dla użytkowników przy użyciu znajdujących się tu wytycznych
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dostosowywanie wyglądu i działania środowiska samoobsługowego resetowania haseł dla firmy
* [**Zasady**](active-directory-passwords-policy.md) — Omówienie zasad haseł usługi Azure AD i ich ustawianie
* [**Raportowanie**](active-directory-passwords-reporting.md) — Czy, kiedy i gdzie użytkownicy uzyskują dostęp do funkcji samoobsługowego resetowania haseł
* [**Szczegóły techniczne**](active-directory-passwords-how-it-works.md) — Informacje o tym, co dzieje się za kulisami tej funkcji i jak ona działa
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — Jak? Dlaczego? Co? Gdzie? Kto? Kiedy? — Odpowiedzi na wszystkie nurtujące Cię pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Informacje o tym, jak rozwiązywać typowe problemy z samoobsługowym resetowaniem haseł

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z tym przewodnikiem szybkiego startu wiesz już, jak skonfigurować samoobsługowe resetowanie haseł dla swoich użytkowników. Aby przejść do witryny Azure Portal w celu wykonania tych kroków, użyj poniższego linku do portalu.

> [!div class="nextstepaction"]
> [Włącz samoobsługowe resetowanie haseł](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)


