---
title: "Konfigurowanie usługi Azure MFA | Dokumentacja firmy Microsoft"
description: "Jest to strony uwierzytelnianie wieloskładnikowe Azure, którą opisano, co należy zrobić dalej za pomocą usługi MFA.  W tym raporty, oszustwa, jednorazowe obejście, niestandardowe wiadomości głosowe, buforowanie zaufanych adresów IP i aplikacji hasła."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 4900707baa875ae4527d82e8189d5bc4d319ae0c
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="configure-azure-multi-factor-authentication-settings---public-preview"></a>Konfigurowanie ustawień usługi Azure Multi-Factor Authentication — publicznej wersji zapoznawczej

Ten artykuł ułatwia zarządzanie Azure Multi-Factor Authentication, skoro masz działa prawidłowo.  Obejmuje ona różnych tematów, które ułatwiają maksymalne wykorzystanie usługi Azure Multi-Factor Authentication.  Nie wszystkie te funkcje są dostępne w każdym [wersja Azure Multi-Factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need).

>[!NOTE]
>Te ustawienia są w publicznej wersji zapoznawczej w portalu Azure. Dokumentacja na temat zarządzania ustawieniami Azure Multi-Factor Authentication w portalu pfweb, zobacz [ustawienia skonfigurować uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-whats-next-pfweb.md).

