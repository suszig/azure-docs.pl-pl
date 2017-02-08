---
title: Wprowadzenie do serwera Azure Multi-Factor Authentication | Microsoft Docs
description: "Ta strona dotycząca usługi Azure Multi-Factor Authentication zawiera informacje umożliwiające rozpoczęcie korzystania z serwera Azure MFA."
services: multi-factor-authentication
keywords: serwer uwierzytelniania, strona aktywacji aplikacji azure multi factor authentication, pobieranie serwera uwierzytelniania
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0fec7a18e098891374b3b0d7313a72918b630918
ms.openlocfilehash: 7fb107922af9d2316fb7490670002f4255572458

---

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Wprowadzenie do serwera Azure Multi-Factor Authentication
<center>![Lokalna usługa MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Po podjęciu decyzji o użyciu lokalnego serwera Multi-Factor Authentication można przejść do kolejnych kroków. Ta strona obejmuje nową instalację serwera oraz jego konfigurację z uwzględnieniem lokalnej usługi Active Directory. Aby uaktualnić zainstalowany wcześniej serwer PhoneFactor, zobacz artykuł [Uaktualnianie do serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-upgrade.md). Informacje na temat instalowania samych usług sieci Web znajdują się w artykule [Wdrażanie usług sieci Web aplikacji mobilnej serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-webservice.md).

## <a name="download-the-azure-multi-factor-authentication-server"></a>Pobieranie serwera Azure Multi-Factor Authentication
Istnieją dwa sposoby na pobranie serwera Azure Multi-Factor Authentication. Oba wymagają użycia witryny Azure Portal. Pierwszy sposób polega na pobraniu serwera bezpośrednio w ramach zarządzania dostawcą usługi Multi-Factor Authentication. Drugi sposób wymaga skorzystania z ustawień usługi. Druga opcja obejmuje skorzystanie z dostawcy usługi Multi-Factor Authentication lub z licencji usługi Azure MFA, usługi Azure AD w wersji Premium lub pakietu Enterprise Mobility Suite.

> [!Important]
> Te dwie opcje wydają się podobne, ale ważne jest rozróżnienie ich użycia. Jeśli użytkownicy korzystają z licencji dostarczonych z usługą MFA, nie należy tworzyć dostawcy usługi Multi-Factor Authentication na potrzeby uzyskania dostępu do pobierania serwera. Zamiast tego należy użyć opcji 2 w celu pobrania serwera ze strony ustawień usługi. 

### <a name="option-1-download-azure-multi-factor-authentication-server-from-the-azure-classic-portal"></a>Opcja 1: pobieranie serwera Azure Multi-Factor Authentication z klasycznej witryny Azure Portal

Użyj tej opcji pobierania, jeśli masz już dostawcę usługi Multi-Factor Authentication, ponieważ usługę MFA opłacono za włączonego użytkownika lub za uwierzytelnianie. 

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator.
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Na stronie usługi Active Directory kliknij pozycję **Dostawcy usługi Multi-Factor Auth**
    ![Dostawcy usługi Multi-Factor Auth](./media/multi-factor-authentication-get-started-server/authproviders.png)
4. Kliknij u dołu pozycję **Zarządzaj**. Zostanie otwarta nowa strona.
5. Kliknij pozycję **Pliki do pobrania**.
6. Kliknij link **Pobierz** powyżej przycisku **Generuj poświadczenia aktywacji**.
   ![Pobieranie](./media/multi-factor-authentication-get-started-server/download4.png)
7. Zapisz pobrany plik.

### <a name="option-2-download-azure-multi-factor-authentication-server-from-the-service-settings"></a>Opcja 2: pobieranie serwera Azure Multi-Factor Authentication przy użyciu ustawień usługi

Użyj tej opcji pobierania, jeśli masz licencje pakietu Enterprise Mobility Suite, usługi Azure AD w wersji Premium lub pakietu Enterprise Cloud Suite. 

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator.
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Kliknij dwukrotnie wystąpienie usługi Azure AD.
4. Kliknij pozycję **Konfiguruj** u góry strony
5. Przewiń do sekcji **uwierzytelnianie wieloskładnikowe** i wybierz pozycję **Zarządzaj ustawieniami usługi**
6. Na stronie ustawień usługi, w dolnej części ekranu, kliknij opcję **Przejdź do portalu**. Zostanie otwarta nowa strona.
   ![Pobieranie](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Kliknij pozycję **Pliki do pobrania**.
8. Kliknij link **Pobierz** powyżej przycisku **Generuj poświadczenia aktywacji**.
    ![Pobieranie](./media/multi-factor-authentication-get-started-server/download4.png)
9. Zapisz pobrany plik.

## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Instalowanie i konfigurowanie serwera Azure Multi-Factor Authentication
Po pobraniu serwera możesz go zainstalować i skonfigurować.  Należy sprawdzić, czy serwer, na którym zamierzasz go zainstalować, spełnia następujące wymagania:

| Wymagania serwera Azure Multi-Factor Authentication | Opis |
|:--- |:--- |
| Sprzęt |<li>200 MB wolnego miejsca na dysku twardym</li><li>Procesor umożliwiający obsługę architektury x32 lub x64</li><li>Co najmniej 1 GB pamięci RAM</li> |
| Oprogramowanie |<li>System Windows Server 2008 lub nowszy, jeśli na hoście znajduje się system operacyjny serwera</li><li>System Windows 7 lub nowszy, jeśli na hoście znajduje się system operacyjny klienta</li><li>Oprogramowanie Microsoft .NET 4.0 Framework</li><li>Usługi IIS 7.0 lub nowsze w przypadku instalacji portalu użytkowników lub zestawu SDK usługi sieci Web</li> |

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Wymagania serwera Azure Multi-Factor Authentication dotyczące zapory
- - -
Każdy serwer MFA musi mieć możliwość komunikacji wychodzącej za pośrednictwem portu 443 z następującymi adresami:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Jeśli zapory ruchu wychodzącego mają ograniczenie na porcie 443, otwórz następujące zakresy adresów IP:

| Podsieć IP | Maska sieci | Zakres adresów IP |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Jeśli nie używasz funkcji potwierdzania zdarzeń i użytkownicy nie korzystają z uwierzytelniania z użyciem aplikacji mobilnych z poziomu urządzeń w sieci firmowej, adresy IP można ograniczyć do następujących zakresów:

| Podsieć IP | Maska sieci | Zakres adresów IP |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Aby zainstalować i skonfigurować serwer Azure Multi-Factor Authentication

1. Kliknij dwukrotnie plik wykonywalny. Spowoduje to rozpoczęcie instalacji.
2. Upewnij się, że na ekranie Wybieranie folderu instalacji wybrany jest prawidłowy folder, a następnie kliknij przycisk **Dalej**.
3. Po ukończeniu instalacji kliknij przycisk **Zakończ**.  Spowoduje to uruchomienie kreatora konfiguracji.
4. Na ekranie powitalnym kreatora konfiguracji zaznacz pole **Pomiń korzystanie z kreatora konfiguracji uwierzytelniania** i kliknij przycisk **Dalej**.  Spowoduje to zamknięcie kreatora i uruchomienie serwera.
    ![Chmura](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Na stronie, z której został pobrany serwer, kliknij przycisk **Generuj poświadczenia aktywacji**. Skopiuj uzyskane informacje do serwera Azure MFA, wpisując je w odpowiednich polach, a następnie kliknij przycisk **Aktywuj**.

Powyższe kroki pokazują kroki instalacji ekspresowej z użyciem kreatora konfiguracji.  Kreatora uwierzytelniania można uruchomić ponownie, wybierając go z menu Narzędzia na serwerze.

## <a name="import-users-from-active-directory"></a>Importowanie użytkowników z usługi Active Directory
Po zainstalowaniu i skonfigurowaniu serwera można szybko zaimportować użytkowników do serwera Azure MFA.

1. Na serwerze Azure MFA, w obszarze po lewej stronie, wybierz pozycję **Użytkownicy**.
2. U dołu ekranu wybierz pozycję **Importuj z usługi Active Directory**.
3. Po wykonaniu tych czynności możesz wyszukiwać poszczególnych użytkowników lub wyszukać w katalogu usługi AD jednostki organizacyjne obejmujące użytkowników.  W tym przypadku jest określana jednostka organizacyjna użytkowników.
4. Zaznacz wszystkich użytkowników po prawej stronie, a następnie kliknij przycisk **Importuj**.  Zostanie wyświetlone okno podręczne informujące, że proces został zakończony pomyślnie.  Zamknij okno importu.

![Chmura](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Wysyłanie wiadomości e-mail do użytkowników
Gdy użytkownicy zostaną zaimportowani na serwer MFA, zalecamy wysłanie wiadomości e-mail w celu poinformowania użytkowników o zarejestrowaniu ich na potrzeby weryfikacji dwuetapowej.

Wiadomość e-mail powinna zależeć od konfiguracji użytkowników na potrzeby weryfikacji dwuetapowej. Jeśli na przykład udało się zaimportować numery telefonów użytkowników z katalogu firmy, wiadomość e-mail powinna zawierać domyślny numer telefonu, aby użytkownicy wiedzieli, czego oczekiwać. Jeśli numery telefonów użytkowników nie zostały zaimportowane lub użytkownicy są skonfigurowani w celu korzystania z aplikacji mobilnej, wyślij wiadomość e-mail z hiperlinkiem do portalu użytkownika usługi Azure Multi-Factor Authentication umożliwiającym ukończenie rejestracji konta.

Treść wiadomości e-mail różni się też w zależności od metody weryfikacji, która została ustawiona dla użytkownika (połączenie telefoniczne, wiadomość SMS lub aplikacja mobilna).  Jeśli na przykład użytkownik musi podczas uwierzytelniania użyć numeru PIN, w wiadomości e-mail zostanie podany początkowy numer PIN.  Użytkownicy muszą zmienić numer PIN podczas pierwszej weryfikacji.


### <a name="configure-email-and-email-templates"></a>Konfigurowanie wiadomości e-mail i szablonów wiadomości e-mail
Kliknij ikonę poczty e-mail z lewej strony, aby skonfigurować ustawienia wysyłania tych wiadomości e-mail. W tym miejscu można wprowadzić dane SMTP serwera poczty oraz wysyłać wiadomości e-mail przez zaznaczenie pola wyboru **Wyślij wiadomości e-mail do użytkowników**.

![Ustawienia poczty e-mail](./media/multi-factor-authentication-get-started-server/email1.png)

Na karcie Zawartość wiadomości e-mail są widoczne szablony wiadomości e-mail, które są dostępne do wyboru. W zależności od ustawień weryfikacji dwuetapowej skonfigurowanych dla użytkowników można wybrać szablon najlepiej odpowiadający bieżącym potrzebom.

![Szablony wiadomości e-mail](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Sposób obsługi danych użytkowników przez serwer Azure Multi-Factor Authentication
W przypadku lokalnego użycia serwera Multi-Factor Authentication (MFA) dane użytkownika są przechowywane na serwerach lokalnych. Żadne trwałe dane użytkowników nie są przechowywane w chmurze. Gdy użytkownik przeprowadza weryfikację dwuetapową, serwer MFA wysyła dane do usługi Azure MFA w chmurze w celu przeprowadzenia weryfikacji. Podczas przesyłania żądań uwierzytelnienia do usługi w chmurze następujące pola są wysyłane w żądaniu i dziennikach, dzięki czemu są one dostępne w ramach raportów klienta dotyczących uwierzytelniania/użycia. Niektóre pola są opcjonalne i można je włączyć lub wyłączyć na serwerze Multi-Factor Authentication. Na potrzeby przesyłania danych z serwera MFA do usługi MFA w chmurze używany jest protokół SSL/TLS i port wyjściowy 443. Wysyłane mogą być następujące pola:

* Unikatowy identyfikator — nazwa użytkownika lub wewnętrzny identyfikator serwera MFA
* Imię i nazwisko (opcjonalnie)
* Adres e-mail (opcjonalnie)
* Numer telefonu — używany w trakcie uwierzytelniania za pomocą połączenia głosowego lub wiadomości SMS
* Tokenu urządzenia — używany podczas uwierzytelniania za pomocą aplikacji mobilnej
* Tryb uwierzytelniania
* Wynik uwierzytelniania
* Nazwa serwera MFA
* Adres IP serwera MFA
* Adres IP klienta — jeśli jest dostępny

Oprócz powyższych pól wraz z danymi uwierzytelniania są przechowywane także wyniki weryfikacji (powodzenie/odmowa) oraz przyczyny odmów. Informacje te są potem dostępne w raportach dotyczących uwierzytelniania/użycia.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkowe informacje na temat zaawansowanej instalacji oraz informacje o konfiguracji, użyj linków znajdujących się w poniższej tabeli:

| Metoda | Opis |
|:--- |:--- |
| [Portal użytkowników](multi-factor-authentication-get-started-portal.md) |Informacje na temat instalacji i konfiguracji portalu użytkowników, w tym na temat wdrażania i samoobsługi użytkowników. |
| [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md) |Informacje dotyczące konfigurowania Azure Multi-Factor Authentication z usługami AD FS. |
| [Uwierzytelnianie usługi RADIUS](multi-factor-authentication-get-started-server-radius.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem usługi RADIUS. Korzystanie z usługi RADIUS umożliwia integrację różnych systemów innych firm z serwerem Azure MFA. |
| [Uwierzytelnianie usług IIS](multi-factor-authentication-get-started-server-iis.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem usług IIS. Korzystanie z usług IIS umożliwia integrację różnych systemów innych firm z serwerem Azure MFA. |
| [Uwierzytelnianie systemu Windows](multi-factor-authentication-get-started-server-windows.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem uwierzytelniania systemu Windows. |
| [Uwierzytelnianie LDAP](multi-factor-authentication-get-started-server-ldap.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z użyciem uwierzytelniania LDAP. Korzystanie z uwierzytelniania LDAP umożliwia integrację różnych systemów innych firm z serwerem Azure MFA. |
| [Brama usług pulpitu zdalnego i serwer Azure Multi-Factor Authentication korzystające z usługi RADIUS](multi-factor-authentication-get-started-server-rdg.md) |Informacje na temat instalacji i konfiguracji serwera Azure MFA z bramą usług pulpitu zdalnego z użyciem usługi RADIUS. |
| [Synchronizacja z usługą Active Directory systemu Windows Server](multi-factor-authentication-get-started-server-dirint.md) |Informacje na temat instalacji i konfiguracji synchronizacji usługi Active Directory i serwera Azure MFA. |
| [Wdrażanie usługi sieci Web aplikacji mobilnej serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server-webservice.md) |Informacje na temat instalacji i konfiguracji usługi sieci Web serwera Azure MFA. |
| [Zaawansowane scenariusze obejmujące usługę Azure Multi-Factor Authentication i sieci VPN innych firm](multi-factor-authentication-advanced-vpn-configurations.md) | Przewodniki krok po kroku dotyczące konfiguracji urządzeń VPN firm Cisco, Citrix i Juniper. |



<!--HONumber=Feb17_HO1-->


