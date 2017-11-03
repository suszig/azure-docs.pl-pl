---
title: "Utwórz i zainstaluj pliki konfiguracji klienta sieci VPN dla połączeń P2S RADIUS: środowiska PowerShell: Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomaga w utworzeniu pliku konfiguracji klienta sieci VPN dla połączenia punkt-lokacja, które używają uwierzytelniania serwera RADIUS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: cherylmc
ms.openlocfilehash: d7d2dbff4bcd0d76b56c6f142afae4ce8359bb37
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication-preview"></a>Utwórz i zainstaluj pliki konfiguracji klienta sieci VPN do uwierzytelniania P2S RADIUS (wersja zapoznawcza)

Pliki konfiguracji klienta sieci VPN znajdują się w pliku zip. Pliki konfiguracji Podaj ustawienia wymagane dla natywnego klienta systemu Windows lub Mac IKEv2 sieci VPN do nawiązania połączenia przez punkt-lokacja sieci wirtualnej. Serwer usługi RADIUS udostępnia wiele opcji uwierzytelniania i, konfiguracja klienta sieci VPN może być różna dla każdej opcji.

### <a name="workflow"></a>Przepływ pracy

  1. Skonfiguruj bramę sieci VPN platformy Azure dla łączności P2S.
  2. Konfigurowanie serwera usługi RADIUS na potrzeby uwierzytelniania. 
  3. Uzyskaj Konfiguracja klienta sieci VPN w przypadku opcji uwierzytelniania wybraną i umożliwia konfigurowanie klienta sieci VPN na urządzeniu z systemem Windows. Dzięki temu można nawiązać połączenia z sieciami wirtualnymi platformy Azure za pośrednictwem połączenia P2S.

>[!IMPORTANT]
>Jeśli wystąpiły jakiekolwiek zmiany konfiguracji sieci VPN typu punkt-lokacja po wygenerowaniu profilu konfiguracji klienta sieci VPN, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, musisz wygenerować i zainstaluj nową konfigurację klienta sieci VPN na urządzeniach użytkownika.
>
>

## <a name="adeap"></a>Uwierzytelnianie nazwy użytkownika i hasła

* **Uwierzytelnianie usługi AD:** popularny scenariusz jest uwierzytelnianie domeny AD. W tym scenariuszu użytkownicy Użyj swoich poświadczeń domeny nawiązać połączenia z sieciami wirtualnymi platformy Azure. Można utworzyć plików konfiguracji klienta sieci VPN do uwierzytelniania RADIUS AD.

* **Uwierzytelnianie bez AD:** można także skonfigurować scenariusz uwierzytelniania RADIUS nazwy użytkownika i hasła bez AD.

Upewnij się, że wszyscy użytkownicy nawiązującego połączenie jest poświadczenia nazwy użytkownika i hasła, które mogą być uwierzytelniane za pośrednictwem usługi RADIUS. Można tworzyć tylko konfiguracji protokołu EAP-MSCHAPv2 uwierzytelnianie nazwy użytkownika i hasła. "-AuthenticationMethod" jest określony jako "EapMSChapv2".

### <a name="usernamefiles"></a>Generowanie plików konfiguracji klienta sieci VPN

Tworzenie konfiguracji klienta VPN za pomocą następującego polecenia:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Po uruchomieniu polecenia zwróci łącza. Skopiuj i Wklej łącze do przeglądarki sieci web, aby pobrać plik zip, o nazwie "VpnClientConfiguration.zip". Rozpakuj plik, aby wyświetlić następujące foldery: 
 
* Foldery o nazwie "WindowsAmd64" i "WindowsX86". Te foldery zawierają odpowiednio pakietów Instalatora Windows w wersji 64-bitowe i 32-bitowych. 
* Folder o nazwie "GenericDevice". Ten folder zawiera ogólne informacje używane do tworzenia własnych Konfiguracja klienta sieci VPN. Ignoruj ten folder.
* Jeśli protokół IKEv2 został skonfigurowany podczas tworzenia bramy sieci wirtualnej, zobacz się folder o nazwie Mac, który zawiera plik o nazwie "mobileconfig". Ten plik jest używany do konfigurowania klientów na komputery Mac.

