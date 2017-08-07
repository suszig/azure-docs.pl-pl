---
title: "Łączenie komputera z siecią wirtualną platformy Azure przy użyciu połączenia typu punkt-lokacja i uwierzytelniania certyfikatu: PowerShell | Microsoft Docs"
description: "Bezpiecznie połącz komputer z siecią wirtualną platformy Azure przez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu uwierzytelniania certyfikatu. Ten artykuł ma zastosowanie w modelu wdrażania przy użyciu usługi Resource Manager i używa programu PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/27/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 6e76ac40e9da2754de1d1aa50af3cd4e04c067fe
ms.openlocfilehash: 27484932f13a85bef29b7a19b4f06b75722b4c38
ms.contentlocale: pl-pl
ms.lasthandoff: 07/31/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-powershell"></a>Konfigurowanie połączenia typu punkt-lokacja z siecią wirtualną przy użyciu uwierzytelniania certyfikatu: PowerShell

W tym artykule pokazano sposób tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja w modelu wdrażania przy użyciu usługi Resource Manager za pomocą witryny programu PowerShell. Ta konfiguracja korzysta z certyfikatów do uwierzytelniania klienta nawiązującego połączenie. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Portal Azure (klasyczny)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenia punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenie sieci VPN typu punkt-lokacja jest inicjowane z komputera klienckiego za pomocą natywnego klienta VPN systemu Windows. Łączący się klienci używają certyfikatów do uwierzytelniania. 

![Łączenie komputera z siecią wirtualną platformy Azure — diagram połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Połączenia typu punkt-lokacja z uwierzytelnianiem certyfikatu mają następujące wymagania:

* Brama sieci VPN oparta na trasie.
* Klucz publiczny (plik cer) dla certyfikatu głównego, przekazany na platformę Azure. Jest uznawany za certyfikat zaufany i używany do uwierzytelniania.
* Certyfikat klienta wygenerowany na podstawie certyfikatu głównego i zainstalowany na każdym komputerze klienckim, który będzie nawiązywać połączenie. Ten certyfikat jest używany do uwierzytelniania klientów.
* Pakiet konfiguracji klienta sieci VPN musi zostać wygenerowany i zainstalowany na każdym komputerze klienckim, który nawiązuje połączenie. Pakiet konfiguracji klienta konfiguruje natywnego klienta sieci VPN, który znajduje się już w systemie operacyjnym klienta, ustawiając w nim informacje niezbędne do łączenia się z siecią wirtualną.

Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN ani lokalnego publicznego adresu IP. Połączenie sieci VPN jest nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Po stronie serwera obsługiwany jest protokół SSTP w wersji 1.0, 1.1 i 1.2. Klient decyduje o wyborze wersji do użycia. W przypadku systemu Windows 8.1 i nowszych protokół SSTP domyślnie używa wersji 1.2. 

Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

## <a name="before-beginning"></a>Przed rozpoczęciem

* Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).
* Zainstaluj najnowszą wersję poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu PowerShell, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

### <a name="example"></a>Przykładowe wartości

