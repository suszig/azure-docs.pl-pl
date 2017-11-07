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
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d97fad04a0aa549d0e3a182282f898302e8e41a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Szybkie wdrażanie samoobsługowego resetowania haseł w usłudze Azure AD

> [!IMPORTANT]
> **Jesteś tutaj, ponieważ masz problemy z logowaniem?** Jeśli tak, [w tym miejscu opisano, jak zmienić i zresetować własne hasło](active-directory-passwords-update-your-own-password.md).

Funkcja samoobsługowego resetowania haseł pozwala administratorom IT w prosty sposób dać użytkownikom możliwość resetowania lub odblokowywania ich haseł lub kont. System obejmuje szczegółowe raporty pozwalające śledzić, kiedy użytkownicy korzystają z systemu, oraz powiadomienia ostrzegające o jego nieprawidłowym wykorzystaniu.

W tym przewodniku założono, że masz już działającą dzierżawę usługi Azure AD, w wersji próbnej lub licencjonowanej. Jeśli potrzebujesz pomocy przy konfigurowaniu usługi Azure AD, zobacz artykuł [Getting Started with Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/) (Rozpoczynanie pracy z usługą Azure AD).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Włączanie funkcji samoobsługowego resetowania haseł dla dzierżawy usługi Azure AD

1. W istniejącej dzierżawie usługi Azure AD wybierz pozycję **„Resetowanie hasła”**

2. Na ekranie **„Właściwości”** pod opcją „Funkcja samoobsługowego resetowania hasła jest włączona” wybierz jedną z następujących pozycji:
    * Brak — nikt nie może korzystać z funkcji samoobsługowego resetowania haseł.
    * Wybrano — z funkcji samoobsługowego resetowania haseł mogą korzystać tylko członkowie określonej, wybranej przez Ciebie grupy usługi Azure AD. Zaleca się zdefiniowanie grupy użytkowników i korzystanie z tego ustawienia podczas wdrażania w celu weryfikacji koncepcji.
    * Wszystkie — z funkcji samoobsługowego resetowania haseł mogą korzystać wszyscy użytkownicy z kontami w Twojej dzierżawie usługi Azure AD. Zaleca się ustawienie tej opcji po osiągnięciu gotowości do wdrożenia tej funkcji w całej dzierżawie po zakończeniu etapu weryfikacji koncepcji.

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
            
    ![Uwierzytelnianie][Authentication]

4. ZALECANE: opcja **„Dostosowanie”** pozwala zmienić link „Skontaktuj się z administratorem” tak, aby wskazywał zdefiniowaną przez Ciebie stronę lub adres e-mail. Zaleca się ustawienie tego linku na adres e-mail lub witrynę internetową, z których korzystają użytkownicy na potrzeby uzyskiwania pomocy technicznej.

5. OPCJONALNE: ekran **„Rejestracja”** zawiera następujące opcje dla administratorów:
    * Czy wymagać od użytkowników rejestrowania się podczas logowania?
    * Liczba dni, zanim użytkownicy zostaną poproszeni o ponowne potwierdzenie swoich informacji uwierzytelniania

6. OPCJONALNE: ekran **„Powiadomienie”** zawiera następujące opcje dla administratorów:
    * Czy powiadamiać użytkowników o resetowaniu hasła?
    * Czy powiadamiać wszystkich administratorów, gdy inni administratorzy zresetują swoje hasło?

**W tym momencie skonfigurowano samoobsługowe resetowanie haseł dla dzierżawy usługi Azure AD**. Użytkownicy mogą teraz używać instrukcji opisanych w artykułach [Rejestrowanie na potrzeby samoobsługowego resetowania hasła](active-directory-passwords-reset-register.md) i [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md) do aktualizowania swoich haseł bez interwencji administratora. Możesz się zatrzymać w tym miejscu, jeśli pracujesz tylko w chmurze, albo kontynuować w celu skonfigurowania synchronizacji haseł z lokalną domeną usługi AD.

