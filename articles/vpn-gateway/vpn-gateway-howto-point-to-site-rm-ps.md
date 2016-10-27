<properties 
   pageTitle="Konfigurowanie połączenia bramy sieci VPN typu punkt-lokacja z siecią wirtualną przy użyciu modelu wdrażania usługi Resource Manager | Microsoft Azure"
   description="Istnieje możliwość bezpiecznego połączenia się z siecią wirtualną platformy Azure poprzez utworzenie połączenia bramy sieci VPN typu punkt-lokacja."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="cherylmc" />


# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurowanie połączenia typu punkt-lokacja z siecią wirtualną przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Model klasyczny — klasyczny portal](vpn-gateway-point-to-site-create.md)

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. 

Ten artykuł przeprowadzi Cię przez proces tworzenia sieci wirtualnej z połączeniem punkt-lokacja w ramach **modelu wdrażania usługi Resource Manager**. Kroki wymagają programu PowerShell. Obecnie nie można utworzyć tego rozwiązania end-to-end w witrynie Azure Portal.

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Aby uzyskać więcej informacji na temat połączeń typu punkt-lokacja, zobacz artykuły [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planning and Design](vpn-gateway-plan-design.md) (Planowanie i projekt).



**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modele wdrażania i narzędzia używane w kontekście połączeń typu punkt-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")


## <a name="about-this-configuration"></a>Informacje o tej konfiguracji

W tym scenariuszu utworzysz sieć wirtualną za pomocą połączenia punkt-lokacja. Przedstawione instrukcje pomogą również generować certyfikaty, które są wymagane dla tej konfiguracji. Połączenie punkt-lokacja składa się z następujących elementów: sieci wirtualnej z bramą sieci VPN, pliku cer certyfikatu głównego (klucz publiczny), certyfikatu klienta i konfiguracji sieci VPN na komputerze klienckim. 

W celu przeprowadzenia konfiguracji należy zastosować następujące wartości. Ustawianie zmiennych opisano w sekcji [1](#declare) artykułu. Można postępować zgodnie z opisanymi krokami i użyć przedstawionych wartości bez ich zmieniania lub zmienić je, aby odzwierciedlały dane środowisko. 

- Nazwa: **VNet1**, przy użyciu przestrzeni adresowych **192.168.0.0/16** i **10.254.0.0/16**. Należy zauważyć, że można użyć więcej niż jednej przestrzeni adresowej dla sieci wirtualnej.
- Nazwa podsieci: **FrontEnd**, przy użyciu **192.168.1.0/24**
- Nazwa podsieci: **BackEnd**, przy użyciu **10.254.1.0/24**
- Nazwa podsieci: **GatewaySubnet**, przy użyciu **192.168.200.0/24**. Nazwa podsieci *GatewaySubnet* jest obowiązkowa, aby brama mogła działać. 
- Pula adresów klienta sieci VPN: **172.16.201.0/24**. Klienci sieci VPN połączeni z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z tej puli.
- Subskrypcja: jeśli masz więcej niż jedną subskrypcję, sprawdź, czy masz tę właściwą.
- Grupa zasobów: **TestRG**
- Lokalizacja: **East US**
- Serwer DNS: **adres IP** serwera DNS, który ma być używany do rozpoznawania nazw.
- Nazwa GW: **GW**
- Publiczny adres IP: **GWIP**
- VpnType: **RouteBased**


## <a name="before-beginning"></a>Przed rozpoczęciem

- Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Zainstaluj polecenia cmdlet programu PowerShell usługi Azure Resource Manager (wersja 1.0.2 lub nowsza). Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell). Podczas pracy z programem PowerShell dla tej konfiguracji upewnij się, że działasz jako administrator. 

## <a name="<a-name="declare"></a>part-1---log-in-and-set-variables"></a><a name="declare"></a>Część 1 — Logowanie się i ustawianie zmiennych

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
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $P2SRootCertName = "ARMP2SRootCert.cer"


## <a name="part-2---configure-a-vnet"></a>Część 2 — Konfigurowanie sieci wirtualnej 


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

## <a name="part-3---add-trusted-certificates"></a>Część 3 — Dodawanie zaufanych certyfikatów

