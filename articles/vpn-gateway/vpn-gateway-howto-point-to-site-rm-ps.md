---
title: "Łączenie komputera z siecią wirtualną platformy Azure przy użyciu połączenia typu punkt-lokacja: PowerShell | Microsoft Docs"
description: "Możesz bezpiecznie połączyć komputer z siecią wirtualną platformy Azure przez utworzenie połączenia bramy sieci VPN typu punkt-lokacja."
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
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 33646ad42b9209b2970c0745acec899aed3ff88c
ms.lasthandoff: 03/21/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurowanie połączenia typu punkt-lokacja z siecią wirtualną przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie punkt-lokacja (P2S) to połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Połączenia punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie z siecią VPN jest nawiązywane z komputera klienckiego.

Ten artykuł przeprowadzi Cię przez proces tworzenia sieci wirtualnej z połączeniem punkt-lokacja w ramach modelu wdrażania usługi Resource Manager i z użyciem środowiska PowerShell. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne metody wdrażania dla konfiguracji typu punkt-lokacja. Jeśli dostępny jest artykuł zawierający kroki konfiguracji, można do niego przejść bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Podstawowy przepływ pracy
![Łączenie komputera z siecią wirtualną platformy Azure — diagram połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

W tym scenariuszu utworzysz sieć wirtualną za pomocą połączenia punkt-lokacja. Przedstawione instrukcje pomogą również generować certyfikaty, które są wymagane dla tej konfiguracji. Połączenie punkt-lokacja składa się z następujących elementów: sieci wirtualnej z bramą sieci VPN, pliku cer certyfikatu głównego (klucz publiczny), certyfikatu klienta i konfiguracji sieci VPN na komputerze klienckim. 

W celu przeprowadzenia konfiguracji należy zastosować następujące wartości. Ustawianie zmiennych opisano w sekcji [1](#declare) artykułu. Można postępować zgodnie z opisanymi krokami i użyć przedstawionych wartości bez ich zmieniania lub zmienić je, aby odzwierciedlały dane środowisko. 

### <a name="example"></a>Przykładowe wartości
* **Nazwa: VNet1**
* **Przestrzeń adresowa: 192.168.0.0/16** i **10.254.0.0/16**<br>W tym przykładzie używamy więcej niż jednej przestrzeni adresowej, aby zilustrować, że ta konfiguracja będzie współpracować z wieloma przestrzeniami adresowymi. Jednak ta konfiguracja nie wymaga wielu przestrzeni adresowych.
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

## <a name="before-beginning"></a>Przed rozpoczęciem
* Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).
* Zainstaluj najnowszą wersję poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Instalowanie i konfigurowanie programu Azure PowerShell). Podczas pracy z programem PowerShell dla tej konfiguracji upewnij się, że działasz jako administrator. 

## <a name="declare"></a>Część 1 — Logowanie się i ustawianie zmiennych
W tej sekcji należy zalogować się i zadeklarować wartości używane dla tej konfiguracji. Zadeklarowane wartości są używane w przykładowych skryptach. Można zmienić wartości, aby odzwierciedlić własne środowisko. Ale można też użyć zadeklarowanych wartości i postępować zgodnie z opisanymi krokami tylko w celach szkoleniowych.

1. W konsoli programu PowerShell zaloguj się do konta platformy Azure. Polecenie cmdlet wyświetla monit o poświadczenia logowania dla konta platformy Azure. Po zalogowaniu pobiera ono ustawienia konta, aby były dostępne dla programu Azure PowerShell.
   
        Login-AzureRmAccount 
2. Pobierz listę subskrypcji platformy Azure.
   
        Get-AzureRmSubscription
3. Wskaż subskrypcję, której chcesz użyć. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
4. Zadeklaruj zmienne, których chcesz użyć. Użyj poniższego przykładu, podstawiając własne wartości tam, gdzie to konieczne.
   
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

## <a name="ConfigureVNet"></a>Część 2 — Konfigurowanie sieci wirtualnej
1. Utwórz grupę zasobów.
   
        New-AzureRmResourceGroup -Name $RG -Location $Location
2. Utwórz konfiguracje podsieci dla sieci wirtualnej, nadając im nazwy *FrontEnd*, *BackEnd* i *GatewaySubnet*. Prefiksy te muszą być częścią zadeklarowanej przestrzeni adresowej sieci wirtualnej.
   
        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
