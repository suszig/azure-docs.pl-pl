---
title: "Integracja sieci VPN z usługą Azure MFA przy użyciu rozszerzenia zasad Sieciowych | Dokumentacja firmy Microsoft"
description: "W tym artykule omówiono integrowanie infrastruktury sieci VPN z usługą Azure MFA platformy Microsoft Azure przy użyciu rozszerzenia serwera zasad sieciowych (NPS)."
services: active-directory
keywords: "Usługa Azure MFA, integracja sieci VPN, Azure Active Directory, serwer zasad sieciowych rozszerzenia"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 8ac4c5d88e724febf21fe6bcc8ecf939283f361e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a>Integrowanie infrastruktury sieci VPN z usługi Azure Multi-Factor Authentication (MFA) na platformie Azure przy użyciu rozszerzenia serwera zasad sieciowych (NPS)

## <a name="overview"></a>Omówienie

Rozszerzenia serwera zasad sieciowych (NPS) na platformie Azure umożliwia organizacjom chronić uwierzytelniania klienta usługi usługi użytkowników zdalnego uwierzytelniania (RADIUS), za pomocą opartej na chmurze [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), zapewniające weryfikacji dwuetapowej.

Ten artykuł zawiera instrukcje dotyczące integracji z usługą Azure MFA infrastruktury serwera NPS przy użyciu rozszerzenia serwera NPS na platformie Azure, aby włączyć weryfikację dwuetapową bezpieczne dla użytkowników próby nawiązania połączenia z siecią za pośrednictwem sieci VPN. 

Zasad sieciowych i dostępu do usługi (NPS) pozwala organizacjom następujące możliwości:

* Określ lokalizację centralnego zarządzania i kontroli żądania sieci, aby określić, kto może się połączyć, jakich porach dnia mogą przyjmować połączenia czas trwania połączenia i poziom zabezpieczeń klientów musi być używana do łączenia i tak dalej. Zamiast określać te zasady na każdym serwerze sieci VPN lub bramy usług pulpitu zdalnego (RD), te zasady można określić jeden raz w centralnej lokalizacji. Protokołu RADIUS służy do zapewnienia uwierzytelniania, autoryzacji i Ewidencjonowanie scentralizowane. 
* Ustanów i wymuszanie zasad dotyczących kondycji klienta ochrony dostępu do sieci (NAP), które określają, czy urządzenia są przyznawane nieograniczonego lub ograniczonego dostępu do zasobów sieciowych.
* Umożliwiają wymuszające uwierzytelnianie i autoryzację na dostęp metodą 802. 1 punkty dostępu bezprzewodowego z obsługą i przełączników Ethernet.    

