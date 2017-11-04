---
title: "Azure Active Directory przekazywanego uwierzytelniania zabezpieczeń szczegółowo | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak uwierzytelnianie przekazywane do usługi Azure Active Directory (Azure AD) chroni konta lokalnego."
services: active-directory
keywords: "Azure AD Connect przekazywanego uwierzytelniania, instalacji usługi Active Directory, wymaganych składników dla usługi Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.openlocfilehash: a5feadd851b166d0a9a77bd1d124cdd599d5d701
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory przekazywanego uwierzytelniania zabezpieczeń szczegółowo

Ten artykuł zawiera bardziej szczegółowy opis dotyczące sposobu działania uwierzytelniania przekazywanego. Zespoły więcej w zakresie ochrony funkcji. W tym temacie będzie interesujące zabezpieczeń i administratorów IT, oficerów dyrektora zgodności i zabezpieczeń i innym specjalistom IT odpowiedzialnych za bezpieczeństwa i zgodności w organizacji małych i średnich i dużych przedsiębiorstwach.

Tematy skierowana obejmują:
- Szczegółowe informacje techniczne dotyczące sposobu zainstalowanego i zarejestrowanego agentów uwierzytelniania.
- Szczegółowe informacje techniczne dotyczące szyfrowania hasła podczas logowania użytkownika.
- Zabezpieczenia kanały między lokalnymi agentów uwierzytelniania i Azure Active Directory (Azure AD).
- Szczegółowe informacje techniczne dotyczące sposobu uwierzytelniania agenci są zabezpieczone pod względem.
- Inne tematy związane z zabezpieczeniami.

## <a name="key-security-capabilities"></a>Funkcje zabezpieczeń klucza

Są to bezpieczeństwo kluczy aspektów tej funkcji:
- Jest on oparty na bezpieczną architekturę dzierżawcza multi, który zapewnia izolację żądań logowania między dzierżawcami.
- Lokalne hasła nigdy nie są przechowywane w chmurze w jakimkolwiek formularzu.
- Agentów uwierzytelniania lokalnego, które nasłuchiwania i odpowiadać na żądania sprawdzania poprawności hasła, należy tylko połączenia wychodzące z sieci. Nie jest wymagane do zainstalowania tych agentów uwierzytelniania w sieci obwodowej (DMZ).
- Tylko na standardowe porty (80 i 443) są używane dla komunikacji wychodzącej czynnikami uwierzytelniania usługi Azure AD. Muszą zostać otwarty na zaporze nie portów przychodzących. 
  - Port 443 jest używany do cała komunikacja wychodząca uwierzytelnionego
  - Port 80 jest używana tylko pobierania odwołania list certyfikatów (CRL), aby upewnić się, że żaden z tych certyfikatów używanych przez funkcję zostały odwołane.
  - Zobacz [tutaj](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) uzyskać pełną listę wymagań sieciowych.
- Hasła podanego przez użytkownika podczas logowania są szyfrowane w chmurze, zanim zostanie zaakceptowany przez lokalnego uwierzytelniania agentów na weryfikację pod kątem usługi Active Directory.
- Kanał HTTPS między usługą Azure AD i lokalnymi Agent uwierzytelniania jest chroniony przez uwierzytelnianie wzajemne.
- Go bezproblemowo integruje się z funkcji ochrony chmury usługi Azure AD, takich jak zasady dostępu warunkowego (w tym usługi Multi-Factor Authentication), ochrony tożsamości i blokady inteligentnej.

## <a name="components-involved"></a>Składniki zaangażowane

