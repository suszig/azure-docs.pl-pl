---
title: "Microsoft Authenticator aplikacji pomocy i obsługi technicznej | Dokumentacja firmy Microsoft"
description: "Zawiera listę często zadawanych pytań i odpowiedzi dotyczące aplikacji Microsoft Authentication i Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: femila
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 4d1302416cfa767415d7bb1e8b426079099caf28
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Aplikacja uwierzytelniania firmy Microsoft — często zadawane pytania

Ten artykuł zawiera odpowiedzi na często zadawane pytania otrzymujących o aplikacji Authenticator firmy Microsoft. Jeśli nie widzisz odpowiedź na swoje pytanie, przejdź do [forum aplikacji Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Mamy także innego często zadawane pytania dotyczące określonych funkcji w aplikacji, [Zaloguj się przy użyciu telefonu — często zadawane pytania](microsoft-authenticator-app-phone-signin-faq.md).

Aplikacja Microsoft Authenticator zastępowane aplikacji Azure Authenticator, a jest zalecaną aplikację, korzystając z usługi Azure Multi-Factor Authentication. Jest dostępna dla aplikacji Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), i [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Co to są kody w aplikacji w? Dlaczego numer keep, licząc w?

Po otwarciu aplikacji Authenticator firmy Microsoft, zobacz konta, które zostały dodane i numer lub ośmiu sześciocyfrowy przez każde z nich. Może pojawić się czasomierza 30 sekund, licząc w.

Te kody są używane podczas logowania się do swojego konta. Po wprowadzeniu nazwy użytkownika i hasła, może zostać poproszona o wprowadź kod weryfikacyjny. Otwórz aplikację Microsoft Authenticator i skopiuj kod, który jest aktualnie wyświetlany. Wprowadź ten kod na stronie logowania, aby zakończyć.

Powód kody zmienić co 30 sekund jest, aby nie używać tego samego kodu dwukrotnie. Nie ma takich jak hasła, które są powinien do zapamiętania. Pomysł jest, że tylko osoba z dostępem do telefonu zna kod weryfikacyjny.

Kody nie wymagają internet lub dane, tak aby nie trzeba się martwić o telefoniczna do logowania. Podczas zamykania aplikacji nie zachować uruchomione w tle, a nie opróżnienia baterii. Możesz zamknąć aplikacji i Ignoruj, dopóki przy następnym logowaniu.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Można tylko otrzymywać powiadomienia, gdy aplikacja na komputerze otwórz. Jeśli aplikacja nie jest otwarty, nie pojawia się powiadomienia.

Jeśli otrzymywać powiadomień, ale nie należy szumu lub Włącz wibrację pomimo Twojej dzwonka w, sprawdź najpierw ustawień aplikacji. Włącz aplikację, aby użyć dźwięku, ani też z jego powiadomienia.

Jeśli w ogóle nie otrzymywać powiadomień, sprawdź następujących przypadkach:

- Twój telefon znajduje się w trybie nie przeszkadzać lub cichego? Ten tryb może zapobiec wysyłanie powiadomień aplikacji.
- Możesz otrzymywać powiadomienia z innych aplikacji? Jeśli nie, może być problem z połączeń sieciowych na telefonie lub kanału powiadomień z systemu Android i Apple. Pierwsza opcja można rozwiązać w ustawienia telefonu, ale może wymagać komunikował się z dostawcą usług, aby uzyskać pomoc dotyczącą drugiej opcji.
- Możesz otrzymywać powiadomienia dla niektórych kont w aplikacji, ale niekoniecznie? Jeśli tak, Usuń konto powodować problemy z aplikacji, a następnie dodaj go ponownie w celu włączenia powiadomień wypychanych.

Jeśli nastąpiła sugestie dotyczące rozwiązywania, ale nadal występują problemy, Wyślij do nas dzienniki dla diagnostyki. Przejdź do ustawień aplikacji, a następnie wybierz **— Pomoc i opinie** i **wysyłanie dzienników**. Następnie przejdź do [forum aplikacji Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) i Daj nam znać, co problem został wyświetlony i jakie kroki próbowałeś wykonanej do tej pory.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Aplikacja Microsoft Authenticator już jest używany do używania kodów weryfikacyjnych. Jak zmienić powiadomień wypychanych jednym kliknięciem?
Zatwierdzanie logowanie za pomocą powiadomień wypychanych jest dostępna tylko dla osobistego konta Microsoft lub pracy oraz szkolnego konta Microsoft, nie dla kont innych firm, takich jak Google lub usługi Facebook. Jeśli masz służbowe konto Microsoft organizacji można wyłączyć tę opcję.

Korzystanie z konta osobistego konta Microsoft, aby przełączyć się do powiadomień wypychanych należy ponownie dodać konto. Ponowne zarejestrowanie urządzenia przy użyciu konta i Konfigurowanie powiadomień wypychanych.  

Jeżeli używasz Authenticator firmy Microsoft do pracy lub konta służbowego, następnie organizacji decyduje o tym, czy zezwolić na powiadomienia o jednym kliknięciem.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Powiadomienia wypychane jednym kliknięciem działają dla kont innych niż firmy Microsoft?
Nie, powiadomienia wypychane działa tylko z kontami Microsoft i kontami usługi Azure Active Directory. Konto służbowe używa konta usługi Azure AD, może wyłączyć tę funkcję.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Moje urządzenie zostały przywrócone z kopii zapasowej, a kody mojego konta są Brak lub nie działa. Co się stało?
Ze względów bezpieczeństwa firma Microsoft nie należy przywracać kont z kopii zapasowej aplikacji.  Po przywróceniu aplikacji, usuwanie konta i dodać je ponownie.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Otrzymano nowe urządzenie. Jak usunąć aplikację Authenticator firmy Microsoft z urządzeniem stary i Przenieś do nowego?
Dodawanie aplikacji Microsoft Authenticator do nowego urządzenia nie są automatycznie usuwane go od innych urządzeń. Do zarządzania, które urządzenia są skonfigurowane dla Twojego konta, odwiedź tę samą witrynę umożliwia zarządzanie weryfikację dwuetapową, a wybrana opcja usunięcia starego aplikacji.

W przypadku osobistego konta Microsoft, jest tej witryny sieci Web z [zabezpieczenia konta](https://account.microsoft.com/security) strony. Dla konta Microsoft służbowe tej witryny sieci Web mogą być [MyApps](https://myapps.microsoft.com) lub niestandardowych portalu, który skonfigurował Twojej organizacji.

### <a name="how-do-i-remove-an-account-from-the-app"></a>Jak usunąć konto z aplikacji?
* iOS: na ekranie głównym, przejdź w lewo na kafelku konta. Wybierz pozycję **Usuń**.
* Windows Phone: Na ekranie głównym kliknij przycisk menu, następnie **edycji kont**. Wybierz **X** obok nazwy konta.
* System android: Na ekranie głównym kliknij przycisk menu, następnie **edycji kont**. Wybierz **X** obok nazwy konta.

Jeśli masz urządzenie jest zarejestrowane w swojej organizacji, może być konieczne wykonanie dodatkowych czynności można usunąć konta. Na tych urządzeniach aplikacji Microsoft Authenticator jest automatycznie dodawane jako administrator urządzenia. Aby całkowicie odinstalować aplikację, musisz najpierw wyrejestrować aplikacji w ustawieniach aplikacji.

### <a name="why-does-the-app-request-so-many-permissions"></a>Dlaczego aplikacji zażądać uprawnień tyle?
Pełna lista uprawnień, które firma Microsoft może poprosić o i sposób ich użycia w aplikacji. Określone uprawnienia, dostępne są zależne od typu telefonu, do których masz.

* **Aparat fotograficzny**: korzystamy z aparatu do skanowania kodów QR po dodaniu pracy, szkole lub kont innych niż Microsoft.
* **Kontakty i phone**: po zalogowaniu osobiste konto Microsoft spróbujemy w celu uproszczenia procesu przez wyszukiwanie istniejących kont używanych w telefonie.
* **SMS**: po zalogowaniu osobiste konto Microsoft po raz pierwszy, musimy upewnić się, że Twój numer telefonu jest zgodna ze strukturą mamy w rekordzie. Wyślemy wiadomość SMS z numerem telefonu którego pobrano aplikację. Komunikat zawiera kod weryfikacyjny 6-8 cyfr. Zamiast pytaniem, aby znaleźć ten kod i wprowadź go w aplikacji, możemy znaleźć go w wiadomości tekstowej dla Ciebie.
* **Rysowanie w innych aplikacjach**: po otrzymaniu powiadomienia, aby zweryfikować swoją tożsamość, wyświetli powiadomienie za pośrednictwem innych aplikacji, która może być uruchomiona.
* **Odbieranie danych z Internetu**: to uprawnienie jest wymagane do wysyłania powiadomień.
* **Uniemożliwić phone uśpiony**: Jeśli zarejestrować urządzenie w Twojej organizacji, mogą zmieniać tych zasad na Twój telefon.
* **Kontrolowanie wibrację**: można wybrać, czy chcesz wibrację zawsze po otrzymaniu powiadomienia, aby zweryfikować swoją tożsamość.
* **Korzystanie ze sprzętu odcisk palca**: niektóre kont służbowych wymagają dodatkowych numeru PIN przy każdym zweryfikować Twoją tożsamość. Aby ułatwić proces, firma Microsoft będzie można korzystać odcisku palca zamiast wprowadzania numeru PIN.
* **Wyświetlanie połączeń sieciowych**: podczas dodawania konta Microsoft, aplikacja wymaga połączenia sieć i internet.
* **Odczytywać zawartość magazynu**: to uprawnienie jest używana tylko, gdy zgłaszasz problem techniczny za pomocą ustawień aplikacji. Niektóre informacje z magazynu są zbierane zdiagnozować problem.
* **Pełny dostęp do sieci**: to uprawnienie jest wymagane do wysyłania powiadomień do zweryfikowania Twojej tożsamości.
* **Uruchom przy uruchamianiu**: należy ponownie uruchomić telefon, to uprawnienie zadba nadal otrzymywać powiadomienia, aby zweryfikować swoją tożsamość.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Dlaczego aplikację uwierzytelniania firmy Microsoft zezwala na zatwierdzenie żądania bez odblokowywania urządzenia?

Nie masz do odblokowania urządzenia w celu weryfikacji żądań zatwierdzenia, ponieważ jest potrzebne w celu potwierdzenia, czy masz telefon z Tobą. Włączono weryfikację dwuetapową wymaga potwierdzania dwie czynności — element, który znasz i operacją, której masz. Element, który znasz jest Twoje hasło. Operacją, której masz jest Twój telefon (skonfigurować przy użyciu aplikacji Microsoft Authenticator i zarejestrowany jako dowód MFA). W związku z tym o telefonu i zatwierdzania żądania spełnia kryteria dotyczące drugiego etapu uwierzytelniania.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Co oznacza ikonę blokady na liście kont?

Ikona kłódki wskazuje, że urządzenie jest zarejestrowane w usłudze Azure AD i zarejestrowana na koncie. Rejestracja urządzeń dla systemu iOS odbywa się podczas rejestracji Microsoft Intune.

## <a name="next-steps"></a>Następne kroki

### <a name="contact-us"></a>Skontaktuj się z nami
Tutaj nie odpowiedzi na pytanie, chcemy poznać Twoją opinię. Przejdź do [forum aplikacji Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) Zgłoś zapytanie i Uzyskaj pomoc od społeczności lub zostaw komentarz na tej stronie.


### <a name="related-topics"></a>Powiązane tematy
* [O weryfikacji dwuetapowej](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) kont Microsoft
* [Wystąpił problem w trakcie weryfikacji dwuetapowej](multi-factor-authentication-end-user-troubleshoot.md) dla swojego konta firmowego lub szkolnego?
* [Użyj Authenticator firmy Microsoft, aby zalogować się w telefonie](microsoft-authenticator-app-phone-signin-faq.md)
