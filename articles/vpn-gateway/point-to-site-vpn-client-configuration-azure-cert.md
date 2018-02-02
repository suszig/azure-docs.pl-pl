---
title: "Utwórz i zainstaluj pliki konfiguracji klienta P2S VPN uwierzytelniania certyfikatu Azure: środowiska PowerShell: Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł ułatwia tworzenie i zainstaluj pliki konfiguracji klienta sieci VPN dla połączenia punkt-lokacja, które korzystają z certyfikatu uwierzytelniania."
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
ms.date: 01/29/2018
ms.author: cherylmc
ms.openlocfilehash: efe5d3db16db83568bb844894198b59a6b39f626
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Utwórz i zainstaluj pliki konfiguracji klienta sieci VPN w przypadku konfiguracji uwierzytelniania P2S natywnego certyfikatu Azure

Pliki konfiguracji klienta sieci VPN znajdują się w pliku zip. Pliki konfiguracji Podaj ustawienia wymagane dla natywnego klienta systemu Windows lub Mac IKEv2 sieci VPN do nawiązania połączenia sieci wirtualnej za pośrednictwem połączenia punkt-lokacja, które używają uwierzytelniania natywnego certyfikatu Azure.

### <a name="workflow"></a>Przepływ pracy P2S

  1. Skonfiguruj bramę sieci VPN platformy Azure dla połączeń P2S.
  2. Generuj certyfikat główny i certyfikaty klientów. Przekazać klucz publiczny certyfikatu głównego na platformie Azure, aby zainstalować certyfikaty klientów na urządzeniach klienckich. Certyfikaty są używane do uwierzytelniania użytkowników nawiązującego połączenie.
  3. Uzyskaj Konfiguracja klienta sieci VPN dla opcji uwierzytelniania wybranych przez użytkownika i umożliwia konfigurowanie klienta sieci VPN na urządzeniach z systemem Windows i Mac. Dzięki temu można nawiązać połączenia z sieciami wirtualnymi platformy Azure za pośrednictwem połączenia punkt-lokacja.

>[!NOTE]
>Pliki konfiguracji klienta są specyficzne dla konfiguracji sieci VPN dla sieci wirtualnej. W przypadku zmiany wprowadzone w konfiguracji sieci VPN typu punkt-lokacja po wygenerowaniu plików konfiguracji klienta sieci VPN, takie jak typ protokołu sieci VPN lub typ uwierzytelniania, należy wygenerować nowe pliki konfiguracji klienta sieci VPN dla urządzeń użytkownika.
>
>

## <a name="generate"></a>Generowanie plików konfiguracji klienta sieci VPN

Przed rozpoczęciem upewnij się, że wszyscy użytkownicy nawiązującego połączenie jest prawidłowy certyfikat zainstalowany na urządzeniu użytkownika. Aby uzyskać więcej informacji na temat instalowania certyfikatu klienta, zobacz [zainstalować certyfikat klienta](point-to-site-how-to-vpn-client-install-azure-cert.md).

Pliki konfiguracji klienta przy użyciu programu PowerShell, można wygenerować lub za pomocą portalu Azure. Każda metoda zwraca tego samego pliku zip. Rozpakuj plik, aby wyświetlić następujące foldery:

  * **WindowsAmd64** i **WindowsX86**, pakietów Instalatora Windows 32-bitowe i 64-bitowe, które zawierają odpowiednio. **WindowsAmd64** pakiet Instalatora jest dla wszystkich obsługiwanych klientów systemu Windows 64-bit Amd nie tylko.
  * **Ogólny**, który zawiera informacje ogólne, służący do tworzenia własnych konfiguracji klienta sieci VPN. Ignoruj ten folder. Folderze ogólnym jest podany, jeśli protokół IKEv2 i SSTP + IKEv2 została skonfigurowana dla bramy. Jeśli jest skonfigurowany protokół SSTP, tylko ogólnego folder nie jest obecny.

### <a name="zipportal"></a>Generowanie plików za pomocą portalu Azure

1. W portalu Azure przejdź do bramy sieci wirtualnej dla sieci wirtualnej, który chcesz nawiązać połączenie.
2. Na stronie bramy sieci wirtualnej, kliknij przycisk **punkt lokacja konfiguracji**.
3. W górnej części strony konfiguracji punkt lokacja, kliknij przycisk **klienta VPN Pobierz**. Trwa kilka minut dla klienta pakiet konfiguracji do wygenerowania.
4. Przeglądarka wskazuje, że plik zip konfiguracji klienta jest dostępny. Nazywa się taką samą nazwę jak bramy. Rozpakuj plik, aby wyświetlić foldery.

### <a name="zipps"></a>Generowanie plików za pomocą programu PowerShell

1. Podczas generowania konfiguracji klienta sieci VPN plików, wartość "-AuthenticationMethod" jest "EapTls". Generuj pliki konfiguracji klienta VPN za pomocą następującego polecenia:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Skopiuj adres URL do przeglądarki, aby pobrać plik zip, a następnie Rozpakuj plik, aby wyświetlić foldery.

