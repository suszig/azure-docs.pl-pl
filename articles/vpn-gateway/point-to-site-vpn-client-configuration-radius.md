---
title: "Utwórz i zainstaluj pliki konfiguracji klienta sieci VPN dla połączeń P2S RADIUS: środowiska PowerShell: Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie klienta systemu Windows, Mac OS X i Linux VPN pliki konfiguracji dla połączeń, które korzystają z uwierzytelniania usługi RADIUS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: ce914d2fd0472855ad7a17bf64ae43a76ceb5743
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Utwórz i zainstaluj pliki konfiguracji klienta sieci VPN do uwierzytelniania P2S RADIUS

Aby połączyć sieć wirtualną za pośrednictwem punkt-lokacja, należy skonfigurować urządzenia klienckiego, z którego będą łączyć. Można utworzyć połączenia sieci VPN P2S z systemu Windows, Mac OS x i Linux urządzeń klienckich. Podczas korzystania z uwierzytelniania usługi RADIUS, istnieje wiele opcji uwierzytelniania: uwierzytelnianie nazwy użytkownika i hasła, uwierzytelnianie certyfikatu, a także inne typy uwierzytelniania. Konfiguracja klienta sieci VPN jest różne dla każdego typu uwierzytelniania. Aby skonfigurować klienta sieci VPN, należy użyć plików konfiguracji klienta, które zawierają wymagane ustawienia. Ten artykuł pomaga utworzyć i zainstalować Konfiguracja klienta sieci VPN dla typu uwierzytelniania usługi RADIUS, którego chcesz używać.

Konfiguracja przepływu pracy do uwierzytelniania P2S RADIUS wygląda następująco:

