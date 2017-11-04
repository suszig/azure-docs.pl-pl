---
title: "Rozwiązywanie problemów z: Azure AD SSPR | Dokumentacja firmy Microsoft"
description: "Rozwiązywania problemów z usługą Azure AD samoobsługowego resetowania hasła"
services: active-directory
keywords: 
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
ms.date: 09/21/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 2eeb49cb6ab39c78612e0f1b3e08130ba88cf356
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-troubleshoot-self-service-password-reset"></a>Jak rozwiązywać problemy z samoobsługowego resetowania hasła

Jeśli występują problemy z samoobsługowego resetowania hasła, elementy, które należy wykonać mogą pomóc zadań pracy szybko.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Rozwiązywanie błędów resetowania hasła samoobsługi, które użytkownik może zobaczyć

| Błąd | Szczegóły | Szczegóły techniczne |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Niestety <br> W tej chwili nie można zresetować hasło, ponieważ administrator wyłączył resetowania haseł dla Twojej organizacji. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś go o włączenie tej funkcji. Aby dowiedzieć się więcej, przeczytaj [pomocy, pamiętam hasła usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Wykryto czy resetowania hasła nie został włączony przez administratora. Skontaktuj się z administratorem i poproś go o włączyć resetowania hasła dla Twojej organizacji. |
| WritebackNotEnabled = 10 |Niestety <br> W tej chwili nie można zresetować hasło, ponieważ administrator nie włączył niezbędne service w organizacji. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś o sprawdzenie konfiguracji w organizacji. Aby dowiedzieć się więcej na temat tej usługi wymagane, przeczytaj [Konfigurowanie funkcji zapisywania zwrotnego haseł](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configuring-password-writeback). | SSPR_0010: Wykryto czy nie włączono funkcji zapisywania zwrotnego haseł. Skontaktuj się z administratorem i poproś go o włączenie funkcji zapisywania zwrotnego haseł. |
| SsprNotEnabledInUserPolicy = 11 | Niestety  <br> W tej chwili nie można zresetować hasło, ponieważ administrator nie skonfigurował resetowania haseł dla Twojej organizacji. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś go o skonfigurowanie resetowania hasła. Aby dowiedzieć się więcej o hasło konfiguracji resetowania, przeczytaj artykuł na temat [Szybki Start: usługi Azure AD samoobsługowego resetowania hasła](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Nie zdefiniowano w organizacji zasad resetowania hasła. Skontaktuj się z administratorem i poproś go o zdefiniowanie zasad resetowania hasła. |
| UserNotLicensed = 12 | Niestety <br> W tej chwili nie można zresetować hasło, ponieważ wymaga licencji brakuje z Twojej organizacji. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś go, aby sprawdzić przypisanie licencji. Aby dowiedzieć się więcej na temat licencjonowania, przeczytaj artykuł na temat [zresetować licencjonowania wymagania dotyczące usługi Azure AD samoobsługi hasła](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Organizacji nie ma licencji wymagane konieczne przeprowadzenie resetowania hasła. Skontaktuj się z administratorem i poproś go o Przejrzyj przypisań licencji. |
| UserNotMemberOfScopedAccessGroup = 13 | Niestety <br> W tej chwili nie można zresetować hasło, ponieważ administrator nie skonfigurował konto, aby korzystanie z funkcji resetowania hasła. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś o skonfigurowanie konta do resetowania hasła. Więcej informacji o konfiguracji konta dla resetowania hasła, przeczytaj artykuł na temat [wprowadzana resetowania hasła dla użytkowników](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Nie jesteś członkiem grupy uprawnień do resetowania hasła. Skontaktuj się z Twojego administratora, jak i poproś o dodanie do grupy. |
| UserNotProperlyConfigured = 14 | Niestety <br> W tej chwili nie można zresetować hasło, ponieważ brakuje wymaganych informacji, z Twojego konta. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś o zresetowania hasła. Po ponownym uzyskaniu dostępu do swojego konta, możesz dowiedzieć się jak zarejestrować koniecznych, wykonując kroki opisane w artykule [rejestracji samoobsługowego resetowania hasła](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: Informacje zabezpieczające dodatkowe jest potrzebny do zresetowania hasła. Aby kontynuować, skontaktuj się z administratorem i poproś o zresetowanie hasła. Po uzyskaniu dostępu do konta należy zarejestrować informacje zabezpieczające dodatkowe w https://aka.ms/ssprsetup. Administrator może Dodaj informacje zabezpieczające dodatkowe do swojego konta, wykonując kroki opisane w [zestawu i odczytu uwierzytelniania dane dotyczące resetowania hasła](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Niestety <br> Firma Microsoft nie może zresetować hasło w tej chwili z powodu problemu z konfiguracją resetowania hasła w organizacji. Nie istnieje żadne dalsze akcje, które należy wykonać, aby rozwiązać ten problem. Skontaktuj się z administratorem i poproś go o zbadać. Aby dowiedzieć się więcej na temat potencjalnych problem, przeczytaj artykuł na temat [Rozwiązywanie problemów z zapisywaniem zwrotnym haseł](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Nie możemy zresetować hasło z powodu błędu konfiguracji lokalnej. Skontaktuj się z administratorem i poproś go o zbadać. |
| OnPremisesConnectivityError = 30 | Niestety <br> Firma Microsoft nie może zresetować hasło w tej chwili z powodu problemów z połączeniem z Twojej organizacji. Nie akcji do wykonania od razu, ale problem można rozwiązać, jeśli ponowisz próbę później. Jeśli problem będzie się powtarzać, skontaktuj się z administratorem i poproś go o zbadać. Aby dowiedzieć się więcej na temat problemów z łącznością odczytu [funkcji zapisywania zwrotnego haseł Rozwiązywanie problemów z łącznością](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Firma Microsoft nie może zresetować hasło, z powodu słabej połączenia ze środowiskiem lokalnym. Skontaktuj się z administratorem i poproś go o zbadać.|


## <a name="troubleshoot-password-reset-configuration-in-the-azure-portal"></a>Rozwiązywanie problemów z konfiguracji resetowania hasła w portalu Azure

| **Błąd** | Rozwiązanie |
| --- | --- |
| Nie widzę **resetowania hasła** sekcję usługi Azure AD w portalu Azure | Może to nastąpić, jeśli nie masz usługi Azure AD Premium lub podstawowa licencją administratorowi wykonywanie operacji. <br> Ten problem można rozwiązać przez przypisywanie licencji do konta administratora w danym przy użyciu tego artykułu [przypisać, sprawdź i rozwiąż problemy z licencjami](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Nie widzę opcji określoną konfigurację | Wiele elementów interfejsu użytkownika są ukryte, dopóki nie potrzebne. Spróbuj, włączyć wszystkie opcje, które mają być wyświetlane. |
| Nie widzę **integracji lokalnego** kartę | Ta opcja tylko staje się widoczna, jeśli zostały pobrane Azure AD Connect i skonfigurować funkcję zapisywania zwrotnego haseł. Aby uzyskać więcej informacji na ten temat, zobacz artykuł [wprowadzenie do korzystania z usługi Azure AD Connect przy użyciu ustawień ekspresowych](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Rozwiązywanie problemów z raportowania resetowania hasła

| **Błąd** | Rozwiązanie |
| --- | --- |
| Nie ma żadnych typów działań zarządzania hasła są wyświetlane w **Samoobsługowe zarządzanie hasłami** kategorii zdarzeń inspekcji | Może to nastąpić, jeśli nie masz usługi Azure AD Premium lub podstawowa licencją administratorowi wykonywanie operacji. <br> Ten problem można rozwiązać przez przypisywanie licencji do konta administratora w danym przy użyciu tego artykułu [przypisać, sprawdź i rozwiąż problemy z licencjami] |
| Rejestracje użytkownika Pokaż wiele razy | Obecnie gdy użytkownik rejestruje, obecnie rejestrujemy każdego z nich poszczególnych danych, który jest zarejestrowany jako oddzielne zdarzenie. <br> Jeśli chcesz agregować dane, można pobrać raportu i otworzyć danych, ponieważ w tabeli przestawnej programu excel do uzyskuje się większą elastyczność.

## <a name="troubleshoot-password-reset-registration-portal"></a>Rozwiązywanie problemów z portalu rejestracji resetowania haseł

| **Błąd** | Rozwiązanie |
| --- | --- |
| Katalog nie jest włączona w celu resetowania haseł **administrator nie włączył można użyć tej funkcji** | Przełącznik **samodzielnego resetowania hasła usługi włączone** flaga **grupy** lub **każdy** i kliknij przycisk **Zapisz** |
| Użytkownik nie ma usługi Azure AD Premium lub podstawowa licencją **administrator nie włączył można użyć tej funkcji** | Może to nastąpić, jeśli nie masz usługi Azure AD Premium lub podstawowa licencją administratorowi wykonywanie operacji. <br> Ten problem można rozwiązać przez przypisywanie licencji do konta administratora w danym przy użyciu tego artykułu [przypisać, sprawdź i rozwiąż problemy z licencjami](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Błąd podczas przetwarzania żądania | Może to być spowodowane przez wiele problemów, ale zazwyczaj ten błąd jest spowodowany przez albo problem dotyczący usługi awarii lub konfiguracji. Jeśli zostanie wyświetlony ten błąd jest wywierania wpływu na firmę, skontaktuj się z pomocy technicznej firmy Microsoft, aby uzyskać dodatkową pomoc. |

## <a name="troubleshoot-password-reset-portal"></a>Rozwiązywanie problemów z portalu resetowania haseł

| **Błąd** | Rozwiązanie |
| --- | --- |
| Katalog nie jest włączona w celu resetowania haseł. | Przełącznik **samodzielnego resetowania hasła usługi włączone** flaga **grupy** lub **każdy** i kliknij przycisk **Zapisz** |
| Użytkownik nie ma usługi Azure AD Premium lub podstawowa przypisanej licencji | Może to nastąpić, jeśli nie masz usługi Azure AD Premium lub podstawowa licencją administratorowi wykonywanie operacji. <br> Ten problem można rozwiązać przez przypisywanie licencji do konta administratora w danym przy użyciu tego artykułu [przypisać, sprawdź i rozwiąż problemy z licencjami](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Katalog jest włączona w celu resetowania haseł, ale użytkownik ma brakujące lub nieprawidłowo sformułowany informacje dotyczące uwierzytelniania | Upewnij się, że użytkownik ma prawidłowo sformułowany dane kontaktowe w pliku w katalogu przed kontynuowaniem. Aby uzyskać więcej informacji na ten temat, zobacz artykuł [dane używane przez usługi Azure AD samoobsługowego resetowania hasła](active-directory-passwords-data.md). |
| Katalog jest włączona w celu resetowania haseł, ale użytkownik ma tylko jeden element danych, skontaktuj się z pomocą na plik, gdy zasady są ustawione na wymagają dwa kroki weryfikacji | Upewnij się, które użytkownik ma co najmniej dwóch metod kontaktu poprawnie skonfigurowana (przykład: telefon komórkowy **i** telefon biurowy) przed kontynuowaniem. |
| Katalog jest włączona do resetowania hasła i użytkownika jest skonfigurowany prawidłowo, ale użytkownik nie będzie mógł można skontaktować się z | Może być spowodowany przez błąd tymczasowy usługi lub nieprawidłowe dane kontaktowe, który firma Microsoft nie może wykryć prawidłowo. <br> <br> Jeśli użytkownik czeka 10 sekund i spróbuj ponownie, a następnie zostanie wyświetlony link "Skontaktuj się z administratorem". Klikając przycisk Spróbuj ponownie ponawia próbę wywołania, konieczne kliknięcie przycisku "Skontaktuj się z administratorem" wysyła wiadomość e-mail z formularza do administratorów żądania resetowania hasła, aby przeprowadzić dla tego konta użytkownika. |
| Użytkownik nie otrzymuje resetowania hasła programu SMS lub połączeń telefonicznych | Może to być wynikiem numeru telefonu nieprawidłowo sformułowany w katalogu. Upewnij się, że numer telefonu jest w formacie "+ ccc xxxyyyzzzzXeeee". <br> <br> Resetowanie hasła nie obsługuje rozszerzeń, nawet jeśli określono w katalogu (one są usuwane wywołanie jest wysyłane). Spróbuj użyć numeru bez rozszerzenia, lub zintegrować z rozszerzeniem numeru telefonu w PBX użytkownika. |
| Użytkownik nie otrzymuje wiadomość e-mail resetowania hasła | Najczęstszą przyczyną tego problemu jest to, że komunikat zostanie odrzucony przez filtr spamu. Sprawdź Twojej wiadomości-śmieci, folder wiadomości-śmieci lub usuniętych elementów dla wiadomości e-mail. <br> Upewnij się również sprawdzasz prawo poczty e-mail dla wiadomości. |
| Ustawię zasady resetowania hasła, ale podczas resetowania hasła korzysta z konta administratora, że zasady nie są stosowane | Firma Microsoft zarządza i formanty zresetować hasła administratora zasad w celu zapewnienia najwyższego poziomu zabezpieczeń. |
| Uniemożliwił próby resetowania zbyt wiele razy w ciągu dnia hasła użytkownika | Aby uniemożliwić użytkownikom wprowadzania mechanizm ograniczania przepustowości automatycznego z próba zresetowania hasła zbyt wiele razy w krótkim czasie. W takim przypadku: <br> 1. Użytkownik próbuje zweryfikować numeru telefonu 5 razy w ciągu godziny. <br> 2. Użytkownik próbuje użyć bramy pytania zabezpieczeń 5 razy w ciągu godziny. <br> 3. Użytkownik próbuje zresetować hasło dla tego samego konta użytkownika 5 razy w ciągu godziny. <br> Aby rozwiązać ten problem, poinstruuj użytkowników oczekiwania po 24 godzinach od ostatniej próby, a użytkownik będzie mógł zresetować swoje hasło. |
| Użytkownik zobaczy następujący błąd podczas sprawdzania poprawności numeru telefonu | Ten błąd występuje, gdy podany numer telefonu jest niezgodny z numerem telefonu w pliku. Upewnij się, że użytkownik jest wprowadzenie pełnego numeru telefonu, kodu obszaru i kraju, w tym podczas próby użycia metody telefoniczną w celu resetowania haseł. |
| Błąd podczas przetwarzania żądania | Może to być spowodowane przez wiele problemów, ale zazwyczaj ten błąd jest spowodowany przez albo problem dotyczący usługi awarii lub konfiguracji. Jeśli zostanie wyświetlony ten błąd jest wywierania wpływu na firmę, skontaktuj się z pomocy technicznej firmy Microsoft, aby uzyskać dodatkową pomoc. |

## <a name="troubleshoot-password-writeback"></a>Rozwiązywanie problemów z zapisywaniem zwrotnym haseł

| **Błąd** | Rozwiązanie |
| --- | --- |
| Usługa resetowania hasła nie zostanie uruchomiona lokalnie z powodu błędu 6800 w dzienniku zdarzeń aplikacji na komputerze Azure AD Connect. <br> <br> Po synchronizacji dołączania federacyjnych lub skrótu hasła użytkowników nie można zresetować hasła. | Po włączeniu funkcji zapisywania zwrotnego haseł, aparat synchronizacji wywołuje biblioteki zapisywania zwrotnego w celu przeprowadzenia konfiguracji (dołączania) przez mówić do dołączenia do usługi w chmurze. Błędów napotkanych podczas dołączania lub podczas uruchamiania punktu końcowego WCF dla funkcji zapisywania zwrotnego haseł powoduje błędy w dzienniku zdarzeń w dzienniku zdarzeń na komputerze Azure AD Connect. <br> <br> Podczas ponownego uruchamiania usługi ADSync Jeśli skonfigurowano zapisywanie zwrotne, punktu końcowego WCF jest uruchomiony. Jednak jeśli uruchomienia punktu końcowego nie powiedzie się, firma Microsoft będzie rejestrowane zdarzenie 6800 i let typ uruchamiania usługi synchronizacji. Obecność to zdarzenie oznacza, że punkt końcowy nie rozpoczął się funkcji zapisywania zwrotnego haseł. Szczegóły dziennika zdarzeń dla tego zdarzenia (6800) oraz wpisy w dzienniku zdarzeń generowanie przez PasswordResetService składnika wskazuje, dlaczego punktu końcowego nie może być uruchamiana. Przejrzyj te błędy dziennika zdarzeń i spróbuj ponownie uruchomić usługę Azure AD Connect, jeśli funkcji zapisywania zwrotnego haseł nie działa. Jeśli problem będzie się powtarzał, spróbuj wyłączyć i ponownie włączyć funkcję zapisywania zwrotnego haseł.
| Gdy użytkownik próbuje zresetować hasło lub odblokować konto z włączoną zapisywania zwrotnego haseł, kończy się niepowodzeniem. <br> <br> Ponadto Zobacz zdarzenia w zawierającego dziennik zdarzeń usługi Azure AD Connect: "aparatu synchronizacji zwrócił błąd hr = 800700CE, wiadomość = nazwa pliku lub rozszerzenie jest zbyt długa" po zakończeniu operacji odblokowania. | Znajdź konto usługi Active Directory programu Azure AD Connect i zresetuj hasło, które ma zawierać nie więcej niż 127 znaków. Następnie otwórz usługę synchronizacji z Start menu. Przejdź do łączników i znajdź łącznik usługi Active Directory. Zaznacz go i kliknij polecenie Właściwości. Przejdź do strony poświadczeń i wprowadź nowe hasło. Wybierz przycisk OK, aby zamknąć stronę. |
| W ostatnim kroku procesu instalacji usługi Azure AD Connect zostanie wyświetlony komunikat o błędzie z informacją o tym, że nie można skonfigurować zapisywania zwrotnego haseł. <br> <br> Dziennik zdarzeń aplikacji usługi Azure AD Connect zawiera błąd 32009 z tekstem "Błąd podczas pobierania uwierzytelniania tokenu". | Ten błąd występuje w dwóch następujących przypadków:<br> <br> a. Określono nieprawidłowe hasło dla konta administratora globalnego określony na początku procesu instalacji usługi Azure AD Connect.<br> b. Podjęto próbę użycia federacyjnych użytkownika dla konta administratora globalnego określony na początku procesu instalacji usługi Azure AD Connect.<br> <br> Aby naprawić ten błąd, upewnij się, że nie jesteś federacyjnych konta administratora globalnego określony na początku procesu instalacji usługi Azure AD Connect i czy podane hasło jest prawidłowe. |
| Dziennik zdarzeń programu Azure AD Connect maszyny zawiera błąd zgłoszony przez PasswordResetService 32002. <br> <br> Odczytuje kod błędu: "błąd podczas nawiązywania połączenia magistrali usług, Dostawca tokenu nie była w stanie tokenu zabezpieczającego..." | W lokalnym środowisku nie będzie mógł nawiązać połączenia z punktem końcowym magistrali usługi w chmurze. Przyczyną tego błędu jest zwykle reguła zapory blokuje połączenia wychodzące z określonego adresu portu lub sieci web. Zobacz [wymagania wstępne dotyczące łączności](./connect/active-directory-aadconnect-prerequisites.md) Aby uzyskać więcej informacji. Po zaktualizowaniu tych reguł ponownego rozruchu komputera Azure AD Connect i zapisywania zwrotnego haseł należy uruchomić ponownie. |
| Po synchronizacji robocze dla niektórych czasu federacyjnych lub skrótu hasła użytkowników nie można zresetować hasła. | W rzadkich przypadkach funkcji zapisywania zwrotnego haseł usługi może się nie powieść Uruchom ponownie po uruchomieniu usługi Azure AD Connect. W takich przypadkach najpierw upewnij się, czy zapisywanie zwrotne haseł wydaje się być włączone na lokalnej Można to zrobić za pomocą Kreatora Azure AD Connect lub środowiska powershell (sekcja HowTos zobacz powyżej). Jeśli funkcja wydaje się być włączone, spróbuj Włączanie lub wyłączanie funkcji ponownie przy użyciu interfejsu użytkownika lub środowiska PowerShell. Jeśli to nie zadziała, spróbuj całkowicie odinstalować i ponownie zainstalować Azure AD Connect. |
| Federacyjnych lub hasło skrótu synchronizacji użytkowników, którzy próba zresetowania hasła wyświetlony błąd po przesłaniu hasła wskazujący, że wystąpił problem z usługi. <br ><br> Oprócz tego podczas operacji resetowania hasła, mogą pojawić się, że błąd dotyczący agenta zarządzania odmówiono dostępu w dziennikach zdarzeń na lokalnym. | Jeśli te błędy w dzienniku zdarzeń, upewnij się, że konto agenta zarządzania usługą AD (który został określony w Kreatorze podczas konfiguracji) ma niezbędne uprawnienia do zapisywania zwrotnego haseł. <br> <br> **Gdy podano to uprawnienie, może potrwać maksymalnie 1 godzina uprawnienia do ścieknie w dół za pomocą zadania w tle sdprop na kontrolerze domeny.** <br> <br> Do resetowania hasła, aby pracować uprawnienie musi widnieć deskryptora zabezpieczeń obiektu użytkownika, resetowanie haseł, których. Dopóki te uprawnienia są wyświetlane na obiekt użytkownika, resetowanie hasła zakończy się niepowodzeniem z powodu odmowy dostępu. |
| Federacyjnych lub hasło skrótu synchronizacji użytkowników, którzy próba zresetowania hasła wyświetlony błąd po przesłaniu hasła wskazujący, że wystąpił problem z usługi. <br> <br> Oprócz tego podczas operacji resetowania hasła, może zostać wyświetlony błąd w dziennikach zdarzeń z usługi Azure AD Connect, co wskazuje na błąd "Nie można odnaleźć obiektu". | Błąd ten zwykle wskazuje, że aparat synchronizacji jest nie można odnaleźć obiektu użytkownika w przestrzeni łącznika AAD lub połączonych MV lub obiektu przestrzeni łącznika usługi Active Directory. <br> <br> Aby rozwiązać ten problem, upewnij się, że użytkownik faktycznie jest zsynchronizowany ze środowiska lokalnego do usługi AAD przez bieżące wystąpienie programu Azure AD Connect i sprawdzić stan obiektów — obszary łączników i MV. Potwierdź, że obiekt usług AD CS jest łącznika do obiektu MV za pomocą reguły "Microsoft.InfromADUserAccountEnabled.xxx".|
| Federacyjnych lub hasło skrótu synchronizacji użytkowników, którzy próba zresetowania hasła wyświetlony błąd po przesłaniu hasła wskazujący, że wystąpił problem z usługi. <br> <br> Oprócz tego podczas operacji resetowania hasła, może zostać wyświetlony błąd w dziennikach zdarzeń z usługi Azure AD Connect wskazujący błąd "Znaleziono wiele dopasowań". | Oznacza to, że aparat synchronizacji wykrył, że obiektu MV jest połączony z więcej niż jeden obiektów usług AD CS za pomocą "Microsoft.InfromADUserAccountEnabled.xxx". Oznacza to, że użytkownik ma włączone konto w więcej niż jednym lesie. **Ten scenariusz nie jest obsługiwany dla funkcji zapisywania zwrotnego haseł.** |
| Operacje na hasłach zakończyć się niepowodzeniem z powodu błędu konfiguracji. Zawiera dziennik zdarzeń aplikacji <br> <br> Azure AD Connect błąd 6329 tekstem: 0x8023061f (operacja nie powiodła się, ponieważ synchronizacja hasła nie jest włączona dla tego agenta zarządzania.) | W takim przypadku konfiguracji usługi Azure AD Connect można dodać nowego lasu usługi AD (lub usuń i readd istniejącego lasu) po funkcji zapisywania zwrotnego haseł została już włączona. Operacje na hasłach dla użytkowników w takich nowo dodanych lasów kończyć się niepowodzeniem. Aby rozwiązać ten problem, wyłącz i ponownie włączyć funkcję zapisywania zwrotnego haseł, po zakończeniu zmiany konfiguracji lasu. |

## <a name="password-writeback-event-log-error-codes"></a>Kody błędów dziennika zdarzeń funkcji zapisywania zwrotnego haseł

Najlepszym rozwiązaniem w przypadku problemów z zapisywaniem zwrotnym haseł do sprawdzenia tego dziennika zdarzeń aplikacji na tym komputerze Azure AD Connect. Ten dziennik zdarzeń zawiera zdarzenia z dwóch źródeł odsetek dla funkcji zapisywania zwrotnego haseł. Źródło PasswordResetService opis operacji i problemy związane z operacji zapisywania zwrotnego haseł. Źródła ADSync opis operacji i problemy związane z ustawieniem hasła w środowisku usługi AD.

### <a name="source-of-event-is-adsync"></a>Źródło zdarzenia jest ADSync

| Kod | Nazwa/wiadomości | Opis |
| --- | --- | --- |
| 6329 | PORĘCZENIE: MMS(4924) 0x80230619 — "ograniczenie zabezpiecza hasła przed zmianami do bieżącej określony". | To zdarzenie występuje, gdy usługa zapisywania zwrotnego haseł próbuje ustawić hasło dla użytkownika katalogu lokalnego, który nie spełnia okres ważności hasła, historii, złożoności lub filtrowania wymagania domeny. <br> <br> Jeśli ma minimalny okres ważności hasła i niedawno zmieniono hasło w danym przedziale czasu nie jest możliwe do zmiany hasła ponownie, dopóki nie osiągnie określony wiek w domenie. Do celów testowych, minimalnym wieku powinna być równa 0. <br> <br> Jeśli zostały włączone wymagania historii haseł, a następnie należy wybrać hasło, które nie było używane w ostatnim czasie N, gdzie N jest ustawienie historii haseł. Jeśli wybierzesz hasła, który został użyty w N czas, następnie zostanie wyświetlony błąd w tym przypadku. Do celów testowych, Historia powinna być równa 0. <br> <br> Jeśli masz wymagania dotyczące złożoności hasła wszystkich z nich są wymuszane podczas próby zmiany lub resetowania hasła. <br> <br> Jeśli mają włączone filtry hasła, a użytkownik wybierze hasło nie spełnia kryteriów filtrowania, a następnie resetowania lub zmiany operacja zakończy się niepowodzeniem. |
| 6329 | MMS(3040): admaexport.cpp(2837): serwer nie zawiera formantu zasad haseł LDAP. | Ten problem występuje, gdy formant LDAP_SERVER_POLICY_HINTS_OID (1.2.840.113556.1.4.2066) nie jest włączony na kontrolerach domeny. Aby użyć funkcji zapisywania zwrotnego haseł, należy włączyć formantu. Aby to zrobić, kontrolery domeny musi być w systemie Windows Server 2008 (z najnowszą SP) lub nowszym. Jeśli Twoje kontrolery domeny są 2008 (pre-R2), a następnie należy również zastosować poprawkę [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Instalator aparatu synchronizacji zwrócił błąd hr = 80230402, wiadomość = próba pobrania obiektu nie powiodło się, ponieważ istnieją zduplikowane wpisy z tej samej kotwicy | To zdarzenie występuje, gdy ten sam identyfikator użytkownika jest włączona w wielu domenach. Na przykład jeśli trwa synchronizacja lasów kont/zasobów i mają ten sam identyfikator użytkownika obecne i włączona w każdym, ten błąd może wystąpić. <br> <br> Ten błąd może również wystąpić, jeśli korzystasz z atrybutem zakotwiczenia nieunikatowy (na przykład alias lub nazwy UPN) i dwa użytkownicy wspólnie korzystają z tego samego atrybutu zakotwiczenia. <br> <br> Aby rozwiązać ten problem, upewnij się, że nie ma żadnych zduplikowanych użytkowników w obrębie domeny i korzystasz z atrybutem zakotwiczenia unikatowy dla każdego użytkownika. |

### <a name="source-of-event-is-passwordresetservice"></a>Źródło zdarzenia jest PasswordResetService

| Kod | Nazwa/wiadomości | Opis |
| --- | --- | --- |
| 31001 | PasswordResetStart | To zdarzenie oznacza, że Usługa lokalna wykrył, że żądania dla federacyjnych lub hasło użytkownika skrótu synchronizowane pochodzących z chmury resetowania hasła. To zdarzenie jest pierwsze zdarzenie każdego hasła resetowania operacji zapisywania zwrotnego. |
| 31002 | PasswordResetSuccess | To zdarzenie oznacza, że użytkownik wybrać nowe hasło podczas operacji resetowania hasła, Ustaliliśmy, że to hasło spełnia wymagania dotyczące haseł firmy, a hasło został zapisany pomyślnie ponownie w środowisku lokalnym AD. |
| 31003 | PasswordResetFail | To zdarzenie oznacza, że użytkownik wybrane hasło, a hasło pomyślnie dotarły do środowiska lokalnego, że wystąpił błąd podczas próby ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów: <br> <br> Hasło użytkownika nie spełniają wieku, Historia, złożoności lub filtrowanie wymagań dla danej domeny. Spróbuj całkowicie nowe hasło, aby rozwiązać ten problem. <br> <br> Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika. <br> <br> Konto użytkownika znajduje się w grupie ochrony, takich jak Administratorzy domeny lub przedsiębiorstwa, który nie zezwala na operacje na zestawie hasła. |
| 31004 | OnboardingEventStart | To zdarzenie występuje, gdy włączysz zapisywanie zwrotne haseł z usługi Azure AD Connect i wskazuje, że firma Microsoft uruchamiana dołączania organizacji z usługą sieci web funkcji zapisywania zwrotnego haseł. |
| 31005 | OnboardingEventSuccess | To zdarzenie oznacza procesu dołączania zakończyła się pomyślnie i czy funkcja zapisywania zwrotnego haseł jest gotowe do użycia. |
| 31006 | ChangePasswordStart | To zdarzenie wskazuje, że Usługa lokalna wykryto żądanie zmiany hasła dla federacyjnych lub hasło użytkownika skrótu synchronizowane pochodzących z chmury. To zdarzenie jest pierwsze zdarzenie w każdej operacji zapisywania zwrotnego zmiany hasła. |
| 31007 | ChangePasswordSuccess | To zdarzenie oznacza, że użytkownik wybrać nowe hasło podczas operacji zmiany hasła, Ustaliliśmy, że to hasło spełnia wymagania dotyczące haseł firmy, a hasło został zapisany pomyślnie ponownie w środowisku lokalnym AD. |
| 31008 | ChangePasswordFail | To zdarzenie oznacza, że użytkownik wybrane hasło, a hasło pomyślnie dotarły do środowiska lokalnego, że wystąpił błąd podczas próby ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów: <br> <br> Hasło użytkownika nie spełniają wieku, Historia, złożoności lub filtrowanie wymagań dla danej domeny. Spróbuj całkowicie nowe hasło, aby rozwiązać ten problem. <br> <br> Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika. <br> <br> Konto użytkownika znajduje się w grupie ochrony, takich jak Administratorzy domeny lub przedsiębiorstwa, który nie zezwala na operacje na zestawie hasła. |
| 31009 | ResetUserPasswordByAdminStart | Usługa lokalna wykryto federacyjnej żądania resetowania hasła lub skrótu hasła zsynchronizowane użytkownika pochodzące od administratora w imieniu użytkownika. To zdarzenie jest pierwsze zdarzenie w każdej operacji zapisywania zwrotnego resetowania hasła inicjowanych przez administratora. |
| 31010 | ResetUserPasswordByAdminSuccess | Administrator wybrać nowe hasło podczas operacji resetowania hasła inicjowanych przez administratora, Ustaliliśmy, że to hasło spełnia wymagania dotyczące haseł firmy, a hasło został zapisany pomyślnie ponownie w środowisku lokalnym AD. |
| 31011 | ResetUserPasswordByAdminFail | Administrator wybrane hasło w imieniu użytkownika i że hasło pomyślnie dotarły do środowiska lokalnego, ale wystąpił błąd podczas próby ustawić hasło w środowisku lokalnym AD. Może się to zdarzyć z kilku powodów: <br> <br> Hasło użytkownika nie spełniają wieku, Historia, złożoności lub filtrowanie wymagań dla danej domeny. Spróbuj całkowicie nowe hasło, aby rozwiązać ten problem. <br> <br> Konto usługi MA nie ma odpowiednich uprawnień, aby ustawić nowe hasło dla danego konta użytkownika. <br> <br> Konto użytkownika znajduje się w grupie ochrony, takich jak Administratorzy domeny lub przedsiębiorstwa, który nie zezwala na operacje na zestawie hasła. |
| 31012 | OffboardingEventStart | To zdarzenie występuje, gdy należy wyłączyć funkcję zapisywania zwrotnego haseł z programem Azure AD Connect i wskazuje, że firma Microsoft uruchamiana zaawansowanej organizacji z usługą sieci web funkcji zapisywania zwrotnego haseł. |
| 31013| OffboardingEventSuccess| To zdarzenie oznacza zaawansowanej powiedzie się i został pomyślnie wyłączony możliwości zapisywania zwrotnego haseł. |
| 31014| OffboardingEventFail| To zdarzenie oznacza, że proces zaawansowanej zakończyła się niepowodzeniem. Może to być spowodowane błędem uprawnienia w chmurze lub lokalne konto administratora określone podczas konfigurowania lub ponieważ próbujesz użyć administratora globalnego federacyjnych chmury po wyłączeniu funkcji zapisywania zwrotnego haseł. Aby rozwiązać ten problem, sprawdź użytkownika administracyjnego uprawnienia i że nie używasz dowolnej federacyjnych konta podczas konfigurowania możliwości zapisywania zwrotnego haseł.|
| 31015| WriteBackServiceStarted| To zdarzenie wskazuje, że usługa zapisywania zwrotnego haseł została pomyślnie uruchomiona i jest gotowy do akceptowania żądań zarządzania hasła z chmury.|
| 31016| WriteBackServiceStopped| To zdarzenie oznacza, czy usługa zapisywania zwrotnego haseł przestała i żądań zarządzania haseł w chmurze nie zostanie zrealizowane.|
| 31017| AuthTokenSuccess| To zdarzenie oznacza, możemy pomyślnie pobrać token autoryzacji dla administratora globalnego określony podczas instalacji usługi Azure AD Connect, aby rozpocząć proces zaawansowanej lub dołączania.|
| 31018| KeyPairCreationSuccess| To zdarzenie oznacza, że utworzyliśmy pomyślnie hasło klucza szyfrowania używanego do szyfrowania haseł z chmury do wysłania do środowiska lokalnego.|
| 32000| Nieznany| To zdarzenie oznacza wystąpił nieznany błąd podczas operacji zarządzania hasła. Spójrz na tekst wyjątku w zdarzeniu, aby uzyskać więcej informacji. Jeśli masz problemy, spróbuj wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł. Jeśli to nie pomaga, dołączenie kopii dziennika zdarzeń oraz podany identyfikator śledzenia wewnętrznymi do pracownika pomocy technicznej.|
| 32001| ServiceError| To zdarzenie wskazuje wystąpił błąd podczas łączenia z hasłem chmury resetowania usługi i zwykle występuje, gdy Usługa lokalna nie może połączyć się z usługą sieci web resetowania hasła.|
| 32002| ServiceBusError| To zdarzenie oznacza, wystąpił błąd podczas nawiązywania połączenia z Twojej dzierżawy usługi magistrali wystąpienia. Może się to zdarzyć, ponieważ są blokuje połączenia wychodzące w środowisku lokalnym. Sprawdź zaporę tak, aby zapewnić połączeń za pośrednictwem protokołu TCP 443 i https://ssprsbprodncu-sb.accesscontrol.windows.net/ i spróbuj ponownie. Jeśli nadal występują problemy, spróbuj wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł.|
| 32003| InPutValidationError| To zdarzenie oznacza, że dane wejściowe przekazane do naszej interfejsu API usługi sieci web była nieprawidłowa. Spróbuj ponownie wykonać operację.|
| 32004| DecryptionError| To zdarzenie oznacza, że wystąpił błąd podczas odszyfrowywania hasła, które dotarły z chmury. Może to być spowodowane odszyfrowywania klucza niezgodność usługi w chmurze oraz w lokalnym środowisku. Aby rozwiązać ten problem, wyłącz i ponownie włączyć funkcję zapisywania zwrotnego haseł w środowisku lokalnym.|
| 32005| ConfigurationError| Podczas dołączania możemy zapisać informacje specyficzne dla dzierżawy w pliku konfiguracji w środowisku lokalnym. To zdarzenie oznacza wystąpił błąd podczas zapisywania tego pliku lub że podczas uruchomienia usługi został błąd odczytu jego pliku. Aby rozwiązać ten problem, spróbuj wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł wymuszenie ponownego pisania tego pliku konfiguracji.|
| 32007| OnBoardingConfigUpdateError| Podczas dołączania możemy wysyłać dane z chmury do lokalnego hasła resetowania usługi. Czy dane są następnie zapisywane do pliku w pamięci przed wysłaniem do usługi synchronizacji Zapisz te informacje w bezpiecznej lokalizacji na dysku. To zdarzenie oznacza problem z zapisywania lub aktualizowania danych w pamięci. Aby rozwiązać ten problem, spróbuj wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł wymuszenie ponownego zapisywania w tej konfiguracji.|
| 32008| ValidationError| To zdarzenie oznacza, że odebrano nieprawidłową odpowiedź z usługi sieci web resetowania hasła. Aby rozwiązać ten problem, spróbuj wyłączenie i ponowne włączanie zapisywania zwrotnego haseł.|
| 32009| AuthTokenError| To zdarzenie oznacza, że firma Microsoft nie można pobrać autoryzacji tokenu dla konta administratora globalnego określone podczas konfigurowania usługi Azure AD Connect. Przyczyną tego błędu może być nieprawidłowa nazwa użytkownika lub hasło podane dla konta administratora globalnego lub ponieważ określone konto administratora globalnego jest federacyjna. Aby rozwiązać ten problem, uruchom ponownie konfigurację z prawidłową nazwę użytkownika i hasło i upewnij się, że administrator konto zarządzane (tylko w chmurze lub synchronizacji haseł).|
| 32010| CryptoError| To zdarzenie oznacza wystąpił błąd podczas generowania hasło klucza szyfrowania lub odszyfrowywania hasła przychodzący z usługi w chmurze. Ten błąd prawdopodobnie wskazuje problem ze środowiskiem. Przeglądania informacji z dziennika zdarzeń, aby dowiedzieć się więcej i rozwiązać ten problem. Możesz również spróbować wyłączenie i ponowne włączenie usługi funkcji zapisywania zwrotnego haseł, aby rozwiązać ten problem.|
| 32011| OnBoardingServiceError| To zdarzenie oznacza, że Usługa lokalna nie mógł prawidłowo skomunikować się z usługą sieci web resetowania hasła do zainicjowania procesu dołączania. Może to być spowodowane reguły zapory lub wystąpił problem podczas pobierania tokenu uwierzytelniania dla dzierżawy. Aby rozwiązać ten problem, upewnij się, że są nie blokuje połączenia wychodzące przez TCP 443 i TCP 9350-9354 lub https://ssprsbprodncu-sb.accesscontrol.windows.net/, a nie jest zintegrowany AAD konto administratora używane do dołączenia.|
| 32013| OffBoardingError| To zdarzenie oznacza, że Usługa lokalna nie mógł prawidłowo skomunikować się z usługą sieci web resetowania hasła, aby zainicjować proces zaawansowanej. Może to być spowodowane reguły zapory lub wystąpił problem podczas pobierania tokenu autoryzacji dla dzierżawy. Aby rozwiązać ten problem, upewnij się, czy nie blokuje połączenia wychodzące za pośrednictwem 443 lub https://ssprsbprodncu-sb.accesscontrol.windows.net/, czy konto administratora usługi AAD używanym do offboard nie jest zintegrowany.|
| 32014| ServiceBusWarning| To zdarzenie oznacza, Musieliśmy spróbuj ponownie nawiązać połączenia z Twojej dzierżawy usługi magistrali wystąpienia. W normalnych warunkach to nie powinny być problemem, lecz jeśli widzisz to zdarzenie wiele razy, należy wziąć pod uwagę sprawdzić połączenie sieciowe z magistralą usług, zwłaszcza, jeśli jest to duże opóźnienie lub połączenie o niskiej przepustowości.|
| 32015| ReportServiceHealthError| W celu monitorowania kondycji usługi funkcji zapisywania zwrotnego haseł, możemy wysyłać danych pulsu do naszej hasła Zresetuj usługę sieci web co 5 minut. To zdarzenie oznacza, że wystąpił błąd podczas wysyłania informacji o tym kondycji usługi sieci web w chmurze. Te informacje o kondycji nie zawiera danych OII lub dane osobowe i jest całkowicie pulsu i statystyki podstawowe usługi, aby firma Microsoft może udostępnić informacje o stanie usługi w chmurze.|
| 33001| ADUnKnownError| To zdarzenie oznacza, że wystąpił nieznany błąd zwrócony przez usługi AD, Sprawdź zdarzenia ze źródła ADSync, aby uzyskać więcej informacji o tym błędzie w dzienniku zdarzeń serwera Azure AD Connect.|
| 33002| ADUserNotFoundError| To zdarzenie oznacza, że użytkownik, który próbuje Resetowanie lub zmienianie hasła nie odnaleziono w katalogu lokalnym. Ten błąd może wystąpić, gdy użytkownik został usunięty lokalnymi, ale nie w chmurze, lub jeśli wystąpi problem z synchronizacją. Sprawdź dzienniki synchronizacji, a Ostatnia synchronizacja kilka Uruchom szczegóły, aby uzyskać więcej informacji.|
| 33003| ADMutliMatchError| Podczas resetowania hasła lub żądania zmiany pochodzi z chmury, używamy zakotwiczenia chmury określony podczas instalacji programu Azure AD Connect do określania, jak połączyć tego żądania użytkownika w środowisku lokalnym. To zdarzenie oznacza, że znaleziono dwóch użytkowników w katalogu lokalnego z tego samego atrybutu zakotwiczenia chmury. Sprawdź dzienniki synchronizacji, a Ostatnia synchronizacja kilka Uruchom szczegóły, aby uzyskać więcej informacji.|
| 33004| ADPermissionsError| To zdarzenie oznacza, że konto usługi agenta zarządzania nie ma odpowiednie uprawnienia ustawione nowe hasło danego konta. Upewnij się, że konto MA w lesie użytkownika uprawnieniami resetowania i zmiany hasła wszystkich obiektów w lesie. Aby uzyskać więcej informacji na temat tej, zobacz krok 4: Konfigurowanie odpowiednich uprawnień usługi Active Directory.|
| 33005| ADUserAccountDisabled| To zdarzenie oznacza, że próby Resetowanie lub zmienianie hasła dla konta, które zostało wyłączone lokalnie. Włącz konto i spróbuj ponownie wykonać operację.|
| 33006| ADUserAccountLockedOut| Zdarzenie oznacza, że próby Resetowanie lub zmienianie hasła dla konta, które zostało zablokowane lokalnie. Blokady może wystąpić, gdy użytkownik ma nastąpiła zmiana lub zresetować operacji hasła zbyt wiele razy w krótkim czasie. Odblokowanie konta i spróbuj ponownie wykonać operację.|
| 33007| ADUserIncorrectPassword| To zdarzenie wskazuje, czy określona przez użytkownika niepoprawnego bieżącego hasła podczas wykonywania hasło zmienić operację. Określ poprawny bieżące hasło i spróbuj ponownie.|
| 33008| ADPasswordPolicyError| To zdarzenie występuje, gdy usługa zapisywania zwrotnego haseł próbuje ustawić hasło dla użytkownika katalogu lokalnego, który nie spełnia okres ważności hasła, historii, złożoności lub filtrowania wymagania domeny. <br> <br> Jeśli ma minimalny okres ważności hasła i niedawno zmieniono hasło w danym przedziale czasu nie jest możliwe do zmiany hasła ponownie, dopóki nie osiągnie określony wiek w domenie. Do celów testowych, minimalnym wieku powinna być równa 0. <br> <br> Jeśli zostały włączone wymagania historii haseł, a następnie należy wybrać hasło, które nie było używane w ostatnim czasie N, gdzie N jest ustawienie historii haseł. Jeśli wybierzesz hasła, który został użyty w N czas, następnie zostanie wyświetlony błąd w tym przypadku. Do celów testowych, Historia powinna być równa 0. <br> <br> Jeśli masz wymagania dotyczące złożoności hasła wszystkich z nich są wymuszane podczas próby zmiany lub resetowania hasła. <br> <br> Jeśli mają włączone filtry hasła, a użytkownik wybierze hasło nie spełnia kryteriów filtrowania, a następnie resetowania lub zmiany operacja zakończy się niepowodzeniem.|
| 33009| ADConfigurationError| To zdarzenie oznacza, że wystąpił pisania problem hasła z powrotem do katalogu lokalnego z usługą Active Directory ze względu na problem z konfiguracją. Sprawdź dziennik zdarzeń aplikacji na komputerze Azure AD Connect komunikatów z usługi ADSync, aby uzyskać więcej informacji, w jaki błąd wystąpił.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Rozwiązywanie problemów z łącznością funkcji zapisywania zwrotnego haseł

Jeśli występują przerw w działaniu usługi za pomocą składnika zapisywania zwrotnego haseł programu Azure AD Connect, Oto niektóre Szybkie kroki, które należy wykonać w celu rozwiązania tego problemu:

* [Upewnij się, połączenie sieciowe](#confirm-network-connectivity)
* [Ponowne uruchomienie programu Azure AD Connect usługi synchronizacji](#restart-the-azure-ad-connect-sync-service)
* [Wyłącz i ponownie włączyć funkcję zapisywania zwrotnego haseł](#disable-and-re-enable-the-password-writeback-feature)
* [Zainstalowanie najnowszej wersji Azure AD Connect](#install-the-latest-azure-ad-connect-release)
* [Rozwiązywanie problemów z zapisywaniem zwrotnym haseł](#troubleshoot-password-writeback)

Ogólnie rzecz biorąc zaleca się, wykonaj następujące kroki w kolejności powyżej odzyskać usługi najszybszych sposób.

### <a name="confirm-network-connectivity"></a>Upewnij się, połączenie sieciowe

I czy Zapora jest najbardziej typowych punktu awarii lub porty serwera proxy i limitów czasu bezczynności są nieprawidłowo skonfigurowane. Przeglądanie wymagań wstępnych łączność w artykule [wymagania wstępne dotyczące usługi Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) Aby uzyskać więcej informacji.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Ponowne uruchomienie programu Azure AD Connect usługi synchronizacji

Ponowne uruchamianie usługi Azure AD Connect usługę synchronizacji może pomóc rozwiązać problemy z łącznością lub innych przejściowych problemów z usługą.

1. Jako administrator, kliknij przycisk **Start** na serwerze z uruchomioną **Azure AD Connect**.
2. Typ **"services.msc"** w polu wyszukiwania i naciśnij klawisz **Enter**.
3. Wyszukaj **Microsoft Azure AD Sync** wpisu.
4. Kliknij prawym przyciskiem myszy wpis usługi, kliknij przycisk **ponowne uruchomienie**i poczekaj na ukończenie tej operacji.

   ![Uruchom ponownie usługę Azure AD Sync][Service Restart]

Te kroki ponownie ustanowić połączenia z usługą w chmurze i rozwiązać wszelkie przestoje, które mogą występować. Jeśli ponowne uruchomienie usługi synchronizacji nie rozwiąże problemu, zaleca się próby wyłączyć i ponownie włączyć funkcję zapisywania zwrotnego haseł w następnym kroku.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Wyłącz i ponownie włączyć funkcję zapisywania zwrotnego haseł

Wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł może pomóc rozwiązać problemy z połączeniem.

1. Jako administrator, otwórz **Kreatora konfiguracji usługi Azure AD Connect**.
2. Na **nawiązywanie połączenia z usługą Azure AD** okna dialogowego, wprowadź użytkownika **poświadczenia administratora globalnego usługi Azure AD**
3. Na **łączenie z usługami AD DS** okna dialogowego, wprowadź użytkownika **poświadczeń administratora usług domenowych w usłudze AD**.
4. Na **unikatowa identyfikacja użytkowników** okna dialogowego, kliknij przycisk **dalej** przycisku.
5. Na **funkcje opcjonalne** okno dialogowe, usuń zaznaczenie pola wyboru **funkcji zapisywania zwrotnego haseł** wyboru.
6. Kliknij przycisk **dalej** na pozostałych stronach okno dialogowe bez wprowadzania żadnych zmian, aż do **wszystko gotowe do skonfigurowania** strony.
7. Upewnij się, że przedstawiono na stronie Konfiguruj **opcji zapisywania zwrotnego haseł jako wyłączone** , a następnie kliknij zielony **Konfiguruj** przycisk, aby zatwierdzić zmiany.
8. Na **Zakończono** okno dialogowe, usuń zaznaczenie **Synchronizuj teraz** , a następnie kliknij przycisk **Zakończ** aby zamknąć kreatora.
9. Otwórz ponownie **Kreatora konfiguracji usługi Azure AD Connect**.
10. **Powtórz kroki 2-8**, z wyjątkiem zapewnisz **zaznacz opcję zapisywania zwrotnego haseł** na **funkcje opcjonalne** ekranu, aby ponownie włączyć usługę.

Te kroki ponownie ustanowić połączenie z naszej usługi w chmurze i rozwiąż przerw w działaniu, które mogą występować.

Jeśli wyłączenie i ponowne włączenie funkcji zapisywania zwrotnego haseł nie rozwiąże problemu, zaleca się, spróbuj zainstalować ponownie Azure AD Connect jako kolejny krok.

### <a name="install-the-latest-azure-ad-connect-release"></a>Zainstalowanie najnowszej wersji Azure AD Connect

Ponowna instalacja Azure AD Connect można rozwiązać, konfiguracji i problemy z połączeniem między naszych usług w chmurze i lokalnym środowisku usługi AD.

Firma Microsoft zaleca, możesz wykonać ten krok tylko po próbie pierwsze dwa kroki opisane powyżej.

> [!WARNING]
> Jeśli dostosowano poza pole reguły synchronizacji, **zarchiwizować te przed kontynuacją uaktualniania i ręcznie wdrożyć je ponownie po zakończeniu**.

1. Pobierz najnowszą wersję programu Azure AD Connect z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Ponieważ użytkownik zainstalował już Azure AD Connect, musisz wykonać uaktualnienie w miejscu do aktualizacji instalacji programu Azure AD Connect do najnowszej wersji.
3. Wykonaj pobranego pakietu i postępuj zgodnie z wyświetlanymi instrukcjami, aby zaktualizować komputer Azure AD Connect.

Te kroki ponownie ustanowić połączenia z usługą chmury i rozwiązać wszelkie przestoje, które mogą występować.

Jeśli instalowania najnowszej wersji serwera usługi Azure AD Connect nie rozwiąże problemu, zaleca się, spróbuj wyłączenie i ponowne włączanie zapisywania zwrotnego haseł jako ostatni krok po zainstalowaniu najnowszej wersji.

## <a name="verify-whether-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Sprawdź, czy usługa Azure AD Connect ma wymagane uprawnienia do zapisywania zwrotnego haseł
Azure AD Connect wymaga AD **Resetuj hasło** uprawnienia do wykonywania funkcji zapisywania zwrotnego haseł. Aby sprawdzić, czy usługa Azure AD Connect ma uprawnienie do danego lokalnego konta użytkownika AD, można użyć funkcji Windows czynne uprawnienia:

1. Zaloguj się do serwera usługi Azure AD Connect i uruchom **Menedżera usługi synchronizacji** (Start → Usługa synchronizacji).
2. W obszarze **łączniki** , a następnie wybierz lokalną **łącznika AD** i kliknij przycisk **właściwości**.  
![Czynne uprawnienia — krok 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. W oknie podręcznym wybierz **Połącz z lasu usługi Active Directory** karcie i zanotuj **nazwy użytkownika** właściwości. To jest konto usług AD DS, używane przez program Azure AD Connect w celu wykonania synchronizacji katalogów. Azure AD Connect wykonać funkcję zapisywania zwrotnego haseł konto usług AD DS musi mieć uprawnienia resetowania hasła.  
![Czynne uprawnienia — krok 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
4. Zaloguj się do kontrolera domeny w lokalnej i uruchomić **użytkownicy usługi Active Directory i komputery** aplikacji.
5. Kliknij przycisk **widoku** i upewnij się, że **funkcje zaawansowane** opcja jest włączona.  
![Czynne uprawnienia — krok 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
6. Poszukaj konta użytkownika AD, który chcesz zweryfikować. Kliknij prawym przyciskiem myszy na konto i wybierz **właściwości**.  
![Czynne uprawnienia — krok 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 
7. W wyskakującym oknie dialogowym, przejdź do **zabezpieczeń** i kliknij polecenie **zaawansowane**.  
![Czynne uprawnienia — krok 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
8. W oknie podręcznym Zaawansowane ustawienia zabezpieczeń, przejdź do **dostęp czynny** kartę.
9. Polecenie **wybierz użytkownika** i wybierz konto usług AD DS używany przez program Azure AD Connect (zobacz krok 3). Następnie kliknij przycisk **Wyświetl dostęp czynny**.  
![Czynne uprawnienia - krok 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
10. Przewiń w dół i poszukaj **resetowania hasła**. Jeśli wpis jest zaznaczone, oznacza to, że konto usługi AD DS jest uprawnień do resetowania hasła wybranego konta użytkowników usługi AD.  
![Czynne uprawnienia - kroku 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Fora o systemie Azure AD

Jeśli masz pytania ogólne dotyczące usługi Azure AD i Samoobsługowe Resetowanie hasła, poproś społeczność pomocy na [fora usługi Azure AD](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Członkowie społeczności obejmują Engineers, menedżerów produktu MVP i twarzy specjalistów IT.

## <a name="contact-microsoft-support"></a>Skontaktuj się z pomocą techniczną firmy Microsoft

Jeśli nie możesz znaleźć odpowiedzi do wystawienia nasze zespoły pomocy technicznej są zawsze dostępne ułatwi dalszych.

Aby pomóc poprawnie, poprosimy Podaj tak szczegółowe, jak to możliwe, podczas otwierania w tym przypadku:

* **Ogólny opis błędu** — co to jest błąd? Jaki był zachowanie wykryto? Jak można odtworzyć błędu? Podaj tak szczegółowe, jak to możliwe.
* **Strona** -strony, jakie były na kiedy wystąpieniem błędu? Podaj adres URL, jeśli są w stanie i tworzenia zrzutów ekranu.
* **Obsługuje kodu** — jaka była obsługa kod generowany, gdy użytkownik był wyświetlany błąd?
    * Aby znaleźć tę wartość, występuje błąd, a następnie kliknij łącze obsługuje kod w dolnej części ekranu i wysłać identyfikator GUID, który daje specjaliście pomocy technicznej.
    ![Znajdź kod obsługi w dolnej części ekranu][Support Code]
    * Jeśli na stronie bez obsługi kodu na dole, naciśnij klawisz F12 i wyszukaj identyfikator SID i CID i wysyłać te dwie specjaliście pomocy technicznej.
* **Daty, godziny i strefy czasowej** -Dołącz dokładnej daty i godziny **ze strefą czasową** wystąpił błąd.
* **Identyfikator użytkownika** — która była użytkownika, który był wyświetlany błąd? (user@contoso.com)
    * Jest to użytkownik federacyjny?
    * Jest to użytkownik synchronizowane wyznaczania wartości skrótu hasła
    * To jest użytkownikiem jedynym w chmurze?
* **Licencjonowanie** — użytkownik ma przypisanej licencji usługi Azure AD Premium lub usługi Azure AD podstawowa?
* **Dziennik zdarzeń aplikacji** — Jeśli używasz funkcji zapisywania zwrotnego haseł, a błąd w infrastruktury lokalnej można dołączyć kopię zip w dzienniku zdarzeń aplikacji z serwera usługi Azure AD Connect po skontaktowaniu się z pomocą techniczną.



[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Uruchom ponownie usługę Azure AD Sync"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Obsługa kodu znajdującego się w prawym dolnym rogu okna"

## <a name="next-steps"></a>Następne kroki

Poniższe linki dają dostęp do dodatkowych informacji dotyczących resetowania haseł za pomocą usługi Azure AD

* [Jak wykonać pomyślne wdrożenie SSPR?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).
* [Masz pytanie Licencjonowanie?](active-directory-passwords-licensing.md)
* [Jakie dane są używane przez SSPR i jakie dane powinny można wypełnić dla użytkowników?](active-directory-passwords-data.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad z SSPR?](active-directory-passwords-policy.md)
* [Co to jest funkcji zapisywania zwrotnego haseł i dlaczego I interesujących go?](active-directory-passwords-writeback.md)
* [Jak zgłosić w działaniu w SSPR](active-directory-passwords-reporting.md)
* [Co to są wszystkie opcje w SSPR i do czego ich znaczenie?](active-directory-passwords-how-it-works.md)
* [Masz pytania, na które nie objęte gdzieś else](active-directory-passwords-faq.md)
