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
ms.date: 06/27/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 4045dd501ba70b04cb8ea06901c76072fc009018
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurowanie połączenia punkt-lokacja z siecią wirtualną za pomocą witryny Azure Portal

W tym artykule pokazano sposób tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja w modelu wdrażania przy użyciu usługi Resource Manager za pomocą witryny Azure Portal. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Portal Azure (klasyczny)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenia punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenie sieci VPN typu punkt-lokacja jest inicjowane z komputera klienckiego za pomocą natywnego klienta VPN systemu Windows. Łączący się klienci używają certyfikatów do uwierzytelniania. 


![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN ani publicznego adresu IP. Połączenie typu punkt-lokacja tworzy połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Po stronie serwera obsługiwany jest protokół SSTP w wersji 1.0, 1.1 i 1.2. Klient decyduje o wyborze wersji do użycia. W przypadku systemu Windows 8.1 i nowszych protokół SSTP domyślnie używa wersji 1.2. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

Dla połączeń punkt-lokacja wymagane są następujące elementy:

* Brama sieci VPN oparta na trasie.
* Klucz publiczny (plik cer) dla certyfikatu głównego, przekazany na platformę Azure. Jest uznawany za certyfikat zaufany i używany do uwierzytelniania.
* Certyfikat klienta wygenerowany na podstawie certyfikatu głównego i zainstalowany na każdym komputerze klienckim, który będzie nawiązywać połączenie. Ten certyfikat jest używany do uwierzytelniania klientów.
* Pakiet konfiguracji klienta sieci VPN musi zostać wygenerowany i zainstalowany na każdym komputerze klienckim, który nawiązuje połączenie. Pakiet konfiguracji klienta konfiguruje natywnego klienta sieci VPN, który znajduje się już w systemie operacyjnym klienta, ustawiając w nim informacje niezbędne do łączenia się z siecią wirtualną.

### <a name="example"></a>Przykładowe wartości

Następujących wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule:

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

## <a name="createvnet"></a>1 — Tworzenie sieci wirtualnej

Przed rozpoczęciem sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial). Jeśli tworzysz tę konfigurację w ramach ćwiczenia, skorzystaj z [przykładowych wartości](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>2 — Określanie przestrzeni adresowej i podsieci

Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz podsieci.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3 — Dodawanie podsieci bramy

Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Usługi bramy korzystają z adresów IP określonych w podsieci bramy. Jeśli to możliwe, utwórz podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

Zrzuty ekranu w tej sekcji służą jedynie jako przykład. Dla opcji GatewaySubnet należy użyć zakresu adresów odpowiadającemu wartościom wymaganym dla danej konfiguracji.

### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4 — Określanie serwera DNS (opcjonalne)

Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Określony serwer DNS powinien być w stanie rozpoznawać nazwy zasobów, z którymi nawiązywane jest połączenie. Pakiet konfiguracji klienta sieci VPN generowany w późniejszym kroku będzie zawierać adresy IP serwerów DNS określonych w tym ustawieniu. Jeśli w przyszłości okaże się konieczne zaktualizowanie listy serwerów DNS, można wygenerować i zainstalować nowe pakiety konfiguracji klienta sieci VPN odzwierciedlające nową listę.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>5 — Tworzenie bramy sieci wirtualnej

Połączenia punkt-lokacja wymagają następujących ustawień:

* Typ bramy: VPN
* Typ sieci VPN: oparta na trasach

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę sieci wirtualnej

[!INCLUDE [create a vnet gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>6 — Generowanie certyfikatów

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Informacje o kluczu publicznym certyfikatu głównego należy przekazać na platformę Azure. Klucz publiczny jest wtedy uważany za „zaufany”. Certyfikaty klienta muszą być generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim w magazynie certyfikatów Certificates-Current User/Personal. Certyfikat jest używany do uwierzytelniania klienta, gdy inicjuje on połączenie z siecią wirtualną. 

Jeśli używasz certyfikatów z podpisem własnym, należy je utworzyć przy użyciu określonych parametrów. Certyfikat z podpisem własnym można utworzyć przy użyciu instrukcji dotyczących [środowiska PowerShell i systemu Windows 10](vpn-gateway-certificates-point-to-site.md) lub polecenia [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Ważne jest, aby wykonać kroki opisane w tych instrukcjach w przypadku pracy z certyfikatami głównymi z podpisem własnym i generowania certyfikatów klienta z certyfikatu głównego z podpisem własnym. W przeciwnym razie utworzone przez Ciebie certyfikaty nie będą zgodne z połączeniami typu punkt-lokacja i zostanie wyświetlony błąd połączenia.

### <a name="getcer"></a>Krok 1 — Uzyskiwanie pliku cer dla certyfikatu głównego

[!INCLUDE [obtain root certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>Krok 2 — Generowanie certyfikatu klienta

[!INCLUDE [generate client certificate](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>7 — Dodawanie puli adresów klienta

Pula adresów klienta to określony przez Ciebie zakres prywatnych adresów IP. Klienci łączący się przez połączenie punkt-lokacja otrzymują adres IP z tego zakresu. Używaj zakresu prywatnych adresów IP nienakładającego się na lokalizację lokalną, z której będziesz się łączyć, ani na sieć wirtualną, z którą chcesz się łączyć.

1. Po utworzeniu bramy sieci wirtualnej przejdź do sekcji **Ustawienia** w bloku bramy sieci wirtualnej. W sekcji **Ustawienia** kliknij pozycję **Konfiguracja punktu do lokacji**, aby otworzyć blok **Konfiguracja**.

  ![Blok punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. Automatycznie wypełniony zakres możesz usunąć, a następnie dodać zakres prywatnych adresów IP, z którego chcesz korzystać. Kliknij pozycję **Zapisz**, aby zweryfikować i zapisać ustawienie.

  ![Pula adresów klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>8 — Przekazywanie pliku cer certyfikatu głównego

Po utworzeniu bramy możesz przekazać plik cer (który zawiera informacje o kluczu publicznym) dla zaufanego certyfikatu głównego na platformę Azure. Po przekazaniu pliku cer platforma Azure może używać go do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta wygenerowany na podstawie zaufanego certyfikatu głównego. Jeśli okaże się to konieczne, dodatkowe pliki zaufanego certyfikatu głównego możesz przekazać później — maksymalnie może ich być 20. 

1. Certyfikaty są dodawane w bloku **Konfiguracja punktu do lokacji** w sekcji **Certyfikat główny**.  
2. Upewnij się, że certyfikat główny został wyeksportowany jako plik X.509 (cer) zaszyfrowany z użyciem algorytmu Base-64. Certyfikat należy wyeksportować w takim formacie, aby możliwe było jego otwarcie za pomocą edytora tekstów.
3. Otwórz certyfikat za pomocą edytora tekstów, takiego jak Notatnik. Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza. Skopiuj tylko następującą sekcję jako jeden ciągły wiersz:

  ![Dane dotyczące certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Wklej dane dotyczące certyfikatu w polu **Dane certyfikatu publicznego**. Korzystając z pola **Nazwa**, określ nazwę certyfikatu, a następnie kliknij przycisk **Zapisz**. Możesz dodać maksymalnie 20 zaufanych certyfikatów głównych.

  ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>9 — Instalowanie pakietu konfiguracji klienta sieci VPN

Aby nawiązać połączenie z siecią wirtualną przy użyciu połączenia sieci VPN typu punkt-lokacja, na każdym kliencie trzeba zainstalować pakiet do konfiguracji natywnego klienta sieci VPN systemu Windows. Pakiet konfiguracji konfiguruje natywnego klienta sieci VPN systemu Windows przy użyciu ustawień niezbędnych do nawiązania połączenia z siecią wirtualną i, jeśli został określony serwer DNS dla sieci wirtualnej, zawiera adres IP serwera DNS, którego klient będzie używać do rozpoznawania nazw. Jeśli określony serwer DNS zmienisz później, po wygenerowaniu pakietu konfiguracji klienta, pamiętaj, aby wygenerować nowy pakiet konfiguracji klienta do zainstalowania na Twoich komputerach klienckich.

Tego samego pakietu konfiguracji klienta VPN można użyć na każdym komputerze klienckim, o ile wersja jest zgodna z architekturą dla klienta. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

### <a name="step-1---download-the-client-configuration-package"></a>Krok 1 — Pobieranie pakietu konfiguracji klienta

1. W bloku **Konfiguracja punktu do lokacji** kliknij przycisk **Download VPN client** (Pobierz klienta VPN), aby otworzyć blok **Download VPN client** (Pobieranie klienta VPN). Wygenerowanie pakietu trwa minutę lub dwie.

  ![Pobieranie klienta VPN — sposób 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Wybierz właściwy pakiet dla klienta, a następnie kliknij przycisk **Pobierz**. Zapisz plik pakietu konfiguracji. Pakiet konfiguracji klienta VPN należy zainstalować na każdym komputerze klienckim, który łączy się z siecią wirtualną.

  ![Pobieranie klienta VPN — sposób 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Krok 2 — Instalowanie pakietu konfiguracji klienta

1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną. 
2. Kliknij dwukrotnie plik exe, aby zainstalować pakiet na komputerze klienckim. Ponieważ pakiet konfiguracji został utworzony przez Ciebie, nie jest podpisany i może zostać wyświetlone ostrzeżenie. Jeśli pojawi się okno podręczne Windows SmartScreen, kliknij pozycję **Więcej informacji** (z lewej strony), a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
3. Zainstaluj pakiet na komputerze klienckim. Jeśli pojawi się okno podręczne Windows SmartScreen, kliknij pozycję **Więcej informacji** (z lewej strony), a następnie pozycję **Uruchom mimo to**, aby zainstalować pakiet.
4. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie z siecią VPN zawiera nazwę sieci wirtualnej, z którą jest nawiązywane połączenie.

## <a name="installclientcert"></a>10 — Instalowanie certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne jest hasło, które zostało utworzone w trakcie eksportowania certyfikatu klienta. Zazwyczaj jest to kwestia dwukrotnego kliknięcia certyfikatu i zainstalowania go. Aby uzyskać więcej informacji, zobacz [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install) (Instalowanie wyeksportowanego certyfikatu klienta).

## <a name="connect"></a>11 — Nawiązywanie połączenia z platformą Azure

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Kliknij przycisk **Kontynuuj**, aby skorzystać z podwyższonego poziomu uprawnień.

2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

  ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Połączenie zostało ustanowione.

  ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

Jeśli występują problemy z połączeniem, sprawdź następujące elementy:

- Otwórz okno **Zarządzaj certyfikatami użytkowników** i przejdź do pozycji **Zaufane główne urzędy certyfikacji\Certyfikaty**. Sprawdź, czy certyfikat główny znajduje się na liście. Aby uwierzytelnianie działało, certyfikat główny musi być obecny. Podczas eksportowania pliku pfx certyfikatu klienta przy użyciu wartości domyślnej „Jeśli jest to możliwe, dołącz wszystkie certyfikaty ze ścieżki certyfikacji” eksportowane są również informacje o certyfikacie głównym. Następnie, gdy instalujesz certyfikat klienta, certyfikat główny jest również instalowany na komputerze klienckim. 

- Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście.


## <a name="verify"></a>12 — Weryfikowanie połączenia

1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP należy do określonej podczas konfiguracji połączenia punkt-lokacja puli adresów klienta sieci VPN. Wyniki są podobne, jak w następującym przykładzie:
   
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


Jeśli występują problemy z nawiązaniem połączenia z maszyną wirtualną za pośrednictwem połączenia P2S, użyj narzędzia „ipconfig”, aby sprawdzić adres IPv4 przypisany do karty Ethernet na komputerze, z którego jest nawiązywane połączenie. Jeśli adres IP znajduje się w zakresie adresów sieci wirtualnej, z którą jest nawiązywane połączenie, lub w zakresie adresów puli VPNClientAddressPool, jest to określane jako nakładająca się przestrzeń adresowa. Kiedy przestrzeń adresowa nakłada się w ten sposób, ruch sieciowy nie dociera do platformy Azure, tylko pozostaje w sieci lokalnej. Jeśli przestrzenie adresowe sieci nie nakładają się i nadal nie można nawiązać połączenia z maszyną wirtualną, zobacz [Rozwiązywanie problemów z połączeniami pulpitu zdalnego z maszyną wirtualną](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="connectVM"></a>Nawiązywanie połączenia z maszyną wirtualną

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Po usunięciu certyfikatu głównego klienci, którzy mają certyfikat wygenerowany na podstawie tego certyfikatu głównego, nie będą w stanie się uwierzytelnić i w związku z tym nie będą mogli nawiązywać połączeń. Jeśli chcesz, aby klienci mogli uwierzytelniać się i nawiązywać połączenia, musisz zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu głównego, który jest traktowany jako zaufany przez platformę Azure (przekazany na tę platformę).

### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Aby uzyskać instrukcje, zobacz sekcję [Przekazywanie zaufanego certyfikatu głównego](#uploadfile) w tym artykule.

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego

1. Aby usunąć zaufany certyfikat główny, przejdź do bloku **Konfiguracja punktu do lokacji** dla bramy Twojej sieci wirtualnej.
2. W sekcji **Certyfikat główny** bloku znajdź certyfikat, który chcesz usunąć.
3. Kliknij wielokropek obok certyfikatu, a następnie kliknij pozycję „Usuń”.

## <a name="revokeclient"></a>Odwoływanie certyfikatu klienta

Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="to-revoke-a-client-certificate"></a>Aby odwołać certyfikat klienta

Certyfikat klienta można odwołać przez dodanie odcisku palca do listy odwołania.

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Jak pobrać odcisk palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów.
3. Przejdź do bloku **Konfiguracja punktu do lokacji** bramy sieci wirtualnej. Korzystając z tego samego bloku, [przekazano zaufany certyfikat główny](#uploadfile).
4. W sekcji **Odwołane certyfikaty** wprowadź przyjazną nazwę certyfikatu (nie musi to być nazwa pospolita certyfikatu).
5. Skopiuj i wklej ciąg odcisku palca do pola **Odcisk palca**.
6. Odcisk palca zostanie zweryfikowany i automatycznie dodany do listy odwołania. Na ekranie zobaczysz komunikat informujący o aktualizowaniu listy. 
7. Po zakończeniu aktualizowania nie będzie można już używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).