Platforma Azure uwierzytelnia klientów, którzy chcą nawiązać połączenie za pośrednictwem połączenia typu punkt-lokacja przy użyciu certyfikatów. Można zaimportować plik cer (klucz publiczny) dla certyfikatu głównego na platformę Azure. Po dodaniu do platformy Azure pliku x.509 szyfrowanego algorytmem Base64 (.cer) otrzymuje ona informację, że może zaufać certyfikatowi głównemu reprezentowanemu przez ten plik.

Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Jeśli nie korzystasz z rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji, możesz utworzyć certyfikat główny z podpisem własnym. Jedną z metod tworzenia certyfikatu z podpisem własnym jest narzędzie makecert. Aby uzyskać instrukcje dotyczące tworzenia certyfikatu głównego z podpisem własnym za pomocą narzędzia *makecert*, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

Bez względu na to, jak uzyskasz certyfikat, należy przekazać plik cer certyfikatu do platformy Azure, a także wygenerować certyfikaty klienta do zainstalowania na komputerach klienckich, które chcą nawiązać połączenie. Nie instaluj certyfikatu z podpisem własnym bezpośrednio na komputerze klienckim. Certyfikaty klienta można wygenerować później, co zostało opisane w sekcji [Konfiguracja klienta](#cc) w dalszej części tego artykułu.
        
Możesz dodać maksymalnie 20 zaufanych certyfikatów do platformy Azure. Aby uzyskać klucz publiczny, należy wyeksportować certyfikat jako plik x.509 szyfrowany algorytmem Base64 (.cer). Jednym ze sposobów eksportowania do pliku cer jest otwarcie narzędzia **certmgr.msc** i umieszczenie certyfikatu w obszarze Osobiste/Certyfikaty. Kliknij prawym przyciskiem myszy, a następnie wyeksportuj certyfikat bez klucza prywatnego jako „certyfikat x.509 szyfrowany algorytmem Base-64 (.CER)”. Zanotuj ścieżkę, do której został wyeksportowany certyfikat w postaci pliku cer. To jest przykład uzyskania reprezentacji ciągu Base64 dla certyfikatu. 

Dodaj zaufany certyfikat do platformy Azure. W tym kroku należy użyć własnej ścieżki pliku cer. Zwróć szczególną uwagę na zmienną $P2SRootCertName = „ARMP2SRootCert.cer”, która została ustawiona w części 1. tego artykułu. Jeśli nazwa certyfikatu jest inna, należy odpowiednio dostosować zmienną.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="part-4---create-the-vpn-gateway"></a>Część 4 — Tworzenie bramy sieci VPN

Skonfiguruj i utwórz bramę sieci wirtualnej dla sieci wirtualnej. Zmienna *- GatewayType* musi przyjąć wartość **Vpn**, a zmienna *- VpnType* musi przyjąć wartość **RouteBased**. Może to potrwać do 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="part-5---download-the-vpn-client-configuration-package"></a>Część 5 — Pobieranie pakietu konfiguracyjnego klienta VPN

Klienci łączący się z platformą Azure za pomocą połączenia typu punkt-lokacja muszą mieć zarówno certyfikat klienta, jak zainstalowany pakiet konfiguracyjny klienta VPN. Pakiety konfiguracyjne klienta VPN są dostępne dla klientów systemu Windows. Pakiet klienta VPN zawiera informacje o tym, jak skonfigurować oprogramowanie klienckie VPN wbudowane w system Windows i specyficzne dla sieci VPN, z którą chcesz nawiązać połączenie. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Aby uzyskać więcej informacji, zobacz temat [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. Po utworzeniu bramy skorzystaj z następującego przykładu, aby pobrać pakiet konfiguracyjny klienta:

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Polecenie cmdlet programu PowerShell zwraca link z adresem URL. Poniżej przedstawiono przykład zwróconego adresu URL:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Skopiuj i wklej link zwrócony do przeglądarki sieci Web, aby pobrać pakiet. Następnie zainstaluj pakiet na komputerze klienckim.

4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać podobnie jak w tym przykładzie: 

    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN client")

## <a name="<a-name="cc"></a>part-6---generate-the-client-certificate"></a><a name="cc"></a>Część 6 — Generowanie certyfikatu klienta

Kolejny krok to generowanie certyfikatów klienta. Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

- Jeśli pracujesz z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu typowego formatu wartości nazwy 'name@yourdomain.com',, a nie w formacie NetBIOS „DOMENA\nazwa_użytkownika”. 

- Jeśli używasz certyfikatu z podpisem własnym i chcesz wygenerować certyfikat klienta, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

## <a name="part-7---install-the-client-certificate"></a>Część 7 — Instalacja certyfikatu klienta

Certyfikat klienta należy zainstalować na każdym komputerze, który zostanie połączony z siecią wirtualną. Certyfikat klienta jest wymagany do uwierzytelniania. Można zautomatyzować instalowanie certyfikatu klienta, ale możliwe jest też przeprowadzenie instalacji ręcznie. Kroki przedstawione poniżej opisują sposób postępowania w przypadku eksportu i ręcznej instalacji certyfikatu klienta.

1. Aby wyeksportować certyfikat klienta, można użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Klucz znajduje się w pliku *pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.
3. Skopiuj plik *.pfx* na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx*, aby go zainstalować. Gdy zostanie wyświetlony stosowny monit, wprowadź hasło. Nie zmieniaj lokalizacji instalacji.


## <a name="part-8---connect-to-azure"></a>Część 8 — Nawiązywanie połączenia z platformą Azure

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 

2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

    ![Klient sieci VPN 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")

3. Połączenie powinno zostać nawiązane.

    ![Klient sieci VPN 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## <a name="part-9---verify-your-connection"></a>Część 9 — Weryfikowanie połączenia

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

## <a name="to-add-or-remove-a-trusted-root-certificate"></a>Aby dodać lub usunąć zaufany certyfikat główny

Certyfikaty są używane do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. W poniższych krokach objaśniono proces dodawania i usuwania certyfikatów głównych. Po dodaniu do platformy Azure pliku x.509 szyfrowanego algorytmem Base64 (.cer) otrzymuje ona informację, że może zaufać certyfikatowi głównemu reprezentowanemu przez ten plik. 

Dodawanie i usuwanie zaufanych certyfikatów głównych może być przeprowadzone przy użyciu programu PowerShell lub w witrynie Azure Portal. Jeśli chcesz to zrobić za pomocą witryny Azure Portal, przejdź kolejno do pozycji **Brama sieci wirtualnej > Ustawienia > Konfiguracja połączenia punkt-lokacja > Certyfikaty główne**. W poniższych krokach objaśniono, jak wykonać te zadania przy użyciu programu PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Dodawanie zaufanego certyfikatu głównego

Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Wykonaj poniższe kroki, aby dodać certyfikat główny.

1. Utwórz i przygotuj nowy certyfikat główny, który zostanie dodany do platformy Azure. Wyeksportuj klucz publiczny jako certyfikat x.509 (.CER) szyfrowany algorytmem Base-64 i otwórz go w edytorze tekstu. Następnie skopiuj tylko sekcję wskazaną poniżej. 
 
    Skopiuj wartości, tak jak to pokazano w poniższym przykładzie:

    ![certyfikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificate")
    
2. Określi nazwę certyfikatu i informacje o kluczu jako zmienną. Zastąp informacje własnymi, tak jak pokazano w poniższym przykładzie:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Dodaj nowy certyfikat główny. Można dodawać tylko jeden certyfikat naraz.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Można sprawdzić, czy nowy certyfikat został poprawnie dodany, korzystając z następującego polecenia cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego

Zaufany certyfikat główny można usunąć z platformy Azure. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.

1. Aby usunąć zaufany certyfikat główny, zmodyfikuj poniższy przykład:

    Zadeklaruj zmienne.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Usuń certyfikat

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Użyj następującego polecenia cmdlet, aby zweryfikować, czy certyfikat został pomyślnie usunięty. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

## <a name="to-manage-the-list-of-revoked-client-certificates"></a>Aby zarządzać listą odwołanych certyfikatów klienta

Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Jeśli usuniesz plik cer certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Jeśli chcesz odwołać konkretny certyfikat klienta, a nie certyfikat główny, możesz to zrobić. Dzięki temu inne certyfikaty, które zostały wygenerowane z certyfikatu głównego, będą nadal ważne. Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="revoke-a-client-certificate"></a>Odwołanie certyfikatu klienta

1. Uzyskaj odcisk palca certyfikatu klienta, którego chcesz odwołać.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Dodaj odcisk palca certyfikatu do listy odwołanych odcisków palców.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Sprawdź, czy odcisk palca został dodany do listy odwołania certyfikatów. Można dodać jeden odcisk palca naraz.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Przywracanie certyfikatu klienta

Certyfikat klienta można przywrócić przez usunięcie odcisku palca z listy odwołanych certyfikatów klienta.

1.  Usuń odcisk palca z listy odcisków palców odwołanego certyfikatu klienta.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Sprawdź, czy odcisk palca został usunięty z listy odwołania.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Następne kroki

Do sieci wirtualnej można dodać maszynę wirtualną. Kroki opisano w sekcji [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).





<!--HONumber=Oct16_HO3-->