## <a name="installwin"></a>Zainstaluj pakiet konfiguracji klienta VPN systemu Windows

Na każdym komputerze klienckim z systemem Windows można użyć tej samej konfiguracji pakietu klienta VPN, tak długo, jak wersja pasuje do architektury dla klienta. Aby uzyskać listę systemów operacyjnych klienta, które są obsługiwane, zobacz sekcję punkt-lokacja [bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#P2S).

Aby skonfigurować natywny klient VPN systemu Windows do uwierzytelniania certyfikatów, wykonaj następujące kroki:

1. Wybierz pliki konfiguracyjne klienta sieci VPN, które odpowiadają architektura komputera z systemem Windows. W przypadku 64-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupAmd64". W przypadku 32-bitowy procesor wybierz pakiet Instalatora "VpnClientSetupX86". 
2. Kliknij dwukrotnie pakiet, aby go zainstalować. Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie. 

## <a name="installmac"></a>Konfiguracja klienta sieci VPN na komputerach Mac. (OS x)

Azure nie zawiera pliku mobileconfig natywnego Azure certyfikat uwierzytelniania. Należy ręcznie skonfigurować natywny klient IKEv2 VPN na każdy Mac, w którym będą łączyć się Azure. **Ogólnego** znajduje się w nim wszystkie informacje, należy skonfigurować go. Jeśli nie widzisz folderze ogólnym w pobieranie jest prawdopodobne, że protokół IKEv2 nie został wybrany jako typ tunelu. Po wybraniu IKEv2 Generowanie pliku zip ponownie, aby pobrać folderze ogólnym. Ogólny folder zawiera następujące pliki:

* **VpnSettings.xml**, który zawiera ważne ustawienia, takie jak adres i tunelu typ serwera. 
* **VpnServerRoot.cer**, który zawiera certyfikat główny, wymaganego do sprawdzenia poprawności bramy sieci VPN platformy Azure, podczas konfigurowania połączenia P2S.

Poniższe kroki umożliwiają konfigurowanie natywny klient VPN w systemie Mac uwierzytelniania certyfikatu. Należy wykonać te kroki na każdym Mac, którym będą łączyć się Azure:

1. Importuj **VpnServerRoot** certyfikatu głównego z komputerów Mac. Można to zrobić przez kopiowanie pliku przez do komputera Mac i go dwukrotnie.  
Kliknij przycisk **Dodaj** do zaimportowania.

  ![Dodawanie certyfikatu](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Dwukrotne kliknięcie certyfikatu nie może być wyświetlany **Dodaj** okna dialogowego, ale certyfikat jest zainstalowany w poprawnym magazynie. Można sprawdzić certyfikatu w łańcuchu kluczy logowania z kategorii certyfikatów.
  
2. Otwórz **sieci** , okno dialogowe **preferencji sieci** i kliknij przycisk **"+"** Aby utworzyć nowy profil połączenia klienta sieci VPN dla połączenia P2S do sieci wirtualnej platformy Azure.

  **Interfejsu** wartość to "VPN" i **typ sieci VPN** wartość to "IKEv2". Określ nazwę profilu w **nazwa usługi** pola, a następnie kliknij przycisk **Utwórz** do tworzenia profilu połączenia VPN klienta.

  ![Sieci](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
3. W **ogólnego** folder, z **VpnSettings.xml** pliku, skopiuj **VpnServer** wartość tagu. Wklej tę wartość w **adres serwera** i **Identyfikator zdalny** pola profilu.

  ![informacje o serwerze](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
4. Kliknij przycisk **ustawienia uwierzytelniania** i wybierz **certyfikatu**. 

  ![ustawienia uwierzytelniania](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
5. Kliknij przycisk **wybierz...** Wybierz certyfikat klienta, który ma być używany do uwierzytelniania. Certyfikat klienta powinien być zainstalowany na komputerze (zobacz krok #2 w **przepływu pracy P2S** powyższej sekcji).

  ![certyfikat](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
6. **Wybierz tożsamość** zostanie wyświetlona lista certyfikatów dla można wybrać. Wybierz odpowiedni certyfikat, a następnie kliknij przycisk **Kontynuuj**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
7. W **identyfikator lokalny** Określ nazwę certyfikatu (z kroku 6). W tym przykładzie jest "ikev2Client.com". Następnie kliknij przycisk **Zastosuj** przycisk, aby zapisać zmiany.

  ![zastosuj](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
8. Na **sieci** okna dialogowego, kliknij przycisk **Zastosuj** można zapisać wszystkich zmian. Następnie kliknij przycisk **Connect** nawiązaniu połączenia P2S do sieci wirtualnej platformy Azure.

## <a name="next-steps"></a>Następne kroki

Wróć do tego artykułu, aby [ukończyć konfigurację P2S](vpn-gateway-howto-point-to-site-rm-ps.md).
