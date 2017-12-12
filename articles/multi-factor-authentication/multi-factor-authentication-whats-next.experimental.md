---
title: "Konfigurowanie usługi Azure MFA | Dokumentacja firmy Microsoft"
description: "Jest to strony uwierzytelnianie wieloskładnikowe Azure, którą opisano, co należy zrobić dalej za pomocą usługi MFA.  W tym raporty, oszustwa, jednorazowe obejście, niestandardowe wiadomości głosowe, buforowanie zaufanych adresów IP i aplikacji hasła."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: joflore
ms.openlocfilehash: a6064e33dca09a304074efa4029c0f96fc19b2a2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurowanie ustawień usługi Azure Multi-Factor Authentication
Ten artykuł ułatwia zarządzanie Azure Multi-Factor Authentication, skoro masz działa prawidłowo.  Obejmuje ona różnych tematów, które ułatwiają maksymalne wykorzystanie usługi Azure Multi-Factor Authentication.  Nie wszystkie te funkcje są dostępne w każdej wersji Azure Multi-Factor Authentication.

| Funkcja | Opis | 
|:--- |:--- |
| [Alert o oszustwie](#fraud-alert) |Alert o oszustwie można konfigurować i skonfigurować, aby użytkownicy mogą raportować fałszywych próbuje uzyskać dostęp do swoich zasobów. |
| [Jednorazowe obejście](#one-time-bypass) |Jednorazowe obejście pozwala na uwierzytelnianie tylko raz, pomijając"" uwierzytelnianie wieloskładnikowe. |
| [Niestandardowe wiadomości głosowe](#custom-voice-messages) |Niestandardowe wiadomości głosowe umożliwiają używanie własnych nagrań lub pozdrowienia przy użyciu uwierzytelniania wieloskładnikowego. |
| [Buforowanie](#caching-in-azure-multi-factor-authentication) |Buforowanie umożliwia ustawienie określony czas okresu, dzięki czemu kolejne próby uwierzytelniania powiedzie się automatycznie. |
| [Listę zaufanych adresów IP](#trusted-ips) |Administratorzy dzierżawy zarządzane lub federacyjnych mogą używać zaufanych adresów IP do pominięcia weryfikacji dwuetapowej dla użytkowników, którzy zalogować się w lokalnym intranecie firmy. |
| [Hasła aplikacji](#app-passwords) |Hasła aplikacji umożliwia aplikacji, która nie jest uwierzytelnianie wieloskładnikowe aware obejście usługi Multi-Factor authentication i kontynuować pracę. |
| [Zapamiętać usługi Multi-Factor Authentication na zapamiętanych urządzeniach i przeglądarki](#remember-multi-factor-authentication-for-devices-that-users-trust) |Służy do zapamiętania urządzeń przez liczbę dni, po użytkownik pomyślnie zalogował się przy użyciu usługi MFA. |
| [Metody wyboru weryfikacji](#selectable-verification-methods) |Umożliwia wybór metody uwierzytelniania, które są dostępne dla użytkowników do użycia. |

## <a name="access-the-azure-mfa-management-portal"></a>Dostęp do portalu zarządzania usługi Azure MFA

Funkcje omówione w tym artykule są konfigurowane w portalu zarządzania Azure Multi-Factor Authentication. Istnieją dwa sposoby dostępu do portalu zarządzania usługi MFA za pośrednictwem klasycznego portalu Azure. Pierwsza to zarządzania dostawcy uwierzytelniania wieloskładnikowego. Drugim jest za pomocą ustawień usługi MFA. 

### <a name="use-an-authentication-provider"></a>Użyj dostawcy uwierzytelniania

Jeśli używasz dostawcy uwierzytelniania wieloskładnikowego na podstawie zużycia MFA, ta metoda umożliwia dostęp do portalu zarządzania.

Aby uzyskać dostęp do portalu zarządzania usługi MFA za pomocą dostawcy usługi Azure Multi-Factor Authentication, zaloguj się do klasycznego portalu Azure jako administrator, a następnie wybierz opcję Active Directory. Kliknij przycisk **dostawców uwierzytelniania wieloskładnikowego** , a następnie wybierz swój katalog a kliknij **Zarządzaj** znajdujący się u dołu.

### <a name="use-the-mfa-service-settings-page"></a>Strona Ustawienia usługi MFA 

Jeśli masz jednej z poniższych licencji służy strona Ustawienia usługi MFA:
- Usługa Azure MFA
- Usługa Azure AD — wersja Premium 
- Enterprise Mobility + Security

Aby uzyskać dostęp do portalu zarządzania usługi MFA za pośrednictwem strony Ustawienia usługi MFA, zaloguj się do klasycznego portalu Azure jako administrator, a następnie wybierz opcję Active Directory. Kliknij katalog, a następnie kliknij kartę **Konfiguracja**. W sekcji uwierzytelniania wieloskładnikowego wybierz pozycję **Zarządzaj ustawieniami usługi**. Na dole strony ustawień usługi MFA kliknij link **Przejdź do portalu**.


## <a name="fraud-alert"></a>Alert oszustwa
Alert o oszustwie można konfigurować i skonfigurować, aby użytkownicy mogą raportować fałszywych próbuje uzyskać dostęp do swoich zasobów.  Użytkownicy mogą raportować oszustwo w aplikacji mobilnej lub przez telefon.

### <a name="set-up-fraud-alert"></a>Konfigurowanie oszustwa
1. Przejdź do portalu zarządzania usługi MFA, zgodnie z instrukcjami w górnej części strony.
2. W portalu zarządzania Azure Multi-Factor Authentication kliknij **ustawienia** w sekcji konfiguracji.
3. W sekcji alarm oszustwa strony ustawień sprawdź **Zezwalaj użytkownikom na przesłanie alertów oszustwa** wyboru.
4. Wybierz **zapisać** Aby zastosować zmiany. 

### <a name="configuration-options"></a>Opcje konfiguracji

- **Blokuj użytkownika, gdy zostaje zgłoszone oszustwo** — Jeśli oszustwa raporty użytkownika, jego konta jest zablokowane.
- **Kod do raportu oszustwa podczas początkowego pozdrowienia** -użytkowników zwykle naciśnięcie przycisku #, aby potwierdzić weryfikacji dwuetapowej. Jeśli chcą zgłoś oszustwo one wprowadzić kod przed naciśnięciem przycisku #. Ten kod jest **0** domyślnie, ale można go dostosować.

> [!NOTE]
> Pozdrowienia głosowe domyślna firmy Microsoft poinstruować użytkowników, aby naciśnięcie przycisku # 0, aby przesłać alert o oszustwie. Jeśli chcesz użyć kodu innego niż 0, należy zarejestrować i przekazać swoje własne niestandardowe pozdrowienia głosowe odpowiednie instrukcje.

![Opcje alertów oszustwa — zrzut ekranu](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Jak użytkownicy zgłosić oszustwo 
Alert o oszustwie mogą być zgłaszane na dwa sposoby.  Albo za pośrednictwem aplikacji mobilnej lub przez telefon.  

#### <a name="report-fraud-with-the-mobile-app"></a>Zgłoś oszustwo w aplikacji mobilnej
1. Podczas weryfikacji jest wysyłany na Twój telefon, zaznacz go, aby otworzyć aplikację Microsoft Authenticator.
2. Wybierz **Odmów** powiadomienie. 
3. Wybierz **zgłoszenia oszustwa**.
4. Zamknij aplikację.

#### <a name="report-fraud-with-a-phone"></a>Zgłoś oszustwo przy użyciu telefonu
1. Gdy połączenie weryfikacyjne połączy się z telefonu, udzielenie odpowiedzi.  
2. Aby zgłosić oszustwo, wprowadź kod oszustwa (wartość domyślna to 0), a następnie znak #. Są następnie powiadomienie, że alarm oszustwa zostało przesłane.
3. Koniec wywołania.

### <a name="view-fraud-reports"></a>Wyświetl raporty dotyczące oszustwa
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję Active Directory.
3. U góry, zaznacz pole wyboru **dostawców uwierzytelniania wieloskładnikowego** wyświetlić listę dostawców uwierzytelniania wieloskładnikowego.
4. Wybierz dostawcę uwierzytelniania wieloskładnikowego, a następnie kliknij przycisk **Zarządzaj** w dolnej części strony. Otwiera portalu zarządzania Azure Multi-Factor Authentication.
5. Polecenie usługi Multi-Factor Authentication portalu zarządzania Azure, w obszarze Wyświetl raport, **alarm oszustwa**.
6. Określ zakres dat, którą chcesz wyświetlić w raporcie. Można również określić nazwy użytkowników, numerów telefonów i stanu użytkownika.
7. Kliknij przycisk **Uruchom** do wyświetlenia raportu alertów oszustwa. Kliknij przycisk **Eksportuj do pliku CSV** aby, wyeksportuj raport.

## <a name="one-time-bypass"></a>Jednorazowe obejście
Jednorazowe obejście pozwala na uwierzytelnianie jeden raz bez przeprowadzania weryfikacji dwuetapowej. Obejście jest tymczasowe i wygasa po upływie określonej liczby sekund. W sytuacjach, w którym aplikacji mobilnej lub telefon nie odbiera powiadomienia lub połączeń telefonicznych można włączyć jednorazowe obejście, więc użytkownik ma dostęp do żądanego zasobu.

### <a name="create-a-one-time-bypass"></a>Utwórz jednorazowe obejście
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do portalu zarządzania usługi MFA, zgodnie z instrukcjami w górnej części strony.
3. Jeśli widzisz Nazwa dostawcy usługi Azure MFA po lewej stronie z  **+**  obok niej, kliknij przycisk  **+** . Różnych grup replikacji serwera usługi MFA i Azure Default grupy są wyświetlane. Wybierz odpowiednią grupę.
4. W obszarze Administracja użytkownika, wybierz **jednorazowe obejście**.
5. Na stronie jednorazowe obejście kliknij **nowe jednorazowe obejście**.

  ![Chmura](./media/multi-factor-authentication-whats-next/create1.png)

6. Wprowadź nazwę użytkownika, czas trwania obejścia i obejście przyczynę. Kliknij przycisk **obejścia**.
7. Limit czasu przechodzi w stan obowiązywać natychmiast, więc użytkownik musi się zalogować przed wygaśnięciem jednorazowe obejście. 

### <a name="view-the-one-time-bypass-report"></a>Wyświetl raport jednorazowe obejście
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję Active Directory.
3. U góry, zaznacz pole wyboru **dostawców uwierzytelniania wieloskładnikowego** wyświetlić listę dostawców uwierzytelniania wieloskładnikowego.
4. Wybierz dostawcę uwierzytelniania wieloskładnikowego, a następnie kliknij przycisk **Zarządzaj** w dolnej części strony. Otwiera portalu zarządzania Azure Multi-Factor Authentication.
5. W portalu Azure usługi Multi-Factor Authentication zarządzania po lewej stronie w widoku raport, kliknij polecenie **jednorazowe obejście**.
6. Określ zakres dat, którą chcesz wyświetlić w raporcie. Można również określić nazwy użytkowników, numerów telefonów i stanu użytkownika.
7. Kliknij przycisk **Uruchom** do wyświetlenia raportu obejścia. Kliknij przycisk **Eksportuj do pliku CSV** aby, wyeksportuj raport.

## <a name="custom-voice-messages"></a>Niestandardowe wiadomości głosowe
Niestandardowe wiadomości głosowe umożliwiają używanie własnych nagrań lub pozdrowienia na potrzeby weryfikacji dwuetapowej. Możesz użyć niestandardowe wiadomości głosowe dodatkowo lub zamienić Microsoft rejestruje.

Przed rozpoczęciem należy pamiętać o tych czynności:

* Obsługiwane formaty plików są .wav lub .mp3.
* Limit rozmiaru plików to 5 MB.
* Uwierzytelnianie wiadomości powinien być krótszy niż 20 sekund. Komunikaty, które są dłuższe niż 20 sekund nie zapewniają użytkownikom wystarczająco dużo czasu na odpowiedź przed upływem czasu weryfikacji.

### <a name="set-up-a-custom-message"></a>Konfigurowanie niestandardowych komunikatów

Istnieją dwie części do tworzenia niestandardowych wiadomości. Najpierw przekazać komunikat, a następnie włączenie go dla użytkowników.

Aby przekazać niestandardowe wiadomości:

1. Tworzenie wiadomości głosowej niestandardowych przy użyciu jednego z obsługiwanych formatów plików.
2. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
3. Przejdź do portalu zarządzania usługi MFA, zgodnie z instrukcjami w górnej części strony.
4. W portalu zarządzania Azure Multi-Factor Authentication kliknij **wiadomości głosowe** w sekcji konfiguracji.
5. W konfiguracji: strona wiadomości głosowych, kliknij przycisk **nowej wiadomości głosowej**.
   ![Chmura](./media/multi-factor-authentication-whats-next/custom1.png)
6. W konfiguracji: nowe wiadomości głosowe strony, kliknij przycisk **zarządzania plikami dźwięk**.
   ![Chmura](./media/multi-factor-authentication-whats-next/custom2.png)
7. W konfiguracji: strona pliki dźwiękowe, kliknij przycisk **przekazać plik dźwiękowy**.
   ![Chmura](./media/multi-factor-authentication-whats-next/custom3.png)
8. W konfiguracji: kliknij pozycję Przekaż plik dźwiękowy **Przeglądaj** i przejdź do wiadomości głosowych, kliknij przycisk **Otwórz**.
9. Dodaj opis, a następnie kliknij przycisk **przekazać**.
10. Po przekazaniu wiadomość głosową, wyświetli się komunikat potwierdzający został pomyślnie przesłany plik.

Aby włączyć wiadomości dla użytkowników:

1. Po lewej stronie, kliknij przycisk **wiadomości głosowe**.
2. W sekcji wiadomości głosowe, kliknij przycisk **nowej wiadomości głosowej**.
3. Z języka listę rozwijaną wybierz język.
4. Jeśli ten komunikat jest dla określonej aplikacji, określ wartość w polu aplikacji.
5. Z rozwijanej Typ komunikatu wybierz typ wiadomości do zastąpienia z nowej wiadomości niestandardowych.
6. Plik dźwiękowy listę rozwijaną zaznacz plik dźwiękowy, który został przekazany w pierwszej części.
7. Kliknij przycisk **Utwórz**. Komunikat potwierdza, że pomyślnie utworzono wiadomości głosowej.
    ![Chmura](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Buforowanie w uwierzytelnianie wieloskładnikowe platformy Azure
Buforowanie pozwala na okres ustawiony określony czas, aby automatycznie powiedzie się kolejne próby uwierzytelniania w tym przedziale czasu. Buforowanie jest używany, gdy systemów lokalnych, takich jak VPN wysyłać wiele żądań weryfikacji podczas pierwszego żądania jest nadal w toku. Zezwalanie na kolejnych żądań po użytkownika powiedzie się pierwszy Weryfikacja w toku powiodło się. 

Buforowanie nie jest przeznaczony do użycia dla logowania do usługi Azure AD.

### <a name="set-up-caching"></a>Konfigurowanie pamięci podręcznej 
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do portalu zarządzania usługi MFA, zgodnie z instrukcjami w górnej części strony.
3. W portalu zarządzania Azure Multi-Factor Authentication kliknij **buforowanie** w sekcji konfiguracji.
4. Kliknij przycisk **nową pamięć podręczną** na Konfiguruj buforowanie strony.
5. Wybierz typ pamięci podręcznej i sekund buforowania. Kliknij przycisk **Utwórz**.

<center>![Chmura](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Zaufane adresy IP
Listę zaufanych adresów IP to funkcja usługi Azure MFA, które umożliwiają administratorom dzierżawy zarządzane lub federacyjnych do pominięcia weryfikacji dwuetapowej. Które jest używane dla użytkowników, którzy są logujący się z lokalny intranet firmy. Ta funkcja jest dostępna w pełnej wersji usługi Azure Multi-Factor Authentication nie bezpłatną wersję dla administratorów. Aby uzyskać szczegółowe informacje dotyczące sposobu uzyskania pełną wersję programu Azure Multi-Factor Authentication, zobacz [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ dzierżawy usługi Azure AD | Dostępne opcje zaufany adres IP |
|:--- |:--- |
| Zarządzane |<li>Określonych zakresów adresów IP — Administratorzy mogą określić zakres adresów IP, które można pominąć weryfikacji dwuetapowej dla użytkowników, którzy są logujący się z sieci intranet.</li> |
| Federacyjne |<li>Wszyscy użytkownicy federacyjnym — wszystkie użytkownicy federacyjni, którzy logowanie z wewnątrz organizacji będzie pominąć weryfikacji dwuetapowej przy użyciu oświadczenia wydane przez usługi AD FS.</li><br><li>Określonych zakresów adresów IP — Administratorzy mogą określić zakres adresów IP, które można pominąć weryfikacji dwuetapowej dla użytkowników, którzy są logujący się z sieci intranet. |

To obejście działa tylko z wewnątrz sieci intranet firmy. 

**Środowisko użytkownika końcowego w sieci corpnet:**

Po wyłączeniu zaufanych adresów IP weryfikacji dwuetapowej jest wymagane dla przepływów przeglądarki, a hasła aplikacji są wymagane dla starszych aplikacji wzbogaconego klienta. 

Po włączeniu zaufanych adresów IP, weryfikacja dwuetapowa to *nie* wymagane dla przepływów przeglądarki. Hasła aplikacji są *nie* wymagane starszych aplikacji wzbogaconego klienta, pod warunkiem, że użytkownik już nie utworzyła hasła aplikacji. Po użyciu hasła aplikacji jest używany, pozostaje wymagane. 

**Corpnet poza środowisko użytkownika końcowego:**

Czy zaufanych adresów IP jest włączone, weryfikacja dwuetapowa jest wymagane dla przepływów przeglądarki, a hasła aplikacji są wymagane dla starszych aplikacji wzbogaconego klienta. 

### <a name="to-enable-trusted-ips"></a>Aby włączyć zaufanych adresów IP
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do strony ustawień usługi MFA, zgodnie z instrukcjami na początku tego artykułu.
3. Na stronie ustawień usługi w obszarze zaufanych adresów IP dostępne są dwie opcje:
   
   * **Żądania od użytkowników federacyjnych pochodzące z moim intranecie** — pole wyboru. Federacyjna wszystkich użytkowników, którzy są logujący się z siecią firmową weryfikacji dwuetapowej obejścia, za pomocą oświadczenia wydane przez usługi AD FS.
   * **Dla żądań z określonego zakresu publicznych adresów IP** — wprowadź adresy IP w polu tekstowym za pomocą notacji CIDR. Na przykład: xxx.xxx.xxx.0/24 dla adresów IP w zakresie xxx.xxx.xxx.1 — xxx.xxx.xxx.254 lub xxx.xxx.xxx.xxx/32 dla jednego adresu IP. Możesz wprowadzić maksymalnie 50 zakresów adresów IP. Użytkownicy, którzy zalogować się w tych adresów IP pominąć weryfikacji dwuetapowej.
4. Kliknij pozycję **Zapisz**.
5. Po zastosowaniu aktualizacji kliknij **Zamknij**.

![Zaufane adresy IP](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Hasła aplikacji
Niektóre aplikacje, takich jak pakiet Office 2010 lub starszy i Apple Mail nie obsługują weryfikacji dwuetapowej. Nie są one skonfigurowane do akceptowania drugi weryfikacji. Aby korzystać z tych aplikacji, należy użyć "haseł aplikacji" zamiast tradycyjnych hasła. Hasła aplikacji umożliwia aplikacji pominąć weryfikację dwuetapową i kontynuować pracę.

> [!NOTE]
> Nowoczesne uwierzytelnianie dla klientów pakietu Office 2013
> 
> Klienci Office 2013 (w tym programu Outlook) i nowszych protokołów nowoczesnego uwierzytelniania pomocy technicznej i można ją włączyć do pracy z weryfikacji dwuetapowej. Po włączeniu hasła aplikacji nie są wymagane dla tych klientów.  Aby uzyskać więcej informacji, zobacz [pakietu Office 2013 nowoczesnego uwierzytelniania anonsowania publicznej wersji](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Ważnych rzeczy, aby dowiedzieć się o hasłach aplikacji
Poniżej przedstawiono listę ważnych rzeczy, o których należy wiedzieć o hasłach aplikacji.

* Hasła aplikacji powinny być wprowadzane w polu wejściowym raz w każdej aplikacji. Użytkownicy nie muszą rejestrować i wprowadzić je za każdym razem.
* Rzeczywiste hasło jest generowane automatycznie i nie jest dostarczony przez użytkownika. Hasło generowane automatycznie jest trudniej odgadnąć osobie atakującej i większe bezpieczeństwo.
* Istnieje limit 40 haseł na użytkownika. 
* Aplikacje, których buforowanie haseł, używany w scenariuszach lokalnych mogą przestać działać, ponieważ hasła aplikacji nie jest znana poza identyfikatora organizacyjnego. Przykładem jest wiadomości e-mail programu Exchange, które są lokalne, ale poczta jest archiwizowana w chmurze. To samo hasło nie działa.
* Po uruchomieniu usługi Multi-Factor authentication, można użyć hasła, z niektórych klientów korzystających z przeglądarki. Nie można użyć hasła aplikacji dla zadań administracyjnych. Upewnij się, możesz utworzyć konto usługi z silne hasło, aby uruchamiać skrypty programu PowerShell i nie należy włączać tego konta na potrzeby weryfikacji dwuetapowej.

> [!WARNING]
> Hasła aplikacji nie działają w środowiskach hybrydowych, w którym klienci komunikować się z lokalnie i w chmurze punkty końcowe wykrywania automatycznego. Hasła domeny są wymagane do uwierzytelniania lokalnych i hasła aplikacji są wymagane do uwierzytelniania w chmurze.

### <a name="naming-guidance-for-app-passwords"></a>Wskazówki dotyczące haseł aplikacji nazewnictwa
Nazwy haseł aplikacji odzwierciedlały urządzenie, na którym są używane. Na przykład, jeśli użytkownik ma komputer przenośny z zainstalowanymi korzystających z przeglądarki, utworzyć hasło aplikacji o nazwie komputer przenośny i użyć tego hasła aplikacji. Następnie utwórz innego hasła aplikacji o nazwie pulpitu dla tej samej aplikacji na komputerze stacjonarnym. 

Firma Microsoft zaleca utworzenie jednego hasła aplikacji na urządzenie, nie jednego hasła aplikacji na aplikację.

### <a name="federated-sso-app-passwords"></a>Hasła aplikacji federacyjnych (SSO)
Usługi Azure AD obsługuje Federacji (logowanie jednokrotne) z lokalnego systemu Windows serwera usług domenowych Active Directory (AD DS). Jeśli Twoja organizacja jest Sfederowane przy użyciu usługi Azure AD i zamierzasz używać usługi Azure Multi-Factor Authentication, informacji o hasłach aplikacji jest ważne. Ta sekcja dotyczy tylko dla klientów usługi federacyjnej (SSO).

* Hasła aplikacji są weryfikowane przez usługę Azure AD i w związku z tym obejście federacji. Federacyjnej jest aktywnie stosowana tylko podczas konfigurowania haseł aplikacji.
* Dla użytkowników federacyjnych (SSO) firma Microsoft nigdy nie przejdź do dostawcy tożsamości (IdP) inaczej niż w przypadku przepływu pasywnego. Hasła są przechowywane w identyfikatora organizacyjnego. Gdy użytkownik opuści firmę, tego informacje zostaną przekazane do identyfikatora organizacyjnego przy użyciu narzędzia DirSync w czasu rzeczywistego. Wyłączenie/usunięcie konta może potrwać do trzech godzin, aby zsynchronizować, opóźnienie wyłączenia/usunięcia hasła aplikacji w usłudze Azure AD.
* Ustawienia lokalnej kontroli dostępu klienta nie są uznawane przez hasło aplikacji.
* Bez uwierzytelniania lokalnego możliwości rejestrowania/inspekcji jest dostępna dla hasła aplikacji.
* Niektórych zaawansowanych rozwiązań architektury może wymagać kombinację nazwy organizacji użytkownika, hasła i haseł aplikacji, gdy z klientami przy użyciu weryfikacji dwuetapowej. Chociaż zależy gdzie uwierzytelniania. Dla klientów, którzy uwierzytelnić infrastrukturę lokalną użyje organizacyjnej nazwy użytkownika i hasła. Dla klientów, którzy uwierzytelniania usługi Azure AD można użyć hasła aplikacji.

  Na przykład załóżmy, że masz architekturę, która składa się z tych wystąpień:

  * Federacji lokalnego wystąpienia usługi Active Directory z usługą Azure AD
  * Korzystasz z usługi Exchange online
  * W przypadku korzystania z Lync, będący w szczególności lokalnego
  * Używasz usługi Azure Multi-Factor Authentication

  ![Biurowego](./media/multi-factor-authentication-whats-next/federated.png)

  W takich przypadkach należy wykonać następujące kroki:

  * Gdy logowanie się do usługi Lync, użyj nazwy użytkownika i hasła Twojej organizacji.
  * Podczas próby uzyskania dostępu w książce adresowej za pomocą klienta programu Outlook, który łączy się z programem Exchange online, należy użyć hasła aplikacji.

### <a name="allow-app-password-creation"></a>Zezwalaj na tworzenie haseł aplikacji
Domyślnie użytkownicy nie mogą tworzyć hasła aplikacji, ale można tę funkcję można włączyć. Aby umożliwić użytkownikom tworzenie haseł aplikacji, należy użyć następującej procedury:

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do strony ustawień usługi MFA, zgodnie z instrukcjami na początku tego artykułu.
3. Zaznacz przycisk radiowy obok **Zezwalaj użytkownikom na tworzenie haseł aplikacji do logowania się do aplikacji niekorzystających z przeglądarki**.

![Tworzenie haseł aplikacji](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Tworzenie haseł aplikacji
Użytkownicy mogą tworzyć hasła aplikacji podczas ich początkowe rejestracyjny. Otrzymują one opcję po zakończeniu procesu rejestracji, która pozwala na tworzenie haseł aplikacji.

Użytkownicy mogą także tworzyć hasła aplikacji po rejestracji. Zmieniając ich ustawień w portalu Azure lub w portalu usługi Office 365 mogą utworzyć hasła aplikacji. Aby uzyskać więcej informacji i uzyskać szczegółowe instrukcje dla użytkowników, zobacz [co to są hasła aplikacji w usłudze Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Należy pamiętać, uwierzytelnianie wieloskładnikowe dla użytkowników zaufanych urządzeniach
Zapamiętywanie uwierzytelniania wieloskładnikowego dla urządzeń i przeglądarki czy zaufanie użytkowników jest bezpłatne funkcji dla wszystkich użytkowników usługi MFA. Uwierzytelnianie wieloskładnikowe umożliwia użytkownikom obejście uwierzytelniania Wieloskładnikowego przez liczbę dni, po zalogowaniu. Ta funkcja rozszerza użyteczność, minimalizując liczbę razy użytkownik może wykonywać weryfikacji dwuetapowej na tym samym urządzeniu.

Jednak w przypadku złamania zabezpieczeń konta lub urządzenia, zapamiętywanie uwierzytelniania Wieloskładnikowego na zaufanych urządzeniach może mieć wpływ na bezpieczeństwo. Jeśli zostanie naruszone bezpieczeństwo konta firmowego lub zaufanego urządzenia utraty lub kradzieży, musisz [Przywróć uwierzytelnianie wieloskładnikowe na wszystkich urządzeniach](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Ta akcja odwołuje zaufanego stanu ze wszystkich urządzeń, a użytkownik jest wymagane do przeprowadzenia weryfikacji dwuetapowej ponownie. Można również Poinstruuj użytkowników, aby przywrócić MFA na urządzeniach z instrukcjami w [zarządzać ustawieniami na potrzeby weryfikacji dwuetapowej](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Jak to działa

Pamiętaj o tym, usługa Multi-Factor Authentication działa przez ustawienie trwały plik cookie w przeglądarce, gdy użytkownik "nie pytaj ponownie o **X** dni" pole podczas logowania. Nie jest monitowany o podanie MFA ponownie z tej przeglądarki do momentu wygaśnięcia pliku cookie. Jeśli użytkownik otwiera innej przeglądarki, w tym samym urządzeniu lub czyści ich pliki cookie, są monitowani o ponownie zweryfikować. 

"Nie pytaj ponownie o **X** dni" pole wyboru nie jest widoczne w aplikacjach korzystających z przeglądarki, czy obsługują one nowoczesnego uwierzytelniania. Te aplikacje używaj tokenów odświeżania, zapewniające nowe tokeny dostępu, co godzinę. Podczas sprawdzania poprawności tokenu odświeżania usługi Azure AD sprawdza, czy ostatni weryfikacji dwuetapowej czasu został wykonany w ciągu o skonfigurowaną liczbę dni. 

Podsumowując Pamiętaj o tym uwierzytelniania Wieloskładnikowego na zaufanych urządzeniach zmniejsza liczbę prób uwierzytelnienia w usłudze aplikacje sieci web (które zwykle Pytaj za każdym razem). Można jednak również zwiększa liczbę uwierzytelnień dla klientów nowoczesnego uwierzytelniania, (które zwykle poproś co 90 dni).

> [!NOTE]
>Ta funkcja nie jest zgodny z funkcją "Wylogowuj mnie" usług AD FS, gdy użytkownicy wykonują weryfikacji dwuetapowej dla usług AD FS za pomocą serwera usługi Azure MFA lub rozwiązanie MFA innych firm. Jeśli użytkownicy w usługach AD FS wybierz opcję "Wylogowuj mnie" i zaznaczyć swoje urządzenia jako zaufane dla usługi MFA, nie będzie mógł zweryfikować po wygaśnięciu "Pamiętaj MFA" liczbę dni. Weryfikacja dwuetapowa świeże żądań usługi Azure AD, ale usług AD FS zwraca token z oryginalnego oświadczeń MFA i Data zamiast wykonywania ponownie weryfikacji dwuetapowej. Ten proces powoduje ustawienie poza pętlę weryfikacji między Azure AD i usług AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Włączanie uwierzytelniania wieloskładnikowego Zapamiętaj
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do strony ustawień usługi MFA, zgodnie z instrukcjami na początku tego artykułu.
3. Na stronie ustawień usługi, w obszarze zarządzania ustawieniami urządzenia użytkownika, sprawdź **Zezwalaj użytkownikom na zapamiętywanie danych uwierzytelniania wieloskładnikowego na zaufanych urządzeniach** pole.
   ![Należy pamiętać, urządzenia](./media/multi-factor-authentication-whats-next/remember.png)
4. Ustaw liczbę dni, które chcesz zezwolić na zaufanych urządzeniach pominąć weryfikacji dwuetapowej. Wartość domyślna to 14 dni.
5. Kliknij pozycję **Zapisz**.
6. Kliknij przycisk **Zamknij**.

### <a name="mark-a-device-as-trusted"></a>Oznacz jako zaufanego urządzenia

Po włączeniu tej funkcji, użytkowników można oznaczyć jako zaufane, gdy urządzenie zalogują się sprawdzając **nie pytaj ponownie**.

![Nie pytaj ponownie — zrzut ekranu](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Metody wyboru weryfikacji
Można wybrać metody weryfikacji, które są dostępne dla użytkowników. Poniższa tabela zawiera krótkie omówienie każdej metody.

Gdy użytkownicy rejestrują swoje konta dla usługi MFA, decydują ich metodę weryfikacji preferowanego poza opcje, które są włączone. Wskazówki dotyczące procesu rejestracji znajdują się w [Skonfiguruj moje konto na potrzeby weryfikacji dwuetapowej](multi-factor-authentication-end-user-first-time.md)

| Metoda | Opis |
|:--- |:--- |
| Połączenie z telefonem |Umieszcza wykonywane automatyczne połączenie głosowe. Użytkownik odbierze połączenie i naciska klawisz # na klawiaturze telefonu w celu uwierzytelnienia. Ten numer telefonu nie jest zsynchronizowany do lokalnej usługi Active Directory. |
| SMS na telefon |Wysyła wiadomość tekstową zawierającą kod weryfikacyjny. Użytkownik jest monitowany o odpowiedź na tekst wiadomości z kodu weryfikacyjnego lub wprowadź kod weryfikacyjny w interfejsie logowania. |
| Powiadomienie przez aplikację mobilną |Wysyła powiadomienia wypychanego na telefonie lub zarejestrowanym urządzeniem. Użytkownik wyświetla powiadomienie i wybiera **Sprawdź** aby ukończyć weryfikację. <br>Jest dostępna dla aplikacji Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Kod weryfikacyjny z aplikacji mobilnej |Aplikacja Microsoft Authenticator generuje nowy kod OATH weryfikacji co 30 sekund. Użytkownik wprowadza ten kod weryfikacyjny w interfejsie logowania.<br>Jest dostępna dla aplikacji Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Jak włączyć/wyłączyć metody uwierzytelniania
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Przejdź do strony ustawień usługi MFA, zgodnie z instrukcjami na początku tego artykułu.
3. Na stronie ustawień usługi w obszarze Opcje weryfikacji wybrać/anulować wybór opcji, które chcesz użyć.
   ![Opcje weryfikacji](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Kliknij pozycję **Zapisz**.
5. Kliknij przycisk **Zamknij**.
