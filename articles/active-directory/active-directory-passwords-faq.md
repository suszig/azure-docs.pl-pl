---
title: "Często zadawane pytania: Azure AD SSPR | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dotyczące usługi Azure AD samoobsługi hasła resetowania"
services: active-directory
keywords: "Zarządzanie hasłami w usłudze Active directory, zarządzanie hasłami, usługi Azure AD samodzielnego resetowania hasła usługi"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 14c565bb67480681e1d398a0a21a11448f405e4e
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="password-management-frequently-asked-questions"></a>Często zadawane pytania dotyczące zarządzania hasłami

Poniżej przedstawiono niektóre często zadawane pytania dla wszystkich czynności związane z resetowania hasła.

Jeśli masz pytania ogólne dotyczące usługi Azure AD i hasło samoobsługowego resetowania, który nie ma odpowiedzi w tym miejscu, można zadawać społeczności pomocy na [fora usługi Azure Ad](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Członkowie społeczności obejmują Engineers, menedżerów produktu MVP i twarzy specjalistów IT.

Często zadawane pytania jest podzielony na następujące sekcje:

* [**Pytania dotyczące rejestracji resetowania hasła**](#password-reset-registration)
* [**Pytania dotyczące resetowania hasła**](#password-reset)
* [**Pytania dotyczące zmiany hasła**](#password-change)
* [**Raporty pytania dotyczące zarządzania hasłami**](#password-management-reports)
* [**Pytania dotyczące funkcji zapisywania zwrotnego haseł**](#password-writeback)

## <a name="password-reset-registration"></a>Rejestracja resetowania haseł

* **Pytanie: czy Moi użytkownicy rejestrowanie własnych danych resetowania hasła?**

  > **Odpowiedź:** tak, jak długo resetowania hasła jest włączona i są one licencjonowane, może przejść do portalu rejestracji resetowania haseł pod adresem http://aka.ms/ssprsetup zarejestrować swoje informacje o uwierzytelnianiu. Użytkownicy mogą również zarejestrować, przechodząc do panelu dostępu pod adresem http://myapps.microsoft.com, klikając kartę profilu i klikając rejestru dla opcji resetowania hasła.
  >
  >
* **Q: czy można zdefiniować dane resetowania hasła w imieniu użytkowników?**

  > **Odpowiedź:** tak, możesz to zrobić z programem Azure AD Connect, programu PowerShell, [portalu Azure](https://portal.azure.com), lub w portalu administracyjnym pakietu Office. Aby uzyskać więcej informacji, zobacz artykuł [dane używane przez usługi Azure AD samoobsługowego resetowania hasła](active-directory-passwords-data.md).
  >
  >
* **Pytanie: czy można synchronizować dane odpowiedzi na pytania zabezpieczające z lokalnie?**

  > **Odpowiedź:** nie jest to możliwe dzisiaj.
  >
  >
* **Pytanie: czy Moi użytkownicy zarejestrować danych w taki sposób, że inni użytkownicy nie widzą te dane?**

  > **Odpowiedź:** tak, gdy użytkownicy rejestrują danych przy użyciu portalu resetowania haseł rejestracji jest zapisywany do pól prywatnych uwierzytelniania, które są tylko widoczne, Administratorzy globalni i użytkownika.
  >
  >
* **Pytanie: czy Moi użytkownicy muszą być zarejestrowane, aby móc korzystać z resetowania hasła?**

  > **Odpowiedź:** nie, w przypadku definiowania wystarczających informacji uwierzytelniania w ich imieniu, użytkownicy muszą zarejestrować. Działania resetowania hasła, tak długo, jak długo ma prawidłowo sformatowane dane przechowywane w odpowiednich polach w katalogu.
  >
  >
* **Pytanie: można zsynchronizować lub ustawić adres E-mail uwierzytelniania lub alternatywny numer telefonu uwierzytelniania pól, numer telefonu uwierzytelniania w imieniu użytkowników?**

  > **Odpowiedź:** nie jest to możliwe dzisiaj.
  >
  >
* **Pytanie: jak portal rejestracji wiedzieć jakie opcje Pokaż moich użytkowników?**

  > **Odpowiedź:** portalu rejestracji resetowania haseł wyświetlane są tylko opcje włączeniu dla użytkowników. Te opcje można znaleźć w sekcji zasady resetowania hasła użytkownika, karta Konfigurowanie w katalogu. Na przykład oznacza to, że jeśli pytania zabezpieczające nie jest włączona, następnie użytkownicy nie mają możliwości rejestrowania dla tej opcji.
  >
  >
* **Pytanie: Jeśli użytkownik jest uznawany zarejestrowane?**

  > **A:** użytkownik jest uznawany w zarejestrowany dla usługi SSPR, gdy zarejestrowanych co najmniej **wiele metod wymaganych do zresetowania** ustawioną [portalu Azure](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Resetowanie hasła

* **Pytanie: jak długo należy czekać do odbierania wiadomości e-mail, SMS lub połączeń telefonicznych z resetowania hasła?**

  > **Odpowiedź:** poczty E-mail, wiadomości SMS i połączeń telefonicznych powinna odbierane w obszarze jedną minutę, z normalna sytuacja 5-20 sekund.
    >Jeśli użytkownik nie otrzyma powiadomienie w tym czasie:
        > * Sprawdź folder wiadomości-śmieci.
        > * Czy numer lub adres e-mail jest nawiązywane połączenie jest oczekiwane.
        > * Sprawdź, czy dane uwierzytelniania w katalogu jest poprawnie sformatowana.
                >     * Przykład: "+ 1 4255551234" lub "user@contoso.com"
  >
  >
* **Pytanie: jakie języki są obsługiwane przez resetowania hasła?**

  > **Odpowiedź:** resetowania hasła użytkownika wiadomości SMS i połączeń głosowych są zlokalizowane w te same języki, które są obsługiwane w usłudze Office 365.
  >
  >
* **Karta na konfigurowanie Q: części środowisko resetowania hasła uzyskać marki, gdy ustawić organizacji znakowania w katalogu Moje?**

  > **Odpowiedź:** portal resetowania haseł pokazuje logo organizacji i pozwala na skonfigurowanie kontaktu link do administratora wskazywał niestandardowe wiadomości e-mail lub adres URL. Każdej wiadomości e-mail, które są wysyłane przez resetowania haseł obejmuje logo organizacji, kolory, nazwy w treści wiadomości e-mail i dostosować na podstawie nazwy.
  >
  >
* **Pytanie: jak można Poinformuj użytkowników o tym, gdzie można przejść na resetowanie haseł?**

  > **A:** wypróbować niektóre sugestii w naszym [artykułu wdrożenia SSPR](active-directory-passwords-best-practices.md#email-based-rollout)
  >
  >
* **Pytanie: czy można użyć tej strony z urządzenia przenośnego?**

  > **Odpowiedź:** tak, ta strona działa na urządzeniach przenośnych.
  >
  >
* **Pytanie: czy odblokowywanie kont z lokalnej usługi active directory są obsługiwane podczas użytkownikom resetowania swoich haseł w?**

  > **Odpowiedź:** tak, gdy użytkownik resetuje hasła i zapisywania zwrotnego haseł został wdrożony za pomocą usługi Azure AD Connect, konto użytkownika jest odblokowany automatycznie podczas resetowania hasła.
  >
  >
* **Pytanie: jak można zintegrować resetowania bezpośrednio do mojego użytkownika logowania środowisko pulpitu**

  > **Odpowiedź:** w przypadku klienta programu Azure AD Premium można zainstalować programu Microsoft Identity Manager bez ponoszenia dodatkowych kosztów i wdrażania rozwiązania resetowania hasła lokalnego, aby spełnić to wymaganie.
  >
  >
* **Pytanie: czy można ustawić pytania zabezpieczeń dla różnych ustawień regionalnych?**

  > **Odpowiedź:** nie jest to możliwe dzisiaj.
  >
  >
* **Pytanie: jak wiele pytań możemy skonfigurować dla opcji uwierzytelniania pytania zabezpieczające?**

  > **Odpowiedź:** można skonfigurować maksymalnie 20 pytania zabezpieczające niestandardowych w [portalu Azure](https://portal.azure.com).
  >
  >
* **Pytanie: jak długo może być pytania zabezpieczające?**

  > **Odpowiedź:** pytania zabezpieczające może mieć długość od 3 do 200 znaków.
  >
  >
* **Pytanie: jak długo może być odpowiedzi na pytania zabezpieczające?**

  > **Odpowiedź:** odpowiedzi może być 3 do 40 znaków.
  >
  >
* **Pytanie: czy zduplikowane odpowiedzi na pytania zabezpieczające odrzucone?**

  > **Odpowiedź:** tak, możemy Odrzuć zduplikowane odpowiedzi na pytania zabezpieczające.
  >
  >
* **Pytanie: czy maja użytkownika zarejestrować na tym samym pytanie zabezpieczające więcej niż raz?**

  > **Odpowiedź:** nie, gdy użytkownik rejestruje dane pytanie, ich nie może zarejestrować dla tego zapytania po raz drugi.
  >
  >
* **Pytanie: czy jest możliwe ustawić minimalny limit pytania zabezpieczające dla rejestracji i zresetować?**

  > **Odpowiedź:** tak, można ustawić limit jeden dla rejestracji i drugi dla resetowania. pytania zabezpieczające 3 – 5 może być wymagany w czasie rejestracji i 3 – 5 mogą być wymagane w celu resetowania.
  >
  >
* **Pytanie: po skonfigurowaniu Moje zasady, aby wymagać od użytkowników użyj pytań zabezpieczających do resetowania, ale administratorzy Azure wydają się inaczej skonfigurowana.**

  > **Odpowiedź:** jest to oczekiwane zachowanie. Microsoft wymusza silne domyślnych zasad resetowania hasła bramy dwóch dla każdej roli administratora platformy Azure. Powoduje wyłączenie administratorów za pomocą pytań zabezpieczających. Więcej informacji na temat tych zasad można znaleźć w artykule [ograniczenia w usłudze Azure Active Directory i zasad haseł](active-directory-passwords-policy.md#administrator-password-policy-differences)
  >
  >
* **Pytanie: Jeśli użytkownik został zarejestrowany w więcej niż maksymalna liczba pytań wymaganych do zresetowania, jak są pytania zabezpieczające wybrane podczas resetowania?**

  > **Odpowiedź:** N pytania zabezpieczające są losowo wybrane poza łączna liczba pytań, użytkownik został zarejestrowany, gdzie N to **liczby pytań wymaganych do zresetowania**. Na przykład jeśli użytkownik ma 5 pytania zabezpieczające w zarejestrowany, ale do resetowania wymagane są tylko 3, 3, 5 są wybierane losowo i przedstawionego na resetowanie. Jeśli użytkownik pobiera nieprawidłowe odpowiedzi na pytania, aby zapobiec atakowaniu pytanie będzie się powtarzał procesu wyboru.
  >
  >
* **Pytanie: czy można uniemożliwić użytkownikom próby resetowania wielokrotnie w krótkim przedziale czasu?**

  > **Odpowiedź:** tak, są wbudowane w resetowania hasła, aby chronić przed niewłaściwym użyciem funkcje zabezpieczeń. Użytkownicy mogą tylko spróbuj 5 próbach resetowania hasła w ciągu godziny przed jest zablokowane przez 24 godziny. Użytkownicy mogą tylko spróbuj można sprawdzić poprawności numeru telefonu 5 razy w ciągu godziny przed jest zablokowane przez 24 godziny. Użytkownicy mogą tylko spróbuj użyć metody pojedynczego uwierzytelniania 5 razy w ciągu godziny przed jest zablokowane przez 24 godziny.
  >
  >
* **Pytanie: na jak długo są prawidłowe poczty e-mail i SMS jednorazowy kod dostępu?**

  > **Odpowiedź:** okres istnienia sesji do resetowania hasła jest 15 minut. Od początku operacji resetowania hasła użytkownika ma 15 minut do zresetowania swojego hasła. Po tym okresie poczty e-mail i SMS jednorazowy kod dostępu są nieprawidłowe.
  >
  >
* **Pytanie: czy można zablokować użytkownikom zresetowania hasła?**

  > **Odpowiedź:** tak, jeśli używasz grupy, aby włączyć samoobsługowe Resetowanie hasła można usunąć je z grupy, która umożliwia im tę możliwość.
  >
  >

## <a name="password-change"></a>Zmienianie hasła

* **Pytanie: gdzie Moi użytkownicy powinien przejść do zmiany haseł?**

  > **A:** użytkownicy mogą zmieniać swoje hasła dowolnym Zobacz ich obraz profilu lub ikonę (w prawym górnym rogu, takich jak ich [usługi Office 365](https://portal.office.com) lub [panelu dostępu](https://myapps.microsoft.com) wykonywania. Użytkownicy mogą zmieniać swoje hasła z [strony panelu dostępu profilu](https://account.activedirectory.windowsazure.com/r#/profile). Użytkownicy mogą również poproszony o zmiany haseł automatycznie ekranu logowania usługi Azure AD, jeśli ich hasła wygasły. Ponadto użytkownicy mogą przechodzić do [Portal zmiany hasła programu Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) bezpośrednio Jeśli chcesz zmienić swoje hasła.
  >
  >
* **Pytanie: czy Moi użytkownicy powiadamianych w portalu usługi Office po wygaśnięciu hasła lokalnego?**

  > **Odpowiedź:** jest to możliwe dzisiaj Jeśli używasz usług AD FS, postępując zgodnie z instrukcjami w tym miejscu: [wysyłanie oświadczeń zasad haseł z usług AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Jeśli używasz synchronizacji skrótu hasła nie jest to możliwe dzisiaj. Jest to spowodowane nie zsynchronizować zasad haseł z lokalnej, więc nie jest możliwe w firmie Microsoft można wysłać powiadomienia o wygasaniu do środowiska chmury. W obu przypadkach istnieje również możliwość [Powiadom użytkowników, których hasła zbliża się termin wygaśnięcia przy użyciu programu PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **Pytanie: czy można zablokować użytkownikom zmianę ich haseł?**

  > **Odpowiedź:** dla użytkowników tylko w chmurze nie mogą zostać zablokowane. Dla użytkowników lokalnych można ustawić `User cannot change password` można sprawdzić i tych użytkowników, nie będzie można zmienić swoje hasło.
  >
  >

## <a name="password-management-reports"></a>Raporty zarządzania hasła

* **Pytanie: jak długo trwa danych wyświetlani w raportach zarządzania hasło?**

  > **Odpowiedź:** danych powinny pojawiać się na raporty zarządzania hasła w ciągu 5 – 10 minut. Go niektórych wystąpień może potrwać do godziny pojawią się.
  >
  >
* **Pytanie: jak można filtrować raporty zarządzania hasło?**

  > **Odpowiedź:** można filtrować raporty zarządzania hasło, klikając małych Lupa extreme po prawej stronie etykiety kolumn, w górnej części raportu. Jeśli chcesz bardziej rozbudowane filtrowanie, można pobrać raportu i utworzyć tabelę przestawną w programie excel.
  >
  >
* **Pytanie: jaka jest maksymalna liczba zdarzeń są przechowywane w raportach dotyczących zarządzania hasło?**

  > **Odpowiedź:** maksymalnie 75,000 resetowania lub hasło rejestracji resetowania hasła zdarzenia są przechowywane w raportach dotyczących zarządzania hasła, obejmujących kopii maksymalnie 30 dni.  Pracujemy nad Rozwiń ten numer, aby zarejestrować więcej zdarzeń.
  >
  >
* **Pytanie: jak daleko raporty zarządzania hasło go?**

  > **Odpowiedź:** zarządzania hasłami raporty operacji Pokaż wykonywanych w ciągu ostatnich 30 dni. Teraz Jeśli chcesz zarchiwizować te dane, musisz można pobrać raporty okresowo i zapisać je w innej lokalizacji.
  >
  >
* **Pytanie: istnieje już maksymalną liczbę wierszy, które mogą być wyświetlane w raportach zarządzania hasło?**

  > **Odpowiedź:** tak, maksymalnie 75,000 wierszy może pojawić się w zależności od raporty zarządzania hasło, czy są są wyświetlane w interfejsie użytkownika lub pobierania.
  >
  >
* **Pytanie: jest interfejs API dostępu do resetowania hasła lub rejestracji dane raportowania?**

  > **Odpowiedź:** tak, zobacz następującą dokumentację, aby dowiedzieć się, jak można pobrać resetowania hasła, strumień danych raportowania.  [Dowiedz się, jak uzyskać dostępu do zdarzeń do raportowania programowane Resetowanie hasła](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Zapisywanie zwrotne haseł

* **Pytanie: jak działa funkcji zapisywania zwrotnego haseł w tle?**

  > **Odpowiedź:** zobacz [sposób działania funkcji zapisywania zwrotnego haseł](active-directory-passwords-writeback.md) dla wyjaśnienie, co się stanie po włączeniu funkcji zapisywania zwrotnego haseł i sposób dane przepływają przez system do środowiska lokalnego.
  >
  >
* **Pytanie: jak długo funkcji zapisywania zwrotnego haseł podąża pracę?  Czy istnieje opóźnienie synchronizacji jak z synchronizacji skrótów haseł?**

  > **Odpowiedź:** błyskawicznych jest funkcji zapisywania zwrotnego haseł. Jest synchroniczne potok, który działa zasadniczo inaczej niż synchronizacji skrótu hasła. Zapisywanie zwrotne haseł umożliwia użytkownikom Uzyskaj opinie w czasie rzeczywistym o udanym ich resetowania hasła lub zmienić operację. Średni czas na pomyślne zapisywania zwrotnego hasła jest w obszarze 500 ms.
  >
  >
* **Pytanie: Jeśli konto lokalne jest wyłączone, Moje konto/dostęp do chmury wpływ?**

  > **Odpowiedź:** wyłączenie Identyfikatora lokalnej chmury identyfikator/access również zostaną wyłączone w następnym interwale czasowym synchronizacji za pomocą AAD Connect domyślnie to co 30 minut.
  >
  >
* **Pytanie: czy Moje konto lokalne jest ograniczane przez zasady haseł lokalnej usługi Active Directory, SSPR przestrzegać tych zasad po zmianie hasła?**

  > **Odpowiedź:** tak, SSPR opiera się na i przestrzega zasad lokalnych zasad haseł usługi AD, w tym typowych zasad haseł domeny usługi AD, a także wszelkie zdefiniowane zasady szczegółowe hasło przeznaczone dla danego użytkownika.
  >
  >
* **Pytanie: jakie typy kont zapisywania zwrotnego haseł działa dla?**

  > **Odpowiedź:** funkcji zapisywania zwrotnego haseł działa w przypadku federacyjnym i synchronizacji skrótów haseł użytkowników.
  >
  >
* **Pytanie: czy funkcji zapisywania zwrotnego haseł wymusić zasady haseł mojej domeny?**

  > **Odpowiedź:** tak, funkcję zapisywania zwrotnego haseł wymusza okres ważności hasła, historii złożoności, filtrów i inne ograniczenia, może umieścić w miejscu na haseł w domenie lokalnej.
  >
  >
* **Pytanie: czy jest bezpieczna funkcji zapisywania zwrotnego haseł?  Jak można mieć pewność, że I nie będzie włamania?**

  > **Odpowiedź:** tak, zapisywanie zwrotne haseł jest bezpieczne. Aby uzyskać więcej informacji na temat cztery warstwy zabezpieczeń zaimplementowanych przez usługę zapisywania zwrotnego haseł, zapoznaj się [model zabezpieczeń zapisywania zwrotnego haseł](active-directory-passwords-writeback.md#password-writeback-security-model) sekcji w sposób działania funkcji zapisywania zwrotnego haseł.
  >
  >

## <a name="next-steps"></a>Następne kroki

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