Wartości przykładowych możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule. Ustawianie zmiennych opisano w sekcji [1](#declare) artykułu. Można postępować zgodnie z opisanymi krokami i użyć przedstawionych wartości bez ich zmieniania lub zmienić je, aby odzwierciedlały dane środowisko. 

* **Nazwa: VNet1**
* **Przestrzeń adresowa: 192.168.0.0/16** i **10.254.0.0/16**<br>W tym przykładzie używamy więcej niż jednej przestrzeni adresowej, aby zilustrować, że ta konfiguracja współpracuje z wieloma przestrzeniami adresowymi. Jednak ta konfiguracja nie wymaga wielu przestrzeni adresowych.
* **Nazwa podsieci: FrontEnd**
  * **Zakres adresów podsieci: 192.168.1.0/24**
* **Nazwa podsieci: BackEnd**
  * **Zakres adresów podsieci: 10.254.1.0/24**
* **Nazwa podsieci: GatewaySubnet**<br>Nazwa podsieci *GatewaySubnet* jest obowiązkowa, aby brama VPN mogła działać.
  * **Zakres adresów podsieci bramy: 192.168.200.0/24** 
* **Pula adresów klienta sieci VPN: 172.16.201.0/24**<br>Klienci sieci VPN łączący się z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z puli adresów klientów sieci VPN.
* **Subskrypcja:** jeśli masz więcej niż jedną subskrypcję, sprawdź, czy korzystasz z właściwej.
* **Grupa zasobów: TestRG**
* **Lokalizacja: Wschodnie stany USA**
* **Serwer DNS: adres IP** serwera DNS, który ma być używany do rozpoznawania nazw.
* **Nazwa bramy: Vnet1GW**
* **Nazwa publicznego adresu IP: VNet1GWPIP**
* **VpnType: RouteBased** 

## <a name="declare"></a>1 — Logowanie się i ustawianie zmiennych

W tej sekcji należy zalogować się i zadeklarować wartości używane dla tej konfiguracji. Zadeklarowane wartości są używane w przykładowych skryptach. Można zmienić wartości, aby odzwierciedlić własne środowisko. Ale można też użyć zadeklarowanych wartości i postępować zgodnie z opisanymi krokami tylko w celach szkoleniowych.

1. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i zaloguj się na konto platformy Azure. Polecenie cmdlet wyświetla monit o poświadczenia logowania. Po zalogowaniu pobiera ono ustawienia konta, aby były dostępne dla programu Azure PowerShell.

  ```powershell
  Login-AzureRmAccount
  ```
2. Pobierz listę subskrypcji platformy Azure.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Wskaż subskrypcję, której chcesz użyć.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Zadeklaruj zmienne, których chcesz użyć. Użyj poniższego przykładu, podstawiając własne wartości tam, gdzie to konieczne.

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "8.8.8.8"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2 — Konfigurowanie sieci wirtualnej

1. Utwórz grupę zasobów.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Utwórz konfiguracje podsieci dla sieci wirtualnej, nadając im nazwy *FrontEnd*, *BackEnd* i *GatewaySubnet*. Prefiksy te muszą być częścią zadeklarowanej przestrzeni adresowej sieci wirtualnej.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Utwórz sieć wirtualną. <br>Serwer DNS jest opcjonalny. Określenie tej wartości nie powoduje utworzenia nowego serwera DNS. Pakiet konfiguracji klienta generowany w późniejszym kroku będzie zawierać adres IP serwera DNS określony w tym ustawieniu. Jeśli w przyszłości okaże się konieczne zaktualizowanie listy serwerów DNS, można wygenerować i zainstalować nowe pakiety konfiguracji klienta sieci VPN odzwierciedlające nową listę. Określony serwer DNS powinien być w stanie rozpoznawać nazwy zasobów, z którymi nawiązywane jest połączenie. W tym przykładzie użyliśmy publicznego adresu IP. Pamiętaj, aby użyć własnych wartości.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. Określ zmienne dla utworzonej sieci wirtualnej.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Brama sieci VPN musi mieć publiczny adres IP. Najpierw żąda się zasobu adresu IP, a następnie odwołuje do niego podczas tworzenia bramy sieci wirtualnej. Adres IP jest dynamicznie przypisywany do zasobu podczas tworzenia bramy sieci VPN. Brama sieci VPN aktualnie obsługuje tylko *dynamiczne* przypisywanie publicznych adresów IP. Nie można zażądać przypisania statycznego publicznego adresu IP. Nie oznacza to jednak, że adres IP zmienia się po przypisaniu go do bramy sieci VPN. Jedyną sytuacją, w której ma miejsce zmiana publicznego adresu IP, jest usunięcie bramy i jej ponowne utworzenie. Nie zmienia się on w przypadku zmiany rozmiaru, zresetowania ani przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN.

  Zażądaj dynamicznie przydzielanego publicznego adresu IP.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="Certificates"></a>3 — Generowanie certyfikatów

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Informacje o kluczu publicznym certyfikatu głównego należy przekazać na platformę Azure. Klucz publiczny jest wtedy uważany za „zaufany”. Certyfikaty klienta muszą być generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim w magazynie certyfikatów Certificates-Current User/Personal. Certyfikat jest używany do uwierzytelniania klienta, gdy inicjuje on połączenie z siecią wirtualną. 

Jeśli używasz certyfikatów z podpisem własnym, należy je utworzyć przy użyciu określonych parametrów. Certyfikat z podpisem własnym można utworzyć przy użyciu instrukcji dotyczących [środowiska PowerShell i systemu Windows 10](vpn-gateway-certificates-point-to-site.md) lub, jeśli nie masz systemu Windows 10, możesz użyć polecenia [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Istotne jest, aby podczas generowania certyfikatów głównych z podpisem własnym i certyfikatów klientów wykonać kroki opisane w instrukcjach. W przeciwnym razie wygenerowane przez Ciebie certyfikaty nie będą zgodne z połączeniami typu punkt-lokacja i zostanie wyświetlony błąd połączenia.

### <a name="cer"></a>Krok 1 — Uzyskiwanie pliku cer dla certyfikatu głównego

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>Krok 2 — Generowanie certyfikatu klienta

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>4 — Przygotowywanie pliku cer certyfikatu głównego do przekazania

Przygotuj do przekazania plik cer (który zawiera informacje o kluczu publicznym) dla zaufanego certyfikatu głównego na platformę Azure. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure może używać go do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta wygenerowany na podstawie zaufanego certyfikatu głównego. Jeśli okaże się to konieczne, dodatkowe pliki zaufanego certyfikatu głównego możesz przekazać później — maksymalnie może ich być 20. W tej sekcji deklarowany jest plik cer certyfikatu głównego, który zostanie skojarzony z bramą sieci VPN podczas tworzenia jej w następnej sekcji.

1. Zadeklaruj zmienną dla nazwy certyfikatu, zastępując wartość swoją własną.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Zastąp ścieżkę, podając własną, a następnie uruchom polecenia cmdlet.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```

## <a name="creategateway"></a>5 — Tworzenie bramy sieci VPN

Skonfiguruj i utwórz bramę sieci wirtualnej dla sieci wirtualnej.

* Zmienna *- GatewayType* musi przyjąć wartość **Vpn**, a zmienna *- VpnType* musi przyjąć wartość **RouteBased**.
* W tym przykładzie klucz publiczny certyfikatu głównego jest kojarzony z bramą sieci VPN za pomocą zmiennej „$p2srootcert” określonej w poprzedniej sekcji.
* W tym przykładzie pula adresów klienta sieci VPN jest deklarowana jako [zmienna](#declare) w kroku 1. Pula adresów klienta sieci VPN to zakres, z którego klienci sieci VPN otrzymują adres IP podczas nawiązywania połączenia. Używaj zakresu prywatnych adresów IP nienakładającego się na lokalizację lokalną, z której będziesz się łączyć, ani na sieć wirtualną, z którą chcesz się łączyć.
* Tworzenie bramy sieci VPN może zająć do 45 minut, zależnie od wybranej [jednostki sku bramy](vpn-gateway-about-vpn-gateway-settings.md).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
-VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert
```

## <a name="clientconfig"></a>6 — Pobieranie pakietu konfiguracyjnego klienta VPN

Aby nawiązać połączenie z siecią wirtualną przy użyciu połączenia sieci VPN typu punkt-lokacja, na każdym kliencie trzeba zainstalować pakiet do konfiguracji natywnego klienta sieci VPN systemu Windows. Pakiet konfiguracji konfiguruje natywnego klienta sieci VPN systemu Windows przy użyciu ustawień niezbędnych do nawiązania połączenia z siecią wirtualną i, jeśli został określony serwer DNS dla sieci wirtualnej, zawiera adres IP serwera DNS, którego klient będzie używać do rozpoznawania nazw. Jeśli określony serwer DNS zmienisz później, po wygenerowaniu pakietu konfiguracji klienta, pamiętaj, aby wygenerować nowy pakiet konfiguracji klienta do zainstalowania na Twoich komputerach klienckich.

Tego samego pakietu konfiguracji klienta VPN można użyć na każdym komputerze klienckim, o ile wersja jest zgodna z architekturą dla klienta. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

1. Po utworzeniu bramy można wygenerować i pobrać pakiet konfiguracyjny klienta. W tym przykładzie pobierany jest pakiet dla klientów 64-bitowych. Aby pobrać 32-bitowego klienta, zamień ciąg „Amd64” na „x86”. Klienta sieci VPN można również pobrać za pomocą witryny Azure Portal.

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. Skopiuj i wklej zwrócony link do przeglądarki internetowej, aby pobrać pakiet. Pamiętaj, aby usunąć znaki cudzysłowu otaczające link. 
3. Pobierz i zainstaluj pakiet na komputerze klienckim. Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**. Możesz także zapisać pakiet w celu zainstalowania go na innych komputerach klienckich.
4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie.

## <a name="clientcertificate"></a>7 — Instalowanie wyeksportowanego certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne jest hasło, które zostało utworzone w trakcie eksportowania certyfikatu klienta. Zazwyczaj jest to kwestia dwukrotnego kliknięcia certyfikatu i zainstalowania go. Aby uzyskać więcej informacji, zobacz [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install) (Instalowanie wyeksportowanego certyfikatu klienta).

## <a name="connect"></a>8 — Nawiązywanie połączenia z platformą Azure
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Kliknij przycisk **Kontynuuj**, aby skorzystać z podwyższonego poziomu uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

  ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Połączenie zostało ustanowione.

  ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

[!INCLUDE [verify client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>9 — Weryfikowanie połączenia

1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP należy do określonej podczas konfiguracji połączenia punkt-lokacja puli adresów klienta sieci VPN. Wyniki są podobne, jak w następującym przykładzie:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Nawiązywanie połączenia z maszyną wirtualną

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Dodawanie lub usuwanie certyfikatu głównego

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Po usunięciu certyfikatu głównego klienci, którzy mają certyfikat wygenerowany na podstawie tego certyfikatu głównego, nie będą w stanie się uwierzytelnić i w związku z tym nie będą mogli nawiązywać połączeń. Jeśli chcesz, aby klienci mogli uwierzytelniać się i nawiązywać połączenia, musisz zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu głównego, który jest traktowany jako zaufany przez platformę Azure (przekazany na tę platformę).

### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer certyfikatu głównego. Poniższa procedura jest pomocna w dodawaniu certyfikatu głównego:

1. Utwórz i przygotuj nowy certyfikat główny, który ma zostać dodany do platformy Azure. Wyeksportuj klucz publiczny jako certyfikat x.509 (.CER) szyfrowany algorytmem Base-64 i otwórz go w edytorze tekstu. Skopiuj wartości, tak jak to pokazano w poniższym przykładzie:

  ![certyfikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza.
  >
  >

2. Określi nazwę certyfikatu i informacje o kluczu jako zmienną. Zastąp informacje własnymi, tak jak pokazano w poniższym przykładzie:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Dodaj nowy certyfikat główny. Można dodawać tylko jeden certyfikat naraz.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. Można sprawdzić, czy nowy certyfikat został poprawnie dodany, korzystając z następującego przykładu:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="to-remove-a-root-certificate"></a>Aby usunąć certyfikat główny

1. Zadeklaruj zmienne.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Usuń certyfikat.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. Użyj następującego przykładu, aby zweryfikować, czy certyfikat został pomyślnie usunięty.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>Odwoływanie certyfikatu klienta

Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="to-revoke-a-client-certificate"></a>Aby odwołać certyfikat klienta

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Jak pobrać odcisk palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów. Ten ciąg jest deklarowany jako zmienna w następnym kroku.
3. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować odcisk palca pobrany w poprzednim kroku.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Dodaj odcisk palca do listy odwołanych certyfikatów. Po dodaniu odcisku palca zostanie wyświetlony komunikat „Powodzenie”.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Sprawdź, czy odcisk palca został dodany do listy odwołania certyfikatów.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. Po dodaniu odcisku palca nie będzie można używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

### <a name="to-reinstate-a-client-certificate"></a>Aby przywrócić certyfikat klienta

Certyfikat klienta można przywrócić przez usunięcie odcisku palca z listy odwołanych certyfikatów klienta.

1. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować poprawny odcisk palca dla certyfikatu, który chcesz przywrócić.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Usuń odcisk palca certyfikatu z listy odwołania certyfikatów.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Sprawdź, czy odcisk palca został usunięty z listy odwołania.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).