> [!IMPORTANT]
> Przetestuj samoobsługowe resetowanie haseł na koncie użytkownika. Nie rób tego na koncie administratora, ponieważ firma Microsoft narzuca silne wymagania w zakresie uwierzytelniania dla kont administratorów platformy Azure. Aby uzyskać więcej informacji na temat zasad haseł administratorów, zobacz nasz [artykuł na temat zasad haseł](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurowanie synchronizacji z istniejącym źródłem tożsamości

Aby włączyć synchronizację lokalnych tożsamości z usługą Azure AD, musisz zainstalować i skonfigurować aplikację [Azure AD Connect](./connect/active-directory-aadconnect.md) na serwerze w swojej organizacji. Ta aplikacja zajmuje się synchronizowaniem użytkowników oraz grup w istniejącym źródle tożsamości do dzierżawy usługi Azure AD.

* [Uaktualnienie z programu DirSync lub Azure AD Sync do programu Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Rozpoczynanie pracy z programem Azure AD Connect przy użyciu ustawień ekspresowych](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurowanie zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#configuring-password-writeback) w celu zapisywania haseł z usługi Azure AD z powrotem w katalogu lokalnym.

### <a name="on-premises-policy-change"></a>Zmienianie zasad lokalnych

Jeśli synchronizujesz użytkowników z lokalnej domeny usługi Active Directory i chcesz dać im możliwość natychmiastowego resetowania swoich haseł, wprowadź następującą zmianę w lokalnych zasadach haseł:

**Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady konta** > **Zasady haseł**

**Minimalny okres ważności hasła** — 0 dni

To ustawienie zabezpieczeń określa czas (w dniach), przez jaki hasło musi być używane, zanim użytkownik będzie mógł je zmienić. Przypisanie dla tego ustawienia wartości **0 dni** umożliwia użytkownikom korzystanie z funkcji samoobsługowego resetowania haseł, jeśli ich hasła zostaną zmienione przez zespoły pomocy technicznej.

![Zasady][Policy]

## <a name="disabling-self-service-password-reset"></a>Wyłączanie samoobsługowego resetowania haseł

Wyłączanie samoobsługowego resetowania haseł sprowadza się do otwarcia dzierżawy usługi Azure AD, przejścia do pozycji **Resetowanie hasła > Właściwości** i wybrania pozycji **Brak** w obszarze **Funkcja samoobsługowego resetowania hasła jest włączona**

### <a name="learn-more"></a>Dowiedz się więcej
Poniższe linki dają dostęp do dodatkowych informacji dotyczących resetowania haseł za pomocą usługi Azure AD

* [Jak wykonać pomyślne wdrożenie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie na potrzeby samoobsługowego resetowania haseł](active-directory-passwords-reset-register.md).
* [Czy masz pytanie dotyczące licencjonowania?](active-directory-passwords-licensing.md)
* [Jakie dane są używane przez funkcję samoobsługowego resetowania haseł i jakie dane powinny zostać wypełnione dla użytkowników?](active-directory-passwords-data.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad dla funkcji samoobsługowego resetowania haseł?](active-directory-passwords-policy.md)
* [Co to jest funkcja zapisywania zwrotnego haseł i dlaczego jest ona tak ważna?](active-directory-passwords-writeback.md)
* [Jak zgłosić działanie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-reporting.md)
* [Jakie są dostępne opcje funkcji samoobsługowego resetowania haseł i do czego one służą?](active-directory-passwords-how-it-works.md)
* [Myślę, że coś działa niewłaściwie. Jak rozwiązywać problemy z funkcją samoobsługowego resetowania haseł?](active-directory-passwords-troubleshoot.md)
* [Mam pytanie, na które nie mogę znaleźć odpowiedzi](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z tym przewodnikiem szybkiego startu wiesz już, jak skonfigurować samoobsługowe resetowanie haseł dla swoich użytkowników. Aby przejść do witryny Azure Portal w celu wykonania tych kroków, użyj poniższego linku do portalu.

> [!div class="nextstepaction"]
> [Włącz samoobsługowe resetowanie haseł](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Dostępne metody uwierzytelniania w usłudze Azure AD i wymagane ilości"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Lokalne zasady grupy haseł ustawione na wartość 0 dni"
