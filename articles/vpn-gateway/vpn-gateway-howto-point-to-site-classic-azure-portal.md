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
ms.date: 06/27/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: f048e344026b0fd930569c949b23a42c3c30fffe
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną przy użyciu klasycznej witryny Azure Portal

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

W tym artykule pokazano sposób tworzenia sieci wirtualnej z połączeniem punkt-lokacja w klasycznym modelu wdrażania przy użyciu witryny Azure Portal. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Portal Azure (klasyczny)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenia punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenie sieci VPN typu punkt-lokacja jest inicjowane z komputera klienckiego za pomocą natywnego klienta VPN systemu Windows. Łączący się klienci używają certyfikatów do uwierzytelniania. 


![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN ani publicznego adresu IP. Połączenie typu punkt-lokacja tworzy połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Po stronie serwera obsługiwany jest protokół SSTP w wersji 1.0, 1.1 i 1.2. Klient decyduje o wyborze wersji do użycia. W przypadku systemu Windows 8.1 i nowszych protokół SSTP domyślnie używa wersji 1.2. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

Dla połączeń punkt-lokacja wymagane są następujące elementy:

* Brama dynamicznej sieci VPN.
* Klucz publiczny (plik cer) dla certyfikatu głównego, przekazany na platformę Azure. Jest uznawany za certyfikat zaufany i używany do uwierzytelniania.
* Certyfikat klienta wygenerowany na podstawie certyfikatu głównego i zainstalowany na każdym komputerze klienckim, który będzie nawiązywać połączenie. Ten certyfikat jest używany do uwierzytelniania klientów.
* Pakiet konfiguracji klienta sieci VPN musi zostać wygenerowany i zainstalowany na każdym komputerze klienckim, który nawiązuje połączenie. Pakiet konfiguracji klienta konfiguruje natywnego klienta sieci VPN, który znajduje się już w systemie operacyjnym klienta, ustawiając w nim informacje niezbędne do łączenia się z siecią wirtualną.

### <a name="example-settings"></a>Przykładowe ustawienia

Następujących wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule:

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

## <a name="vnetvpn"></a>Sekcja 1 — Tworzenie sieci wirtualnej i bramy sieci VPN

Przed rozpoczęciem sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).

