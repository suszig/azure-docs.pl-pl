---
title: "Konfigurowanie połączenia bramy sieci VPN typu punkt-lokacja z siecią wirtualną przy użyciu modelu wdrażania usługi Resource Manager i witryny Azure Portal | Microsoft Docs"
description: "Istnieje możliwość bezpiecznego połączenia się z usługą Azure Virtual Network przez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu usługi Resource Manager i witryny Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aeebed0c733b9fbac964cdc532ff9d364683609b


---
# <a name="configure-a-pointtosite-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną z użyciem witryny Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. 

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Aby uzyskać więcej informacji na temat połączeń typu punkt-lokacja, zobacz artykuły [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planning and Design](vpn-gateway-plan-design.md) (Planowanie i projekt).

W tym artykule opisano kolejne kroki procesu tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja w modelu wdrażania przy użyciu usługi Resource Manager za pomocą witryny Azure Portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne metody wdrażania dla konfiguracji typu punkt-lokacja. Jeśli dostępny jest artykuł zawierający kroki konfiguracji, można do niego przejść bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Podstawowy przepływ pracy
![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Przykładowe wartości
* **Nazwa: VNet1**
* **Przestrzeń adresowa: 192.168.0.0/16**<br>W tym przykładzie zostanie wykorzystana tylko jedna przestrzeń adresowa. Istnieje możliwość użycia więcej niż jednej przestrzeni adresowej dla sieci wirtualnej.
* **Nazwa podsieci: FrontEnd**
* **Zakres adresów podsieci: 192.168.1.0/24**
* **Subskrypcja:** jeśli masz więcej niż jedną subskrypcję, sprawdź, czy korzystasz z właściwej.
* **Grupa zasobów: TestRG**
* **Lokalizacja: Wschodnie stany USA**
* **GatewaySubnet: 192.168.200.0/24**
* **Nazwa bramy sieci wirtualnej: VNet1GW**
* **Typ bramy: VPN**
* **Typ sieci VPN: oparta na trasach**
* **Publiczny adres IP: VNet1GWpip**
* **Typ połączenia: Punkt-lokacja**
* **Pula adresów klienta: 172.16.201.0/24**<br>Klienci sieci VPN połączeni z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z puli adresów klientów.

## <a name="before-beginning"></a>Przed rozpoczęciem
* Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="a-namecreatevnetapart-1-create-a-virtual-network"></a><a name="createvnet"></a>Część 1 — tworzenie sieci wirtualnej
Jeśli tworzysz tę konfigurację w ramach ćwiczenia, skorzystaj z [przykładowych wartości](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

### <a name="2-add-additional-address-space-and-subnets"></a>2. Dodawanie dodatkowej przestrzeni adresowej i podsieci
Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz podsieci.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

### <a name="3-create-a-gateway-subnet"></a>3. Tworzenie podsieci bramy
Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Jeśli to możliwe, najlepiej utworzyć podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

Zrzuty ekranu w tej sekcji służą jedynie jako przykład. Dla opcji GatewaySubnet należy użyć zakresu adresów odpowiadającemu wartościom wymaganym dla danej konfiguracji.

#### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="a-namednsa4-specify-a-dns-server-optional"></a><a name="dns"></a>4. Określanie serwera DNS (opcjonalne)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-2-create-a-virtual-network-gateway"></a><a name="creategw"></a>Część 2 — tworzenie bramy sieci wirtualnej
Połączenia punkt-lokacja wymagają następujących ustawień:

* Typ bramy: VPN
* Typ sieci VPN: oparta na trasach

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę sieci wirtualnej
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-3-generate-certificates"></a><a name="generatecert"></a>Część 3 — generowanie certyfikatów
Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Dane certyfikatu publicznego (nie klucz prywatny) należy wyeksportować jako plik X.509 cer z kodowaniem Base-64 z certyfikatu głównego z podpisem własnym lub certyfikatu głównego wygenerowanego przez certyfikat przedsiębiorstwa. Następnie dane certyfikatu publicznego należy zaimportować z certyfikatu głównego do platformy Azure. Ponadto należy wygenerować certyfikat klienta z certyfikatu głównego dla klientów. Każdy klient, który chce się łączyć z siecią wirtualną za pomocą połączenia typu punkt-lokacja, musi posiadać zainstalowany certyfikat klienta, który został wygenerowany z certyfikatu głównego.

### <a name="a-namegetcera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. Uzyskiwanie pliku cer dla certyfikatu głównego
Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Jeśli nie korzystasz z rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji, możesz utworzyć certyfikat główny z podpisem własnym. Jedną z metod tworzenia certyfikatu z podpisem własnym jest narzędzie makecert.

* Jeśli używasz systemu certyfikatów przedsiębiorstwa, możesz uzyskać plik cer dla certyfikatu głównego, którego chcesz użyć. 
* Jeśli nie używasz certyfikatu przedsiębiorstwa, musisz wygenerować certyfikat główny z podpisem własnym. Kroki dla systemu Windows 10 można znaleźć w temacie [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym w konfiguracjach typu punkt-lokacja).

1. Aby uzyskać plik cer z certyfikatu, otwórz plik **certmgr.msc** i zlokalizuj certyfikat główny. Kliknij prawym przyciskiem myszy certyfikat główny z podpisem własnym, kliknij pozycję **wszystkie zadania**, a następnie kliknij pozycję **eksport**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W kreatorze kliknij pozycję **Dalej**, wybierz opcję **Nie eksportuj klucza prywatnego**, a następnie kliknij przycisk **Dalej**.
3. Na stronie **Format pliku eksportu** wybierz pozycję **Certyfikat X.509 szyfrowany algorytmem Base-64 (.CER)**. Następnie kliknij przycisk **Dalej**. 
4. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat.

### <a name="a-namegenerateclientcerta2-generate-a-client-certificate"></a><a name="generateclientcert"></a>2. Generowanie certyfikatu klienta
Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

* Jeśli pracujesz z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu typowego formatu wartości nazwy 'name@yourdomain.com',, a nie w formacie „nazwa_domeny\nazwa_użytkownika”. 
* Jeśli używasz certyfikatu z podpisem własnym i chcesz wygenerować certyfikat klienta, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3. Eksportowanie certyfikatu klienta
Certyfikat klienta jest wymagany do uwierzytelniania. Po wygenerowaniu certyfikatu klienta należy go wyeksportować. Wyeksportowany certyfikat klienta zostanie zainstalowany później na poszczególnych komputerach klienckich.

1. Aby wyeksportować certyfikat klienta, można użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Klucz znajduje się w pliku *pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.

## <a name="a-nameaddresspoolapart-4-add-the-client-address-pool"></a><a name="addresspool"></a>Część 4 — dodawanie puli adresów klienta
1. Po utworzeniu bramy sieci wirtualnej przejdź do sekcji **Ustawienia** w bloku bramy sieci wirtualnej. W sekcji **Ustawienia** kliknij pozycję **Konfiguracja punktu do lokacji**, aby otworzyć blok **Konfiguracja**.
   
    ![blok punkt do lokacji](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "point to site blade")
2. **Pula adresów** to pula adresów IP, z których łączące się klienty będą otrzymywać adresy IP. Dodaj pulę adresów, a następnie kliknij przycisk **Zapisz**.
   
    ![pula adresów klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "client address pool")

## <a name="a-nameuploadfileapart-5-upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>Część 5 — przekazywanie pliku cer certyfikatu głównego
Po utworzeniu bramy można przekazać plik cer dla zaufanego certyfikatu głównego platformy Azure. Można przekazać pliki dla maksymalnie 20 certyfikatów głównych. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure używa go do uwierzytelniania klientów łączących się z siecią wirtualną.

1. Przejdź do bloku **Konfiguracja punktu do lokacji**. W sekcji **Certyfikat główny** tego bloku dodaj pliki cer.
   
    ![blok punkt do lokacji](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "point to site blade")
2. Upewnij się, że certyfikat główny został wyeksportowany jako plik X.509 (cer) zaszyfrowany z użyciem algorytmu Base-64. Certyfikat główny należy wyeksportować w takim formacie, aby możliwe było jego otwarcie za pomocą edytora tekstów.
3. Otwórz certyfikat za pomocą edytora tekstów, takiego jak Notatnik. Skopiuj wyłącznie następujący fragment:
   
    ![dane dotyczące certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "certificate data")
4. Wklej dane dotyczące certyfikatu w sekcji **Dane certyfikatu publicznego**. Wpisz nazwę certyfikatu w polu **Nazwa**, a następnie kliknij przycisk **Zapisz**. Możesz dodać maksymalnie 20 zaufanych certyfikatów głównych.
   
    ![przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "certificate upload")

## <a name="a-nameclientconfigapart-6-download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Część 6 — pobieranie i instalacja pakietu konfiguracyjnego klienta VPN
Klienty łączące się z platformą Azure za pośrednictwem połączenia typu punkt-lokacja muszą zarówno posiadać certyfikat klienta, jak i mieć zainstalowany pakiet konfiguracyjny klienta VPN. Pakiety konfiguracyjne klienta VPN są dostępne dla klientów systemu Windows. 

Pakiet klienta VPN zawiera informacje umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Konfiguracja jest ściśle powiązana z siecią VPN, z którą ma zostać nawiązane połączenie. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Aby uzyskać więcej informacji, zobacz temat [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. W bloku **Konfiguracja punktu do lokacji** kliknij przycisk **Download VPN client** (Pobierz klienta VPN), aby otworzyć blok **Download VPN client** (Pobieranie klienta VPN).
   
    ![Pobieranie klienta VPN](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN client download")
2. Wybierz właściwy pakiet dla klienta, a następnie kliknij przycisk **Pobierz**. W przypadku klientów 64-bitowych należy wybrać pakiet **AMD64**. W przypadku klientów 32-bitowych należy wybrać pakiet **x86**.
3. Zainstaluj pakiet na komputerze klienckim. Jeśli pojawi się okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać podobnie jak w tym przykładzie: 
   
    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN client")

## <a name="a-nameinstallclientcertapart-7-install-the-client-certificate"></a><a name="installclientcert"></a>Część 7 — instalacja certyfikatu klienta
Każdy komputer kliencki musi mieć certyfikat klienta w celu uwierzytelniania. Podczas instalowania certyfikatu klienta potrzebne będzie hasło, które zostało utworzone podczas eksportowania certyfikatu klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik pfx, aby go zainstalować. Nie zmieniaj lokalizacji instalacji.

## <a name="a-nameconnectapart-8-connect-to-azure"></a><a name="connect"></a>Część 8 — nawiązywanie połączenia z platformą Azure
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.
   
    ![Klient sieci VPN 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")
3. Połączenie powinno zostać nawiązane.
   
    ![Klient sieci VPN 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## <a name="a-nameverifyapart-9-verify-your-connection"></a><a name="verify"></a>Część 9 — weryfikowanie połączenia
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

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych
Zaufany certyfikat główny można usunąć z platformy Azure. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.

Listą odwołanych certyfikatów klienta można zarządzać w bloku **Konfiguracja punktu do lokacji**. Korzystając z tego bloku, [przekazano zaufany certyfikat główny](#uploadfile).

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>Zarządzanie listą odwołanych certyfikatów klienta
Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Jeśli usuniesz plik cer certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Jeśli chcesz odwołać konkretny certyfikat klienta, a nie certyfikat główny, możesz to zrobić. Dzięki temu inne certyfikaty, które zostały wygenerowane z certyfikatu głównego, będą nadal ważne. 

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

Listą odwołanych certyfikatów klienta można zarządzać w bloku **Konfiguracja punktu do lokacji**. Korzystając z tego bloku, [przekazano zaufany certyfikat główny](#uploadfile).

## <a name="next-steps"></a>Następne kroki
Do sieci wirtualnej można dodać maszynę wirtualną. Kroki opisano w sekcji [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).




<!--HONumber=Nov16_HO2-->