Aby uzyskać ogólne informacje o usłudze operational, usługi Azure AD i bezpieczeństwa danych, zobacz [Centrum zaufania](https://azure.microsoft.com/support/trust-center/). Przekazywanego uwierzytelniania jest używana dla logowania użytkownika obejmuje następujące składniki:
- **Azure AD STS**: bezstanowej zabezpieczenia tokenu usługi (STS) przetwarza żądania logowania, która wystawia tokeny zabezpieczające do przeglądarek użytkowników, klientów lub usług, zgodnie z wymaganiami.
- **Usługa Azure Service Bus**: zapewnia włączoną obsługę chmury komunikacji z obsługą wiadomości przedsiębiorstwa i komunikacji przekaźniki, która ułatwia łączenie rozwiązań lokalnych z chmurą.
- **Agent Azure AD Connect uwierzytelniania (Agent uwierzytelniania)**: składnik lokalnymi nasłuchuje i odpowiada na żądania sprawdzania poprawności hasła.
- **Baza danych SQL Azure**: przechowuje informacje dotyczące agentów uwierzytelniania swojej dzierżawy, łącznie z jej metadane i kluczy szyfrowania.
- **Active Directory (AD)**: lokalnej usługi Active Directory, gdzie są przechowywane kont użytkowników (i haseł).

## <a name="installation-and-registration-of-authentication-agents"></a>Instalowanie i rejestrowanie agentów uwierzytelniania

Agenci uwierzytelniania są zainstalowane i zarejestrowane w usłudze Azure AD podczas możesz [włączenia uwierzytelniania przekazywanego za pomocą usługi Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature) lub gdy możesz [dodać dodatkowe agentów uwierzytelniania, aby zapewnić wysoką dostępność z żądania rejestracji](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability). Pobieranie pracy Agent uwierzytelniania obejmuje trzy główne fazy:

- Instalacja agenta uwierzytelniania
- Rejestracja agenta uwierzytelniania
- Inicjowanie agenta uwierzytelniania

Każdy z tych zostanie dokładnie omówione szczegółowo w następujących tematach.

### <a name="authentication-agent-installation"></a>Instalacja agenta uwierzytelniania

Tylko administratorzy globalni można zainstalować agenta uwierzytelniania (przy użyciu usługi Azure AD Connect lub autonomiczne) na serwerze lokalnym. Instalacja dodaje te dwa nowe wpisy, aby **Panel sterowania -> programy -> programy i funkcje** listy:
- Agent uwierzytelniania aplikacji. Ta funkcja jest uruchamiana z [usługi sieciowej](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) uprawnienia.
- Aplikacja aktualizacji używane do automatycznego aktualizowania Agent uwierzytelniania. Ta funkcja jest uruchamiana z [systemu lokalnego](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) uprawnienia.


### <a name="authentication-agent-registration"></a>Rejestracja agenta uwierzytelniania

Po zainstalowaniu agenta uwierzytelniania, musi się zarejestrować w usłudze Azure AD. Usługi Azure AD robi to przez przypisanie każdego agenta uwierzytelnianie certyfikatu tożsamości cyfrowych, której można użyć do zapewnienia bezpiecznej komunikacji z usługą Azure AD.

Procedury rejestracji także wiąże Agent uwierzytelniania z dzierżawą będzie wówczas traktował tej usługi Azure AD, że ten określonych Agent uwierzytelniania jest tylko jeden autoryzowane do obsługi żądań sprawdzania poprawności hasła dla dzierżawy. Czynności te należy powtórzyć dla każdego nowego agenta uwierzytelniania należy zarejestrować.

Oto, jak agenci uwierzytelniania rejestrują się z usługą Azure AD:

![Rejestracja agenta](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Usługi Azure AD po raz pierwszy żąda logować się do usługi Azure AD przy użyciu poświadczeń administratora globalnego. Podczas logowania Agent uwierzytelniania uzyskuje dostęp do tokenu której można użyć w imieniu administratora globalnego.
2. Następnie Agent uwierzytelniania generuje parę kluczy — klucz publiczny i klucz prywatny.
    - Tej pary kluczy jest generowany przy użyciu standardu **RSA 2048-bitowego** szyfrowania.
    - Klucz prywatny nigdy nie opuszcza lokalnego serwera, na którym został zainstalowany Agent uwierzytelniania.
3.  Agent uwierzytelniania wysyła żądanie "rejestracji" do usługi Azure AD przy użyciu protokołu HTTPS, z następujących składników, które są zawarte w żądaniu:
    - Token dostępu uzyskaną w kroku 1.
    - Klucz publiczny wygenerowany w kroku 2.
    - A **żądania podpisywania certyfikatu** (CSR lub żądania certyfikatu). Ma to zastosowanie certyfikatu tożsamości cyfrowych, za pomocą usługi Azure AD jako jego urzędu certyfikacji.
4. Usługi Azure AD weryfikuje Token dostępu w żądaniu rejestracji i sprawdza, czy żądanie pochodzi z administratora globalnego.
5. Usługi Azure AD, a następnie podpisuje i wystawia certyfikat tożsamości cyfrowych Agent uwierzytelniania.
    - Certyfikat jest podpisany przy użyciu **usługi Azure AD głównego urzędu certyfikacji,**. Należy zauważyć, że ten urząd certyfikacji _nie_ w systemie Windows **zaufane główne urzędy certyfikacji** przechowywania.
    - Ten urząd certyfikacji jest używany tylko przez funkcję uwierzytelniania przekazywanego. Po prostu służy do obsługi podpisywania podczas uwierzytelniania agenta rejestracji.
    - Ten urząd certyfikacji nie jest używany przez inne usługi w usłudze Azure AD.
    - Podmiot certyfikatu (**nazwa wyróżniająca lub nazwa Wyróżniająca**) ma ustawioną wartość Twojej **identyfikator dzierżawcy**. Jest to identyfikator GUID, który unikatowo identyfikuje dzierżawców. Pozwoli to zawęzić zakres certyfikat do użytku z dzierżawą tylko.
6. Usługi Azure AD przechowuje klucz publiczny Agent uwierzytelniania w bazie danych Azure SQL, która tylko usługi Azure AD ma dostęp do.
7. Certyfikat (wydane w kroku 5) jest przechowywany na serwerze lokalnym w **magazynu certyfikatów systemu Windows** (w szczególności w  **[CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE)**  Lokalizacja) i jest używana przez agenta uwierzytelniania i aktualizacji aplikacji.

### <a name="authentication-agent-initialization"></a>Inicjowanie agenta uwierzytelniania

Po uruchomieniu agenta uwierzytelniania po raz pierwszy po rejestracji lub serwerze uruchom ponownie, jego musi mieć możliwość bezpiecznego komunikowania się z usługą Azure AD i uruchomić akceptować żądania sprawdzania poprawności hasła.

![Inicjowanie agenta](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Oto, jak są inicjowane agentów uwierzytelniania:



1. Agent uwierzytelniania rozpoczyna się od ruchu wychodzącego "ładowania początkowego" żądaniu skierowanym do usługi Azure AD. 
    - To żądanie inicjowania jest nawiązywane za pośrednictwem portu 443 i jest za pośrednictwem uwierzytelnionego wzajemnie kanału HTTPS (przy użyciu ten sam certyfikat wystawiony podczas rejestracji agenta uwierzytelniania).
2. Usługi Azure AD odpowiada na żądania ładowania początkowego zapewniając **klucz dostępu** do kolejki usługi Azure Service Bus, która jest unikatowa dla Twojej dzierżawy (identyfikowanego przez identyfikator dzierżawy).
3. Agent uwierzytelniania tworzy trwały wychodzące połączenie HTTPS (przez port 443) kolejki. 
    - Jest teraz gotowy do pobierania i obsługiwać żądania sprawdzania poprawności hasła.

Jeśli masz wielu agentów uwierzytelniania zarejestrowane na Twojej dzierżawy, a następnie procedura inicjowania zapewnia, że każdy z nich łączy się z tej samej kolejki usługi Azure Service Bus. 

## <a name="processing-sign-in-requests"></a>Przetwarzanie żądań logowania 

Na poniższym diagramie przedstawiono sposób uwierzytelniania przekazywanego przetwarza żądania logowania użytkownika.

![Przetwarzanie logowania](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Uwierzytelniania przekazywanego obsługi żądania logowania użytkownika w następujący sposób: 

1. Użytkownik próbuje uzyskać dostęp do aplikacji (na przykład Outlook Web App - [https://outlook.office365.com/owa](https://outlook.office365.com/owa).)
2. Jeśli użytkownik nie jest już zalogowany, aplikacja przekierowuje przeglądarki do strony logowania usługi Azure AD.
3. Usługa STS usługi Azure AD odpowiada z powrotem na stronę logowania użytkownika.
4. Użytkownik wprowadza nazwę użytkownika i hasło do strony logowania usługi Azure AD i kliknie przycisk "Zaloguj".
5. Nazwa użytkownika i hasło są przesyłane do usługi Azure AD STS w żądaniu POST protokołu HTTPS.
6. Usługi Azure AD STS pobiera klucze publiczne dla wszystkich agentów uwierzytelniania zarejestrowane na Twojej dzierżawy z bazy danych Azure SQL i szyfrowanie hasła za ich pomocą. 
    - Generuje zaszyfrowane hasło "n" wartości "n" uwierzytelnianie agentów zarejestrowane na Twojej dzierżawy.
7. Usługi Azure AD STS umieszcza żądania (nazwa użytkownika i wartości zaszyfrowane hasło) sprawdzania poprawności hasła na kolejki usługi Azure Service Bus przeznaczony dla Twojej dzierżawy.
8. Ponieważ zainicjowane agentów uwierzytelniania trwale są podłączone do kolejki usługi Azure Service Bus, pobiera jeden z dostępnych agentów uwierzytelniania żądania sprawdzenia poprawności hasła.
9. Agent uwierzytelniania lokalizuje wartość zaszyfrowane hasło, która jest specyficzna dla swojego klucza publicznego (przy użyciu identyfikatora) i odszyfrowuje ją przy użyciu jego klucz prywatny.
10. Agent uwierzytelniania podejmuje próbę zweryfikowania nazwy użytkownika i hasła przed potencjalnym użyciem lokalnej usługi Active Directory  **[Win32 API funkcji LogonUser](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)**  (z **dwLogonType** ustawiona **LOGON32_LOGON_NETWORK**). 
    - Jest to tego samego interfejsu API używany przez usługi Active Directory Federation Services (AD FS) do logowania użytkowników w federacyjnym scenariuszu logowania.
    - To zależy od procesu rozpoznawania standardowego w systemie Windows Server, aby zlokalizować sterownika domeny.
11. Agent uwierzytelniania odbiera wynik z usługi Active Directory (Powodzenie, nazwa użytkownika lub hasło jest niepoprawne hasło wygasło, użytkownik jest zablokowany, i tak dalej).
12. Agent uwierzytelniania przesyła wynik do usługi Azure AD STS za pośrednictwem uwierzytelnionego wzajemnie wychodzącego kanału HTTPS przez port 443. Uwierzytelnianie wzajemne używa tego samego certyfikatu wcześniej wydane agentowi uwierzytelnianie podczas rejestracji.
13. Usługi Azure AD STS sprawdza, czy wynik skorelowany z określonym żądaniem logowania w dzierżawie.
14. Usługi Azure AD STS będzie kontynuowane przy użyciu procedury logowania, zgodnie z konfiguracją. Na przykład jeśli sprawdzenie poprawności hasła zakończyło się pomyślnie, użytkownik może być wezwał uwierzytelnianie wieloskładnikowe lub przekierowany z powrotem do aplikacji.

## <a name="operational-security-of-authentication-agents"></a>Bezpieczeństwa operacyjnego agentów uwierzytelniania

Aby upewnić się, że uwierzytelniania przekazywanego pozostaje pod względem bezpiecznego, usługi Azure AD okresowo odnawia swoich certyfikatów. Odnowienia te są wyzwalane z usługi Azure AD i nie są regulowane przez siebie agentów uwierzytelniania.

![Bezpieczeństwo działania](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Oto, jak Agent uwierzytelniania odnawia jego zaufania z usługą Azure AD:

1. Agent uwierzytelniania okresowo wysyła pakiet usługi ping Azure AD (co kilka godzin), aby sprawdzić, czy jest może odnowić swój certyfikat. 
    - To sprawdzenie odbywa się za pośrednictwem uwierzytelnionego wzajemnie kanału HTTPS (przy użyciu ten sam certyfikat wystawiony podczas rejestracji).
2. Jeśli usługa wskazuje, czy nadszedł czas, aby odnowić, Agent uwierzytelniania generuje parę kluczy: klucz publiczny i klucz prywatny.
    - Klucze te są generowane przy użyciu standardowego **RSA 2048-bitowego** szyfrowania.
    - Klucz prywatny nigdy nie opuszcza serwera lokalnego.
3. Agent uwierzytelniania następnie żąda "odnawiania certyfikatu" do usługi Azure AD przy użyciu protokołu HTTPS, z następujących składników, które są zawarte w żądaniu:
    - Istniejący certyfikat (pobierane z **CERT_SYSTEM_STORE_LOCAL_MACHINE** lokalizacji w magazynie certyfikatów systemu Windows). Brak nie administratora globalnego związane z tej procedury, więc nie ma potrzeby imieniu administratora globalnego nie tokenu dostępu.
    - Klucz publiczny wygenerowany w kroku 2.
    - A **żądania podpisywania certyfikatu** (CSR lub żądania certyfikatu). Ma to zastosowania nowego certyfikatu tożsamości cyfrowych, z usługą Azure AD jako jego urzędu certyfikacji.
4. Usługi Azure AD weryfikuje istniejącego certyfikatu w żądanie odnowienia certyfikatu. Następnie weryfikuje, czy żądanie pochodzi z agenta uwierzytelniania zarejestrowane na Twojej dzierżawy.
5. W przypadku istniejący certyfikat jest nadal ważny, usługa Azure AD następnie podpisuje nowego certyfikatu tożsamości cyfrowych i wystawia nowego świadectwa Agent uwierzytelniania. 
6. Jeśli istniejący certyfikat wygasł, usługi Azure AD Usuwa agenta uwierzytelniania z Twojej dzierżawy listę zarejestrowanych agentów uwierzytelniania. Następnie Administrator globalny musi ręcznie zainstaluj i Zarejestruj nowy Agent uwierzytelniania.
    - Certyfikat jest podpisany przy użyciu **usługi Azure AD głównego urzędu certyfikacji,**.
    - Podmiot certyfikatu (**nazwa wyróżniająca lub nazwa Wyróżniająca**) ma ustawioną wartość Twojej **identyfikator dzierżawcy** identyfikator GUID, który unikatowo identyfikuje dzierżawców. Oznacza to, że certyfikat obejmuje tylko dzierżawy.
6. Usługi Azure AD przechowuje klucz publiczny "new" Agent uwierzytelniania w bazie danych Azure SQL, którą tylko ma dostęp do. I unieważnia "starego" klucza publicznego skojarzonego z agentem uwierzytelniania.
7. Nowy certyfikat (który został wystawiony w kroku 5) jest następnie przechowywane na serwerze w **magazynu certyfikatów systemu Windows** (w szczególności w  **[CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER)**  Lokalizacja).
    - Ponieważ procedurę odnowienia zaufania występuje nieinteraktywnie (bez obecności administratora globalnego), Agent uwierzytelniania nie ma już dostępu do aktualizacji istniejącego certyfikatu w **CERT_SYSTEM_STORE_LOCAL_MACHINE** lokalizacji. Należy pamiętać, że sam certyfikat w **CERT_SYSTEM_STORE_LOCAL_MACHINE** lokalizacji nie jest usuwany podczas wykonywania tej procedury.
8. Nowy certyfikat jest używany do uwierzytelniania z tego punktu na. Każdy z kolejnych odnowienia certyfikatu zastępuje certyfikatu w **CERT_SYSTEM_STORE_LOCAL_MACHINE** lokalizacji.

## <a name="auto-update-of-authentication-agents"></a>Automatyczne aktualizowanie agentów uwierzytelniania

Po wydaniu nowej wersji aplikacji Updater automatycznie aktualizuje Agent uwierzytelniania. Nie obsługuje wszystkie żądania weryfikacji hasła dla dzierżawy. 

Usługi Azure AD obsługuje nowej wersji oprogramowania jako zalogowany **pakietu Instalatora Windows (MSI)**. Plik MSI jest podpisany przy użyciu [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) z **SHA256** jako algorytmu skrótu. 

![Automatyczne aktualizacje](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Oto, jak Agent uwierzytelniania pobiera zaktualizowane automatycznie:

1. Aktualizacji okresowo wysyła pakiet usługi ping Azure AD (co godzinę), aby sprawdzić, czy jest nowa wersja agenta uwierzytelniania. 
    - To sprawdzenie odbywa się za pośrednictwem uwierzytelnionego wzajemnie kanału HTTPS (przy użyciu ten sam certyfikat wystawiony podczas rejestracji). Agent uwierzytelniania i aktualizacji Udostępnianie certyfikatów przechowywanych na serwerze.
2. Jeśli dostępna jest nowa wersja, usługi Azure AD zwraca podpisem MSI do aktualizacji.
3. Aktualizacji sprawdza, czy plik MSI jest podpisany przez firmę Microsoft.
4. Uruchamia Updater MSI. Ta akcja wykonuje następujące czynności (należy pamiętać, że jest uruchamiany aktualizacji [systemu lokalnego](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) uprawnienia):
    - Zatrzymuje usługę agenta uwierzytelniania.
    - Zainstalowana nowa wersja agenta uwierzytelniania na serwerze.
    - Uruchamia ponownie usługę Agent uwierzytelniania.

>[!NOTE]
>Jeśli masz wielu agentów uwierzytelniania zarejestrowane na Twojej dzierżawy usługi Azure AD nie odnawiać swoje certyfikaty lub zaktualizuj je w tym samym czasie. Zamiast tego usługi Azure AD jest stopniowo aby zapewnić wysoką dostępność żądań logowania.


## <a name="next-steps"></a>Następne kroki
- [**Bieżące ograniczenia** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) — Dowiedz się, jakie scenariusze są obsługiwane i zostały.
- [**Szybki Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) — Uzyskaj i systemem Azure AD przekazywanego uwierzytelniania.
- [**Inteligentne blokady** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) — funkcja Konfigurowanie blokady inteligentnej na dzierżawy do ochrony kont użytkowników.
- [**Jak działa** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) — Poznaj podstawy działania usługi Azure AD przekazywanego uwierzytelniania.
- [**Często zadawane pytania** ](active-directory-aadconnect-pass-through-authentication-faq.md) — odpowiedzi na często zadawane pytania.
- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**Azure AD SSO bezproblemowe** ](active-directory-aadconnect-sso.md) — Dowiedz się więcej o tej funkcji uzupełniające.

