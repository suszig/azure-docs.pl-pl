<properties
   pageTitle="Konfigurowanie połączenia sieci VPN typu punkt-lokacja z siecią wirtualną platformy Azure przy użyciu witryny Azure Portal | Microsoft Azure"
   description="Istnieje możliwość bezpiecznego połączenia się z siecią wirtualną platformy Azure poprzez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu witryny Azure Portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="cherylmc"/>


# Konfigurowanie połączenia punkt-lokacja z siecią wirtualną za pomocą witryny Azure Portal

> [AZURE.SELECTOR]
- [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Model klasyczny — klasyczny portal](vpn-gateway-point-to-site-create.md)

W tym artykule opisano proces tworzenia sieci wirtualnej z połączeniem punkt-lokacja w ramach klasycznego modelu wdrażania przy użyciu witryny Azure Portal. Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów.

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Aby uzyskać więcej informacji na temat połączeń typu punkt-lokacja, zobacz artykuły [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [VPN Gateway — informacje](vpn-gateway-about-vpngateways.md#point-to-site).


### Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne narzędzia wdrażania dla każdego modelu wdrażania. Kiedy ten artykuł będzie dostępny, udostępnimy link do niego.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## Podstawowy przepływ pracy 

![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")


W poniższych sekcjach opisano kroki niezbędne w celu utworzenia bezpiecznego połączenia typu punkt-lokacja z siecią wirtualną. 

1. Tworzenie sieci wirtualnej i bramy sieci VPN
2. Generowanie certyfikatów klienta
3. Przekazywanie pliku cer
4. Generowanie pakietu konfiguracyjnego klienta VPN
5. Konfigurowanie komputera klienckiego
6. Nawiązywanie połączenia z usługą Azure

### Przykładowe ustawienia

Można użyć następujących przykładowych ustawień:

- **Nazwa: VNet1**
- **Przestrzeń adresowa: 192.168.0.0/16**
- **Nazwa podsieci: FrontEnd**
- **Zakres adresów podsieci: 192.168.1.0/24**
- **Subskrypcja:** jeśli masz więcej niż jedną subskrypcję, sprawdź, czy masz właściwą subskrypcję.
- **Grupa zasobów: TestRG**
- **Lokalizacja: Wschodnie stany USA**
- **Typ połączenia: punkt-lokacja**
- **Przestrzeń adresowa klienta: 172.16.201.0/24**. Klienci sieci VPN połączeni z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z określonej puli.
- **GatewaySubnet: 192.168.200.0/24**. Podsieć bramy musi używać nazwy „GatewaySubnet”.
- **Rozmiar:** Wybierz jednostkę SKU bramy, która ma być używana.
- **Typ routingu: Dynamiczny**

## <a name="vnetvpn"></a>Sekcja 1 — tworzenie sieci wirtualnej i bramy sieci VPN

### Część 1. Tworzenie sieci wirtualnej

Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj poniższe kroki. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi.

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.

2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

    ![Wyszukiwanie bloku Sieć wirtualna](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Search for virtual network blade")

3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Klasyczny**, a następnie kliknij przycisk **Utwórz**.

    ![Wybieranie modelu wdrożenia](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Select deployment model")

4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku należy dodać pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Create virtual network blade")

5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.

6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](resource-group-overview.md#resource-groups).

7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Lokalizacja określi miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.

8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.
    
    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Pin to dashboard")


9. Po kliknięciu przycisku Utwórz na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek tworzenia sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Creating virtual network tile")

10. Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Otwórz ustawienia dla sieci wirtualnej, kliknij sekcję Serwery DNS i dodaj adres IP serwera DNS, który ma być używany. To ustawienie nie powoduje utworzenia nowego serwera DNS. Należy pamiętać o dodaniu serwera DNS, z którym zasoby mogą się komunikować.

Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal pojawi się wartość **Utworzona**.

### Część 2: Tworzenie podsieci bramy i bramy o dynamicznym routingu

W tym kroku zostanie utworzona podsieć bramy i brama o dynamicznym routingu. W witrynie Azure Portal w przypadku klasycznego modelu wdrażania tworzenie podsieci bramy i bramy można wykonać za pomocą tych samych bloków konfiguracji.

1. W witrynie portalu przejdź do sieci wirtualnej, dla której chcesz utworzyć bramę.

2. W bloku **Przegląd** dla odpowiedniej sieci wirtualnej, w sekcji Połączenia VPN kliknij pozycję **Brama**.

    ![Kliknij tutaj, aby utworzyć bramę](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Click here to create a gateway")


3. W bloku **Nowe połączenie VPN**wybierz opcję **Punkt-lokacja**.

    ![Typ połączenia P2S](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S connection type")

4. W obszarze **Przestrzeń adresowa klienta** dodaj zakres adresów IP. Jest to zakres, z którego klienci VPN będą otrzymywali adres IP podczas nawiązywania połączenia. Usuń zakres wypełniony automatycznie i dodaj własny.

    ![Przestrzeń adresowa klienta](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Client address space")

5. Zaznacz pole wyboru **Utwórz bramę natychmiast**.

    ![Utwórz bramę natychmiast](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Create gateway immediately")

6. Kliknij pozycję **Opcjonalna konfiguracja bramy**, aby otworzyć blok **Konfiguracja bramy**.

    ![Kliknij pozycję Opcjonalna konfiguracja bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Click optional gateway configuration")

7. Kliknij pozycje **Podsieć > Skonfiguruj wymagane ustawienia**, aby dodać **podsieć bramy**. Chociaż możliwe jest utworzenie małej podsieci bramy /29, zaleca się wybranie podsieci przynajmniej /28 lub /27, aby zawierała więcej adresów. Zapewni to wystarczająco dużo adresów, aby możliwe były dodatkowe konfiguracje, które mogą być potrzebne w przyszłości.

    >[AZURE.IMPORTANT] Podczas pracy z podsieciami bramy należy unikać kojarzenia sieciowej grupy zabezpieczeń (NSG, Network Security Group) z podsiecią bramy. Skojarzenie sieciowej grupy zabezpieczeń z tą podsiecią spowoduje, że brama sieci VPN przestanie działać zgodnie z oczekiwaniami. Aby uzyskać więcej informacji na temat sieciowych grup zabezpieczeń, zobacz [What is a Network Security Group?](../articles/virtual-network/virtual-networks-nsg.md) (Co to jest sieciowa grupa zabezpieczeń?)

    ![Dodawanie podsieci GatewaySubnet](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Add GatewaySubnet")

8. Wybierz **rozmiar** bramy. To jest jednostka SKU bramy, która będzie używana do tworzenia bramy sieci wirtualnej. W portalu domyślną jednostką SKU jest **Podstawowa**. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Informacje o ustawieniach bramy VPN Gateway](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![rozmiar bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Wybierz **Typ routingu** dla bramy. Konfiguracje P2S wymagają typu routingu **Dynamiczny**. Kliknij przycisk **OK** po zakończeniu konfigurowania tego bloku.

    ![Konfigurowanie typu routingu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Configure routing type")

10. W bloku **Nowe połączenie VPN** kliknij przycisk **OK** u dołu bloku, aby rozpocząć tworzenie bramy sieci wirtualnej. Może to potrwać do 45 minut. 


## <a name="generatecerts"></a>Sekcja 2 — Generowanie certyfikatów

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Można użyć pliku cer z certyfikatu głównego z podpisem własnym lub certyfikatu głównego wygenerowanego przez certyfikat przedsiębiorstwa. W tej sekcji zostanie pobrany plik cer dla cer dla certyfikatu głównego i certyfikat klienta wygenerowany z certyfikatu głównego.

### <a name="root"></a>Część 1: Uzyskiwanie pliku cer dla certyfikatu głównego

- Jeśli używasz systemu certyfikatów przedsiębiorstwa, możesz uzyskać plik cer dla certyfikatu głównego, którego chcesz użyć. 

- Jeśli nie używasz certyfikatu przedsiębiorstwa, musisz wygenerować certyfikat główny z podpisem własnym. Kroki dla systemu Windows 10 można znaleźć w temacie [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym w konfiguracjach typu punkt-lokacja). W tym artykule opisano sposób korzystania z narzędzia makecert w celu wygenerowania certyfikatu z podpisem własnym, a następnie wyeksportowania pliku cer.

### Część 2: Generowanie certyfikatu klienta

Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

- Jeśli pracujesz z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu typowego formatu wartości nazwy „imię@twoja_domena.com”, a nie w formacie „nazwa domeny\nazwa_użytkownika”. 

- Jeśli używasz certyfikatu z podpisem własnym i chcesz wygenerować certyfikat klienta, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

### Część 3: Eksportowanie certyfikatu klienta

Certyfikat klienta należy zainstalować na każdym komputerze, który zostanie połączony z siecią wirtualną. Certyfikat klienta jest wymagany do uwierzytelniania. Można zautomatyzować instalowanie certyfikatu klienta, ale możliwe jest też przeprowadzenie instalacji ręcznie. Kroki przedstawione poniżej opisują sposób postępowania w przypadku eksportu i ręcznej instalacji certyfikatu klienta.

1. Aby wyeksportować certyfikat klienta, można użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Klucz znajduje się w pliku *pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.

## <a name="upload"></a>Sekcja 3 — Przekazywanie pliku cer

Po utworzeniu bramy można przekazać plik cer dla zaufanego certyfikatu głównego platformy Azure. Można przekazać pliki dla maksymalnie 20 certyfikatów głównych. Nie przekazuj klucza prywatnego dla certyfikatu do platformy Azure. Po przekazaniu pliku cer platforma Azure używa go do uwierzytelniania klientów łączących się z siecią wirtualną.

1. W sekcji **Połączenia sieci VPN** bloku danej sieci wirtualnej kliknij grafikę **klienci**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.

    ![Klienci](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")

2. W bloku **Połączenie punkt-lokacja** kliknij przycisk **Zarządzanie certyfikatami**, aby otworzyć blok **Certyfikaty**.<br>

    ![Blok Certyfikaty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. W bloku **Certyfikaty** kliknij pozycję **Przekaż**, aby otworzyć blok **Przekaż certyfikat**.<br>

    ![Blok przekazywania certyfikatów](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Kliknij grafikę folderu, aby przejść do pliku cer. Wybierz plik, a następnie kliknij przycisk **OK**. Odśwież stronę, aby wyświetlić przekazany certyfikat w bloku **Certyfikaty**.

    ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Sekcja 4 — Generowanie pakietu konfiguracyjnego klienta VPN

Aby nawiązać połączenie z siecią wirtualną, należy skonfigurować także klienta sieci VPN. W celu nawiązania połączenia komputer kliencki wymaga zarówno certyfikatu klienta, jak i odpowiedniego pakietu konfiguracyjnego klienta sieci VPN.

Pakiet klienta VPN zawiera informacje konfiguracyjne umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Ustawienia są specyficzne dla sieci wirtualnej, z którą chcesz nawiązać połączenie. Lista obsługiwanych systemów operacyjnych klienta znajduje się w temacie Brama sieci VPN — często zadawane pytania w sekcji [Połączenia typu punkt-lokacja](vpn-gateway-vpn-faq.md#point-to-site-connections). 

### Aby wygenerować pakiet konfiguracyjny klienta VPN

1. W witrynie Azure Portal, w bloku **Omówienie** dla danej sieci wirtualnej kliknij grafikę klienta w obszarze **Połączenia VPN**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.
2. W górnej części bloku **Połączenie sieci VPN typu punkt-lokacja** kliknij pakiet pobierania, który odpowiada systemowi operacyjnemu klienta, na którym zostanie zainstalowany:

 - Dla klientów 64-bitowych wybierz pakiet **Klient sieci VPN (64-bitowy)**.
 - Dla klientów 32-bitowych wybierz pakiet **Klient sieci VPN (32-bitowy)**.

    ![Pobieranie pakietu konfiguracyjnego klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Pojawi się komunikat, że Azure generuje pakiet konfiguracyjny klienta VPN dla sieci wirtualnej. Po kilku minutach pakiet zostanie wygenerowany i na komputerze lokalnym wyświetlony komunikat informujący, że pakiet został pobrany. Zapisz plik pakietu konfiguracyjnego. Zainstalujesz go na każdym komputerze klienckim, który będzie łączyć się z siecią wirtualną przy użyciu połączenia typu punkt-lokacja.


## Sekcja 5 — Konfigurowanie komputera klienckiego

### Część 1: Instalacja certyfikatu klienta

Każdy komputer kliencki musi mieć certyfikat klienta w celu uwierzytelniania. Podczas instalowania certyfikatu klienta potrzebne będzie hasło, które zostało utworzone podczas eksportowania certyfikatu klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik pfx, aby go zainstalować. 

### Część 2: Instalowanie pakietu konfiguracyjnego klienta VPN

Tego samego pakietu konfiguracyjnego klienta VPN można użyć na każdym komputerze klienckim, pod warunkiem, że wersja jest zgodna z architekturą dla klienta.

1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną, a następnie kliknij dwukrotnie plik z rozszerzeniem exe. 

2. Gdy pakiet zostanie zainstalowany, możesz uruchomić połączenie sieci VPN. Pakiet konfiguracyjny nie jest podpisany przez firmę Microsoft. Możesz podpisać pakiet, korzystając z urządzenia używanego w tym celu w Twojej organizacji, lub samodzielnie, przy użyciu narzędzia [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Z pakietu można korzystać nawet, jeśli nie jest podpisany. Jeśli jednak pakiet nie jest podpisany, podczas instalowania go zostanie wyświetlone ostrzeżenie.

3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać następująco: 

    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN client")

## Sekcja 6 — Nawiązywanie połączenia z platformą Azure

### Nawiązywanie połączenia z siecią wirtualną

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 

2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

    ![Połączenie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN client connection")

3. Połączenie powinno zostać nawiązane.

    ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Connection established")

### Sprawdzenie połączenia sieci VPN

1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP jest jednym z adresów z zakresu dla połączenia typu punkt-lokacja określonego podczas tworzenia sieci wirtualnej. Wyniki powinny być podobne do poniższego kodu:

Przykład:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## Następne kroki

Do sieci wirtualnej można dodać maszyny wirtualne. Zobacz artykuł [How to create a custom virtual machine](../virtual-machines/virtual-machines-windows-classic-createportal.md) (Tworzenie niestandardowej maszyny wirtualnej).


<!--HONumber=Oct16_HO3-->


