---
title: "Jak to działa? Azure AD SSPR | Dokumentacja firmy Microsoft"
description: "Azure AD samoobsługowego resetowania hasła nowości"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: fd9515120049dd3837a43c95de8a9b6822719e19
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Samoobsługowe Resetowanie w usłudze Azure AD nowości haseł

Jak działa SSPR Co oznacza tej opcji w interfejsie? Materiały, aby dowiedzieć się więcej na temat usługi Azure AD samoobsługi hasła resetowania.

## <a name="how-does-the-password-reset-portal-work"></a>Jak hasło zresetować portalu pracy

Gdy użytkownik przechodzi do portalu resetowania haseł, przepływu pracy zostało rozpoczęte ustalenie:

   * Jak powinny być lokalizowany strony?
   * Konto użytkownika jest prawidłowy?
   * Jakie organizacji czy użytkownik należy do?
   * Gdy hasło użytkownika jest zarządzany
   * Użytkownik jest licencji na korzystanie z funkcji?


Strony resetowania odczytu kolejnych kroków, aby dowiedzieć się więcej o logiki hasło.

1. Użytkownik klika polecenie nie może uzyskać dostępu z połączenie z kontem lub umieszczane bezpośrednio do [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
2. Oparte na ustawienia regionalne przeglądarki, że środowisko jest odwzorowywany w odpowiednim języku. Środowisko resetowania hasła jest zlokalizowana na te same języki obsługuje usługi Office 365.
3. Użytkownik wprowadza nazwę użytkownika i przekazuje captcha.
4. Usługi Azure AD sprawdza, czy użytkownik jest w stanie korzystać z tej funkcji, wykonując następujące czynności kontrolne:
   * Sprawdza, czy użytkownik ma ta funkcja jest włączona i przypisanej licencji usługi Azure AD.
     * Jeśli użytkownik nie ma, ta funkcja jest włączona lub przypisać licencję, użytkownik jest proszony o ich administratora, aby zresetować swoje hasło.
   * Sprawdza, czy użytkownik ma prawo żądanie danych zdefiniowanych na koncie, zgodnie z zasadami administratora.
     * Jeśli zasady są wymagane tylko w jednym z wyzwań, następnie zapewnia się, że użytkownik ma odpowiednie dane zdefiniowane dla co najmniej jednym z wyzwań włączone przez zasady administratora.
       * Jeśli użytkownik nie jest skonfigurowane, użytkownik jest zalecane jest administratora do zresetowania swojego hasła.
     * Jeśli zasady są wymagane dwa problemy, następnie zapewnia się, że użytkownik ma odpowiednie dane zdefiniowane dla co najmniej dwóch wyzwania włączone przez zasady administratora.
       * Jeśli użytkownik nie jest skonfigurowany, a następnie możemy użytkownik jest zalecana administratora do zresetowania swojego hasła.
   * Sprawdza, czy hasło użytkownika odbywa się lokalnie (federacyjnych lub synchronizacji skrótów haseł).
     * Jeśli zapisywania zwrotnego jest wdrożone i hasło użytkownika odbywa się lokalnie, aby przejść do uwierzytelniania i resetowania hasła jest dozwolone użytkownika.
     * Jeśli zapisywania zwrotnego nie została wdrożona i hasło użytkownika jest zarządzane lokalnie, użytkownik jest proszony o ich administratora, aby zresetować swoje hasło.
5. Jeśli okaże się, że użytkownik jest w stanie pomyślnie zresetować swoje hasło, użytkownik jest sterowana przez proces resetowania.

## <a name="authentication-methods"></a>Metody uwierzytelniania

Po włączeniu funkcji samoobsługowego resetowania hasła (SSPR), musisz wybrać co najmniej jeden z następujących opcji dla metod uwierzytelniania. Czasami może wysłuchać te opcje określone jako bramy. Zdecydowanie zaleca się wybranie co najmniej dwóch metod uwierzytelniania, dzięki czemu użytkownicy mają większą elastyczność.

* Adres e-mail
* Telefon komórkowy
* Telefon biurowy
* Pytania zabezpieczające

![Uwierzytelnianie][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Jakie pola są używane w katalogu danych uwierzytelniania

* Telefon biurowy odpowiada telefon biurowy
    * Użytkownicy są nie można ustawić pola się muszą być zdefiniowane przez administratora
* Telefon komórkowy odpowiada numer telefonu uwierzytelniania (publicznie niewidoczny) lub telefonu komórkowego (widocznego publicznie)
    * Usługa najpierw szuka numer telefonu uwierzytelniania, następnie powraca na telefon komórkowy Jeśli nie są zainstalowane
* Alternatywny adres E-mail odpowiada E-mail uwierzytelniania (publicznie niewidoczny) lub alternatywny adres E-mail
    * Usługa najpierw szuka uwierzytelniania wiadomości E-mail, a następnie powrót po awarii do alternatywny adres E-mail

Domyślnie tylko atrybuty chmury telefon biurowy i telefon komórkowy są synchronizowane z katalogu lokalnego dla danych uwierzytelniania katalogu w chmurze.

Użytkownicy mogą tylko zresetować swoje hasło, gdy mają danych zawartych w metod uwierzytelniania, których administrator włączył i wymaga.

Jeśli użytkownik nie ma ich numer telefonu komórkowego, aby były widoczne w katalogu, lecz nadal chcesz używać go do resetowania hasła, Administratorzy powinien nie wypełnić go w katalogu, a użytkownik należy następnie wypełnij ich **numer telefonu uwierzytelniania** atrybutu za pośrednictwem [portalu rejestracji resetowania haseł](http://aka.ms/ssprsetup). Administratorzy mogą zobaczyć te informacje w profilu użytkownika, ale nie jest opublikowana w innym miejscu.

### <a name="number-of-authentication-methods-required"></a>Wiele metod uwierzytelniania wymagany

Ta opcja określa minimalną liczbę dostępne metody uwierzytelniania lub bramy, użytkownik musi przejść do resetowania lub odblokowania hasła i może być równa 1 lub 2.

Użytkownicy, można podać więcej metod uwierzytelniania, jeśli są one włączone przez administratora.

Jeśli użytkownik nie ma minimalnych wymaganych metod zarejestrowany, zostanie wyświetlona strona błędu, który kieruje żądanie do zresetowania swojego hasła administratora.

#### <a name="changing-authentication-methods"></a>Zmiana metody uwierzytelniania

Jeśli rozpoczyna się zasady, która ma tylko jedną metodę uwierzytelniania wymagany do resetowania lub odblokować zarejestrowane i zmiany, że do dwóch co się stanie?

| Liczba metod w zarejestrowany | Wiele metod wymagane | wynik |
| :---: | :---: | :---: |
| co najmniej 1 | 1 | **Możliwe** zresetować lub odblokowania |
| 1 | 2 | **Nie można** zresetować lub odblokowania |
| co najmniej 2 | 2 | **Możliwe** zresetować lub odblokowania |

Jeśli zmienisz typy metod uwierzytelniania, użytkownik może używać możesz przypadkowo może przestać użytkownicy mogli używać SSPR, jeśli nie mają minimalnej ilości dostępnych danych.

Przykład: 
1. Oryginalny zasady skonfigurowane z 2 metod uwierzytelniania, wymagany przy użyciu tylko office pytania telefonu i zabezpieczeń. 
2. Administrator może zmienić zasady nie są już używane pytań zabezpieczających, ale zezwalaj na korzystanie z telefonów komórkowych i alternatywny adres e-mail.
3. Użytkownicy bez telefon komórkowy i pól alternatywny adres e-mail nie można zresetować hasła.

### <a name="how-secure-are-my-security-questions"></a>Jak bezpieczne czy moje pytania zabezpieczające

Jeśli Użyj pytań zabezpieczających, zaleca się ich używany z innej metody jako może być mniej bezpieczne od innych metod, ponieważ niektóre osoby mogą dowiedzieć się odpowiedzi na pytania innego użytkownika.

> [!NOTE] 
> Pytania zabezpieczające są przechowywane przez użytkowników i bezpiecznie obiektu użytkownika w katalogu i można tylko udzielane przez użytkowników podczas rejestracji. Nie istnieje sposób na odczyt lub modyfikacja pytań i odpowiedzi użytkownika przez administratora.
>

### <a name="security-question-localization"></a>Lokalizacja pytanie zabezpieczeń

Wszystkie wstępnie zdefiniowanych pytania, które należy wykonać są zlokalizowane w pełny zestaw języków usługi Office 365 oparte na ustawienia regionalne przeglądarki użytkownika.

* W jakim mieście poznałeś/poznałaś swoją pierwszą współmałżonkę lub partnerkę albo swojego pierwszego współmałżonka lub partnera?
* W jakim mieście spotkali się Twoi rodzice?
* Jakie jest najbliższe miasto, w którym mieszka Twoje rodzeństwo?
* W jakim mieście urodził się Twój ojciec?
* W jakim mieście podjąłeś/podjęłaś swoją pierwszą pracę?
* W jakim mieście urodziła się Twoja matka?
* W jakim mieście byłaś/eś w dniu Nowego Roku 2000?
* Co to jest nazwisko Twój ulubiony nauczyciel w dużej * służbowego?
* Na jaką uczelnię próbowałeś/próbowałaś się dostać, ale się nie udało?
* Gdzie odbyło się Twoje pierwsze wesele?
* Jak ma na drugie imię Twój ojciec?
* Jaka jest Twoja ulubiona potrawa?
* Jak ma na imię i nazwisko Twoja babcia od strony matki?
* Jakie jest drugie imię Twojej matki?
* Co to jest data urodzenia miesiąc i rok Twojego najstarszego rodzeństwa? (np. listopad 1985)
* Jak ma na drugie imię Twój najstarszy brat/siostra?
* Jak miał na imię i nazwisko Twój dziadek od strony ojca?
* Jakie jest drugie imię Twojego najmłodszego rodzeństwa?
* Jak nazywała się Twoja szkoła podstawowa?
* Jak miał na imię i nazwisko Twój najlepszy przyjaciel w dzieciństwie?
* Jak miała na imię i nazwisko Twoja pierwsza poważna sympatia?
* Jak miał na nazwisko Twój ulubiony nauczyciel w szkole podstawowej?
* Jaka była marka i model Twojego pierwszego samochodu lub motocykla?
* Jak się nazywała Twoja pierwsza szkoła?
* Jak nazywał się szpital Twoich narodzin?
* Przy jakiej ulicy znajdował się Twój pierwszy dom z dzieciństwa?
* Jak się nazywał Twój bohater z dzieciństwa?
* Jak się nazywała Twoja ulubiona maskotka?
* Jak się wabiło Twoje pierwsze zwierzę domowe?
* Jaki był Twój pseudonim w dzieciństwie?
* Jaki był Twój ulubiony sport w szkole średniej?
* Jaka była Twoja pierwsza praca?
* Jakie były cztery ostatnie cyfry Twojego numeru telefonu z dzieciństwa?
* Kim chciałeś/chciałaś zostać w dzieciństwie, gdy dorośniesz?
* Jak się nazywa najpopularniejsza poznana przez Ciebie osoba?

### <a name="custom-security-questions"></a>Pytania zabezpieczające niestandardowych

Pytania zabezpieczające niestandardowe nie są zlokalizowane dla różnych ustawień regionalnych. Wszystkie pytania niestandardowe są wyświetlane w tym samym języku, jaki są wprowadzane w interfejsie użytkownika administracyjnego, nawet jeśli ustawienia regionalne przeglądarki użytkownika jest inna. Jeśli potrzebujesz zlokalizowanych pytania, użyj wstępnie zdefiniowanych pytań.

Maksymalna długość pytanie zabezpieczające niestandardowych jest 200 znaków.

### <a name="security-question-requirements"></a>Wymagania dotyczące pytanie zabezpieczeń

* Ograniczenie liczby znaków minimalna odpowiedzi jest 3 znaków
* Odpowiedzi maksymalny limit znaków wynosi 40 znaków
* Użytkownicy nie mógł odpowiedzieć na tego samego zapytania więcej niż jeden raz
* Użytkownicy mogą nie zapewniać tego samego odpowiedź na pytanie więcej niż jeden
* Każdy zestaw znaków mogą służyć do zdefiniowania pytania i odpowiedzi w tym znaków Unicode
* Liczba pytań zdefiniowanych przez musi być większa lub równa liczbie pytań wymaganych do rejestracji

## <a name="registration"></a>Rejestracja

### <a name="require-users-to-register-when-signing-in"></a>Czy wymagać od użytkowników rejestrowania się podczas logowania?

Włączenie tej opcji wymaga się, że użytkownik, który jest włączony dla resetowania hasła, aby ukończyć hasło rejestracji resetowania, jeśli zalogować się do aplikacji przy użyciu usługi Azure AD do logowania się w takich jak te, które należy wykonać:

* Office 365
* Azure Portal
* Panel dostępu
* Aplikacji federacyjnych
* Niestandardowe aplikacje przy użyciu usługi Azure AD

Po wyłączeniu to użytkownicy będą mogli nadal ręcznie zarejestrować swoje informacje kontaktowe, odwiedzając [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) lub przez kliknięcie przycisku **zarejestrować do resetowania hasła** link na karcie profil w panel dostępu.

> [!NOTE]
> Użytkownicy można odrzucić portalu rejestracji resetowania haseł przez kliknięcie przycisku Anuluj lub zamykania okna, ale są monitowani o każdym razem, gdy są one logowania aż do chwili zakończenia rejestracji.
>
> Spowoduje to nie przerwanie połączenia użytkownika, jeśli są one już zalogowany.

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Liczba dni, zanim użytkownicy zostaną poproszeni o ponowne potwierdzenie swoich informacji uwierzytelniania

Ta opcja określa okres czasu między ustawienie i reconfirming informacje dotyczące uwierzytelniania i jest dostępna tylko po włączeniu **wymagają rejestracji użytkowników podczas logowania** opcji.

Prawidłowe wartości to 0 730 dni od 0, co oznacza nigdy nie pytaj użytkowników, aby potwierdzić swoje informacje dotyczące uwierzytelniania

## <a name="notifications"></a>Powiadomienia

### <a name="notify-users-on-password-resets"></a>Czy powiadamiać użytkowników o resetowaniu hasła?

Jeśli ta opcja jest ustawiona tak, użytkownik, który jest zresetowania hasła otrzymuje wiadomość e-mail z informacją, że ich hasło zostało zmienione za pośrednictwem portalu SSPR na ich adresy e-mail podstawowe i alternatywne w pliku w usłudze Azure AD. Żaden inny użytkownik jest powiadamiany o resetowania zdarzeń.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Powiadamianie wszystkich administratorów innych administratorów resetowanie haseł

Jeśli ta opcja jest ustawiona tak, następnie **wszystkich administratorów** otrzymasz wiadomość e-mail do ich podstawowego adresu e-mail w pliku w usłudze Azure AD z informacją, że inny administrator zmienił ich hasła przy użyciu funkcji SSPR.

Przykład: Istnieją cztery Administratorzy w środowisku. Administrator "A" Resetuje hasła przy użyciu funkcji SSPR. Administratorzy B, C i D otrzymywać wiadomości e-mail powiadamiające ich sytuacja.

## <a name="on-premises-integration"></a>Integracja z lokalnymi

Jeśli zostały zainstalowane, skonfigurowane i włączone Azure AD Connect, masz następujące dodatkowe opcje integracji z lokalnymi. Jeśli te opcje są wyszarzone w poziomie, a następnie zapisywania zwrotnego nie został poprawnie skonfigurowany zobacz [Konfigurowanie funkcji zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#configuring-password-writeback) Aby uzyskać więcej informacji.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Zapisywania zwrotnego haseł do katalogu lokalnego

Określa, czy włączono funkcję zapisywania zwrotnego haseł dla tego katalogu, a jeśli zapisywania zwrotnego jest włączona, wskazuje stan lokalną usługą zapisywania zwrotnego. Jest to przydatne, jeśli chcesz tymczasowo wyłączyć funkcję zapisywania zwrotnego haseł, bez konieczności ponownej konfiguracji usługi Azure AD Connect.

* Jeśli przełącznik ma wartość tak, następnie zapisywania zwrotnego jest włączona i federacyjnych i hasło skrótu synchronizowane użytkownicy będą mogli resetować hasła.
* Jeśli przełącznik jest ustawiona na nie, następnie zapisywania zwrotnego jest wyłączone i federacyjnych i użytkowników synchronizacji skrótu hasła są nie można zresetować hasła.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Zezwalaj użytkownikom na odblokowanie kont bez konieczności zresetowania hasła

Określa, czy użytkownicy, którzy odwiedzają witrynę portalu resetowania haseł powinien mieć możliwość do odblokowania ich lokalnych kont usługi Active Directory bez zresetowania hasła. Domyślnie program Azure AD umożliwia odblokowanie konta podczas resetowania hasła, to ustawienie służy do oddzielania tych dwóch operacji. 

* Jeśli ustawiono wartość "tak", następnie użytkownicy otrzymują możliwość do zresetowania swojego hasła i odblokowania konta lub odblokować bez potrzeby resetowania hasła.
* Jeśli wartość "nie", a następnie użytkownicy są tylko do wykonania Scalonej hasła resetowania i operacji odblokowania konta.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Jak hasło zresetować pracy B2B użytkowników?
Resetowanie hasła i zmiany są w pełni obsługiwane we wszystkich konfiguracjach B2B. Następujące trzy przypadki są obsługiwane w przypadku resetowania hasła użytkownika B2B.

1. **Użytkownicy z organizacji partnera, z istniejącą dzierżawą usługi Azure AD** — Jeśli w organizacji są partnerstwo z jest istniejącej dzierżawy usługi Azure AD, firma Microsoft **przestrzegać niezależnie od zasady resetowania hasła są włączone w tej dzierżawie**. Dla hasła zresetować pracy partnera organizacji tylko konieczność upewnij się, że usługi Azure AD SSPR jest włączona, co stanowi bez dodatkowych opłat, klientów usługi Office 365, i można ją włączyć, wykonując kroki opisane w naszym [wprowadzenie do zarządzania hasłami](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords)przewodnik.
2. **Użytkownicy, którzy konta, za pomocą [samoobsługowego tworzenia konta](active-directory-self-service-signup.md)**  — Jeśli w organizacji są partnerstwo z używanych [samoobsługowego tworzenia konta](active-directory-self-service-signup.md) funkcji, aby pobrać do dzierżawcy, nie możemy udostępnić zresetowana z zastosowaniem adres e-mail, który one zarejestrowane.
3. **Użytkownicy B2B** -żadnych nowych użytkowników B2B utworzone za pomocą nowej [możliwości B2B usługi Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md) będzie można resetować hasła z wiadomością e-mail one zarejestrowane podczas procesu zaproszenia.

Do przetestowania tego scenariusza, przejdź do http://passwordreset.microsoftonline.com jeden z tych użytkowników z firm partnerskich. Tak długo, jak długo mają alternatywny adres e-mail lub uwierzytelniania wiadomości e-mail zdefiniowanych resetowania haseł działa zgodnie z oczekiwaniami.

> [!NOTE]
> Dzierżawy konta Microsoft, którym udzielono dostępu dla gości do usługi Azure AD, takich jak zasoby z usługi Outlook.com, Hotmail.com lub innych osobistych adresów e-mail nie będą mogli używać usługi Azure AD SSPR i trzeba będzie zresetować hasło przy użyciu informacji zamieszczonych w artykuł [po nie logowania się do konta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).

## <a name="next-steps"></a>Następne kroki

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

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Dostępne metody uwierzytelniania w usłudze Azure AD i wymagane ilości"