3. Utwórz sieć wirtualną. Określony serwer DNS powinien być w stanie rozpoznawać nazwy zasobów, z którymi nawiązywane jest połączenie. W tym przykładzie użyliśmy publicznego adresu IP. Pamiętaj, aby użyć własnych wartości.
   
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
4. Określ zmienne dla utworzonej sieci wirtualnej.
   
        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
5. Zażądaj dynamicznie przydzielanego publicznego adresu IP. Ten adres IP jest niezbędny do poprawnego działania bramy. Później połączysz bramę ze skonfigurowanym adresem IP bramy.
   
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip


## <a name="Certificates"></a>Część 3 — Certyfikaty
Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Po utworzeniu certyfikatu głównego dane certyfikatu publicznego (nie klucz prywatny) należy wyeksportować jako plik cer X.509 z kodowaniem Base-64. Następnie dane certyfikatu publicznego należy przekazać z certyfikatu głównego do platformy Azure.

Na każdym komputerze klienckim nawiązującym połączenie z siecią wirtualną za pomocą połączenia typu punkt-lokacja musi być zainstalowany certyfikat klienta w celu uwierzytelniania. Certyfikat klienta jest generowany na podstawie certyfikatu głównego i instalowany na każdym komputerze klienckim. Jeśli prawidłowy certyfikat klienta nie jest zainstalowany, a klient próbuje nawiązać połączenie z siecią wirtualną, uwierzytelnianie nie powiedzie się.

### <a name="cer"></a>Krok 1 — uzyskiwanie pliku cer dla certyfikatu głównego

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
 
Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Uzyskaj plik cer dla certyfikatu głównego, którego chcesz użyć.

####<a name="self-signed-root-certificate"></a>Certyfikat główny z podpisem własnym

Jeśli nie używasz rozwiązania z certyfikatem przedsiębiorstwa, musisz utworzyć certyfikat główny z podpisem własnym. Aby utworzyć certyfikat główny z podpisem własnym, który zawiera pola niezbędne do uwierzytelniania punkt-lokacja, możesz użyć programu PowerShell. Temat [Create a self-signed root certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Tworzenie certyfikatu głównego z podpisem własnym dla połączeń punkt-lokacja za pomocą programu PowerShell) zawiera opis kroków tworzenia certyfikatu głównego z podpisem własnym.

> [!NOTE]
> Wcześniej zalecaną metodą tworzenia certyfikatów głównych z podpisem własnym i generowania certyfikatów klienta dla połączeń punkt-lokacja było użycie narzędzia makecert. Teraz do utworzenia tych certyfikatów można użyć programu PowerShell. Jedną z zalet używania programu PowerShell jest możliwość tworzenia certyfikatów SHA-2. Wymagane wartości są opisane w temacie [Create a self-signed root certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Tworzenie certyfikatu głównego z podpisem własnym dla połączeń punkt-lokacja za pomocą programu PowerShell).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Aby wyeksportować klucz publiczny dla certyfikatu głównego z podpisem własnym

Połączenia punkt-lokacja wymagają przesłania klucza publicznego (plik cer) na platformę Azure. Wykonanie poniższych kroków ułatwia wyeksportowanie pliku cer dla certyfikatu głównego z podpisem własnym.

1. Aby uzyskać plik cer z certyfikatu, otwórz narzędzie **certmgr.msc**. Zlokalizuj certyfikat główny z podpisem własnym — zwykle znajduje się w katalogu „Certyfikaty - bieżący użytkownik\Osobisty\Certyfikat” — a następnie kliknij go prawym przyciskiem myszy. Kliknij pozycję **Wszystkie zadania**, a następnie kliknij pozycję **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W Kreatorze kliknij pozycję **Dalej**. Wybierz pozycję **Nie eksportuj klucza prywatnego**, a następnie kliknij pozycję **Dalej**.
3. Na stronie **Format pliku eksportu** wybierz pozycję **Certyfikat X.509 szyfrowany algorytmem Base-64 (.CER)**, a następnie kliknij pozycję **Dalej**. 
4. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat. Wyświetlony zostanie komunikat **Eksport zakończył się pomyślnie**. Kliknij przycisk **OK**, aby zamknąć kreatora.

### <a name="generate"></a>Krok 2 — generowanie certyfikatu klienta
Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
- Jeśli używasz rozwiązania z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu formatu wartości nazwy pospolitej „name@yourdomain.com”, a nie formatu „nazwa_domeny\nazwa_użytkownika”.
- Upewnij się, że wydawany certyfikat klienta jest oparty na szablonie certyfikatu „Użytkownik”, którego pierwszym elementem na liście użycia jest „Uwierzytelnienie klienta”, a nie Logowanie karty inteligentnej itp. Certyfikat można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i wyświetlenie pozycji **Szczegóły > Ulepszone użycie klucza**.

