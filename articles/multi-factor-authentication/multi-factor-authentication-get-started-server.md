---
title: Wprowadzenie do serwera Azure Multi-Factor Authentication
description: "Ta strona dotycząca usługi Azure Multi-Factor Authentication zawiera informacje umożliwiające rozpoczęcie korzystania z serwera Azure MFA."
services: multi-factor-authentication
keywords: serwer uwierzytelniania, strona aktywacji aplikacji azure multi factor authentication, pobieranie serwera uwierzytelniania
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 76e768d6640eb72292556c4fe14467cf5072dc21


---
# <a name="getting-started-with-the-azure-multifactor-authentication-server"></a>Wprowadzenie do serwera Azure Multi-Factor Authentication
<center>![Chmura](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Po podjęciu decyzji o użyciu lokalnego uwierzytelniania wieloskładnikowego można przejść do kolejnych kroków. Ta strona obejmuje nową instalację serwera oraz jego konfigurację z uwzględnieniem lokalnej usługi Active Directory. Aby uaktualnić zainstalowany wcześniej serwer PhoneFactor, zobacz artykuł [Uaktualnianie do serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-upgrade.md). Informacje na temat instalowania samych usług sieci Web znajdują się w artykule [Wdrażanie usług sieci Web aplikacji mobilnej serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-webservice.md).

## <a name="download-the-azure-multifactor-authentication-server"></a>Pobieranie serwera Azure Multi-Factor Authentication
Istnieją dwa sposoby na pobranie serwera Azure Multi-Factor Authentication. Oba wymagają użycia witryny Azure Portal. Pierwszy sposób polega na pobraniu serwera bezpośrednio w ramach zarządzania dostawcą usługi Multi-Factor Authentication. Drugi sposób wymaga skorzystania z ustawień usługi. Druga opcja obejmuje skorzystanie z dostawcy usługi Multi-Factor Authentication lub z licencji usługi Azure MFA, usługi Azure AD w wersji Premium lub pakietu Enterprise Mobility Suite.

### <a name="to-download-the-azure-multifactor-authentication-server-from-the-azure-portal"></a>Aby pobrać serwer Azure Multi-Factor Authentication z witryny Azure Portal
- - -
1. Zaloguj się do witryny Azure Portal jako administrator.
2. W obszarze po lewej stronie wybierz pozycję Active Directory.
3. Na górze strony usługi Active Directory kliknij pozycję **Dostawcy usługi Multi-Factor Auth**
4. Kliknij u dołu pozycję **Zarządzaj**.
5. Spowoduje to otwarcie nowej strony.  Kliknij pozycję **Pliki do pobrania**.
   ![Pobieranie](./media/multi-factor-authentication-sdk/download.png).
6. Kliknij pozycję **Pobierz**.
   ![Pobieranie](./media/multi-factor-authentication-get-started-server/download4.png) nad przyciskiem **Generuj poświadczenia aktywacji**.
7. Zapisz pobrany plik.

### <a name="to-download-the-azure-multifactor-authentication-server-via-the-service-settings"></a>Aby pobrać serwer Azure Multi-Factor Authentication z użyciem ustawień usługi
1. Zaloguj się do witryny Azure Portal jako administrator.
2. W obszarze po lewej stronie wybierz pozycję Active Directory.
3. Kliknij dwukrotnie wystąpienie usługi Azure AD.
4. Kliknij pozycję **Konfiguruj**
   ![Pobieranie](./media/multi-factor-authentication-sdk/download2.png) na górze strony.
5. W obszarze uwierzytelniania wieloskładnikowego wybierz pozycję **Zarządzaj ustawieniami usługi**
6. Na stronie ustawień usługi, w dolnej części ekranu, kliknij opcję **Przejdź do portalu**.
   ![Pobieranie](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Spowoduje to otwarcie nowej strony. Kliknij pozycję **Pliki do pobrania**.
8. Kliknij pozycję **Pobierz** nad przyciskiem **Generuj poświadczenia aktywacji**.
9. Zapisz pobrany plik.

## <a name="install-and-configure-the-azure-multifactor-authentication-server"></a>Instalowanie i konfigurowanie serwera Azure Multi-Factor Authentication
Po pobraniu serwera możesz go zainstalować i skonfigurować.  Należy sprawdzić, czy serwer, na którym zamierzasz go zainstalować, spełnia następujące wymagania:

| Wymagania serwera Azure Multi-Factor Authentication | Opis |
|:--- |:--- |
| Sprzęt |<li>200 MB wolnego miejsca na dysku twardym</li><li>Procesor umożliwiający obsługę architektury x32 lub x64</li><li>Co najmniej 1 GB pamięci RAM</li> |
| Oprogramowanie |<li>System Windows Server 2008 lub nowszy, jeśli na hoście znajduje się system operacyjny serwera</li><li>System Windows 7 lub nowszy, jeśli na hoście znajduje się system operacyjny klienta</li><li>Oprogramowanie Microsoft .NET 4.0 Framework</li><li>Usługi IIS 7.0 lub nowsze w przypadku instalacji portalu użytkowników lub zestawu SDK usługi sieci Web</li> |

### <a name="azure-multifactor-authentication-server-firewall-requirements"></a>Wymagania serwera Azure Multi-Factor Authentication dotyczące zapory
- - -
Każdy serwer MFA musi mieć możliwość komunikacji wychodzącej za pośrednictwem portu 443 z następującymi adresami:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Jeśli zapory ruchu wychodzącego mają ograniczenie na porcie 443, konieczne będzie otwarcie następujących zakresów adresów IP:

| Podsieć IP | Maska sieci | Zakres adresów IP |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Jeśli nie używasz funkcji potwierdzenia zdarzenia usługi Azure Multi-Factor Authentication i jeśli użytkownicy nie korzystają z uwierzytelniania z użyciem aplikacji mobilnej Multi-Factor Auth z poziomu urządzenia należącego do sieci firmowej, zakresy adresów IP można ograniczyć do następujących:

| Podsieć IP | Maska sieci | Zakres adresów IP |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

### <a name="to-install-and-configure-the-azure-multifactor-authentication-server"></a>Aby zainstalować i skonfigurować serwer Azure Multi-Factor Authentication
- - -
1. Kliknij dwukrotnie plik wykonywalny. Nastąpi rozpoczęcie instalacji.
2. Upewnij się, że na ekranie Wybieranie folderu instalacji wybrany jest prawidłowy folder, a następnie kliknij przycisk Dalej.
3. Po ukończeniu instalacji kliknij przycisk Zakończ.  Spowoduje to uruchomienie kreatora konfiguracji.
4. Na ekranie powitalnym kreatora konfiguracji zaznacz pole opcji **Pomiń korzystanie z kreatora konfiguracji uwierzytelniania** i kliknij przycisk **Dalej**.  Spowoduje to zamknięcie kreatora i uruchomienie serwera.
    ![Chmura](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Na stronie, z której został pobrany serwer, kliknij przycisk **Generuj poświadczenia aktywacji**.  Skopiuj uzyskane informacje do serwera Azure MFA, wpisując je w odpowiednich polach, a następnie kliknij przycisk **Aktywuj**.

Powyższe kroki pokazują kroki instalacji ekspresowej z użyciem kreatora konfiguracji.  Kreatora uwierzytelniania można uruchomić ponownie, wybierając go z menu Narzędzia na serwerze.

## <a name="import-users-from-active-directory"></a>Importowanie użytkowników z usługi Active Directory
Po zainstalowaniu i skonfigurowaniu serwera można szybko zaimportować użytkowników do serwera Azure MFA.

### <a name="to-import-users-from-active-directory"></a>Aby zaimportować użytkowników z usługi Active Directory
- - -
1. Na serwerze Azure MFA, w obszarze po lewej stronie, wybierz pozycję **Użytkownicy**.
2. U dołu ekranu wybierz pozycję **Importuj z usługi Active Directory**.
3. Po wykonaniu tych czynności możesz wyszukiwać poszczególnych użytkowników lub wyszukać w katalogu usługi AD jednostki organizacyjne obejmujące użytkowników.  W tym przypadku określimy jednostkę organizacyjną użytkowników.
4. Zaznacz wszystkich użytkowników po prawej stronie, a następnie kliknij przycisk **Importuj**.  Zostanie wyświetlone okno podręczne informujące, że proces został zakończony pomyślnie.  Zamknij okno importu.

![Chmura](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Wysyłanie wiadomości e-mail do użytkowników
Po zaimportowaniu użytkowników do serwera usługi Multi-Factor Authentication zaleca się wysłać użytkownikom wiadomość e-mail informującą o ich zarejestrowaniu w usłudze uwierzytelniania wieloskładnikowego.

Serwer Azure Multi-Factor Authentication oferuje różne sposoby konfigurowania użytkowników do korzystania z uwierzytelniania wieloskładnikowego.  Użytkownicy otrzymują wiadomość e-mail z informacją, że zostali oni objęci procedurą uwierzytelniania wieloskładnikowego Azure. Jeśli znasz numery telefonu użytkowników lub masz możliwość zaimportowania numerów telefonu do serwera Azure Multi-Factor Authentication z katalogu firmy, przekaż użytkownikom w tej wiadomości instrukcje dotyczące korzystania z usługi Azure Multi-Factor Authentication wraz z numerem telefonu, na który będą otrzymywać uwierzytelnienia.  

Treść wiadomości e-mail będzie się różnić w zależności od metody uwierzytelniania, która została ustawiona dla użytkownika (np. połączenie telefoniczne, wiadomość SMS, aplikacja mobilna).  Jeśli na przykład użytkownik musi podczas uwierzytelniania użyć numeru PIN, w wiadomości e-mail zostanie podany początkowy numer PIN.  Od użytkowników wymaga się zwykle zmiany numeru PIN podczas pierwszego uwierzytelniania.

Jeśli numery telefonów użytkowników nie zostały skonfigurowane lub nie zostały zaimportowany do serwera Azure Multi-Factor Authentication lub jeśli wstępna konfiguracja zakłada korzystanie przez użytkowników z aplikacji mobilnej w celu uwierzytelnienia, można wysyłać użytkownikom wiadomość e-mail z informacją, że zostali objęci procedurą uwierzytelniania wieloskładnikowego w ramach usługi Azure Multi-Factor Authentication, oraz hiperlinkiem pozwalającym ukończyć rejestrację ich konta za pośrednictwem portalu użytkowników usługi Azure Multi-Factor Authentication.  Wiadomość będzie zawierać hiperlink, który użytkownik może kliknąć w celu przejścia do portalu użytkowników. Gdy użytkownik kliknie hiperlink, nastąpi otwarcie przeglądarki sieci Web i przejście do firmowego portalu użytkowników usługi Azure Multi-Factor Authentication.   

### <a name="configuring-email-and-email-templates"></a>Konfigurowanie wiadomości e-mail i szablonów wiadomości e-mail
Klikając ikonę poczty e-mail z lewej strony, można skonfigurować ustawienia wysyłania tych wiadomości e-mail.  To tutaj można wprowadzić dane SMTP serwera poczty. Można stąd także wysyłać zbiorcze wiadomości e-mail poprzez zaznaczenie pola wyboru Wyślij wiadomości e-mail do użytkowników.

![Ustawienia poczty e-mail](./media/multi-factor-authentication-get-started-server/email1.png)

Na karcie Zawartość wiadomości e-mail są widoczne wszystkie szablony wiadomości e-mail, które są dostępne do wyboru.  W zależności od ustawień uwierzytelniania wieloskładnikowego skonfigurowanych dla użytkowników można wybrać szablon najlepiej odpowiadający bieżącym potrzebom.

![Szablony wiadomości e-mail](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multifactor-authentication-server-handles-user-data"></a>Sposób obsługi danych użytkowników przez serwer Azure Multi-Factor Authentication
W przypadku lokalnego użycia serwera Multi-Factor Authentication (MFA) dane użytkownika są przechowywane na serwerach lokalnych. Żadne trwałe dane użytkowników nie są przechowywane w chmurze. W momencie gdy użytkownik korzysta z procedury uwierzytelniania dwuskładnikowego, serwer MFA wysyła dane do usługi Azure MFA w chmurze w celu przeprowadzenia uwierzytelnienia. Podczas przesyłania żądań uwierzytelnienia do usługi w chmurze następujące pola są wysyłane w żądaniu i dziennikach, dzięki czemu są one dostępne w ramach raportów klienta dotyczących uwierzytelniania/użycia. Niektóre pola są opcjonalne i można je włączyć lub wyłączyć na serwerze Multi-Factor Authentication. Na potrzeby przesyłania danych z serwera MFA do usługi MFA w chmurze używany jest protokół SSL/TLS i port wyjściowy 443. Wysyłane mogą być następujące pola:

* Unikatowy identyfikator — nazwa użytkownika lub wewnętrzny identyfikator serwera MFA
* Imię i nazwisko — opcjonalnie
* Adres e-mail — opcjonalnie
* Numer telefonu — używany w trakcie uwierzytelniania za pomocą połączenia głosowego lub wiadomości SMS
* Tokenu urządzenia — używany podczas uwierzytelniania za pomocą aplikacji mobilnej
* Tryb uwierzytelniania
* Wynik uwierzytelniania
* Nazwa serwera MFA
* Adres IP serwera MFA
* Adres IP klienta — jeśli jest dostępny

Poza powyższymi polami wraz z danymi uwierzytelniania są przechowywane także wyniki uwierzytelniania (powodzenie/odmowa), jak również przyczyny wszelkich odmów. Informacje te są potem dostępne w raportach dotyczących uwierzytelniania/użycia.

## <a name="advanced-azure-multifactor-authentication-server-configurations"></a>Zaawansowane konfiguracje serwera Azure Multi-Factor Authentication
Dodatkowe informacje na temat zaawansowanej instalacji, jak również informacje o konfiguracji, można znaleźć przy użyciu poniższej tabeli.

| Metoda | Opis |
|:--- |:--- |
| [Portal użytkowników](multi-factor-authentication-get-started-portal.md) |Informacje na temat instalacji i konfiguracji portalu użytkowników, w tym na temat wdrażania i samoobsługi użytkowników. |
| [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md) |Informacje dotyczące konfigurowania Azure Multi-Factor Authentication z usługami AD FS. |
| [Uwierzytelnianie usługi RADIUS](multi-factor-authentication-get-started-server-radius.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem usługi RADIUS. |
| [Uwierzytelnianie usług IIS](multi-factor-authentication-get-started-server-iis.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem usług IIS. |
| [Uwierzytelnianie systemu Windows](multi-factor-authentication-get-started-server-windows.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem uwierzytelniania systemu Windows. |
| [Uwierzytelnianie LDAP](multi-factor-authentication-get-started-server-ldap.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem uwierzytelniania LDAP. |
| [Brama usług pulpitu zdalnego i serwer Azure Multi-Factor Authentication korzystające z usługi RADIUS](multi-factor-authentication-get-started-server-rdg.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z bramą usług pulpitu zdalnego z użyciem usługi RADIUS. |
| [Synchronizacja z usługą Active Directory systemu Windows Server](multi-factor-authentication-get-started-server-dirint.md) |Informacje na temat instalacji i konfiguracji synchronizacji usługi Active Directory i serwera Azure MFA. |
| [Wdrażanie usługi sieci Web aplikacji mobilnej serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-webservice.md) |Informacje na temat instalacji i konfiguracji usługi sieci Web serwera Azure MFA. |




<!--HONumber=Nov16_HO2-->


