---
title: "Łączenie komputera z siecią wirtualną platformy Azure przy użyciu połączenia typu punkt-lokacja: portal | Microsoft Docs"
description: "Istnieje możliwość bezpiecznego połączenia się z usługą Azure Virtual Network przez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu usługi Resource Manager i witryny Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: bc3d0252b3aef6a62c863b79839557e165d9f121
ms.lasthandoff: 03/03/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną za pomocą witryny Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. 

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

W tym artykule opisano proces tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja przy użyciu witryny Azure Portal. Te kroki mają zastosowanie w modelu wdrażania przy użyciu usługi Resource Manager.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne metody wdrażania dla konfiguracji typu punkt-lokacja. Jeśli dostępny jest artykuł zawierający kroki konfiguracji, można do niego przejść bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Podstawowy przepływ pracy
![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="example"></a>Przykładowe wartości
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
* Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).

## <a name="createvnet"></a>Część 1 — tworzenie sieci wirtualnej
Jeśli tworzysz tę konfigurację w ramach ćwiczenia, skorzystaj z [przykładowych wartości](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>Część 2 — określanie przestrzeni adresowej i podsieci
Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz podsieci.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a> Część 3 — dodawanie podsieci bramy

Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Usługi bramy korzystają z adresów IP określonych w podsieci bramy. Jeśli to możliwe, utwórz podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

Zrzuty ekranu w tej sekcji służą jedynie jako przykład. Dla opcji GatewaySubnet należy użyć zakresu adresów odpowiadającemu wartościom wymaganym dla danej konfiguracji.

###<a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>Część 4 — określanie serwera DNS (opcjonalne)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Część 5 — tworzenie bramy sieci wirtualnej
Połączenia punkt-lokacja wymagają następujących ustawień:

* Typ bramy: VPN
* Typ sieci VPN: oparta na trasach

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę sieci wirtualnej
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Część 6 — generowanie certyfikatów
Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Dane certyfikatu publicznego (nie klucz prywatny) należy wyeksportować jako plik X.509 cer z kodowaniem Base-64 z certyfikatu głównego z podpisem własnym lub certyfikatu głównego wygenerowanego przez certyfikat przedsiębiorstwa. Następnie dane certyfikatu publicznego należy zaimportować z certyfikatu głównego do platformy Azure. Ponadto należy wygenerować certyfikat klienta z certyfikatu głównego dla klientów. Każdy klient, który chce się łączyć z siecią wirtualną za pomocą połączenia typu punkt-lokacja, musi posiadać zainstalowany certyfikat klienta, który został wygenerowany z certyfikatu głównego.

### <a name="getcer"></a>Krok 1 — uzyskiwanie pliku cer dla certyfikatu głównego

Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Uzyskaj plik cer dla certyfikatu głównego, którego chcesz użyć.

Jeśli nie używasz rozwiązania z certyfikatem przedsiębiorstwa, musisz utworzyć certyfikat główny z podpisem własnym. Aby utworzyć certyfikat z podpisem własnym, który zawiera pola niezbędne do uwierzytelniania punkt-lokacja, użyj narzędzia makecert. Temat [Tworzenie certyfikatu głównego z podpisem własnym dla połączeń punkt-lokacja](vpn-gateway-certificates-point-to-site.md) zawiera opis kroków tworzenia certyfikatu głównego z podpisem własnym. Zdajemy sobie sprawę, że narzędzie makecert jest przestarzałe, ale tym razem jest to wspierane rozwiązanie.

>[!NOTE]
>Chociaż tworzenie certyfikatów z podpisem własnym przy użyciu programu PowerShell jest możliwe, certyfikaty wygenerowane w ten sposób nie zawierają pól niezbędnych do uwierzytelniania punkt-lokacja.
>


#### <a name="to-obtain-the-cer-file-from-a-self-signed-root-certificate"></a>Aby uzyskać plik cer z certyfikatu głównego z podpisem własnym

1. Aby uzyskać plik cer z certyfikatu głównego z podpisem własnym, otwórz plik **certmgr.msc** i zlokalizuj utworzony przez Ciebie certyfikat główny. Certyfikat znajduje się zwykle w lokalizacji „Certificates-Current User/ Personal/Certificates” i ma nazwę nadaną mu podczas tworzenia. Kliknij prawym przyciskiem myszy certyfikat główny z podpisem własnym, kliknij pozycję **wszystkie zadania**, a następnie kliknij pozycję **eksport**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W kreatorze kliknij pozycję **Dalej**, wybierz opcję **Nie eksportuj klucza prywatnego**, a następnie kliknij przycisk **Dalej**.
3. Na stronie **Format pliku eksportu** wybierz pozycję **Certyfikat X.509 szyfrowany algorytmem Base-64 (.CER)**. Następnie kliknij przycisk **Dalej**.
4. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat.

### <a name="generateclientcert"></a>Krok 2 — generowanie certyfikatu klienta
Można wygenerować unikatowy certyfikat dla każdego klienta, który będzie się łączył z siecią wirtualną, albo użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, będzie konieczne wygenerowanie i zainstalowanie nowych certyfikatów dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
- Jeśli używasz rozwiązania z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu formatu wartości nazwy pospolitej „name@yourdomain.com”, a nie formatu „nazwa_domeny\nazwa_użytkownika”.
- Upewnij się, że wydawany certyfikat klienta jest oparty na szablonie certyfikatu „Użytkownik”, którego pierwszym elementem na liście użycia jest „Uwierzytelnienie klienta”, a nie Logowanie karty inteligentnej itp. Certyfikat można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i wyświetlenie pozycji **Szczegóły > Ulepszone użycie klucza**.

####<a name="self-signed-certificate"></a>Certyfikat z podpisem własnym 
Jeśli używasz certyfikatu z podpisem własnym i chcesz wygenerować certyfikat klienta, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

### <a name="exportclientcert"></a>Krok 3 — eksportowanie certyfikatu klienta
Certyfikat klienta jest wymagany do uwierzytelniania. Po wygenerowaniu certyfikatu klienta należy go wyeksportować. Wyeksportowany certyfikat klienta zostanie zainstalowany później na poszczególnych komputerach klienckich.

1. Aby wyeksportować certyfikat klienta, można użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Klucz znajduje się w pliku *pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.

## <a name="addresspool"></a>Część 7 — dodawanie puli adresów klienta
1. Po utworzeniu bramy sieci wirtualnej przejdź do sekcji **Ustawienia** w bloku bramy sieci wirtualnej. W sekcji **Ustawienia** kliknij pozycję **Konfiguracja punktu do lokacji**, aby otworzyć blok **Konfiguracja**.
   
    ![Blok punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Pula adresów** to pula adresów IP, z których łączące się klienty będą otrzymywać adresy IP. Dodaj pulę adresów, a następnie kliknij przycisk **Zapisz**.
   
    ![Pula adresów klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>Część 8 — przekazywanie pliku cer certyfikatu głównego
Po utworzeniu bramy można przekazać plik cer dla zaufanego certyfikatu głównego platformy Azure. Można przekazać pliki dla maksymalnie 20 certyfikatów głównych. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure używa go do uwierzytelniania klientów łączących się z siecią wirtualną.

1. Certyfikaty są dodawane w bloku **Konfiguracja punktu do lokacji** w sekcji **Certyfikat główny**.  
2. Upewnij się, że certyfikat główny został wyeksportowany jako plik X.509 (cer) zaszyfrowany z użyciem algorytmu Base-64. Certyfikat główny należy wyeksportować w takim formacie, aby możliwe było jego otwarcie za pomocą edytora tekstów.
3. Otwórz certyfikat za pomocą edytora tekstów, takiego jak Notatnik. Skopiuj tylko następującą sekcję jako jeden ciągły wiersz:
   
    ![Dane dotyczące certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza.                                                                                                                                                                            
    >
    >

4. Wklej dane dotyczące certyfikatu w polu **Dane certyfikatu publicznego**. Korzystając z pola **Nazwa**, określ nazwę certyfikatu, a następnie kliknij przycisk **Zapisz**. Możesz dodać maksymalnie 20 zaufanych certyfikatów głównych.
   
    ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>Część 9 — pobieranie i instalowanie pakietu konfiguracyjnego klienta VPN
Klienty łączące się z platformą Azure za pośrednictwem połączenia typu punkt-lokacja muszą zarówno posiadać certyfikat klienta, jak i mieć zainstalowany pakiet konfiguracyjny klienta VPN. Pakiety konfiguracyjne klienta VPN są dostępne dla klientów systemu Windows. 

Pakiet klienta VPN zawiera informacje umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Konfiguracja jest ściśle powiązana z siecią VPN, z którą ma zostać nawiązane połączenie. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania.

Tego samego pakietu konfiguracyjnego klienta VPN można użyć na każdym komputerze klienckim, pod warunkiem, że wersja jest zgodna z architekturą dla klienta.

### <a name="step-1---download-the-client-configuration-package"></a>Krok 1 — Pobieranie pakietu konfiguracji klienta

1. W bloku **Konfiguracja punktu do lokacji** kliknij przycisk **Download VPN client** (Pobierz klienta VPN), aby otworzyć blok **Download VPN client** (Pobieranie klienta VPN). Wygenerowanie pakietu trwa minutę lub dwie.
   
    ![Pobieranie klienta VPN — sposób 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Wybierz właściwy pakiet dla klienta, a następnie kliknij przycisk **Pobierz**. Zapisz plik pakietu konfiguracyjnego. Zainstalujesz go na każdym komputerze klienckim, który będzie łączyć się z siecią wirtualną.

    ![Pobieranie klienta VPN — sposób 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/client.png)

   * W przypadku klientów 64-bitowych należy wybrać pakiet **AMD64**.
   * W przypadku klientów 32-bitowych należy wybrać pakiet **x86**.

### <a name="step-2---install-the-client-configuration-package"></a>Krok 2 — Instalowanie pakietu konfiguracji klienta

1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną. 
2. Kliknij dwukrotnie plik exe, aby zainstalować pakiet na komputerze klienckim. Ponieważ pakiet konfiguracji został utworzony przez Ciebie, nie jest podpisany. Oznacza to, że może pojawić się ostrzeżenie. Jeśli pojawi się okno podręczne Windows SmartScreen, kliknij pozycję **Więcej informacji** (z lewej strony), a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać następująco:
3. Zainstaluj pakiet na komputerze klienckim. Jeśli pojawi się okno podręczne Windows SmartScreen, kliknij pozycję **Więcej informacji** (z lewej strony), a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać podobnie jak w tym przykładzie: 
   
    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="installclientcert"></a>Część 10 — instalowanie certyfikatu klienta
Każdy komputer kliencki musi mieć certyfikat klienta na potrzeby uwierzytelniania. Podczas instalowania certyfikatu klienta potrzebne będzie hasło, które zostało utworzone podczas eksportowania certyfikatu klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik pfx, aby go zainstalować. Nie zmieniaj lokalizacji instalacji.

## <a name="connect"></a>Część 11 — nawiązywanie połączenia z platformą Azure
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.
   
    ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Połączenie powinno zostać nawiązane.
   
    ![Klient sieci VPN połączony z platformą Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście. 
>
>

## <a name="verify"></a>Część 12 — weryfikowanie połączenia
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

## <a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych
Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Aby uzyskać instrukcje, zobacz sekcję [Przekazywanie zaufanego certyfikatu głównego](#uploadfile) w tym artykule.

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego

1. Aby usunąć zaufany certyfikat główny, przejdź do bloku **Konfiguracja punktu do lokacji** dla bramy Twojej sieci wirtualnej.
2. W sekcji **Certyfikat główny** bloku znajdź certyfikat, który chcesz usunąć.
3. Kliknij wielokropek obok certyfikatu, a następnie kliknij pozycję „Usuń”.

## <a name="revokeclient"></a>Odwoływanie certyfikatu klienta
Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania połączeń punkt-lokacja.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="to-revoke-a-client-certificate"></a>Aby odwołać certyfikat klienta

Certyfikat klienta można odwołać przez dodanie odcisku palca do listy odwołania.

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Jak pobrać odcisk palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów.
3. Przejdź do bloku **Konfiguracja punktu do lokacji** bramy sieci wirtualnej. Korzystając z tego bloku, [przekazano zaufany certyfikat główny](#uploadfile).
4. W sekcji **Odwołane certyfikaty** wprowadź przyjazną nazwę certyfikatu (nie musi to być nazwa pospolita certyfikatu).
5. Skopiuj i wklej ciąg odcisku palca do pola **Odcisk palca**.
6. Odcisk palca zostanie zweryfikowany i automatycznie dodany do listy odwołania. Na ekranie zobaczysz komunikat informujący o aktualizowaniu listy. 
7. Po zakończeniu aktualizowania nie będzie można już używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).


