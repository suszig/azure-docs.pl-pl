---
title: "Łączenie komputera z siecią wirtualną platformy Azure przy użyciu połączenia typu punkt-lokacja: Azure Portal: wdrożenie klasyczne | Microsoft Docs"
description: "Istnieje możliwość bezpiecznego połączenia się z klasyczną usługą Azure Virtual Network poprzez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu witryny Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: d6e58625e7f90e6290268b55e488540436d3a4da
ms.lasthandoff: 03/11/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną przy użyciu klasycznej witryny Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie punkt-lokacja (P2S) to połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Połączenia punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie z siecią VPN jest nawiązywane z komputera klienckiego.

W tym artykule opisano proces tworzenia sieci wirtualnej z połączeniem punkt-lokacja w ramach klasycznego modelu wdrażania przy użyciu witryny Azure Portal. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne metody wdrażania dla konfiguracji typu punkt-lokacja. Jeśli dostępny jest artykuł zawierający kroki konfiguracji, można do niego przejść bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Podstawowy przepływ pracy
![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

W poniższych sekcjach opisano kroki niezbędne w celu utworzenia bezpiecznego połączenia typu punkt-lokacja z siecią wirtualną.

1. Tworzenie sieci wirtualnej i bramy sieci VPN
2. Generowanie certyfikatów klienta
3. Przekazywanie pliku cer
4. Generowanie pakietu konfiguracyjnego klienta VPN
5. Konfigurowanie komputera klienckiego
6. Nawiązywanie połączenia z usługą Azure

### <a name="example-settings"></a>Przykładowe ustawienia
Można użyć następujących przykładowych ustawień:

* **Nazwa: VNet1**
* **Przestrzeń adresowa: 192.168.0.0/16**<br>W tym przykładzie zostanie wykorzystana tylko jedna przestrzeń adresowa. Istnieje możliwość użycia więcej niż jednej przestrzeni adresowej dla sieci wirtualnej.
* **Nazwa podsieci: FrontEnd**
* **Zakres adresów podsieci: 192.168.1.0/24**
* **Subskrypcja:** jeśli masz więcej niż jedną subskrypcję, sprawdź, czy korzystasz z właściwej.
* **Grupa zasobów: TestRG**
* **Lokalizacja: Wschodnie stany USA**
* **Typ połączenia: Punkt-lokacja**
* **Przestrzeń adresowa klienta: 172.16.201.0/24**. Klienci sieci VPN połączeni z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z określonej puli.
* **GatewaySubnet: 192.168.200.0/24**. Podsieć bramy musi używać nazwy „GatewaySubnet”.
* **Rozmiar:** Wybierz jednostkę SKU bramy, która ma być używana.
* **Typ routingu: Dynamiczny**



## <a name="vnetvpn"></a>Sekcja 1 — tworzenie sieci wirtualnej i bramy sieci VPN

Przed rozpoczęciem sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).
### <a name="createvnet"></a>Część 1: Tworzenie sieci wirtualnej
Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi. Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj poniższe kroki:

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

    ![Wyszukiwanie bloku Sieć wirtualna](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Klasyczny**, a następnie kliknij przycisk **Utwórz**.

    ![Wybieranie modelu wdrożenia](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku należy dodać pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Lokalizacja określi miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.
8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.

    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Po kliknięciu przycisku Utwórz na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek tworzenia sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Otwórz ustawienia dla sieci wirtualnej, kliknij sekcję Serwery DNS i dodaj adres IP serwera DNS, który ma być używany. To ustawienie nie powoduje utworzenia nowego serwera DNS. Należy pamiętać o dodaniu serwera DNS, z którym zasoby mogą się komunikować.

Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal pojawi się wartość **Utworzona**.

### <a name="gateway"></a>Część 2: Tworzenie podsieci bramy i bramy o dynamicznym routingu
W tym kroku zostanie utworzona podsieć bramy i brama o dynamicznym routingu. W witrynie Azure Portal w przypadku klasycznego modelu wdrażania tworzenie podsieci bramy i bramy można wykonać za pomocą tych samych bloków konfiguracji.

1. W witrynie portalu przejdź do sieci wirtualnej, dla której chcesz utworzyć bramę.
2. W bloku **Przegląd** dla odpowiedniej sieci wirtualnej, w sekcji Połączenia VPN kliknij pozycję **Brama**.

    ![Kliknij w celu utworzenia bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. W bloku **Nowe połączenie VPN**wybierz opcję **Punkt-lokacja**.

    ![Połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. W obszarze **Przestrzeń adresowa klienta** dodaj zakres adresów IP. Jest to zakres, z którego klienci VPN będą otrzymywali adres IP podczas nawiązywania połączenia. Usuń zakres wypełniony automatycznie i dodaj własny.

    ![Przestrzeń adresowa klienta](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Zaznacz pole wyboru **Utwórz bramę natychmiast**.

    ![Utwórz bramę natychmiast](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Kliknij pozycję **Opcjonalna konfiguracja bramy**, aby otworzyć blok **Konfiguracja bramy**.

    ![Kliknij pozycję Opcjonalna konfiguracja bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Kliknij pozycje **Podsieć > Skonfiguruj wymagane ustawienia**, aby dodać **podsieć bramy**. Chociaż możliwe jest utworzenie małej podsieci bramy /29, zaleca się wybranie podsieci przynajmniej /28 lub /27, aby zawierała więcej adresów. Zapewni to wystarczająco dużo adresów, aby możliwe były dodatkowe konfiguracje, które mogą być potrzebne w przyszłości.

   > [!IMPORTANT]
   > Podczas pracy z podsieciami bramy należy unikać kojarzenia sieciowej grupy zabezpieczeń (NSG, Network Security Group) z podsiecią bramy. Skojarzenie sieciowej grupy zabezpieczeń z tą podsiecią spowoduje, że brama sieci VPN przestanie działać zgodnie z oczekiwaniami. Aby uzyskać więcej informacji na temat sieciowych grup zabezpieczeń, zobacz [What is a Network Security Group?](../virtual-network/virtual-networks-nsg.md) (Co to jest sieciowa grupa zabezpieczeń?)
   >
   >

    ![Dodawanie podsieci GatewaySubnet](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Wybierz **rozmiar** bramy. To jest jednostka SKU bramy, która będzie używana do tworzenia bramy sieci wirtualnej. W portalu domyślną jednostką SKU jest **Podstawowa**. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Informacje o ustawieniach bramy VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![rozmiar bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Wybierz **Typ routingu** dla bramy. Konfiguracje P2S wymagają typu routingu **Dynamiczny**. Kliknij przycisk **OK** po zakończeniu konfigurowania tego bloku.

    ![Konfigurowanie typu routingu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. W bloku **Nowe połączenie VPN** kliknij przycisk **OK** u dołu bloku, aby rozpocząć tworzenie bramy sieci wirtualnej. Może to potrwać do 45 minut.

## <a name="generatecerts"></a>Sekcja 2 — tworzenie certyfikatów
Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Po utworzeniu certyfikatu głównego dane certyfikatu publicznego (nie klucz prywatny) należy wyeksportować jako plik cer X.509 z kodowaniem Base-64. Następnie dane certyfikatu publicznego należy przekazać z certyfikatu głównego do platformy Azure.

Na każdym komputerze klienckim nawiązującym połączenie z siecią wirtualną za pomocą połączenia typu punkt-lokacja musi być zainstalowany certyfikat klienta w celu uwierzytelniania. Certyfikat klienta jest generowany na podstawie certyfikatu głównego i instalowany na każdym komputerze klienckim. Jeśli prawidłowy certyfikat klienta nie jest zainstalowany, a klient próbuje nawiązać połączenie z siecią wirtualną, uwierzytelnianie nie powiedzie się.

### <a name="cer"></a>Część 1. Uzyskiwanie klucza publicznego (pliku cer) dla certyfikatu głównego

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
 
Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Uzyskaj plik cer dla certyfikatu głównego, którego chcesz użyć.

####<a name="self-signed-root-certificate"></a>Certyfikat główny z podpisem własnym

Jeśli nie używasz rozwiązania z certyfikatem przedsiębiorstwa, musisz utworzyć certyfikat główny z podpisem własnym. Aby utworzyć certyfikat z podpisem własnym, który zawiera pola niezbędne do uwierzytelniania punkt-lokacja, możesz użyć programu PowerShell. Temat [Create a self-signed certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Tworzenie certyfikatu z podpisem własnym dla połączeń punkt-lokacja za pomocą programu PowerShell) zawiera opis kroków tworzenia certyfikatu głównego z podpisem własnym.

> [!NOTE]
> Wcześniej zalecaną metodą tworzenia certyfikatów głównych z podpisem własnym i generowania certyfikatów klienta dla połączeń punkt-lokacja było użycie narzędzia makecert. Teraz do utworzenia tych certyfikatów można użyć programu PowerShell. Jedną z zalet używania programu PowerShell jest możliwość tworzenia certyfikatów SHA-2. Wymagane wartości są opisane w temacie [Create a self-signed certificate for Point-to-Site connections using PowerShell](vpn-gateway-certificates-point-to-site.md) (Tworzenie certyfikatu z podpisem własnym dla połączeń punkt-lokacja za pomocą programu PowerShell).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Aby wyeksportować klucz publiczny dla certyfikatu głównego z podpisem własnym

Połączenia punkt-lokacja wymagają przesłania klucza publicznego (plik cer) na platformę Azure. Wykonanie poniższych kroków ułatwia wyeksportowanie pliku cer dla certyfikatu głównego z podpisem własnym.

1. Aby uzyskać plik cer z certyfikatu, otwórz narzędzie **certmgr.msc**. Zlokalizuj certyfikat główny z podpisem własnym — zwykle znajduje się w katalogu „Certyfikaty - bieżący użytkownik\Osobisty\Certyfikat” — a następnie kliknij go prawym przyciskiem myszy. Kliknij pozycję **Wszystkie zadania**, a następnie kliknij pozycję **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W Kreatorze kliknij pozycję **Dalej**. Wybierz pozycję **Nie eksportuj klucza prywatnego**, a następnie kliknij pozycję **Dalej**.
3. Na stronie **Format pliku eksportu** wybierz pozycję **Certyfikat X.509 szyfrowany algorytmem Base-64 (.CER)**, a następnie kliknij pozycję **Dalej**. 
4. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat. Wyświetlony zostanie komunikat **Eksport zakończył się pomyślnie**. Kliknij przycisk **OK**, aby zamknąć kreatora.

### <a name="genclientcert"></a>Część 2: Generowanie certyfikatu klienta

Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
- Jeśli używasz rozwiązania z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu formatu wartości nazwy pospolitej „name@yourdomain.com”, a nie formatu „nazwa_domeny\nazwa_użytkownika”.
- Upewnij się, że wydawany certyfikat klienta jest oparty na szablonie certyfikatu „Użytkownik”, którego pierwszym elementem na liście użycia jest „Uwierzytelnienie klienta”, a nie Logowanie karty inteligentnej itp. Certyfikat można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i wyświetlenie pozycji **Szczegóły > Ulepszone użycie klucza**.

####<a name="self-signed-root-certificate"></a>Certyfikat główny z podpisem własnym 
Jeśli używasz certyfikatu głównego z podpisem własnym, zobacz temat [Generate a client certificate using PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) (Generowanie certyfikatu klienta za pomocą programu PowerShell), aby uzyskać kroki służące do wygenerowania certyfikatu klienta zgodnego z połączeniami punkt-lokacja.

### <a name="exportclientcert"></a>Część 3: Eksportowanie certyfikatu klienta
Jeśli certyfikat klienta jest generowany na podstawie certyfikatu głównego z podpisem własnym za pomocą instrukcji dla programu [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert), jest on automatycznie instalowany na komputerze użytym do jego wygenerowania. Aby zainstalować certyfikat klienta na innym komputerze klienckim, należy go wyeksportować.

1. Aby wyeksportować certyfikat klienta, otwórz narzędzie **certmgr.msc**. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W kreatorze kliknij pozycję **Dalej**, wybierz opcję **Tak, eksportuj klucz prywatny**, a następnie kliknij pozycję **Dalej**.
3. Na stronie **Format pliku eksportu** możesz pozostawić wybrane wartości domyślne. Następnie kliknij przycisk **Next** (Dalej). 
4. Na stronie **Zabezpieczenia** należy włączyć ochronę klucza prywatnego. Jeśli wybierzesz opcję użycia hasła, zapisz lub zapamiętaj hasło ustawione dla tego certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
6. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat.

## <a name="upload"></a>Sekcja 3 — Przekazywanie pliku cer certyfikatu głównego
Po utworzeniu bramy można przekazać plik cer dla zaufanego certyfikatu głównego platformy Azure. Można przekazać pliki dla maksymalnie 20 certyfikatów głównych. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure używa go do uwierzytelniania klientów łączących się z siecią wirtualną.

1. W sekcji **Połączenia sieci VPN** bloku danej sieci wirtualnej kliknij grafikę **klienci**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.

    ![Klienci](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. W bloku **Połączenie punkt-lokacja** kliknij przycisk **Zarządzanie certyfikatami**, aby otworzyć blok **Certyfikaty**.<br>

    ![Blok Certyfikaty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. W bloku **Certyfikaty** kliknij pozycję **Przekaż**, aby otworzyć blok **Przekaż certyfikat**.<br>

    ![Blok przekazywania certyfikatów](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Kliknij grafikę folderu, aby przejść do pliku cer. Wybierz plik, a następnie kliknij przycisk **OK**. Odśwież stronę, aby wyświetlić przekazany certyfikat w bloku **Certyfikaty**.

    ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Sekcja 4 — Generowanie pakietu konfiguracyjnego klienta VPN
Aby nawiązać połączenie z siecią wirtualną, należy skonfigurować także klienta sieci VPN. W celu nawiązania połączenia komputer kliencki wymaga zarówno certyfikatu klienta, jak i odpowiedniego pakietu konfiguracyjnego klienta sieci VPN.

Pakiet klienta VPN zawiera informacje konfiguracyjne umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Ustawienia są specyficzne dla sieci wirtualnej, z którą chcesz nawiązać połączenie. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

### <a name="to-generate-the-vpn-client-configuration-package"></a>Aby wygenerować pakiet konfiguracyjny klienta VPN
1. W witrynie Azure Portal, w bloku **Omówienie** dla danej sieci wirtualnej kliknij grafikę klienta w obszarze **Połączenia VPN**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.
2. W górnej części bloku **Połączenie sieci VPN typu punkt-lokacja** kliknij pakiet pobierania, który odpowiada systemowi operacyjnemu klienta, na którym zostanie zainstalowany:

   * Dla klientów 64-bitowych wybierz pakiet **Klient sieci VPN (64-bitowy)**.
   * Dla klientów 32-bitowych wybierz pakiet **Klient sieci VPN (32-bitowy)**.

     ![Pobieranie pakietu konfiguracyjnego klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Pojawi się komunikat, że Azure generuje pakiet konfiguracyjny klienta VPN dla sieci wirtualnej. Po kilku minutach pakiet zostanie wygenerowany i na komputerze lokalnym wyświetlony komunikat informujący, że pakiet został pobrany. Zapisz plik pakietu konfiguracyjnego. Zainstalujesz go na każdym komputerze klienckim, który będzie łączyć się z siecią wirtualną przy użyciu połączenia typu punkt-lokacja.

## <a name="clientconfiguration"></a>Sekcja 5 — Konfigurowanie komputera klienckiego
### <a name="part-1-install-an-exported-client-certificate"></a>Część 1. Instalowanie wyeksportowanego certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne będzie hasło, które zostało utworzone podczas eksportowania certyfikatu klienta.

1. Znajdź plik *pfx* i skopiuj go na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx*, aby go zainstalować. Pozostaw wartość pola **Lokalizacja magazynu** jako **Bieżący użytkownik**, a następnie kliknij pozycję **Dalej**.
2. Na stronie importowania **Plik** nie wprowadzaj żadnych zmian. Kliknij przycisk **Dalej**.
3. Na stronie **Ochrona klucza prywatnego** wprowadź hasło dla certyfikatu, jeśli zostało użyte, lub sprawdź, czy podmiot zabezpieczeń instalujący certyfikat jest poprawny, a następnie kliknij pozycję **Dalej**.
4. Na stronie **Magazyn certyfikatów** pozostaw lokalizację domyślną, a następnie kliknij pozycję **Dalej**.
5. Kliknij przycisk **Finish** (Zakończ). Na stronie **Ostrzeżenie o zabezpieczeniach** dla instalacji certyfikatu kliknij pozycję **Tak**. Możesz bez obaw kliknąć pozycję „Tak”, ponieważ wygenerowano certyfikat. Certyfikat został pomyślnie zaimportowany.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Część 2: Instalowanie pakietu konfiguracyjnego klienta VPN
Tego samego pakietu konfiguracyjnego klienta VPN można użyć na każdym komputerze klienckim, pod warunkiem, że wersja jest zgodna z architekturą dla klienta.

1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną. 
2. Kliknij dwukrotnie plik exe, aby zainstalować pakiet na komputerze klienckim. Pakiet konfiguracji nie jest podpisany, ponieważ został utworzony przez Ciebie. Oznacza to, że może pojawić się ostrzeżenie. Jeśli pojawi się okno podręczne Windows SmartScreen, kliknij pozycję **Więcej informacji** (z lewej strony), a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać następująco:

    ![Klient sieci VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png)

## <a name="connect"></a>Sekcja 6 — Nawiązywanie połączenia z platformą Azure
### <a name="connect-to-your-vnet"></a>Nawiązywanie połączenia z siecią wirtualną
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień.
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

    ![Połączenie klienta sieci VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Połączenie powinno zostać nawiązane.

    ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

> [!NOTE]
> Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście. 
>
>

### <a name="verify-the-vpn-connection"></a>Sprawdzenie połączenia sieci VPN
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

## <a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Usunięcie zaufanego certyfikatu spowoduje, że certyfikaty klienta wygenerowane na podstawie certyfikatu głównego nie będą już mogły nawiązać połączenia z platformą Azure przy pomocy połączenia punkt-lokacja. Jeśli chcesz, aby klienci mogli nawiązać połączenie, muszą oni zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu, który jest traktowany jako zaufany przez platformę Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Aby uzyskać instrukcje, zobacz [Sekcja 3 — Przekazywanie pliku cer certyfikatu głównego](#upload).

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego


1. W sekcji **Połączenia sieci VPN** bloku danej sieci wirtualnej kliknij grafikę **klienci**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.

    ![Klienci](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. W bloku **Połączenie punkt-lokacja** kliknij przycisk **Zarządzanie certyfikatami**, aby otworzyć blok **Certyfikaty**.<br>

    ![Blok Certyfikaty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. W bloku **Certyfikaty** kliknij wielokropek obok certyfikatu, który chcesz usunąć, a następnie kliknij pozycję **Usuń**.

     ![Usuwanie certyfikatu głównego](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>


## <a name="revokeclient"></a>Odwoływanie certyfikatu klienta
Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania połączeń punkt-lokacja.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="to-revoke-a-client-certificate"></a>Aby odwołać certyfikat klienta

Certyfikat klienta można odwołać przez dodanie odcisku palca do listy odwołania.

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Instrukcje: Pobieranie odcisku palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów.
3. Przejdź do bloku **„nazwa klasycznej sieci wirtualnej” > Połączenie VPN punkt-lokacja > Certyfikaty**, a następnie kliknij pozycję **Lista odwołania**, aby otworzyć blok Lista odwołania. 
4. W bloku **Lista odwołania** kliknij pozycję **+ Dodaj certyfikat**, aby otworzyć blok **Dodawanie certyfikatu do listy odwołania**.
5. W bloku **Dodawanie certyfikatu do listy odwołania** wklej odcisk palca certyfikatu jako jeden ciągły wiersz tekstu, bez spacji. Kliknij przycisk **OK** w dolnej części bloku.
6. Po zakończeniu aktualizowania nie będzie można już używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.


## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).