####<a name="self-signed-root-certificate"></a>Certyfikat główny z podpisem własnym 
Jeśli używasz certyfikatu głównego z podpisem własnym, zobacz temat [Generate a client certificate using PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) (Generowanie certyfikatu klienta za pomocą programu PowerShell), aby uzyskać kroki służące do wygenerowania certyfikatu klienta zgodnego z połączeniami punkt-lokacja.


### <a name="exportclientcert"></a>Krok 3 — eksportowanie certyfikatu klienta

Jeśli certyfikat klienta jest generowany na podstawie certyfikatu głównego z podpisem własnym za pomocą instrukcji dla programu [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert), jest on automatycznie instalowany na komputerze użytym do jego wygenerowania. Aby zainstalować certyfikat klienta na innym komputerze klienckim, należy go wyeksportować.
 
1. Aby wyeksportować certyfikat klienta, otwórz narzędzie **certmgr.msc**. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W kreatorze kliknij pozycję **Dalej**, wybierz opcję **Tak, eksportuj klucz prywatny**, a następnie kliknij pozycję **Dalej**.
3. Na stronie **Format pliku eksportu** pozostaw wybrane wartości domyślne. Upewnij się, że jest zaznaczona pozycja **Jeśli jest to możliwe, dołącz wszystkie certyfikaty do ścieżki certyfikacji**. Następnie kliknij przycisk **Next** (Dalej).
4. Na stronie **Zabezpieczenia** należy włączyć ochronę klucza prywatnego. Jeśli wybierzesz opcję użycia hasła, zapisz lub zapamiętaj hasło ustawione dla tego certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
6. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat.

### <a name="upload"></a>Krok 4 — przekazywanie pliku cer certyfikatu głównego

Po utworzeniu bramy można przekazać plik cer dla zaufanego certyfikatu głównego platformy Azure. Można przekazać pliki dla maksymalnie 20 certyfikatów głównych. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure używa go do uwierzytelniania klientów łączących się z siecią wirtualną.

Zadeklaruj zmienną dla nazwy certyfikatu, zastępując wartość dowolną wartością:

        $P2SRootCertName = "Mycertificatename.cer"

Zastąp ścieżkę, podając własną, a następnie uruchom polecenia cmdlet.

        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Część 4 — Tworzenie bramy sieci VPN
Skonfiguruj i utwórz bramę sieci wirtualnej dla sieci wirtualnej. Zmienna *- GatewayType* musi przyjąć wartość **Vpn**, a zmienna *- VpnType* musi przyjąć wartość **RouteBased**. Może to potrwać do 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Część 5 — Pobieranie pakietu konfiguracyjnego klienta VPN
Klienci łączący się z platformą Azure za pomocą połączenia typu punkt-lokacja muszą mieć zarówno certyfikat klienta, jak zainstalowany pakiet konfiguracyjny klienta VPN. Pakiety konfiguracyjne klienta VPN są dostępne dla klientów systemu Windows.

Pakiet klienta VPN zawiera informacje konfiguracyjne umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Ustawienia są specyficzne dla sieci wirtualnej, z którą chcesz nawiązać połączenie. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

1. Po utworzeniu bramy można pobrać pakiet konfiguracyjny klienta. W tym przykładzie pobierany jest pakiet dla klientów 64-bitowych. Aby pobrać 32-bitowego klienta, zamień ciąg „Amd64” na „x86”. Klienta sieci VPN można również pobrać za pomocą witryny Azure Portal.
   
        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
2. Polecenie cmdlet programu PowerShell zwraca link z adresem URL. Poniżej przedstawiono przykład zwróconego adresu URL:
   
        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
3. Skopiuj i wklej link zwrócony do przeglądarki sieci Web, aby pobrać pakiet. Następnie zainstaluj pakiet na komputerze klienckim. Jeśli pojawi się okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać podobnie jak w tym przykładzie: 
   
    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png)

## <a name="clientcertificate"></a>Część 6. Instalowanie wyeksportowanego certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne będzie hasło, które zostało utworzone podczas eksportowania certyfikatu klienta.