Aby uzyskać więcej informacji, zobacz [serwera zasad sieciowych (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Aby zwiększyć bezpieczeństwo i zapewniają wysoki poziom zgodności, organizacje mogą stosować serwera NPS z usługą Azure MFA, aby upewnić się, czy użytkownicy przy użyciu weryfikacji dwuetapowej aby można było nawiązać połączenia z portu wirtualnego na serwerze sieci VPN. Aby użytkownicy mogli uzyskać dostęp użytkownik musi podać ich kombinacja nazwy użytkownika i hasła z informacjami, które użytkownik ma w ich kontroli. Te informacje musi być zaufany i nie jest łatwo zduplikowany, takie jak numer telefonu komórkowego, numer telefonu stacjonarnego aplikacji na urządzeniu przenośnym i tak dalej.

Wcześniej dostępności rozszerzenia serwera zasad Sieciowych na platformie Azure klientów, którzy chcieliby implementuje weryfikację dwuetapową dla zintegrowanego środowiska serwera zasad Sieciowych i usługi Azure MFA było konfigurowania i konserwacji osobny serwer usługi MFA w środowisku lokalnym, zgodnie z opisem w bramy usług pulpitu zdalnego i Azure przy użyciu usługi RADIUS serwera Multi-Factor Authentication.

Dostępność rozszerzenia serwera zasad Sieciowych na platformie Azure pozwala organizacjom wyboru do wdrożenia rozwiązania MFA lokalne lub rozwiązanie MFA oparte na chmurze do bezpiecznego uwierzytelniania klienta RADIUS.
 
## <a name="authentication-flow"></a>Przepływ uwierzytelniania
Gdy użytkownik łączy się do wirtualnego portu na serwerze sieci VPN, muszą najpierw zostać uwierzytelnione przy użyciu różnych protokołów, które umożliwiają użycie kombinacji nazwy użytkownika i hasła i metody uwierzytelniania opartego na certyfikatach. 

Oprócz uwierzytelniania i weryfikacji tożsamości, użytkownicy muszą mieć odpowiednie uprawnienia. W przypadku prostych implementacji te uprawnienia umożliwiające dostęp są ustawiane bezpośrednio na obiekty użytkownika usługi Active Directory. 

 ![Właściwości użytkownika](./media/nps-extension-vpn/image1.png)

W przypadku prostych implementacji każdy serwer sieci VPN lub go przydziela nie zezwala na dostęp na podstawie zasad zdefiniowanych każdego lokalnego serwera sieci VPN.

W implementacji większych i bardziej skalowalne zasady tego Udziel lub Odmów dostępu do sieci VPN są scentralizowane na serwerach usługi RADIUS. W takim przypadku serwer sieci VPN działa jako serwer dostępu (klienta RADIUS), która przekazuje żądania połączenia i konta wiadomości do serwera usługi RADIUS. Aby połączyć portu wirtualnego na serwerze sieci VPN, użytkownicy musi zostać uwierzytelniony i spełniać warunki definiowane centralnie na serwerach usługi RADIUS. 

Gdy rozszerzenia serwera zasad Sieciowych na platformie Azure jest zintegrowany z serwer NPS, przepływ pomyślne uwierzytelnienie jest następujący:

1. Serwer sieci VPN otrzymuje żądanie uwierzytelnienia od użytkownik sieci VPN, który zawiera nazwę użytkownika i hasło, aby połączyć się z zasobem, takich jak sesji pulpitu zdalnego. 
2. Działając jako klient usługi RADIUS, serwer sieci VPN konwertuje żądanie na komunikat żądania dostępu usługi RADIUS i wysyła wiadomość (hasło jest szyfrowane) do serwera RADIUS (NPS), którym zainstalowano rozszerzenia serwera NPS. 
3. Kombinacja nazwy użytkownika i hasła została zweryfikowana w usłudze Active Directory. Jeśli nazwa użytkownika / hasło jest nieprawidłowe, serwer usługi RADIUS wysyła komunikat odmowy dostępu. 
4. Jeśli są spełnione wszystkie warunki określone w żądanie połączenia serwera zasad Sieciowych i zasad sieciowych (na przykład pory dnia lub grupy ograniczeniami członkostwa), żądania uwierzytelniania dodatkowego z usługą Azure MFA wyzwala rozszerzenia serwera NPS. 
5. Usługa Azure MFA komunikuje się z usługą Azure Active Directory, pobiera szczegóły użytkownika, a następnie wykonuje dodatkowego uwierzytelniania przy użyciu metody skonfigurowany przez użytkownika (wiadomości tekstowej, aplikacji mobilnych itd.). 
6. Na powodzenie żądanie uwierzytelniania MFA uwierzytelnianie wieloskładnikowe Azure komunikuje się wynik, który ma rozszerzenie serwera NPS.
7. Po próba połączenia zarówno uwierzytelnieniu i autoryzacji, serwer NPS, w którym zainstalowano rozszerzenia wysyła komunikat udzielenia dostępu RADIUS do serwera sieci VPN (klienta RADIUS).
8. Użytkownik uzyskuje dostęp do wirtualnego portu na serwerze sieci VPN i ustanawia szyfrowane tunel VPN.

## <a name="prerequisites"></a>Wymagania wstępne
Ta sekcja zawiera szczegóły dotyczące wymagań wstępnych konieczne przed Integrowanie usługi Azure MFA z bramy usług pulpitu zdalnego. Przed rozpoczęciem, musi mieć następujące wymagania wstępne w miejscu.

* Infrastrukturę sieci VPN.
* Rola zasad sieciowych i dostępu do usługi (NPS)
* Licencja usługi Azure MFA
* Oprogramowanie Windows Server
* Biblioteki
* Zsynchronizowane usługi Azure AD z lokalnej usługi AD 
* Identyfikator GUID usługi Azure Active Directory

### <a name="vpn-infrastructure"></a>Infrastrukturę sieci VPN.
W tym artykule przyjęto założenie, czy masz pracy infrastrukturę sieci VPN, przy użyciu systemu Microsoft Windows Server 2016 w miejscu i że serwer sieci VPN aktualnie nie skonfigurowano przesyłanie żądań połączeń do serwera RADIUS. Skonfiguruj infrastrukturę sieci VPN do użycia w tym przewodniku centralnego serwera RADIUS.

Jeśli nie masz infrastruktury pracę w miejscu, tej infrastruktury może szybko utworzyć z zachowaniem wskazówek zamieszczonych w wiele samouczki ustawienia sieci VPN, który znajduje się w witrynie firmy Microsoft i innych witryn. 

### <a name="network-policy-and-access-services-nps-role"></a>Rola zasad sieciowych i dostępu do usługi (NPS)

Usługi roli serwera NPS zawiera serwer usługi RADIUS i funkcjonalności klienta. W tym artykule przyjęto założenie, że jest zainstalowana rola serwera NPS na serwer członkowski lub kontroler domeny w danym środowisku. Konfiguracji usługi RADIUS dla konfiguracji sieci VPN, w tym przewodniku. Instalowanie roli serwera NPS na serwerze _innych_ niż serwer sieci VPN.

Informacje na temat instalowania roli serwera zasad Sieciowych usługa systemu Windows Server 2012 lub nowszym, zobacz [zainstalować serwer zasad dotyczących kondycji ochrony dostępu do sieci](https://technet.microsoft.com/library/dd296890.aspx). Zasady dostępu do sieci (NAP) jest przestarzała w systemie Windows Server 2016. Opis najlepszych rozwiązań dla serwera NPS, w tym zalecenie, aby zainstalować serwer zasad Sieciowych na kontrolerze domeny, zobacz [najlepsze rozwiązania dotyczące serwera NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Licencja usługi Azure MFA

Wymagana jest licencja dla usługi Azure MFA, który jest dostępny za pośrednictwem usługi Azure AD Premium, Enterprise Mobility plus Security (EMS) lub subskrypcji usługi MFA. Aby uzyskać więcej informacji, zobacz [sposobu uzyskania usługi Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Do celów testowych, możesz użyć subskrypcji wersji próbnej.

### <a name="windows-server-software"></a>Oprogramowanie Windows Server

Rozszerzenie serwera NPS wymaga systemu Windows Server 2008 R2 z dodatkiem SP1 lub nowszy z zainstalowaną usługą roli serwera NPS. Wszystkie kroki opisane w tym przewodniku zostały wykonane przy użyciu systemu Windows Server 2016.

### <a name="libraries"></a>Biblioteki

Te biblioteki są automatycznie instalowane z rozszerzeniem.

-   [Pakiety redystrybucyjne języka Visual C++ dla programu Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory modułu dla Windows PowerShell w wersji 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory modułu dla środowiska Windows PowerShell jest zainstalowany, jeśli nie jest już obecny za pomocą skryptu konfiguracji uruchomienia w ramach procesu instalacji. Nie istnieje potrzeba do zainstalowania tego modułu wcześniejsze, jeśli nie jest już zainstalowany.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Usługa Azure Active Directory zsynchronizowane z lokalną usługą Active Directory 

Aby użyć rozszerzenia serwera NPS, lokalnych użytkowników musi być zsynchronizowane z usługą Azure Active Directory i włączone uwierzytelnianie wieloskładnikowe. W tym przewodniku założono, że użytkowników lokalnych są zsynchronizowane z usługą Azure Active Directory za pomocą AD Connect. Instrukcje dotyczące włączania uwierzytelniania Wieloskładnikowego użytkownicy znajdują się poniżej.
Aby uzyskać informacje dotyczące usługi Azure AD connect, zobacz [integrację katalogów lokalnych z usługą Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Identyfikator GUID usługi Azure Active Directory 
Aby zainstalować rozszerzenie serwera NPS, musisz znać identyfikator GUID usługi Azure Active Directory. Instrukcje dotyczące znajdowania identyfikator GUID usługi Azure Active Directory znajdują się w następnej sekcji.

## <a name="configure-radius-for-vpn-connections"></a>Konfigurowanie serwera RADIUS dla połączeń sieci VPN

Po zainstalowaniu roli serwera NPS na serwerze członkowskim, należy skonfigurować do uwierzytelniania i autoryzacji klienta sieci VPN, czy żądania połączenia sieci VPN. 

W tej sekcji założono instalacji roli serwera zasad sieciowych, ale nie skonfigurowano go do użytku w infrastrukturze.

>[!NOTE]
>Jeśli masz już działający serwer sieci VPN do uwierzytelniania używa centralny serwer usługi RADIUS, możesz pominąć tę sekcję.
>

### <a name="register-server-in-active-directory"></a>Zarejestruj serwer w usłudze Active Directory
Działał prawidłowo, w tym scenariuszu, serwer zasad Sieciowych musi być zarejestrowane w usłudze Active Directory.

1. Otwórz Menedżera serwera.
2. W Menedżerze serwera kliknij **narzędzia**, a następnie kliknij przycisk **serwera zasad sieciowych**. 
3. W konsoli serwera zasad sieciowych, kliknij prawym przyciskiem myszy **serwer NPS (lokalny)**, a następnie kliknij przycisk **Zarejestruj serwer w usłudze Active Directory**. Kliknij przycisk **OK** dwa razy.

 ![Serwer zasad sieciowych](./media/nps-extension-vpn/image2.png)

4. Pozostaw otwarte do następnej procedury konsoli.

### <a name="use-wizard-to-configure-radius-server"></a>Użyj kreatora, aby skonfigurować serwer usługi RADIUS
Można użyć standardowego (opartej na kreatorze) lub zaawansowanych opcji konfiguracji, aby skonfigurować serwer usługi RADIUS. W tej sekcji założono użycie opcji opartej na Kreatorze konfiguracji standardowej.

1. W konsoli serwera zasad sieciowych, kliknij polecenie **serwer NPS (lokalny)**.
2. W obszarze konfiguracji standardowej, wybierz **serwera RADIUS w celu telefonicznych i połączeń sieci VPN**, a następnie kliknij przycisk **Konfigurowanie sieci VPN lub Dial-Up**.

 ![Konfigurowanie sieci VPN](./media/nps-extension-vpn/image3.png)

3. Wybierz telefonicznych lub typu połączenia sieci prywatnej wirtualnego strony, wybierz **połączenia wirtualnej sieci prywatnej**i kliknij przycisk **dalej**.

 ![Wirtualna sieć prywatna](./media/nps-extension-vpn/image4.png)

4. Na stronie Określ Dial-Up lub serwer sieci VPN kliknij **Dodaj**.
5. W **klienta RADIUS nowe** okno dialogowe, Podaj przyjazną nazwę, wprowadź rozpoznawalną nazwę lub adres IP serwera VPN i wprowadź tajny wspólne hasło. Należy to wspólne hasło tajny długich i złożonych. To hasło, należy zarejestrować potrzebną dla czynności opisane w następnej sekcji.

 ![Nowy klient RADIUS](./media/nps-extension-vpn/image5.png)

6. Kliknij przycisk **OK**, a następnie **dalej**.
7. Na **Konfigurowanie metod uwierzytelniania** Zaakceptuj wybór domyślny (uwierzytelnianie szyfrowane firmy Microsoft w wersji 2 (MS-CHAPv2) lub wybierz inną opcję i kliknij przycisk **dalej**.

  >[!NOTE]
  >Jeśli konfigurujesz protokołu uwierzytelniania rozszerzonego (EAP), należy używać MS-CHAPv2 lub PEAP. Nie protokołu EAP jest obsługiwana.
 
8. Na stronie Określ grupy użytkowników, kliknij przycisk **Dodaj** i wybierz odpowiednią grupę, jeśli taka istnieje. W przeciwnym razie pozostaw pole puste, aby przyznać dostęp wszystkim użytkownikom wybór.

 ![Określ grupy użytkowników](./media/nps-extension-vpn/image7.png)

9. Kliknij przycisk **Dalej**.
10. Na stronie określ filtry IP kliknij **dalej**.
11. Na stronie Określ ustawienia szyfrowania, zaakceptuj ustawienia domyślne, a następnie kliknij przycisk **dalej**.

 ![Określ szyfrowania](./media/nps-extension-vpn/image8.png)

12. Na stronie Określ nazwę obszaru nazwa jest pusta, zaakceptuj ustawienia domyślne, a następnie kliknij przycisk **dalej**.

 ![Określ nazwę obszaru](./media/nps-extension-vpn/image9.png)

13. Kończenie nowego telefoniczne lub połączenia wirtualnej sieci prywatnej i RADIUS strony klientów, wybierz polecenie **Zakończ**.

 ![Zakończenie połączenia](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Sprawdź konfigurację usługi RADIUS
Ta sekcja zawiera szczegóły dotyczące konfiguracji, które zostały utworzone za pomocą kreatora.

1. Na serwerze NPS w konsoli serwera NPS (lokalny), rozwiń węzeł klientów usługi RADIUS, a następnie wybierz **klientów RADIUS**.
2. W okienku szczegółów kliknij prawym przyciskiem myszy klienta usługi RADIUS, które zostały utworzone za pomocą kreatora, a następnie kliknij przycisk **właściwości**. Właściwości klienta RADIUS (serwer sieci VPN) powinny być podobne do tych, pokazano poniżej.

 ![Właściwości sieci VPN](./media/nps-extension-vpn/image11.png)

3. Kliknij przycisk **anulować**.
4. Na serwerze NPS w konsoli serwera NPS (lokalny), rozwiń węzeł **zasady**i wybierz **zasady żądań połączeń**. Powinny pojawić się zasady połączeń sieci VPN, podobny obraz poniżej.

 ![Żądania połączeń](./media/nps-extension-vpn/image12.png)

5. W obszarze zasady, wybierz **zasady sieciowe**. Należy podobny obraz poniżej zasady połączenia wirtualnej sieci prywatnej (VPN).

 ![Właściwości sieci](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a>Skonfiguruj serwer sieci VPN do uwierzytelniania RADIUS
W tej sekcji możesz skonfigurować serwer sieci VPN do uwierzytelniania RADIUS. W tej sekcji założono mają działającej konfiguracji serwera sieci VPN, ale nie skonfigurowano serwer sieci VPN do uwierzytelniania RADIUS. Po skonfigurowaniu serwera sieci VPN, upewnieniu się, że konfiguracja działa zgodnie z oczekiwaniami.

>[!NOTE]
>Jeśli masz już działającej konfiguracji serwera sieci VPN korzysta z uwierzytelniania usługi RADIUS, możesz pominąć tę sekcję.
>

### <a name="configure-authentication-provider"></a>Konfigurowanie dostawcy uwierzytelniania
1. Na serwerze sieci VPN Otwórz Menedżera serwera.
2. W Menedżerze serwera kliknij **narzędzia**, a następnie **Routing i dostęp zdalny**.
3. W konsoli Routing i dostęp zdalny, kliknij prawym przyciskiem myszy  **\[nazwy serwera\] (local)**, a następnie kliknij przycisk **właściwości**.

 ![Routing i dostęp zdalny](./media/nps-extension-vpn/image14.png)
 
4. W **[nazwa serwera} (lokalne) właściwości** okno dialogowe, kliknij przycisk **zabezpieczeń** kartę. 
5. Na **zabezpieczeń** kliknij kartę, w obszarze dostawcy uwierzytelniania **uwierzytelnianie usługi RADIUS**, a następnie **Konfiguruj**.

 ![Uwierzytelnianie usługi RADIUS](./media/nps-extension-vpn/image15.png)
 
6. W oknie dialogowym uwierzytelniania usługi RADIUS kliknij **Dodaj**.
7. W Dodaj serwer usługi RADIUS, w polu Nazwa serwera Dodaj nazwę lub adres IP serwera usługi RADIUS, które skonfigurowano w poprzedniej sekcji.
8. W wspólny klucz tajny kliknij **zmiany** i Dodaj udostępniony poufne hasło utworzone i zarejestrowane wcześniej.
9. W polu Limit czasu (w sekundach), zmień wartość na wartość z zakresu od **30** i **60**. Jest to konieczne umożliwić wystarczająco dużo czasu na ukończenie drugi składnik uwierzytelniania.
 
 ![Dodawanie serwera RADIUS](./media/nps-extension-vpn/image16.png)
 
10. Kliknij przycisk **OK** dopiero po wykonaniu zamknięcie wszystkich okien dialogowych.

### <a name="test-vpn-connectivity"></a>Przetestowanie łączności z siecią VPN
W tej sekcji upewnieniu się, że klient VPN uwierzytelnieniu i autoryzacji przez serwer usługi RADIUS podczas próby nawiązania połączenia sieci VPN portu wirtualnego. W tej sekcji założono, że używasz systemu Windows 10 jako klient sieci VPN. 

>[!NOTE]
>Jeśli skonfigurowano klienta sieci VPN w taki sposób, aby połączyć się z serwerem sieci VPN i zapisano ustawienia, można pominąć kroki dotyczące konfigurowania i zapisywanie obiektu połączenia sieci VPN.
>

1. Na komputerze klienckim sieci VPN kliknij **Start**, a następnie **ustawienia** (koło zębate ikonę).
2. W oknie Ustawienia, kliknij przycisk **sieć i Internet**.
3. Kliknij przycisk **VPN**.
4. Kliknij przycisk **dodać połączenie VPN**.
5. W dodać połączenie VPN, określ systemu Windows (wbudowane) jako dostawcę sieci VPN, a następnie wypełnij pozostałe pola, zgodnie z potrzebami i kliknij **zapisać**. 

 ![Dodaj połączenie VPN](./media/nps-extension-vpn/image17.png)
 
6. Otwórz **Centrum sieci i udostępniania** w Panelu sterowania.
7. Kliknij przycisk **Zmień ustawienia karty**.

 ![Zmień ustawienia karty sieciowej](./media/nps-extension-vpn/image18.png)

8. Kliknij prawym przyciskiem myszy połączenie sieci VPN, a następnie kliknij przycisk Właściwości. 

 ![Właściwości sieci VPN](./media/nps-extension-vpn/image19.png)

9. W oknie dialogowym właściwości sieci VPN kliknij **zabezpieczeń** kartę. 
10. Na karcie Zabezpieczenia, upewnij się, że tylko **Microsoft CHAP Version 2 (MS-CHAP v2)** jest zaznaczone, a następnie kliknij przycisk OK.

 ![Zezwolić protokołów](./media/nps-extension-vpn/image20.png)

11. Kliknij prawym przyciskiem myszy połączenie sieci VPN, a następnie kliknij przycisk **Connect**.
12. Na stronie Ustawienia kliknij **Connect**.

Udane połączenie zostanie wyświetlony w dzienniku zabezpieczeń na serwerze RADIUS jako zdarzenia 6272 identyfikator, jak pokazano poniżej.

 ![Właściwości zdarzenia](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Rozwiązywanie problemów z przewodnika
Załóżmy, że przed skonfigurowaniem serwera sieci VPN do użycia centralny serwer usługi RADIUS do uwierzytelniania i autoryzacji pracy konfigurację sieci VPN. W tym przypadku jest prawdopodobne, że problem może być spowodowane błędem konfiguracji serwera RADIUS lub użycia nieprawidłowej nazwy użytkownika lub hasło. Na przykład, jeśli używasz alternatywny sufiks nazwy UPN użytkownika, próba logowania może zakończyć się niepowodzeniem (należy używać tej samej nazwy konta w celu uzyskania najlepszych wyników). 

Aby rozwiązać te problemy, doskonale nadaje się do uruchomienia jest zbadanie dziennikami zdarzeń zabezpieczeń na serwerze RADIUS. Aby zapisać czas wyszukiwania dla zdarzeń, służy opartej na rolach zasad sieciowych i dostępu do serwera widok niestandardowy w Podglądzie zdarzeń, jako wyświetlane poniżej. Identyfikator zdarzenia 6273 wskazuje zdarzeń, gdy serwer zasad sieciowych odmowy dostępu dla użytkownika. 

 ![Podgląd zdarzeń](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Skonfiguruj usługę Multi-Factor Authentication
Ta sekcja zawiera instrukcje dotyczące włączania użytkowników dla usługi MFA i konfigurowanie kont na potrzeby weryfikacji dwuetapowej. 

### <a name="enable-multi-factor-authentication"></a>Włączanie uwierzytelniania wieloskładnikowego
W tej sekcji można włączyć konta usługi Azure AD MFA. Użyj **klasyczny portal** aby umożliwić użytkownikom w usłudze MFA. 

1. Otwórz przeglądarkę i przejdź do [https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Zaloguj się jako administrator.
3. W portalu, a na lewym pasku nawigacyjnym kliknij **usługi ACTIVE DIRECTORY**.

 ![Domyślny katalog](./media/nps-extension-vpn/image23.png)

4. W kolumnie Nazwa kliknij **katalog domyślny** (lub innego katalogu, w razie potrzeby).
5. Na stronie Szybki Start kliknij **Konfiguruj**.

 ![Konfigurowanie domyślnych](./media/nps-extension-vpn/image24.png)

6. Na stronie Konfiguracja przewiń w dół, a następnie w sekcji uwierzytelnianie wieloskładnikowe, kliknij przycisk **Zarządzaj ustawieniami usługi**.

 ![Zarządzaj ustawieniami usługi MFA](./media/nps-extension-vpn/image25.png)
 
7. Na stronie uwierzytelnianie wieloskładnikowe, przejrzyj ustawienia domyślne usługi, a następnie kliknij **użytkowników**. 

 ![Użytkownicy usługi MFA](./media/nps-extension-vpn/image26.png)
 
8. Na stronie użytkowników wybierz użytkowników, którzy mają zostać włączone dla usługi MFA, a następnie kliknij przycisk **włączyć**.

 ![Właściwości](./media/nps-extension-vpn/image27.png)
 
9. Po wyświetleniu monitu kliknij przycisk **Włącz uwierzytelnianie wieloskładnikowe**.

 ![Włączenie uwierzytelniania MFA](./media/nps-extension-vpn/image28.png)
 
10. Kliknij przycisk **Zamknij**. 
11. Odśwież stronę. Stan uwierzytelniania Wieloskładnikowego jest zmieniany na włączone.

Aby uzyskać informacje na temat umożliwić użytkownikom uwierzytelnianie wieloskładnikowe, zobacz [wprowadzenie do korzystania z usługi Azure Multi-Factor Authentication w chmurze](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurowanie konta na potrzeby weryfikacji dwuetapowej
Po włączeniu konta dla usługi MFA, użytkownicy nie będą mogli zalogować się do zasobów objętych zasady MFA, aż pomyślnie skonfigurowano zaufanego urządzenia na potrzeby drugiego etapu uwierzytelniania o używane weryfikacji dwuetapowej.

W tej sekcji skonfigurujesz zaufanego urządzenia do użycia w trakcie weryfikacji dwuetapowej. Dostępnych jest kilka opcji można konfigurować, takie jak następujące:

* **Aplikacja mobilna**. Aplikacja Microsoft Authenticator należy zainstalować na urządzeniu Windows Phone, Android lub iOS. W zależności od zasad organizacji, należy skorzystać z aplikacji w jednym z dwóch trybów: otrzymywać powiadomienia na potrzeby weryfikacji (powiadomienie jest wypychane na urządzenie) lub użyj kodu weryfikacyjnego (konieczne jest podanie kodu weryfikacyjnego, która aktualizuje co 30 sekund). 
* **Telefonu komórkowego połączenia lub wiadomości SMS**. Można albo otrzymywać automatyczne połączenia telefonicznego lub wiadomości SMS. Z opcją połączenia telefonicznego odebranie połączenia i naciśnij klawisz znaku # do uwierzytelniania. Z opcją tekstu możesz Odpowiedz na wiadomość SMS lub wprowadź kod weryfikacyjny w interfejsie logowania.
* **Z telefonem biurowym**. Ten proces jest taka sama, jak te opisane na automatyczne połączenia telefoniczne powyżej.

Wykonaj te instrukcje dotyczące konfigurowania urządzenia na potrzeby odbierania powiadomień wypychanych do weryfikacji w aplikacji mobilnej.

1. Zaloguj się do [https://aka.ms/mfasetup](https://aka.ms/mfasetup) lub dowolnej lokacji, takich jak [https://portal.azure.com](https://portal.azure.com), gdzie wymagane do uwierzytelniania przy użyciu poświadczeń konta z obsługą uwierzytelniania Wieloskładnikowego. 
2. Po zarejestrowaniu się przy użyciu nazwy użytkownika i hasła, są prezentowane ekran z monitem o skonfigurowanie konta dodatkowej weryfikacji zabezpieczeń.

 ![Dodatkowe zabezpieczenia](./media/nps-extension-vpn/image29.png)

3. Kliknij przycisk **teraz skonfigurować**.
4. Na stronie weryfikacji dodatkowe zabezpieczenia wybierz typ kontaktu (numer telefonu uwierzytelniania, telefon biurowy lub aplikacja mobilna). Następnie wybierz kraj lub region, a następnie wybierz metodę. Metoda zależy od wybranego typu kontaktu. Na przykład jeśli aplikacja mobilna, można wybrać czy otrzymywać powiadomienia na potrzeby weryfikacji, czy Użyj kodu weryfikacyjnego. Czynności, które wykonują założono, możesz wybrać **aplikacji mobilnej** jako typ kontaktu.

 ![Telefonu uwierzytelniania](./media/nps-extension-vpn/image30.png)

5. Wybierz aplikację mobilną, kliknij przycisk **otrzymywać powiadomienia na potrzeby weryfikacji**, a następnie **Konfigurowanie**. 

 ![Weryfikacja aplikacji mobilnej](./media/nps-extension-vpn/image31.png)
 
6. Jeśli jeszcze tego nie zrobiono tego wcześniej, zainstaluj aplikację mobilną wystawcy uwierzytelnienia na urządzeniu. 
7. Postępuj zgodnie z instrukcjami w aplikacji mobilnej w celu skanowania przedstawioną kod kreskowy lub ręcznie wprowadzić informacje o, a następnie kliknij **gotowe**.

 ![Konfigurowanie aplikacji mobilnej](./media/nps-extension-vpn/image32.png)

8. Na stronie weryfikacji dodatkowe zabezpieczenia, kliknij przycisk **kontaktu mnie** i Odpowiedz na powiadomienie do Twojego urządzenia.
9. Na stronie weryfikacji dodatkowe zabezpieczenia, wprowadź numer kontaktu w przypadku utraty dostępu do aplikacji mobilnej i kliknij przycisk **dalej**.

 ![Numer telefonu komórkowego](./media/nps-extension-vpn/image33.png)
 
10. Na dodatkowej weryfikacji zabezpieczeń, kliknij przycisk **gotowe**.

Urządzenie jest teraz skonfigurowane do zapewniają drugi metodę weryfikacji. Aby uzyskać informacje na temat konfigurowania konta na potrzeby weryfikacji dwuetapowej, zobacz [Skonfiguruj moje konto na potrzeby weryfikacji dwuetapowej](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Instalowanie i Konfigurowanie rozszerzeń serwera NPS

Ta sekcja zawiera instrukcje dotyczące konfigurowania sieci VPN do użycia usługi Azure MFA na potrzeby uwierzytelniania klienta z serwerem sieci VPN.

Po zainstalowaniu i skonfigurowaniu rozszerzenia serwera NPS, wszystkie uwierzytelnianie klienta usługi RADIUS, który jest przetwarzany przez ten serwer jest wymagana do używania usługi Azure MFA. Jeśli nie wszystkich użytkowników sieci VPN są zarejestrowane w usłudze Azure MFA, do uwierzytelniania użytkowników, którzy nie są skonfigurowane do używania uwierzytelniania MFA można skonfigurować innego serwera RADIUS. Lub można utworzyć wpis rejestru, który umożliwia kwestionowanych zapewnić drugi składnik uwierzytelniania tylko wtedy, gdy są one rejestrowane w MFA. 

Utwórz nową wartość ciągu o nazwie _REQUIRE_USER_MATCH w HKLM\SOFTWARE\Microsoft\AzureMfa_, a następnie ustaw wartość PRAWDA lub FAŁSZ. 

 ![Wymagaj dopasowania użytkownika](./media/nps-extension-vpn/image34.png)
 
Jeśli wartość jest równa TRUE, lub nie jest ustawiona, wszystkie żądania uwierzytelniania podlegają żądanie uwierzytelniania MFA. Jeśli wartość jest równa FALSE, wyzwania MFA są wystawiane tylko do użytkowników, które są zarejestrowane w MFA. Ustawienia FALSE podczas testowania lub w środowiskach produkcyjnych w okresie dołączania należy używać tylko.

### <a name="acquire-azure-active-directory-guid-id"></a>Uzyskać identyfikator GUID usługi Azure Active Directory

W ramach konfiguracji rozszerzenia serwera NPS musisz podać poświadczenia administratora i identyfikator usługi Azure Active Directory dla dzierżawy usługi Azure AD. Poniższe kroki opisano, jak uzyskać identyfikator dzierżawcy.

1. Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com) jako administrator globalny dzierżawy platformy Azure.
2. W obszarze nawigacji po lewej stronie, kliknij przycisk **usługi Azure Active Directory** ikony.
3. Kliknij pozycję **Właściwości**.
4. Aby skopiować identyfikator katalogu do Schowka, zaznacz **kopiowania** ikony.
 
 ![Identyfikator katalogu](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>Zainstaluj rozszerzenie serwera NPS
Rozszerzenia serwera NPS należy zainstalować na serwerze zasad sieciowych i zainstalowaną rolą usług dostępu (NPS) i tej funkcji jako serwer RADIUS w projekcie. Nie należy instalować rozszerzenia serwera NPS na serwerze usług pulpitu zdalnego.

1. Pobierz rozszerzenie serwera NPS z [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Kopiowanie pliku wykonywalnego Instalatora (NpsExtnForAzureMfaInstaller.exe) na serwerze zasad Sieciowych.
3. Na serwerze NPS, kliknij dwukrotnie **NpsExtnForAzureMfaInstaller.exe**. Jeśli zostanie wyświetlony monit, kliknij przycisk **Uruchom**.
4. W oknie dialogowym serwera NPS rozszerzenia dla usługi Azure MFA instalacji, przejrzyj postanowienia licencyjne dotyczące oprogramowania, sprawdź **zgadzam się z warunkami licencji**i kliknij przycisk **zainstalować**.

 ![Rozszerzenia serwera NPS](./media/nps-extension-vpn/image36.png)
 
5. W oknie dialogowym serwera NPS rozszerzenia dla usługi Azure MFA instalacji, kliknij przycisk **Zamknij**.  

 ![Instalator pomyślnie](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Konfigurowanie certyfikatów do użytku z rozszerzeniem serwera NPS przy użyciu skryptu programu PowerShell
W celu zapewnienia bezpiecznej komunikacji i gwarantują, należy skonfigurować certyfikaty do użycia przez rozszerzenie serwera NPS. Składniki serwera NPS obejmują skrypt programu Windows PowerShell, który konfiguruje certyfikatu z podpisem własnym do użytku z serwera NPS. 

Skrypt wykonuje następujące czynności:

* Tworzy certyfikat z podpisem własnym
* Kojarzy klucz publiczny certyfikatu do usługi podmiotu zabezpieczeń w usłudze Azure AD
* Zapisuje certyfikat w magazynie komputera lokalnego
* Zapewnia dostęp do klucza prywatnego certyfikatu użytkownika sieci
* Uruchamia ponownie usługę Serwer zasad sieciowych

Jeśli chcesz użyć własnych certyfikatów, należy skojarzyć klucz publiczny certyfikatu do nazwy głównej usługi w usłudze Azure AD i tak dalej.
Aby użyć skryptu, dostarczają rozszerzenie poświadczeń administracyjnych usługi Azure Active Directory i skopiowane wcześniej Identyfikatora dzierżawy usługi Azure Active Directory. Uruchom skrypt na każdym serwerze NPS, w którym zainstalowano rozszerzenia serwera NPS.

1. Otwórz administracyjny wiersz środowiska Windows PowerShell.
2. W wierszu polecenia programu PowerShell wpisz _cd "c:\Program Files\Microsoft\AzureMfa\Config"_i naciśnij klawisz **ENTER**.
3. Typ _.\AzureMfsNpsExtnConfigSetup.ps1_i naciśnij klawisz **ENTER**. 
 * Skrypt sprawdza, czy jest zainstalowany moduł programu PowerShell usługi Azure Active Directory. Jeśli nie jest zainstalowany, skrypt zainstaluje moduł.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Po skrypt sprawdza poprawność instalacji modułu programu PowerShell, wyświetla okno dialogowe modułu programu PowerShell usługi Azure Active Directory. W oknie dialogowym Wprowadź poświadczenia administratora usługi Azure AD i hasło, a następnie kliknij przycisk **Zaloguj**. 
 
 ![Logowanie do programu PowerShell](./media/nps-extension-vpn/image39.png)
 
5. Po wyświetleniu monitu Wklej identyfikator dzierżawcy wcześniej zostały skopiowane do Schowka i naciśnij klawisz **ENTER**. 

 ![Identyfikator dzierżawy](./media/nps-extension-vpn/image40.png)

6. Skrypt tworzy certyfikat z podpisem własnym i wykonuje inne zmiany w konfiguracji. Dane wyjściowe są jak obraz, pokazano poniżej.

 ![Certyfikatu z podpisem własnym](./media/nps-extension-vpn/image41.png)

7. Uruchom ponownie serwer.
 
### <a name="verify-configuration"></a>Weryfikowanie konfiguracji
Aby sprawdzić konfigurację, należy nawiązać nowe połączenie sieci VPN z serwerem sieci VPN. Po pomyślnym wprowadzania poświadczeń dla uwierzytelniania podstawowego, połączenie sieci VPN czeka dodatkowego uwierzytelniania się powodzeniem przed połączenie zostanie nawiązane, jak pokazano poniżej. 

 ![Weryfikowanie konfiguracji](./media/nps-extension-vpn/image42.png)

W przypadku pomyślnego uwierzytelnienia za pomocą metody dodatkowej weryfikacji wcześniej skonfigurowane w usłudze Azure MFA, są połączone z zasobem. Jednak dodatkowego uwierzytelniania zakończy się niepowodzeniem, jest odmowa dostępu do zasobu. 

W poniższym przykładzie aplikacji uwierzytelniania w systemie Windows phone służy do zapewnienia dodatkowego uwierzytelniania.

 ![Sprawdź konto](./media/nps-extension-vpn/image43.png)

Po zostały pomyślnie uwierzytelniony przy użyciu metody pomocnicze, mają prawo dostępu do portu wirtualnego na serwerze sieci VPN. Jednak ponieważ były wymagane do użycia dodatkowej metody uwierzytelniania przy użyciu aplikacji mobilnej na zaufanym urządzeniu dziennika w procesie jest bardziej bezpieczne, nie może być używa tylko nazwy użytkownika / kombinacja hasła.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Sprawdź dzienniki Podglądu zdarzeń dla zdarzeń pomyślnego logowania
Do wyświetlania zdarzeń pomyślnego logowania w dziennikach podglądu zdarzeń systemu Windows, należy wydać następujące polecenie programu Windows PowerShell do badania dziennika zabezpieczeń systemu Windows na serwerze NPS.

Aby sprawdzić zdarzeń pomyślnego logowania w dziennikach podglądu zdarzeń zabezpieczeń, użyj następującego polecenia
* _Get-WinEvent - Nazwa_dziennika zabezpieczeń_ | gdzie {$_.ID - eq "6272"} | FL 

 ![Podgląd zdarzeń zabezpieczeń](./media/nps-extension-vpn/image44.png)
 
Możesz również wyświetlić dziennik zabezpieczeń lub widok niestandardowy usług zasad sieciowych i dostępu w sposób przedstawiony poniżej:

 ![Dostęp do zasad sieci](./media/nps-extension-vpn/image45.png)

Na serwerze, na którym zainstalowano rozszerzenia serwera zasad Sieciowych dla usługi Azure MFA, można znaleźć dzienniki Podglądu zdarzeń aplikacji specyficzne dla rozszerzenia w **aplikacji i usług Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent - Nazwa_dziennika zabezpieczeń_ | gdzie {$_.ID - eq "6272"} | FL

 ![Liczba zdarzeń (event)](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Rozwiązywanie problemów z przewodnika
Jeśli konfiguracja nie działa zgodnie z oczekiwaniami, najlepiej zacząć Rozwiązywanie problemów z jest Sprawdź, czy użytkownik jest skonfigurowany do używania usługi Azure MFA. Użytkownik powinien połączyć się z [https://portal.azure.com](https://portal.azure.com). Jeśli monit o podanie dodatkowego uwierzytelniania i można pomyślnie uwierzytelnić, można wyeliminować niepoprawnej konfiguracji usługi Azure MFA.

Jeśli usługi Azure MFA działa dla użytkowników, należy przejrzeć odpowiednich dzienników zdarzeń. Obejmują one zdarzeń zabezpieczeń, operacyjne bramy i dzienniki usługi Azure MFA, które opisano w poprzedniej sekcji. 

Poniżej przedstawiono przykładowe dane wyjściowe dziennika zabezpieczeń przedstawiający zdarzenia logowania nie powiodło się (6273 identyfikator zdarzenia):

 ![Dziennik zabezpieczeń](./media/nps-extension-vpn/image47.png)

Poniżej znajdują się zdarzenia związane z dzienników AzureMFA:

 ![Dzienniki usługi Azure MFA](./media/nps-extension-vpn/image48.png)

Aby wykonywać zaawansowane rozwiązywanie problemów z opcjami, sprawdź pliki dziennika format bazy danych serwera NPS zainstalowaną usługę NPS. Te pliki dziennika są tworzone w _%SystemRoot%\System32\Logs_ folderu plików tekstowych rozdzielanych przecinkami. Aby uzyskać opis tych plików dziennika, zobacz [zinterpretować pliki dziennika Format bazy danych serwera NPS](https://technet.microsoft.com/library/cc771748.aspx). 

Wpisy w tych plikach dziennika są trudne do interpretowania bez importowania ich do arkusza kalkulacyjnego lub bazy danych. Liczba IAS analizatory składni online ułatwić interpretowanie pliki dziennika można znaleźć. Poniżej znajdują się dane wyjściowe tego takie do pobrania [aplikacji "shareware"](http://www.deepsoftware.com/iasviewer): 

 ![Aplikacja "shareware"](./media/nps-extension-vpn/image49.png)

Na koniec, aby uzyskać dodatkowe Rozwiązywanie problemów z opcjami, możesz użyć analizatora protokołów, takich jak Wireshark lub [programu Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Na poniższej ilustracji z programu Wireshark przedstawiono komunikaty RADIUS między serwerem sieci VPN i serwer zasad Sieciowych.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Aby uzyskać więcej informacji, zobacz [integracji istniejącej infrastrukturze serwera NPS z usługi Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Następne kroki
[Jak uzyskać usługę Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Brama usług pulpitu zdalnego i serwer Azure Multi-Factor Authentication korzystające z usługi RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrowanie katalogów lokalnych z usługą Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