### <a name="createvnet"></a>Część 1. Tworzenie sieci wirtualnej

Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi. Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj poniższe kroki:

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

  ![Wyszukiwanie bloku Sieć wirtualna](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Klasyczny**, a następnie kliknij przycisk **Utwórz**.

  ![Wybieranie modelu wdrożenia](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku dodaj pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe.

  ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę zasobów, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.
8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.

  ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Po kliknięciu przycisku Utwórz na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

  ![Kafelek tworzenia sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal będzie widoczna wartość **Utworzona**.
11. Dodaj serwer DNS (opcjonalnie). Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS na potrzeby rozpoznawania nazw. Określony serwer DNS powinien być tym, który może rozpoznawać nazwy dla zasobów w sieci wirtualnej.<br>Aby dodać serwer DNS, otwórz ustawienia dla swojej sieci wirtualnej, kliknij serwery DNS i dodaj adres IP serwera DNS, którego chcesz użyć. Pakiet konfiguracji klienta generowany w późniejszym kroku będzie zawierać adresy IP serwerów DNS określonych w tym ustawieniu. Jeśli w przyszłości okaże się konieczne zaktualizowanie listy serwerów DNS, można wygenerować i zainstalować nowe pakiety konfiguracji klienta sieci VPN odzwierciedlające zaktualizowaną listę.

### <a name="gateway"></a>Część 2. Tworzenie podsieci bramy i bramy o dynamicznym routingu

W tym kroku tworzona jest podsieć bramy i brama o dynamicznym routingu. W witrynie Azure Portal w przypadku klasycznego modelu wdrażania tworzenie podsieci bramy i bramy można wykonać za pomocą tych samych bloków konfiguracji.

1. W witrynie portalu przejdź do sieci wirtualnej, dla której chcesz utworzyć bramę.
2. W bloku **Przegląd** dla odpowiedniej sieci wirtualnej, w sekcji Połączenia VPN kliknij pozycję **Brama**.

  ![Kliknij w celu utworzenia bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. W bloku **Nowe połączenie VPN**wybierz opcję **Punkt-lokacja**.

  ![Połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. W obszarze **Przestrzeń adresowa klienta** dodaj zakres adresów IP. Jest to zakres, z którego klienci VPN otrzymują adres IP podczas nawiązywania połączenia. Używaj zakresu prywatnych adresów IP nienakładającego się na lokalizację lokalną, z której będziesz się łączyć, ani na sieć wirtualną, z którą chcesz się łączyć. Automatycznie wypełniony zakres możesz usunąć, a następnie dodać zakres prywatnych adresów IP, z którego chcesz korzystać.

  ![Przestrzeń adresowa klienta](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Zaznacz pole wyboru **Utwórz bramę natychmiast**.

  ![Utwórz bramę natychmiast](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Kliknij pozycję **Opcjonalna konfiguracja bramy**, aby otworzyć blok **Konfiguracja bramy**.

  ![Kliknij pozycję Opcjonalna konfiguracja bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Kliknij pozycje **Podsieć > Skonfiguruj wymagane ustawienia**, aby dodać **podsieć bramy**. Chociaż możliwe jest utworzenie małej podsieci bramy /29, zaleca się wybranie podsieci przynajmniej /28 lub /27, aby zawierała więcej adresów. Zapewni to wystarczająco dużo adresów, aby możliwe były dodatkowe konfiguracje, które mogą być potrzebne w przyszłości. Podczas pracy z podsieciami bramy należy unikać kojarzenia sieciowej grupy zabezpieczeń (NSG, Network Security Group) z podsiecią bramy. Skojarzenie sieciowej grupy zabezpieczeń z tą podsiecią spowoduje, że brama sieci VPN przestanie działać zgodnie z oczekiwaniami.

  ![Dodawanie podsieci GatewaySubnet](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Wybierz **rozmiar** bramy. Rozmiar to jednostka SKU bramy sieci wirtualnej. W portalu domyślną jednostką SKU jest **Podstawowa**. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Informacje o ustawieniach bramy VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Rozmiar bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Wybierz **Typ routingu** dla bramy. Konfiguracje P2S wymagają typu routingu **Dynamiczny**. Kliknij przycisk **OK** po zakończeniu konfigurowania tego bloku.

  ![Konfigurowanie typu routingu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. W bloku **Nowe połączenie VPN** kliknij przycisk **OK** u dołu bloku, aby rozpocząć tworzenie bramy sieci wirtualnej. Tworzenie bramy sieci VPN może zająć do 45 minut, zależnie od wybranej jednostki sku bramy.

## <a name="generatecerts"></a>Sekcja 2 — Tworzenie certyfikatów

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Informacje o kluczu publicznym certyfikatu głównego należy przekazać na platformę Azure. Klucz publiczny jest wtedy uważany za „zaufany”. Certyfikaty klienta muszą być generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim w magazynie certyfikatów Certificates-Current User/Personal. Certyfikat jest używany do uwierzytelniania klienta, gdy inicjuje on połączenie z siecią wirtualną. 

Jeśli używasz certyfikatów z podpisem własnym, należy je utworzyć przy użyciu określonych parametrów. Certyfikat z podpisem własnym można utworzyć przy użyciu instrukcji dotyczących [środowiska PowerShell i systemu Windows 10](vpn-gateway-certificates-point-to-site.md) lub polecenia [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Ważne jest, aby wykonać kroki opisane w tych instrukcjach w przypadku pracy z certyfikatami głównymi z podpisem własnym i generowania certyfikatów klienta z certyfikatu głównego z podpisem własnym. W przeciwnym razie utworzone przez Ciebie certyfikaty nie będą zgodne z połączeniami typu punkt-lokacja i zostanie wyświetlony błąd połączenia.

### <a name="cer"></a>Część 1. Uzyskiwanie klucza publicznego (pliku cer) dla certyfikatu głównego

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>Część 2. Generowanie certyfikatu klienta

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>Sekcja 3 — Przekazywanie pliku cer certyfikatu głównego

Po utworzeniu bramy możesz przekazać plik cer (który zawiera informacje o kluczu publicznym) dla zaufanego certyfikatu głównego na platformę Azure. Nie przekazuj klucza prywatnego dla certyfikatu głównego do platformy Azure. Po przekazaniu pliku cer platforma Azure może używać go do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta wygenerowany na podstawie zaufanego certyfikatu głównego. Jeśli okaże się to konieczne, dodatkowe pliki zaufanego certyfikatu głównego możesz przekazać później — maksymalnie może ich być 20.  

1. W sekcji **Połączenia sieci VPN** bloku danej sieci wirtualnej kliknij grafikę **klienci**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.

  ![Klienci](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. W bloku **Połączenie punkt-lokacja** kliknij przycisk **Zarządzanie certyfikatami**, aby otworzyć blok **Certyfikaty**.<br>

  ![Blok Certyfikaty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. W bloku **Certyfikaty** kliknij pozycję **Przekaż**, aby otworzyć blok **Przekaż certyfikat**.<br>

    ![Blok przekazywania certyfikatów](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Kliknij grafikę folderu, aby przejść do pliku cer. Wybierz plik, a następnie kliknij przycisk **OK**. Odśwież stronę, aby wyświetlić przekazany certyfikat w bloku **Certyfikaty**.

  ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Sekcja 4 — Konfigurowanie klienta

Aby nawiązać połączenie z siecią wirtualną przy użyciu połączenia sieci VPN typu punkt-lokacja, na każdym kliencie trzeba zainstalować pakiet do konfiguracji natywnego klienta sieci VPN systemu Windows. Pakiet konfiguracji konfiguruje natywnego klienta sieci VPN systemu Windows przy użyciu ustawień niezbędnych do nawiązania połączenia z siecią wirtualną i, jeśli został określony serwer DNS dla sieci wirtualnej, zawiera adres IP serwera DNS, którego klient będzie używać do rozpoznawania nazw. Jeśli określony serwer DNS zmienisz później, po wygenerowaniu pakietu konfiguracji klienta, pamiętaj, aby wygenerować nowy pakiet konfiguracji klienta do zainstalowania na Twoich komputerach klienckich.

Tego samego pakietu konfiguracji klienta VPN można użyć na każdym komputerze klienckim, o ile wersja jest zgodna z architekturą dla klienta. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

### <a name="part-1-generate-and-install-the-vpn-client-configuration-package"></a>Część 1: Generowanie i instalowanie pakietu konfiguracyjnego klienta VPN

1. W witrynie Azure Portal, w bloku **Omówienie** dla danej sieci wirtualnej kliknij grafikę klienta w obszarze **Połączenia VPN**, aby otworzyć blok **Połączenie sieci VPN typu punkt-lokacja**.
2. W górnej części bloku **Połączenie sieci VPN typu punkt-lokacja** kliknij pakiet pobierania, który odpowiada systemowi operacyjnemu klienta, na którym zostanie zainstalowany:

  * Dla klientów 64-bitowych wybierz pakiet **Klient sieci VPN (64-bitowy)**.
  * Dla klientów 32-bitowych wybierz pakiet **Klient sieci VPN (32-bitowy)**.

  ![Pobieranie pakietu konfiguracji klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Po wygenerowaniu pakietu pobierz go i zainstaluj na komputerze klienckim. Jeśli zostanie wyświetlone okno podręczne SmartScreen, kliknij pozycję **Więcej informacji**, a następnie pozycję **Uruchom mimo to**. Możesz także zapisać pakiet w celu zainstalowania go na innych komputerach klienckich.

### <a name="part-2-install-the-client-certificate"></a>Część 2. Instalowanie certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne jest hasło, które zostało utworzone w trakcie eksportowania certyfikatu klienta. Zazwyczaj jest to kwestia dwukrotnego kliknięcia certyfikatu i zainstalowania go. Aby uzyskać więcej informacji, zobacz [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install) (Instalowanie wyeksportowanego certyfikatu klienta).

## <a name="connect"></a>Sekcja 5 — Łączenie z platformą Azure

### <a name="connect-to-your-vnet"></a>Nawiązywanie połączenia z siecią wirtualną

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień.
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

  ![Połączenie klienta sieci VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Połączenie zostało ustanowione.

  ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

Jeśli występują problemy z połączeniem, sprawdź następujące elementy:

- Otwórz okno **Zarządzaj certyfikatami użytkowników** i przejdź do pozycji **Zaufane główne urzędy certyfikacji\Certyfikaty**. Sprawdź, czy certyfikat główny znajduje się na liście. Aby uwierzytelnianie działało, certyfikat główny musi być obecny. Podczas eksportowania pliku pfx certyfikatu klienta przy użyciu wartości domyślnej „Jeśli jest to możliwe, dołącz wszystkie certyfikaty ze ścieżki certyfikacji” eksportowane są również informacje o certyfikacie głównym. Następnie, gdy instalujesz certyfikat klienta, certyfikat główny jest również instalowany na komputerze klienckim. 

- Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście. 

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

 
 Jeśli występują problemy z nawiązaniem połączenia z maszyną wirtualną za pośrednictwem połączenia P2S, użyj narzędzia „ipconfig”, aby sprawdzić adres IPv4 przypisany do karty Ethernet na komputerze, z którego jest nawiązywane połączenie. Jeśli adres IP znajduje się w zakresie adresów sieci wirtualnej, z którą jest nawiązywane połączenie, lub w zakresie adresów puli VPNClientAddressPool, jest to określane jako nakładająca się przestrzeń adresowa. Kiedy przestrzeń adresowa nakłada się w ten sposób, ruch sieciowy nie dociera do platformy Azure, tylko pozostaje w sieci lokalnej. Jeśli przestrzenie adresowe sieci nie nakładają się i nadal nie można nawiązać połączenia z maszyną wirtualną, zobacz [Rozwiązywanie problemów z połączeniami pulpitu zdalnego z maszyną wirtualną](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="connectVM"></a>Nawiązywanie połączenia z maszyną wirtualną

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Po usunięciu certyfikatu głównego klienci, którzy mają certyfikat wygenerowany na podstawie tego certyfikatu głównego, nie będą w stanie się uwierzytelnić i w związku z tym nie będą mogli nawiązywać połączeń. Jeśli chcesz, aby klienci mogli uwierzytelniać się i nawiązywać połączenia, musisz zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu głównego, który jest traktowany jako zaufany przez platformę Azure (przekazany na tę platformę).

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
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).