1. Znajdź plik *pfx* i skopiuj go na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx*, aby go zainstalować. Pozostaw wartość pola **Lokalizacja magazynu** jako **Bieżący użytkownik**, a następnie kliknij pozycję **Dalej**.
2. Na stronie importowania **Plik** nie wprowadzaj żadnych zmian. Kliknij przycisk **Dalej**.
3. Na stronie **Ochrona klucza prywatnego** wprowadź hasło dla certyfikatu, jeśli zostało użyte, lub sprawdź, czy podmiot zabezpieczeń instalujący certyfikat jest poprawny, a następnie kliknij pozycję **Dalej**.
4. Na stronie **Magazyn certyfikatów** pozostaw lokalizację domyślną, a następnie kliknij pozycję **Dalej**.
5. Kliknij przycisk **Finish** (Zakończ). Na stronie **Ostrzeżenie o zabezpieczeniach** dla instalacji certyfikatu kliknij pozycję **Tak**. Możesz bez obaw kliknąć pozycję „Tak”, ponieważ wygenerowano certyfikat. Certyfikat został pomyślnie zaimportowany.

## <a name="connect"></a>Część 7: Nawiązywanie połączenia z platformą Azure
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.
   
    ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Połączenie powinno zostać nawiązane.
   
    ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

> [!NOTE]
> Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście. 
>
>

## <a name="verify"></a>Część 8 — Weryfikowanie połączenia
1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP należy do określonej podczas konfiguracji połączenia punkt-lokacja puli adresów klienta sieci VPN. Wyniki powinny być podobne do poniższego kodu:
   
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

## <a name="addremovecert"></a>Dodawanie lub usuwanie zaufanego certyfikatu głównego

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.


### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny
Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Wykonaj poniższe kroki, aby dodać certyfikat główny.

1. Utwórz i przygotuj nowy certyfikat główny, który zostanie dodany do platformy Azure. Wyeksportuj klucz publiczny jako certyfikat x.509 (.CER) szyfrowany algorytmem Base-64 i otwórz go w edytorze tekstu. Następnie skopiuj tylko sekcję wskazaną poniżej. 
   
    Skopiuj wartości, tak jak to pokazano w poniższym przykładzie:
   
    ![certyfikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

    > [!NOTE]
    > Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza.                                                                                                                                                                            
    >


2. Określi nazwę certyfikatu i informacje o kluczu jako zmienną. Zastąp informacje własnymi, tak jak pokazano w poniższym przykładzie:
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
3. Dodaj nowy certyfikat główny. Można dodawać tylko jeden certyfikat naraz.
   
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
4. Można sprawdzić, czy nowy certyfikat został poprawnie dodany, korzystając z następującego przykładu:
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego
Zaufany certyfikat główny można usunąć z platformy Azure. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.

1. Zadeklaruj zmienne.
   
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
2. Usuń certyfikat.
   
        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
3. Użyj następującego przykładu, aby zweryfikować, czy certyfikat został pomyślnie usunięty. 
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Odwoływanie certyfikatu klienta
Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania połączeń punkt-lokacja.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="to-revoke-a-client-certificate"></a>Aby odwołać certyfikat klienta

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Instrukcje: Pobieranie odcisku palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów. Ten ciąg zostanie zadeklarowany jako zmienna.
3. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować odcisk palca pobrany w poprzednim kroku.
   
        $RevokedClientCert1 = "NameofCertificate"
        $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"
4. Dodaj odcisk palca do listy odwołanych certyfikatów. Po dodaniu odcisku palca zobaczysz komunikat „Powodzenie”.
   
        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
        -Thumbprint $RevokedThumbprint1
5. Sprawdź, czy odcisk palca został dodany do listy odwołania certyfikatów.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
6. Po dodaniu odcisku palca nie będzie można używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

### <a name="to-reinstate-a-client-certificate"></a>Aby przywrócić certyfikat klienta
Certyfikat klienta można przywrócić przez usunięcie odcisku palca z listy odwołanych certyfikatów klienta.

1. Zadeklaruj zmienne. Pamiętaj, aby zadeklarować poprawny odcisk palca dla certyfikatu, który chcesz przywrócić.
 
        $RevokedClientCert1 = "NameofCertificate"
        $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
        $GWName = "Name_of_virtual_network_gateway"
        $RG = "Name_of_resource_group"

2. Usuń odcisk palca certyfikatu z listy odwołania certyfikatów.
   
       Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
       -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
3. Sprawdź, czy odcisk palca został usunięty z listy odwołania.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).