1. [Konfigurowanie bramy sieci VPN platformy Azure dla łączności P2S](point-to-site-how-to-radius-ps.md).
2. [Konfigurowanie serwera usługi RADIUS na potrzeby uwierzytelniania](point-to-site-how-to-radius-ps.md#radius). 
3. **Uzyskaj Konfiguracja klienta sieci VPN w przypadku opcji uwierzytelniania wybraną i umożliwia konfigurowanie klienta sieci VPN**. (W tym artykule)
4. [Ukończenie konfiguracji P2S i połącz](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Jeśli wystąpiły jakiekolwiek zmiany konfiguracji sieci VPN typu punkt-lokacja po wygenerowaniu profilu konfiguracji klienta sieci VPN, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, musisz wygenerować i zainstaluj nową konfigurację klienta sieci VPN na urządzeniach użytkownika.
>
>

Aby użyć sekcje w tym artykule, najpierw zdecydować, jaki typ uwierzytelniania do użycia: nazwy użytkownika i hasła, certyfikatu lub inne typy uwierzytelniania. W każdej sekcji istnieją kroki dla systemu Windows, Mac OS X i Linux (ograniczone procedura w tej chwili niedostępna).

## <a name="adeap"></a>Uwierzytelnianie nazwy użytkownika i hasła

Istnieją dwa sposoby konfigurowania uwierzytelnianie nazwy użytkownika i hasła. Można albo skonfigurować uwierzytelnianie przy użyciu usługi AD lub uwierzytelniania bez użycia usługi AD. Z każdym z tych scenariuszy upewnij się, że wszyscy użytkownicy nawiązującego połączenie jest poświadczenia nazwy użytkownika i hasła, które mogą być uwierzytelniane za pośrednictwem usługi RADIUS.

* Konfigurując uwierzytelnianie nazwy użytkownika i hasła, można tworzyć tylko konfiguracji protokołu EAP-MSCHAPv2 uwierzytelnianie nazwy użytkownika i hasła.
* "-AuthenticationMethod" jest "EapMSChapv2".

### <a name="usernamefiles"></a> 1. Generowanie plików konfiguracji klienta sieci VPN

Generowanie plików konfiguracji klienta sieci VPN do użycia z uwierzytelnianie nazwy użytkownika i hasła. Można wygenerować pliki konfiguracji klienta VPN za pomocą następującego polecenia:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Uruchamiając polecenie zwraca łącze. Skopiuj i Wklej łącze do przeglądarki sieci web, aby pobrać "VpnClientConfiguration.zip". Rozpakuj plik, aby wyświetlić następujące foldery: 
 
* **WindowsAmd64** i **WindowsX86** -te foldery zawierają odpowiednio pakietów Instalatora Windows w wersji 64-bitowe i 32-bitowych. 
* **Ogólny** — ten folder zawiera ogólne informacje używane do tworzenia własnych Konfiguracja klienta sieci VPN. Ten folder nie jest wymagany dla nazwy użytkownika i hasła konfiguracji uwierzytelniania.
* **Mac** — Jeśli IKEv2 zostało skonfigurowane podczas tworzenia bramy sieci wirtualnej, zobacz folder o nazwie Mac, który zawiera **mobileconfig** pliku. Ten plik jest używany do konfigurowania klientów na komputery Mac.

Jeśli utworzono już klienta pliki konfiguracji, można je odzyskać za pomocą polecenia cmdlet "Get-AzureRmVpnClientConfiguration". Jednak jeśli wprowadzisz zmiany w konfiguracji sieci VPN P2S, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, konfiguracja nie jest aktualizowany automatycznie. Należy uruchomić polecenia cmdlet "New-AzureRmVpnClientConfiguration" Aby utworzyć nowe pobranie konfiguracji.

Aby pobrać pliki konfiguracji wcześniej wygenerowanego klienta, użyj następującego polecenia:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. Konfigurowanie klientów sieci VPN

Można skonfigurować następujące klientów sieci VPN:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Systemu Linux przy użyciu strongSwan](#adlinuxcli)
 
#### <a name="adwincli"></a>Konfiguracja klienta VPN systemu Windows

Na każdym komputerze klienckim z systemem Windows można użyć tej samej konfiguracji pakietu klienta VPN, tak długo, jak wersja pasuje do architektury dla klienta. Aby uzyskać listę systemów operacyjnych klienta, które są obsługiwane, zobacz sekcję punkt-lokacja [— często zadawane pytania](vpn-gateway-vpn-faq.md#P2S).

Aby skonfigurować natywny klient VPN systemu Windows do uwierzytelniania certyfikatów, wykonaj następujące kroki:

1. Wybierz pliki konfiguracyjne klienta sieci VPN, które odpowiadają architektura komputera z systemem Windows. W przypadku 64-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupAmd64". W przypadku 32-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupX86". 
2. Aby zainstalować pakiet, kliknij dwukrotnie. Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie. 

#### <a name="admaccli"></a>Instalacja klienta sieci VPN Mac (OS X)

1. Wybierz **VpnClientSetup mobileconfig** plików oraz wysyłać je do poszczególnych użytkowników. Możesz użyć poczty e-mail lub innej metody.

2. Zlokalizuj **mobileconfig** plików na komputerach Mac.

  ![Znajdź plik mobilconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Kliknij dwukrotnie profilu, aby go zainstalować, kliknij przycisk **Kontynuuj**. Nazwa profilu jest taka sama jak nazwa sieci wirtualnej.

  ![Zainstaluj](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Kliknij przycisk **Kontynuuj** ufasz nadawcy profilu i kontynuować instalację.

  ![kontynuuj](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Podczas instalacji profilu podano opcję, aby określić nazwę użytkownika i hasło używane do uwierzytelniania sieci VPN. Nie jest to konieczne, aby wprowadzić te informacje. Jeśli zostanie określona, informacje są zapisywane i automatycznie używany podczas inicjowania połączenia. Kliknij przycisk **zainstalować** aby kontynuować.

  ![settings](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Wprowadź nazwę użytkownika i hasło niezbędne uprawnienia wymagane do zainstalowania profilu na komputerze. Kliknij przycisk **OK**.

  ![Nazwa użytkownika i hasło](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Po zainstalowaniu profilu jest widoczny w **profile** okno dialogowe. W tym oknie dialogowym można również otworzyć później z **preferencjach systemowych**.

  ![Preferencje systemu](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Aby uzyskać dostęp do połączeń sieci VPN, należy otworzyć **sieci** okna dialogowego z **preferencjach systemowych**.

  ![Sieci](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. Połączenia sieci VPN jest pokazywana jako **IkeV2 VPN**. Nazwę można zmienić, aktualizując **mobileconfig** pliku.

  ![połączenie](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Kliknij przycisk **ustawienia uwierzytelniania**. Wybierz **Username** w listy rozwijanej i wprowadź swoje poświadczenia. Jeśli wprowadzone poświadczenia wcześniej, następnie **Username** zostanie automatycznie wybrany w listy rozwijanej i nazwa użytkownika i hasło są wstępnie wypełnione. Kliknij pozycję **OK**, aby zapisać ustawienia. To powoduje przejście do okna dialogowego sieci.

  ![uwierzytelniania](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Kliknij przycisk **Zastosuj** Aby zapisać zmiany. Aby zainicjować połączenie, kliknij przycisk **Connect**.

#### <a name="adlinuxcli"></a>Konfiguracja klienta VPN systemu Linux przy użyciu strongSwan

Poniższe instrukcje zostały utworzone przy użyciu strongSwan 5.5.1 na Ubuntu 17.0.4. Rzeczywiste Ekrany mogą się różnić w zależności od używanej wersji systemu Linux i strongSwan.

1. Otwórz **terminali** do zainstalowania **strongSwan** i jego Menedżera sieci, za pomocą polecenia w przykładzie. Jeśli zostanie wyświetlony błąd związany z "libcharon-extra wtyczek", należy ją zastąpić "strongswan wtyczka eap-mschapv2".

  ```Terminal
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Kliknij przycisk **Menedżera sieci** ikona (up strzałkę/strzałki w dół), a następnie wybierz **edytować połączenia**.

  ![Edytowanie połączenia](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Kliknij przycisk **Dodaj** przycisk, aby utworzyć nowe połączenie.

  ![Dodaj połączenie](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Wybierz **IPsec/IKEv2 (strongswan)** z menu rozwijanego, następnie kliknij przycisk **Utwórz**. Można zmienić nazwę połączenia w tym kroku.

  ![Dodaj ikev2](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Otwórz **VpnSettings.xml** plik z **ogólnego** folderu plików konfiguracji klienta pobrane. Znajdź tag o nazwie **VpnServer** i skopiuj nazwę, począwszy od "azuregateway" i kończąc ". cloudapp.net".

  ![Ustawienia sieci VPN](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Wklej tej nazwy do **adres** pole nowego połączenia sieci VPN, w obszarze **bramy** sekcji. Następnie kliknij ikonę folderu na końcu **certyfikatu** pola, przejdź do folderu, ogólny, a następnie wybierz **VpnServerRoot** plik znajdujący się.
7. W obszarze **klienta** sekcji połączenia, wybierz **EAP** dla **uwierzytelniania**, a następnie wprowadź nazwę użytkownika/hasło. Konieczne może być wybierz ikonę blokady po prawej stronie, aby zapisać te informacje. Następnie kliknij przycisk **zapisać**.

  ![Edytowanie ustawień połączenia](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Kliknij przycisk **Menedżera sieci** ikony (up strzałkę/strzałki w dół), jak i zatrzymaj wskaźnik myszy **połączeń sieci VPN**. Zostanie wyświetlone połączenia sieci VPN, który został utworzony. Aby zainicjować połączenie, wybierz połączenia.

  ![połączenie usługi radius](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Uwierzytelnianie certyfikatu
 
Klient sieci VPN może tworzyć pliki konfiguracji uwierzytelniania certyfikatu usługi RADIUS, który korzysta z protokołu EAP-TLS. Zwykle certyfikat wystawiony przedsiębiorstwa jest używany do uwierzytelniania użytkownika dla sieci VPN. Upewnij się, że wszyscy użytkownicy łączącego mają certyfikat zainstalowany na urządzeniu użytkownika i czy można zweryfikować certyfikatu przez serwer usługi RADIUS.
 
* "-AuthenticationMethod" jest "EapTls".
* Podczas uwierzytelniania certyfikatu klienta sprawdza serwer RADIUS weryfikując swojego certyfikatu. -RadiusRootCert jest pliku .cer zawierającego certyfikatu głównego, który jest używany do sprawdzania poprawności serwera RADIUS.
* Każde urządzenie klienta sieci VPN wymaga zainstalowany certyfikat klienta.
* Czasami urządzenia systemu Windows ma wiele certyfikatów klienta. Podczas uwierzytelniania może to spowodować podręcznego okna dialogowego Lista wszystkich certyfikatów. Użytkownik musi następnie wybierz certyfikat do użycia. Prawidłowy certyfikat można odfiltrowane przez określenie certyfikatu głównego, do którego należy łańcucha certyfikatów klienta. "-ClientRootCert" plik .cer, który zawiera certyfikat główny. Jest parametrem opcjonalnym. Jeśli urządzenie, z którym chcesz się połączyć ma tylko jeden certyfikat klienta, ten parametr nie muszą być określone.

### <a name="certfiles"></a>1. Generowanie plików konfiguracji klienta sieci VPN

Generowanie plików konfiguracji klienta sieci VPN do użycia przy użyciu uwierzytelniania certyfikatu. Można wygenerować pliki konfiguracji klienta VPN za pomocą następującego polecenia:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Uruchamiając polecenie zwraca łącze. Skopiuj i Wklej łącze do przeglądarki sieci web, aby pobrać "VpnClientConfiguration.zip". Rozpakuj plik, aby wyświetlić następujące foldery:

* **WindowsAmd64** i **WindowsX86** -te foldery zawierają odpowiednio pakietów Instalatora Windows w wersji 64-bitowe i 32-bitowych. 
* **GenericDevice** — ten folder zawiera ogólne informacje używane do tworzenia własnych Konfiguracja klienta sieci VPN.

Jeśli utworzono już klienta pliki konfiguracji, można je odzyskać za pomocą polecenia cmdlet "Get-AzureRmVpnClientConfiguration". Jednak jeśli wprowadzisz zmiany w konfiguracji sieci VPN P2S, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, konfiguracja nie jest aktualizowany automatycznie. Należy uruchomić polecenia cmdlet "New-AzureRmVpnClientConfiguration" Aby utworzyć nowe pobranie konfiguracji.

Aby pobrać pliki konfiguracji wcześniej wygenerowanego klienta, użyj następującego polecenia:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. Konfigurowanie klientów sieci VPN

Można skonfigurować następujące klientów sieci VPN:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (obsługiwane, nie artykułu kroki jeszcze)

#### <a name="certwincli"></a>Konfiguracja klienta VPN systemu Windows

1. Wybierz pakiet konfiguracji i zainstaluj go na urządzeniu klienckim. W przypadku 64-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupAmd64". W przypadku 32-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupX86". Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**. Możesz także zapisać pakiet w celu zainstalowania go na innych komputerach klienckich.
2. Każdy klient wymaga certyfikatu klienta w celu uwierzytelnienia. Zainstaluj certyfikat klienta. Informacji o certyfikatach klientów, zobacz [certyfikaty klientów dla lokacji punktu](vpn-gateway-certificates-point-to-site.md). Aby zainstalować certyfikat, który został wygenerowany, zobacz [zainstalować certyfikat na klientach systemu Windows](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie.

#### <a name="certmaccli"></a>Instalacja klienta sieci VPN Mac (OS X)

Należy utworzyć osobny profil dla wszystkich urządzeń Mac, które łączy sieć wirtualną Azure. Jest tak, ponieważ te urządzenia wymagają certyfikatu użytkownika do uwierzytelniania należy określić w profilu. **Ogólnego** znajduje się w nim wszystkie informacje wymagane do utworzenia profilu.

  * **VpnSettings.xml** zawiera ważne ustawienia, takie jak adres i tunelu typ serwera.
  * **VpnServerRoot.cer** wymaganego do sprawdzenia poprawności bramy sieci VPN podczas instalacji połączenie P2S certyfikatem głównym.
  * **RadiusServerRoot.cer** zawiera certyfikat główny, wymaganego do sprawdzenia poprawności serwera usługi RADIUS podczas uwierzytelniania.

Konfigurowanie natywny klient VPN w systemie Mac uwierzytelniania certyfikatów, wykonaj następujące kroki:

1. Importuj **VpnServerRoot** i **RadiusServerRoot** główny certyfikatów na komputerze Macintosh. Można to zrobić przez kopiowanie pliku przez do komputera Mac i kliknij go dwukrotnie.  
Kliknij przycisk **Dodaj** do zaimportowania.

  *Add VpnServerRoot*

  ![Dodawanie certyfikatu](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  *Add RadiusServerRoot*

  ![Dodawanie certyfikatu](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Każdy klient wymaga certyfikatu klienta w celu uwierzytelnienia. Zainstaluj certyfikat klienta na urządzeniu klienckim.
3. Otwórz **sieci** , okno dialogowe **preferencji sieci** i kliknij przycisk **"+"** Aby utworzyć nowy profil połączenia klienta sieci VPN dla połączenia P2S do sieci wirtualnej platformy Azure.

  **Interfejsu** wartość to "VPN" i **typ sieci VPN** wartość to "IKEv2". Określ nazwę profilu w **nazwa usługi** pola, a następnie kliknij przycisk **Utwórz** do tworzenia profilu połączenia VPN klienta.

  ![Sieci](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. W **ogólnego** folder, z **VpnSettings.xml** pliku, skopiuj **VpnServer** wartość tagu. Wklej tę wartość w **adres serwera** i **Identyfikator zdalny** pola profilu. Pozostaw **identyfikator lokalny** pole puste.

  ![informacje o serwerze](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Kliknij przycisk **ustawienia uwierzytelniania** i wybierz **certyfikatu**. 

  ![ustawienia uwierzytelniania](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Kliknij przycisk **wybierz...** Aby wybrać certyfikat, który ma być używany do uwierzytelniania.

  ![certyfikat](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Wybierz tożsamość** zostanie wyświetlona lista certyfikatów dla można wybrać. Wybierz odpowiedni certyfikat, a następnie kliknij przycisk **Kontynuuj**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. W **identyfikator lokalny** Określ nazwę certyfikatu (z kroku 6). W tym przykładzie jest "ikev2Client.com". Następnie kliknij przycisk **Zastosuj** przycisk, aby zapisać zmiany.

  ![zastosuj](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. Na **sieci** okna dialogowego, kliknij przycisk **Zastosuj** można zapisać wszystkich zmian. Następnie kliknij przycisk **Connect** nawiązaniu połączenia P2S do sieci wirtualnej platformy Azure.

## <a name="otherauth"></a>Praca z innych typów uwierzytelniania lub protokołów

Aby użyć typu uwierzytelniania inny (na przykład OTP), a nie nazwy użytkownika i hasła lub certyfikatów lub użycia innego protokołu uwierzytelniania (np. PEAP-MS-CHAPv2, zamiast protokołu EAP-MSCHAPv2), należy utworzyć własny profil konfiguracji klienta sieci VPN. Aby utworzyć profil, potrzebne informacje, takie jak adres IP bramy sieci wirtualnej, Typ tunelu i trasy tunelowania podzielonego. Te informacje można uzyskać za pomocą następujących kroków:

1. Generowanie konfiguracji klienta sieci VPN dla EapMSChapv2 za pomocą polecenia cmdlet "Get-AzureRmVpnClientConfiguration". Aby uzyskać instrukcje, zobacz [w tej sekcji](#ccradius) artykułu.

2. Rozpakuj plik VpnClientConfiguration.zip i wyszukaj GenenericDevice folder. Ignoruj foldery zawierające pliki instalacyjne systemu Windows dla architektur 64-bitowe i 32-bitowych.
 
3. GenenericDevice folder zawiera plik XML o nazwie VpnSettings. Ten plik zawiera wszystkie wymagane informacje.

  * VpnServer - FQDN bramy sieci VPN platformy Azure. Jest to adres, który klient łączy się.
  * VpnType — Typ tunelu, który umożliwia połączenie.
  * Trasy - tras, na których należy skonfigurować w profilu, aby ruch powiązany tylko sieć wirtualna Azure jest przesyłane przez tunel P2S.
  * GenenericDevice folder zawiera także plik .cer o nazwie "VpnServerRoot". Ten plik zawiera certyfikat główny, wymaganego do sprawdzenia poprawności bramy sieci VPN platformy Azure, podczas konfigurowania połączenia P2S. Zainstaluj certyfikat na wszystkich urządzeniach, które będą łączyć się z sieci wirtualnej platformy Azure.

## <a name="next-steps"></a>Kolejne kroki

Wróć do tego artykułu, aby [ukończyć konfigurację P2S](point-to-site-how-to-radius-ps.md).

Aby uzyskać informacje dotyczące rozwiązywania problemów P2S [połączenia punkt lokacja Azure Rozwiązywanie problemów z](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).