Można pobrać plików konfiguracji klienta, które zostały już utworzone. Polecenia cmdlet "Get-AzureRmVpnClientConfiguration" zwraca adres URL (łącze), z której można pobrać pliku VpnClientConfiguration.zip. Jeśli wprowadzisz zmiany w konfiguracji sieci VPN P2S, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, konfiguracja nie jest automatycznie aktualizowany. Należy uruchomić polecenie cmdlet "New-AzureRmVpnClientConfiguration", aby odtworzyć konfiguracji.

Aby pobrać pliki konfiguracji wcześniej wygenerowanego klienta, użyj następującego polecenia:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```
 
### <a name="adwincli"></a>Konfigurowanie klienta VPN systemu Windows

Na każdym komputerze klienckim z systemem Windows można użyć tej samej konfiguracji pakietu klienta VPN, tak długo, jak wersja pasuje do architektury dla klienta. Aby uzyskać listę systemów operacyjnych klienta, które są obsługiwane, zobacz sekcję punkt-lokacja [— często zadawane pytania](vpn-gateway-vpn-faq.md#P2S).

Aby skonfigurować natywny klient VPN systemu Windows do uwierzytelniania certyfikatów, wykonaj następujące kroki:

1. Wybierz pliki konfiguracyjne klienta sieci VPN, które odpowiadają architektura komputera z systemem Windows. W przypadku 64-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupAmd64". W przypadku 32-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupX86". 
2. Kliknij dwukrotnie pakiet, aby go zainstalować. Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie. 

### <a name="admaccli"></a>Konfigurowanie klienta sieci VPN Mac (OS x)

1. Wybierz **VpnClientSetup mobileconfig** plików oraz wysyłać je do poszczególnych użytkowników. W tym celu można użyć poczty e-mail lub innej metody.

2. Zlokalizuj **mobileconfig** plików na komputerach Mac.

  ![Znajdź plik mobilconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Kliknij dwukrotnie profilu, aby go zainstalować, kliknij przycisk **Kontynuuj**. Nazwa profilu jest taka sama jak nazwa sieci wirtualnej.

  ![Zainstaluj](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Kliknij przycisk **Kontynuuj** ufasz nadawcy profilu i kontynuować instalację.

  ![Kontynuuj](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Podczas instalacji profilu podano opcję, aby określić nazwę użytkownika i hasło używane do uwierzytelniania sieci VPN. Nie jest to konieczne, aby wprowadzić te informacje. Jeśli zostanie określona, informacje są zapisywane i automatycznie używany podczas inicjowania połączenia. Kliknij przycisk **zainstalować** aby kontynuować.

  ![settings](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Wprowadź nazwę użytkownika i hasło niezbędne uprawnienia wymagane do zainstalowania profilu na komputerze. Kliknij przycisk **OK**.

  ![Nazwa użytkownika i hasło](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Po zainstalowaniu profilu jest widoczny w **profile** okno dialogowe. W tym oknie dialogowym można również otworzyć później z **preferencjach systemowych**.

  ! [preferencjach systemowych]] (. / media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Aby uzyskać dostęp do połączeń sieci VPN, należy otworzyć **sieci** okna dialogowego z **preferencjach systemowych**.

  ![Sieci](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. Połączenia sieci VPN jest pokazywana jako **IkeV2 VPN**. Nazwę można zmienić, aktualizując **mobileconfig** pliku.

  ![połączenie](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Kliknij przycisk **ustawienia uwierzytelniania**. Wybierz **Username** w listy rozwijanej i wprowadź swoje poświadczenia. Jeśli wprowadzone poświadczenia wcześniej, następnie **Username** zostanie automatycznie wybrany w listy rozwijanej i nazwa użytkownika i hasło są wstępnie wypełnione. Kliknij pozycję **OK**, aby zapisać ustawienia. To powoduje przejście do okna dialogowego sieci.

  ![uwierzytelniania](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Kliknij przycisk **Zastosuj** Aby zapisać zmiany. Aby zainicjować połączenie, kliknij przycisk **Connect**.

## <a name="certeap"></a>Uwierzytelnianie certyfikatu
 
Klient sieci VPN może tworzyć pliki konfiguracji uwierzytelniania certyfikatu usługi RADIUS, który korzysta z protokołu EAP-TLS. Zwykle certyfikat wystawiony przedsiębiorstwa jest używany do uwierzytelniania użytkownika dla sieci VPN. Upewnij się, że wszyscy użytkownicy łączącego mają certyfikat zainstalowany na urządzeniu użytkownika i czy można zweryfikować certyfikatu przez serwer usługi RADIUS.
 
* "-AuthenticationMethod" jest "EapTls".
* Podczas uwierzytelniania certyfikatu klienta sprawdza serwer RADIUS weryfikując swojego certyfikatu. -RadiusRootCert jest pliku .cer zawierającego certyfikatu głównego, który jest używany do sprawdzania poprawności serwera RADIUS.  
* Czasami urządzenia systemu Windows ma wiele certyfikatów klienta. Podczas uwierzytelniania może to spowodować podręcznego okna dialogowego Lista wszystkich certyfikatów. Użytkownik musi następnie wybierz certyfikat do użycia. Prawidłowy certyfikat można odfiltrowane przez określenie certyfikatu głównego, do którego należy łańcucha certyfikatów klienta. "-ClientRootCert" plik .cer, który zawiera certyfikat główny. Jest parametrem opcjonalnym. Jeśli urządzenie, z którym chcesz się połączyć ma tylko jeden certyfikat klienta, ten parametr nie muszą być określone.

### <a name="certfiles"></a>Generowanie plików konfiguracji klienta sieci VPN

Tworzenie konfiguracji klienta VPN za pomocą następującego polecenia:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root>
```