| Funkcja | Opis | 
|:--- |:--- |
| [Blokować i odblokowywać użytkowników](#block-and-unblock) |Zablokuj/Odblokuj użytkownikom można uniemożliwić użytkownikom odbieranie żądań uwierzytelniania. |
| [Alert o oszustwie](#fraud-alert) |Alert o oszustwie można konfigurować i skonfigurować, aby użytkownicy mogą raportować fałszywych próbuje uzyskać dostęp do swoich zasobów. |
| [Jednorazowe obejście](#one-time-bypass) |Jednorazowe obejście pozwala na uwierzytelnianie tylko raz, pomijając"" uwierzytelnianie wieloskładnikowe. |
| [Niestandardowe wiadomości głosowe](#custom-voice-messages) |Niestandardowe wiadomości głosowe umożliwiają używanie własnych nagrań lub pozdrowienia przy użyciu uwierzytelniania wieloskładnikowego. |
| [Buforowanie](#caching-in-azure-multi-factor-authentication) |Buforowanie umożliwia ustawienie określony czas okresu, dzięki czemu kolejne próby uwierzytelniania powiedzie się automatycznie. |
| [Listę zaufanych adresów IP](#trusted-ips) |Administratorzy dzierżawy zarządzane lub federacyjnych mogą używać zaufanych adresów IP do pominięcia weryfikacji dwuetapowej dla użytkowników, którzy zalogować się w lokalnym intranecie firmy. |
| [Hasła aplikacji](#app-passwords) |Hasła aplikacji umożliwia aplikacji, która nie jest uwierzytelnianie wieloskładnikowe aware obejście usługi Multi-Factor authentication i kontynuować pracę. |
| [Zapamiętać usługi Multi-Factor Authentication na zapamiętanych urządzeniach i przeglądarki](#remember-multi-factor-authentication-for-devices-that-users-trust) |Służy do zapamiętania urządzeń przez liczbę dni, po użytkownik pomyślnie zalogował się przy użyciu usługi MFA. |
| [Metody wyboru weryfikacji](#selectable-verification-methods) |Umożliwia wybór metody uwierzytelniania, które są dostępne dla użytkowników do użycia. |

## <a name="block-and-unblock"></a>Blokować i odblokowywać
Zablokuj/Odblokuj użytkownikom można uniemożliwić użytkownikom odbieranie żądań uwierzytelniania. Wszelkie próby uwierzytelnienia dla zablokowanych użytkowników będą automatycznie odrzucane. Zablokowanych użytkowników pozostanie zablokowane do 90 dni od czasu są zablokowane.

### <a name="block-a-user"></a>Blokuj użytkownika
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **Zablokuj/Odblokuj użytkowników**.
3. Kliknij przycisk **Dodaj** aby uniemożliwić użytkownikowi.
4. Wybierz **grupy replikacji**, wprowadź zablokowanych username jako  **username@domain.com** i wprowadź komentarz w **Przyczyna** pola.
5. Kliknij przycisk **Dodaj** na zakończenie zablokowania użytkownika.

### <a name="unblock-a-user"></a>Odblokowanie użytkownika
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **Zablokuj/Odblokuj użytkowników**.
3. Kliknij przycisk **Odblokuj** w **akcji** kolumnę obok użytkownika, którą chcesz odblokować.
4. Wprowadź komentarz w **Przyczyna odblokowania** pola.
5. Kliknij przycisk **Odblokuj** na zakończenie odblokowania użytkownika.

## <a name="fraud-alert"></a>Alert oszustwa
Alert o oszustwie można konfigurować i skonfigurować, aby użytkownicy mogą raportować fałszywych próbuje uzyskać dostęp do swoich zasobów.  Użytkownicy mogą raportować oszustwo w aplikacji mobilnej lub przez telefon.

### <a name="turn-on-fraud-alert"></a>Włącz oszustwa
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **oszustwa**.

   ![Alert dotyczący wykrycia oszustwa](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Włącz **Zezwalaj użytkownikom na przesłanie alertów oszustwa** do **na**.
4. Wybierz pozycję **Zapisz**.

### <a name="configuration-options"></a>Opcje konfiguracji

- **Blokuj użytkownika, gdy zostaje zgłoszone oszustwo** — Jeśli oszustwa raporty użytkownika, jego konta jest zablokowane przez 90 dni lub dopóki administrator odblokowuje swojego konta. Administrator można przejrzeć logowania przy użyciu raportu logowania i podjąć odpowiednie działania w celu zapobiegania oszustwom przyszłych. Administrator może następnie [odblokować](#unblock-a-user) konta użytkownika.
- **Kod zgłoszenia oszustwa w trakcie początkowego pozdrowienia** — gdy użytkownik odbierze połączenie telefoniczne do przeprowadzenia weryfikacji dwuetapowej one zwykle naciśnięcie przycisku #, aby potwierdzić ich logowania. Jeśli chcą zgłoś oszustwo one wprowadzić kod przed naciśnięciem przycisku #. Ten kod jest **0** domyślnie, ale można go dostosować.

> [!NOTE]
> Pozdrowienia głosowe domyślna firmy Microsoft poinstruować użytkowników, aby naciśnięcie przycisku # 0, aby przesłać alert o oszustwie. Jeśli chcesz użyć kodu innego niż 0, należy zarejestrować i przekazać swoje własne niestandardowe pozdrowienia głosowe odpowiednie instrukcje.

### <a name="view-fraud-reports"></a>Wyświetl raporty dotyczące oszustwa
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. W dolnej części strony ustawień usługi, wybierz **przejdź do portalu**.
7. W portalu Azure Multi-Factor Authentication zarządzania w obszarze Wyświetl raport, kliknij przycisk **alarm oszustwa**.
8. Określ zakres dat, którą chcesz wyświetlić w raporcie. Można również określić nazwy użytkowników, numerów telefonów i stanu użytkownika.
9. Kliknij pozycję **Run** (Uruchom). Spowoduje to wyświetlenie raportu alertów oszustwa. Kliknij przycisk **Eksportuj do pliku CSV** Jeśli chcesz wyeksportować raport.

## <a name="one-time-bypass"></a>Jednorazowe obejście
Jednorazowe obejście pozwala na uwierzytelnianie jeden raz bez przeprowadzania weryfikacji dwuetapowej. Obejście jest tymczasowe i wygasa po upływie określonej liczby sekund. W sytuacjach, w którym aplikacji mobilnej lub telefon nie odbiera powiadomienia lub połączeń telefonicznych można włączyć jednorazowe obejście, więc użytkownik ma dostęp do żądanego zasobu.

### <a name="create-a-one-time-bypass"></a>Utwórz jednorazowe obejście

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **jednorazowe obejście**.

   ![Jednorazowe obejście](./media/multi-factor-authentication-whats-next/onetimebypass.png)
3. Wybierz pozycję **Dodaj**.
4. Jeśli to konieczne, wybierz grupę replikacji tego obejścia.
5. Wprowadź nazwę użytkownika (w postaci username@domain.com), liczba sekund, które wystąpią obejście i przyczynę obejścia. 
6. Wybierz pozycję **Dodaj**. Limit czasu przechodzi w stan obowiązywać natychmiast, więc użytkownik musi się zalogować przed wygaśnięciem jednorazowe obejście. 

### <a name="view-the-one-time-bypass-report"></a>Wyświetl raport jednorazowe obejście
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. W dolnej części strony ustawień usługi, wybierz **przejdź do portalu**.
7. W portalu Azure Multi-Factor Authentication zarządzania w obszarze Wyświetl raport, kliknij przycisk **jednorazowe obejście**.
8. Określ zakres dat, którą chcesz wyświetlić w raporcie. Można również określić nazwy użytkowników, numerów telefonów i stanu użytkownika.
9. Kliknij pozycję **Run** (Uruchom). Spowoduje to wyświetlenie raportu obejścia. Kliknij przycisk **Eksportuj do pliku CSV** Jeśli chcesz wyeksportować raport.

## <a name="custom-voice-messages"></a>Niestandardowe wiadomości głosowe
Niestandardowe wiadomości głosowe umożliwiają używanie własnych nagrań lub pozdrowienia na potrzeby weryfikacji dwuetapowej. Mogą być one używane dodatkowe lub zamienić Microsoft rejestruje.

Przed rozpoczęciem należy pamiętać o następujących czynności:

* Obsługiwane formaty plików są .wav lub .mp3.
* Limit rozmiaru plików to 5 MB.
* Uwierzytelnianie wiadomości powinien być krótszy niż 20 sekund. Cokolwiek dłużej, niż może to spowodować weryfikacji się niepowodzenie, ponieważ użytkownik może nie odpowiadać przed zakończeniem wiadomości, powodując weryfikacji przekraczają limit czasu.

### <a name="set-up-a-custom-message"></a>Konfigurowanie niestandardowych komunikatów

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **ustawienia połączeń telefonicznych**.

   ![Ustawienia połączenia telefonicznego](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Wybierz **pozdrowienia Dodaj**.
4. Wybierz typ pozdrowienia i język.
5. Wybierz plik dźwiękowy MP3 lub WAV do przekazania.
6. Wybierz pozycję **Dodaj**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Buforowanie w uwierzytelnianie wieloskładnikowe platformy Azure
Buforowanie pozwala na okres ustawiony określony czas, aby automatycznie powiedzie się kolejne próby uwierzytelniania w tym przedziale czasu. Służy to głównie systemów lokalnych, takich jak VPN wysyłania wielu żądań weryfikacji podczas pierwszego żądania jest nadal w toku. Dzięki temu kolejnych żądań po użytkownika powiedzie się pierwszy Weryfikacja w toku powiodło się. 

Buforowanie nie jest przeznaczony do użycia dla logowania do usługi Azure AD.

### <a name="set-up-caching"></a>Konfigurowanie pamięci podręcznej 
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **serwera usługi MFA** > **buforowanie reguły**.

   ![Reguły buforowania](./media/multi-factor-authentication-whats-next/cachingrules.png)

4. Wybierz pozycję **Dodaj**.
5. Wybierz z listy rozwijanej Typ pamięci podręcznej i określ liczbę sekund buforowania max. 
6. Jeśli to konieczne, wybierz typ uwierzytelniania i określ aplikacji. 
7. Wybierz pozycję **Dodaj**.


## <a name="trusted-ips"></a>Zaufane adresy IP
Zaufanych adresów IP to funkcja usługi Azure MFA używanego przez administratorów dzierżawy zarządzane lub federacyjnych do pominięcia weryfikacji dwuetapowej dla użytkowników, którzy są logujący się z lokalny intranet firmy. Ta funkcja jest dostępna w pełnej wersji usługi Azure Multi-Factor Authentication nie bezpłatną wersję dla administratorów. Aby uzyskać szczegółowe informacje dotyczące sposobu uzyskania pełną wersję programu Azure Multi-Factor Authentication, zobacz [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ dzierżawy usługi Azure AD | Dostępne opcje zaufany adres IP |
|:--- |:--- |
| Zarządzane |<li>Określonych zakresów adresów IP — Administratorzy mogą określić zakres adresów IP, które można pominąć weryfikacji dwuetapowej dla użytkowników, którzy są logujący się z sieci intranet.</li> |
| Federacyjna |<li>Wszyscy użytkownicy federacyjnym — wszystkie użytkownicy federacyjni, którzy logowanie z wewnątrz organizacji będzie pominąć weryfikacji dwuetapowej przy użyciu oświadczenia wydane przez usługi AD FS.</li><br><li>Określonych zakresów adresów IP — Administratorzy mogą określić zakres adresów IP, które można pominąć weryfikacji dwuetapowej dla użytkowników, którzy są logujący się z sieci intranet. |

To obejście działa tylko z wewnątrz sieci intranet firmy. Na przykład jeśli wybrano federacyjnych wszystkich użytkowników, a użytkownik zaloguje się z spoza sieci intranet, ten użytkownik ma do uwierzytelniania przy użyciu weryfikacji dwuetapowej, nawet jeśli użytkownik przedstawia oświadczenia usług AD FS. 

**Środowisko użytkownika końcowego w sieci corpnet:**

Po wyłączeniu zaufanych adresów IP weryfikacji dwuetapowej jest wymagane dla przepływów przeglądarki, a hasła aplikacji są wymagane dla starszych aplikacji wzbogaconego klienta. 

Po włączeniu zaufanych adresów IP, weryfikacja dwuetapowa to *nie* wymagane dla przepływów przeglądarki, a następnie hasła aplikacji są *nie* wymagane starszych aplikacji wzbogaconego klienta, pod warunkiem, że użytkownik już nie utworzyła aplikację hasło. Po użyciu hasła aplikacji jest używany, pozostaje wymagane. 

**Corpnet poza środowisko użytkownika końcowego:**

Czy zaufanych adresów IP jest włączone, weryfikacja dwuetapowa jest wymagane dla przepływów przeglądarki, a hasła aplikacji są wymagane dla starszych aplikacji wzbogaconego klienta. 

### <a name="to-enable-trusted-ips"></a>Aby włączyć zaufanych adresów IP
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. Na stronie ustawień usługi w obszarze zaufanych adresów IP dostępne są dwie opcje:
   
   * **Żądania od użytkowników federacyjnych pochodzące z moim intranecie** — pole wyboru. Wszystkich użytkowników federacyjnych zalogowanych z sieci firmowej będzie pomijać weryfikację dwuetapową, za pomocą oświadczenia wydane przez usługi AD FS. Upewnij się, że usługi AD FS ma zasadę, aby dodawać oświadczenia intranet na odpowiedni ruch. Należy utworzyć następującą regułę w usługach AD FS, jeśli jeszcze nie istnieje: "c: [typu =="http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] = > issue(claim = c);"



   * **Dla żądań z określonego zakresu publicznych adresów IP** — wprowadź adresy IP w polu tekstowym za pomocą notacji CIDR. Na przykład: xxx.xxx.xxx.0/24 dla adresów IP w zakresie xxx.xxx.xxx.1 — xxx.xxx.xxx.254 lub xxx.xxx.xxx.xxx/32 dla jednego adresu IP. Możesz wprowadzić maksymalnie 50 zakresów adresów IP. Użytkownicy, którzy zalogować się w tych adresów IP pominąć weryfikacji dwuetapowej.
7. Kliknij pozycję **Zapisz**.
8. Po zastosowaniu aktualizacji kliknij **Zamknij**.

![Zaufane adresy IP](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Hasła aplikacji
Niektóre aplikacje, takich jak pakiet Office 2010 lub starszy i Apple Mail nie obsługują weryfikacji dwuetapowej. Nie są one skonfigurowane do akceptowania drugi weryfikacji. Aby korzystać z tych aplikacji, należy użyć "haseł aplikacji" zamiast tradycyjnych hasła. Hasła aplikacji umożliwia aplikacji pominąć weryfikację dwuetapową i kontynuować pracę.

> [!NOTE]
> Nowoczesne uwierzytelnianie dla klientów pakietu Office 2013
> 
> Klienci Office 2013 (w tym programu Outlook) i nowszych protokołów nowoczesnego uwierzytelniania pomocy technicznej i można ją włączyć do pracy z weryfikacji dwuetapowej. Po włączeniu hasła aplikacji nie są wymagane dla tych klientów.  Aby uzyskać więcej informacji, zobacz [pakietu Office 2013 nowoczesnego uwierzytelniania anonsowania publicznej wersji](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Ważnych rzeczy, aby dowiedzieć się o hasłach aplikacji
Poniżej znajduje się ważne listę rzeczy, które należy poznać dotyczące haseł aplikacji.

* Haseł aplikacji należy tylko nie trzeba wprowadzać raz dla aplikacji. Użytkownicy nie muszą rejestrować i wprowadzić je za każdym razem.
* Rzeczywiste hasło jest generowane automatycznie i nie jest dostarczony przez użytkownika. To jest, ponieważ jest trudniejsze osobie atakującej odgadnąć hasło generowane automatycznie i jest bardziej bezpieczne.
* Istnieje limit 40 haseł na użytkownika. 
* Aplikacje, których buforowanie haseł, używany w scenariuszach lokalnych mogą przestać działać, ponieważ hasła aplikacji nie jest znana poza identyfikatora organizacyjnego. Przykładem jest wiadomości e-mail programu Exchange, które są lokalne, ale poczta jest archiwizowana w chmurze. To samo hasło nie działa.
* Włączenie uwierzytelniania wieloskładnikowego na koncie użytkownika, hasła aplikacji mogą być używane z większości klientów korzystających z przeglądarki, takich jak Outlook i Lync, ale nie można wykonać działania administracyjne, przy użyciu hasła aplikacji za pomocą aplikacji korzystających z przeglądarki, takich jak Windows Nawet jeśli użytkownik ma konto administracyjne programu PowerShell.  Upewnij się, możesz utworzyć konto usługi z silne hasło, aby uruchamiać skrypty programu PowerShell i nie należy włączać tego konta na potrzeby weryfikacji dwuetapowej.

> [!WARNING]
> Hasła aplikacji nie działają w środowiskach hybrydowych, w którym klienci komunikować się z lokalnie i w chmurze punkty końcowe wykrywania automatycznego. Jest to spowodowane hasła domeny są wymagane do uwierzytelniania lokalnych i hasła aplikacji są wymagane do uwierzytelniania w chmurze.

### <a name="naming-guidance-for-app-passwords"></a>Wskazówki dotyczące haseł aplikacji nazewnictwa
Nazwy haseł aplikacji odzwierciedlały urządzenie, na którym są używane. Na przykład, jeśli użytkownik ma komputer przenośny z zainstalowanymi aplikacji korzystających z przeglądarki, takich jak Outlook, Word i Excel, utworzyć hasło aplikacji o nazwie komputer przenośny i użyć tego hasła aplikacji w tych aplikacjach. Następnie utwórz innego hasła aplikacji o nazwie pulpitu dla tej samej aplikacji na komputerze stacjonarnym. 

Firma Microsoft zaleca utworzenie jednego hasła aplikacji na urządzenie, nie jednego hasła aplikacji na aplikację.

### <a name="federated-sso-app-passwords"></a>Hasła aplikacji federacyjnych (SSO)
Usługi Azure AD obsługuje Federacji (logowanie jednokrotne) z lokalnego systemu Windows serwera usług domenowych Active Directory (AD DS). Jeśli Twoja organizacja jest Sfederowane przy użyciu usługi Azure AD i zamierzasz używać usługi Azure Multi-Factor Authentication, następujące informacje dotyczące haseł aplikacji jest ważne. Ta sekcja dotyczy tylko dla klientów usługi federacyjnej (SSO).

* Hasła aplikacji są weryfikowane przez usługę Azure AD i w związku z tym obejście federacji. Federacyjnej jest aktywnie stosowana tylko podczas konfigurowania haseł aplikacji.
* Dla użytkowników federacyjnych (SSO) firma Microsoft nigdy nie przejdź do dostawcy tożsamości (IdP) inaczej niż w przypadku przepływu pasywnego. Hasła są przechowywane w identyfikatora organizacyjnego. Gdy użytkownik opuści firmę, że informacje zostaną przekazane do identyfikatora organizacyjnego przy użyciu narzędzia DirSync w czasie rzeczywistym. Wyłączenie/usunięcie konta może potrwać do trzech godzin, aby zsynchronizować, opóźnienie wyłączenia/usunięcia hasła aplikacji w usłudze Azure AD.
* Ustawienia lokalnej kontroli dostępu klienta nie są uznawane przez hasło aplikacji.
* Bez uwierzytelniania lokalnego możliwości rejestrowania/inspekcji jest dostępna dla hasła aplikacji.
* Niektórych zaawansowanych rozwiązań architektury może wymagać kombinację organizacji nazwy użytkownika i hasła i haseł aplikacji, podczas klientom, w zależności od tego, gdzie uwierzytelniania przy użyciu weryfikacji dwuetapowej. Dla klientów, którzy uwierzytelniania względem infrastruktury lokalnej użyje organizacyjnej nazwy użytkownika i hasła. Dla klientów, którzy uwierzytelniania usługi Azure AD można użyć hasła aplikacji.

  Na przykład załóżmy, że masz architekturę, która składa się z następujących czynności:

  * Federacji lokalnego wystąpienia usługi Active Directory z usługą Azure AD
  * Korzystasz z usługi Exchange online
  * W przypadku korzystania z Lync, będący w szczególności lokalnego
  * Używasz usługi Azure Multi-Factor Authentication

  ![Biurowego](./media/multi-factor-authentication-whats-next/federated.png)

  W takich przypadkach wykonaj następujące czynności:

  * Gdy logowanie się do usługi Lync, użyj nazwy użytkownika i hasła Twojej organizacji.
  * Podczas próby uzyskania dostępu w książce adresowej za pomocą klienta programu Outlook, który łączy się z programem Exchange online, należy użyć hasła aplikacji.

### <a name="allow-app-password-creation"></a>Zezwalaj na tworzenie haseł aplikacji
Domyślnie użytkownicy nie mogą tworzyć hasła aplikacji. Ta funkcja musi być włączona. Aby umożliwić użytkownikom tworzenie haseł aplikacji, należy użyć następującej procedury:

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. Zaznacz przycisk radiowy obok **Zezwalaj użytkownikom na tworzenie haseł aplikacji do logowania się do aplikacji niekorzystających z przeglądarki**.

![Tworzenie haseł aplikacji](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Tworzenie haseł aplikacji
Użytkownicy mogą tworzyć hasła aplikacji podczas ich początkowe rejestracyjny. Otrzymują one opcję po zakończeniu procesu rejestracji, która pozwala na tworzenie haseł aplikacji.

Użytkownicy mogą także tworzyć hasła aplikacji po zarejestrowaniu, zmieniając ich ustawień w portalu Azure lub w portalu usługi Office 365. Aby uzyskać więcej informacji i uzyskać szczegółowe instrukcje dla użytkowników, zobacz [co to są hasła aplikacji w usłudze Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Należy pamiętać, uwierzytelnianie wieloskładnikowe dla użytkowników zaufanych urządzeniach
Zapamiętywanie uwierzytelniania wieloskładnikowego dla urządzeń i przeglądarki czy zaufanie użytkowników jest bezpłatne funkcji dla wszystkich użytkowników usługi MFA. Umożliwia nadawanie użytkownikom opcję obejście uwierzytelniania Wieloskładnikowego przez liczbę dni, po wykonaniu pomyślnego logowania przy użyciu usługi MFA. To zwiększają użyteczność, minimalizując liczbę razy użytkownik może wykonywać weryfikacji dwuetapowej na tym samym urządzeniu.

Jednak w przypadku złamania zabezpieczeń konta lub urządzenia, zapamiętywanie uwierzytelniania Wieloskładnikowego na zaufanych urządzeniach może mieć wpływ na bezpieczeństwo. Jeśli zostanie naruszone bezpieczeństwo konta firmowego lub zaufanego urządzenia utraty lub kradzieży, [Przywróć uwierzytelnianie wieloskładnikowe na wszystkich urządzeniach](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Ta akcja odwołuje zaufanego stanu ze wszystkich urządzeń, a użytkownik jest wymagane do przeprowadzenia weryfikacji dwuetapowej ponownie. Można również Poinstruuj użytkowników, aby przywrócić MFA na urządzeniach z instrukcjami w [zarządzać ustawieniami na potrzeby weryfikacji dwuetapowej](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Jak to działa

Pamiętaj o tym, usługa Multi-Factor Authentication działa przez ustawienie trwały plik cookie w przeglądarce, gdy użytkownik "nie pytaj ponownie o **X** dni" pole podczas logowania. Nie jest monitowany o podanie MFA ponownie z tym broswer do momentu wygaśnięcia pliku cookie. Jeśli użytkownik otwiera innej przeglądarki, w tym samym urządzeniu lub czyści ich pliki cookie, są monitowani o ponownie zweryfikować. 

"Nie pytaj ponownie o **X** dni" pole wyboru nie jest widoczne w aplikacjach korzystających z przeglądarki, czy obsługują one nowoczesnego uwierzytelniania. Te aplikacje używaj tokenów odświeżania, zapewniające nowe tokeny dostępu, co godzinę. Jeśli token odświeżania jest weryfikowany, Azure kontroli AD wykonanie ostatniego weryfikacji dwuetapowej czasu należała skonfigurowaną liczbę dni. 

W związku z tym w zapamiętywanie uwierzytelniania Wieloskładnikowego na zaufanych urządzeniach zmniejsza liczbę prób uwierzytelnienia w usłudze aplikacje sieci web (które zwykle monit o każdym), ale zwiększa liczbę uwierzytelnień dla klientów uwierzytelniania nowoczesny, (które zwykle Monituj co 90 dni).

> [!NOTE]
>Ta funkcja nie jest zgodny z funkcją "Wylogowuj mnie" usług AD FS, gdy użytkownicy wykonują weryfikacji dwuetapowej dla usług AD FS za pomocą serwera usługi Azure MFA lub rozwiązanie MFA innych firm. Jeśli użytkownicy w usługach AD FS wybierz opcję "Wylogowuj mnie" i zaznaczyć swoje urządzenia jako zaufane dla usługi MFA, nie będzie mógł zweryfikować po wygaśnięciu "Pamiętaj MFA" liczbę dni. Weryfikacja dwuetapowa świeże żądań usługi Azure AD, ale usług AD FS zwraca token z oryginalnego oświadczeń MFA i Data zamiast wykonywania ponownie weryfikacji dwuetapowej. To powoduje rozpoczęcie pętlę weryfikacji między Azure AD i usług AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Włączanie uwierzytelniania wieloskładnikowego Zapamiętaj
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. Na stronie ustawień usługi, w obszarze zarządzania ustawieniami urządzenia użytkownika, sprawdź **Zezwalaj użytkownikom na zapamiętywanie danych uwierzytelniania wieloskładnikowego na zaufanych urządzeniach** pole.
   ![Należy pamiętać, urządzenia](./media/multi-factor-authentication-whats-next/remember.png)
7. Ustaw liczbę dni, które chcesz zezwolić na zaufanych urządzeniach pominąć weryfikacji dwuetapowej. Wartość domyślna to 14 dni.
8. Kliknij pozycję **Zapisz**.
9. Kliknij przycisk **Zamknij**.

### <a name="mark-a-device-as-trusted"></a>Oznacz jako zaufanego urządzenia

Po włączeniu tej funkcji, użytkowników można oznaczyć jako zaufane, gdy urządzenie zalogują się sprawdzając **nie pytaj ponownie**.

![Nie pytaj ponownie — zrzut ekranu](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Metody wyboru weryfikacji
Można wybrać metody weryfikacji, które są dostępne dla użytkowników. Poniższa tabela zawiera krótkie omówienie każdej metody.

Gdy użytkownicy rejestrują swoje konta dla usługi MFA, decydują ich metodę weryfikacji preferowanego poza opcje, które są włączone. Wskazówki dotyczące procesu rejestracji znajdują się w [Skonfiguruj moje konto na potrzeby weryfikacji dwuetapowej](multi-factor-authentication-end-user-first-time.md)

| Metoda | Opis |
|:--- |:--- |
| Połączenie z telefonem |Umieszcza wykonywane automatyczne połączenie głosowe. Użytkownik odbierze połączenie i naciska klawisz # na klawiaturze telefonu w celu uwierzytelnienia. Ten numer telefonu nie jest zsynchronizowany do lokalnej usługi Active Directory. |
| SMS na telefon |Wysyła wiadomość tekstową zawierającą kod weryfikacyjny. Użytkownik jest monitowany o odpowiedź na wiadomość SMS z kodem weryfikacyjnym lub aby wprowadzić kod weryfikacyjny w interfejsie logowania. |
| Powiadomienie przez aplikację mobilną |Wysyła powiadomienia wypychanego na telefonie lub zarejestrowanym urządzeniem. Użytkownik wyświetla powiadomienie i wybiera **Sprawdź** aby ukończyć weryfikację. <br>Jest dostępna dla aplikacji Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Kod weryfikacyjny z aplikacji mobilnej |Aplikacja Microsoft Authenticator generuje nowy kod OATH weryfikacji co 30 sekund. Użytkownik wprowadza ten kod weryfikacyjny w interfejsie logowania.<br>Jest dostępna dla aplikacji Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Jak włączyć/wyłączyć metody uwierzytelniania
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Wybierz katalog, w którym chcesz zarządzać. 
4. Wybierz **skonfigurować**
5. W obszarze usługi Multi-Factor Authentication, zaznacz **Zarządzaj ustawieniami usługi**.
6. Na stronie ustawień usługi w obszarze Opcje weryfikacji wybrać/anulować wybór opcji, które chcesz użyć.
   ![Opcje weryfikacji](./media/multi-factor-authentication-whats-next/authmethods.png)
7. Kliknij pozycję **Zapisz**.
8. Kliknij przycisk **Zamknij**.