Wynik polecenia cmdlet zwraca łącze. Skopiuj i Wklej łącze do przeglądarki sieci web, aby pobrać plik zip, o nazwie "VpnClientConfiguration.zip". Rozpakuj plik, aby wyświetlić następujące foldery:

* Foldery o nazwie "WindowsAmd64" i "WindowsX86". Te foldery zawierają odpowiednio pakietów Instalatora Windows w wersji 64-bitowe i 32-bitowych. 
* Folder o nazwie "GenericDevice". Ten folder zawiera ogólne informacje używane do tworzenia własnych Konfiguracja klienta sieci VPN.

Można pobrać plików konfiguracji klienta, które zostały już utworzone. Polecenia cmdlet "Get-AzureRmVpnClientConfiguration" zwraca adres URL (łącze), z której można pobrać pliku VpnClientConfiguration.zip. Jeśli wprowadzisz zmiany w konfiguracji sieci VPN P2S, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, konfiguracja nie jest automatycznie aktualizowany. Należy uruchomić polecenie cmdlet "New-AzureRmVpnClientConfiguration", aby odtworzyć konfiguracji.

Aby pobrać pliki konfiguracji wcześniej wygenerowanego klienta, użyj następującego polecenia:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```
 
### <a name="certwincli"></a>Konfigurowanie klienta VPN systemu Windows

1. Wybierz pakiet konfiguracji i zainstaluj go na urządzeniu klienckim. W przypadku 64-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupAmd64". W przypadku 32-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupX86". Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**. Możesz także zapisać pakiet w celu zainstalowania go na innych komputerach klienckich.
2. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie.

### <a name="certmaccli"></a>Konfigurowanie klienta sieci MAC VPN

Należy utworzyć osobny profil dla wszystkich urządzeń Mac, które łączy sieć wirtualną Azure. Jest tak, ponieważ te urządzenia wymagają certyfikatu użytkownika do uwierzytelniania należy określić w profilu. **Ogólnego** znajduje się w nim wszystkie informacje wymagane do utworzenia profilu.

  * **VpnSettings.xml** zawiera ważne ustawienia, takie jak adres i tunelu typ serwera.
  * **VpnServerRoot.cer** wymaganego do sprawdzenia poprawności bramy sieci VPN podczas instalacji połączenie P2S certyfikatem głównym.
  * **RadiusServerRoot.cer** zawiera certyfikat główny, wymaganego do sprawdzenia poprawności serwera usługi RADIUS podczas uwierzytelniania.

Konfigurowanie natywny klient VPN w systemie Mac uwierzytelniania certyfikatów, wykonaj następujące kroki:

1. Importuj **VpnServerRoot** i **RadiusServerRoot** główny certyfikatów na komputerze Macintosh. Można to zrobić przez kopiowanie pliku przez do komputera Mac i kliknij go dwukrotnie.  
Kliknij przycisk **Dodaj** do zaimportowania.

  **Dodaj VpnServerRoot**

  ![Dodawanie certyfikatu](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  **Dodaj RadiusServerRoot**

  ![Dodawanie certyfikatu](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Otwórz **sieci** , okno dialogowe **preferencji sieci** i kliknij przycisk **"+"** Aby utworzyć nowy profil połączenia klienta sieci VPN dla połączenia P2S do sieci wirtualnej platformy Azure.

  **Interfejsu** wartość to "VPN" i **typ sieci VPN** wartość to "IKEv2". Określ nazwę profilu w **nazwa usługi** pola, a następnie kliknij przycisk **Utwórz** do tworzenia profilu połączenia VPN klienta.

  ![Sieci](./media/point-to-site-vpn-client-configuration-radius/network.png)
3. W **ogólnego** folder, z **VpnSettings.xml** pliku, skopiuj **VpnServer** wartość tagu. Wklej tę wartość w **adres serwera** i **Identyfikator zdalny** pola profilu. Pozostaw **identyfikator lokalny** pole puste.

  ![informacje o serwerze](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
4. Kliknij przycisk **ustawienia uwierzytelniania** i wybierz **certyfikatu**. 

  ![ustawienia uwierzytelniania](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
5. Kliknij przycisk **wybierz...** Aby wybrać certyfikat, który ma być używany do uwierzytelniania.

  ![certyfikat](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
6. **Wybierz tożsamość** zostanie wyświetlona lista certyfikatów dla można wybrać. Wybierz odpowiedni certyfikat, a następnie kliknij przycisk **Kontynuuj**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
7. W **identyfikator lokalny** Określ nazwę certyfikatu (z kroku 5). W tym przykładzie jest "ikev2Client.com". Następnie kliknij przycisk **Zastosuj** przycisk, aby zapisać zmiany.

  ![Zastosuj](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
8. Na **sieci** okna dialogowego, kliknij przycisk **Zastosuj** można zapisać wszystkich zmian. Następnie kliknij przycisk **Connect** nawiązaniu połączenia P2S do sieci wirtualnej platformy Azure.

## <a name="otherauth"></a>Inne typy uwierzytelniania lub protokołów

Aby użyć typu uwierzytelniania inny (na przykład OTP), a nie nazwy użytkownika i hasła lub certyfikatów lub użycia innego protokołu uwierzytelniania (np. PEAP-MS-CHAPv2, zamiast protokołu EAP-MSCHAPv2), należy utworzyć własny profil konfiguracji klienta sieci VPN. Aby utworzyć profil, potrzebne informacje, takie jak adres IP bramy sieci wirtualnej, Typ tunelu i trasy tunelowania podzielonego. Te informacje można uzyskać za pomocą następujących kroków:

1. Generowanie konfiguracji klienta sieci VPN dla EapMSChapv2 za pomocą polecenia cmdlet "Get-AzureRmVpnClientConfiguration". Aby uzyskać instrukcje, zobacz [w tej sekcji](#ccradius) artykułu.

2. Rozpakuj plik VpnClientConfiguration.zip i wyszukaj GenenericDevice folder. Ignoruj foldery zawierające pliki instalacyjne systemu Windows dla architektur 64-bitowe i 32-bitowych.
 
3. GenenericDevice folder zawiera plik XML o nazwie VpnSettings. Ten plik zawiera wszystkie wymagane informacje.

  * VpnServer - FQDN bramy sieci VPN platformy Azure. Jest to adres, który klient łączy się.
  * VpnType — Typ tunelu, który umożliwia połączenie.
  * Trasy - tras, na których należy skonfigurować w profilu, aby ruch powiązany tylko sieć wirtualna Azure jest przesyłane przez tunel P2S.
  * GenenericDevice folder zawiera także plik .cer o nazwie "VpnServerRoot". Ten plik zawiera certyfikat główny, wymaganego do sprawdzenia poprawności bramy sieci VPN platformy Azure, podczas konfigurowania połączenia P2S. Zainstaluj certyfikat na wszystkich urządzeniach, które będą łączyć się z sieci wirtualnej platformy Azure. 
 
## <a name="next-steps"></a>Następne kroki

Wróć do tego artykułu, aby [ukończyć konfigurację P2S](point-to-site-how-to-radius-ps.md).
