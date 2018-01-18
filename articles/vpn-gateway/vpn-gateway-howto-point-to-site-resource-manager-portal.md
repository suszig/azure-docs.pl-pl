---
title: "Łączenie komputera z siecią wirtualną platformy Azure przy użyciu połączenia typu punkt-lokacja i natywnego uwierzytelniania certyfikatu platformy Azure: Azure Portal | Microsoft Docs"
description: "Bezpiecznie połącz komputer z siecią wirtualną platformy Azure przez utworzenie połączenia bramy sieci VPN typu punkt-lokacja przy użyciu uwierzytelniania certyfikatu. Ten artykuł ma zastosowanie w modelu wdrażania przy użyciu usługi Resource Manager i użyto w nim witryny Azure Portal."
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
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 39129572ac9908429dc9b9ef64930e896afc355f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-azure-portal"></a>Konfigurowanie połączenia typu punkt-lokacja z siecią wirtualną przy użyciu natywnego uwierzytelniania certyfikatu platformy Azure: Azure Portal

W tym artykule pokazano sposób tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja w modelu wdrażania przy użyciu usługi Resource Manager (Menedżer zasobów) za pomocą witryny Azure Portal. W tej konfiguracji do uwierzytelniania używane są certyfikaty. W tej konfiguracji brama Azure VPN Gateway przeprowadza weryfikację certyfikatu, a nie serwera RADIUS. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Portal Azure (klasyczny)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Brama sieci VPN typu punkt-lokacja (P2S, Point-to-Site) pozwala utworzyć bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenia sieci VPN typu punkt-lokacja przydają się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, podczas pracy zdalnej z domu lub konferencji. Połączenie sieci VPN typu punkt-lokacja jest również przydatne zamiast połączenia sieci VPN typu lokacja-lokacja w przypadku niewielkiej liczby klientów, którzy muszą się łączyć z siecią wirtualną. Połączenie sieci VPN typu punkt-lokacja jest uruchamiane z urządzeń z systemem Windows i urządzeń Mac.

Przy łączeniu klientów mogą być używane następujące metody uwierzytelniania:

* Serwer RADIUS
* Natywne uwierzytelnianie certyfikatu platformy Azure przez bramę VPN Gateway

Ten artykuł pomaga skonfigurować konfigurację połączenia typu punkt-lokacja z uwierzytelnianiem za pomocą natywnego uwierzytelniania certyfikatu platformy Azure. Jeśli chcesz używać protokołu RADIUS do uwierzytelniania łączących się użytkowników, zobacz [P2S using RADIUS authentication](point-to-site-how-to-radius-ps.md) (Nawiązywanie połączeń typu punkt-lokacja z użyciem uwierzytelniania za pomocą protokołu RADIUS).

![Łączenie komputera z siecią wirtualną platformy Azure — diagram połączenie typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/p2snativeps.png)

Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN ani publicznego adresu IP. Połączenie typu punkt-lokacja tworzy połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol) lub IKEv2.

* Protokół SSTP to tunel sieci VPN bazujący na protokole SSL, który jest obsługiwany wyłącznie na platformach klienckich Windows. Może przechodzić przez zapory, co czyni go idealnym do nawiązywania połączenia z platformą Azure z dowolnego miejsca. Po stronie serwera obsługiwany jest protokół SSTP w wersji 1.0, 1.1 i 1.2. Klient decyduje o wyborze wersji do użycia. W przypadku systemu Windows 8.1 i nowszych protokół SSTP domyślnie używa wersji 1.2.

* Sieć VPN z protokołem IKEv2 to oparte na standardach rozwiązanie sieci VPN korzystające z protokołu IPsec. Sieci VPN z protokołem IKEv2 można używać do łączenia z urządzeniami Mac (z systemem OSX 10.11 lub nowszym).

Połączenia typu punkt-lokacja z natywnym uwierzytelnianiem certyfikatu platformy Azure mają następujące wymagania:

* Brama sieci VPN oparta na trasie.
* Klucz publiczny (plik cer) dla certyfikatu głównego, przekazany na platformę Azure. Przekazany certyfikat jest uznawany za certyfikat zaufany i używany do uwierzytelniania.
* Certyfikat klienta wygenerowany na podstawie certyfikatu głównego i zainstalowany na każdym komputerze klienckim, który będzie nawiązywać połączenie z siecią wirtualną. Ten certyfikat jest używany do uwierzytelniania klientów.
* Konfiguracja klienta sieci VPN. Pliki konfiguracji klienta sieci VPN zawierają informacje niezbędne klientowi do połączenia się z siecią wirtualną. Pliki te służą do konfigurowania istniejącego, natywnego dla systemu operacyjnego klienta sieci VPN . Każdy klient, który nawiązuje połączenie, musi być skonfigurowany przy użyciu ustawień w tych plikach konfiguracji.

Aby uzyskać więcej informacji na temat połączeń punkt-lokacja, zobacz [About Point-to-Site connections](point-to-site-about.md) (Informacje o połączeniach punkt-lokacja).

#### <a name="example"></a>Przykładowe wartości

Następujących wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule:

* **Nazwa sieci wirtualnej:** VNet1
* **Przestrzeń adresowa:** 192.168.0.0/16<br>W tym przykładzie zostanie wykorzystana tylko jedna przestrzeń adresowa. Istnieje możliwość użycia więcej niż jednej przestrzeni adresowej dla sieci wirtualnej.
* **Nazwa podsieci:** FrontEnd
* **Zakres adresów podsieci:** 192.168.1.0/24
* **Subskrypcja:** jeśli masz więcej niż jedną subskrypcję, sprawdź, czy korzystasz z właściwej.
* **Grupa zasobów:** TestRG
* **Lokalizacja:** Wschodnie stany USA
* **GatewaySubnet:** 192.168.200.0/24<br>
* **Serwer DNS:** (opcjonalny) adres IP serwera DNS, który ma być używany do rozpoznawania nazw.
* **Nazwa bramy sieci wirtualnej:** VNet1GW
* **Typ bramy:** VPN
* **Typ sieci VPN:** oparta na trasach
* **Publiczny adres IP:** VNet1GWpip
* **Typ połączenia:** Punkt-lokacja
* **Pula adresów klienta:** 172.16.201.0/24<br>Klienci sieci VPN połączeni z siecią wirtualną, którzy korzystają z tego połączenia punkt-lokacja, otrzymują adresy IP z puli adresów klientów.

## <a name="createvnet"></a>1. Tworzenie sieci wirtualnej

Przed rozpoczęciem sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).
[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Dodawanie podsieci bramy

Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Usługi bramy korzystają z adresów IP określonych w podsieci bramy. Jeśli to możliwe, utwórz podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Określanie serwera DNS (opcjonalne)

Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Serwer DNS jest opcjonalny w przypadku tej konfiguracji, ale jest wymagany, jeśli chcesz korzystać z rozpoznawania nazw. Określenie wartości nie powoduje utworzenia nowego serwera DNS. Określony adres IP serwera DNS powinien być adresem serwera będącego w stanie rozpoznawać nazwy zasobów, z którymi nawiązywane jest połączenie. W tym przykładzie użyto prywatnego adresu IP, ale może to nie być adres IP Twojego serwera DNS. Pamiętaj, aby użyć własnych wartości. Wartość, którą określasz, jest używana przez zasoby wdrażane w sieci wirtualnej, nie przez połączenie typu punkt-lokacja czy klienta sieci VPN.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Tworzenie bramy sieci wirtualnej

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generowanie certyfikatów klienta

Certyfikaty są używane przez platformę Azure do uwierzytelniania klientów łączących się z siecią wirtualną za pośrednictwem połączenia sieci VPN typu punkt-lokacja. Po uzyskaniu certyfikatu głównego należy [przekazać](#uploadfile) informacje o kluczu publicznym do platformy Azure. Certyfikat główny jest następnie uznawany przez platformę Azure za zaufany dla połączeń typu punkt-lokacja z siecią wirtualną. Ponadto certyfikaty klienta są generowane na podstawie zaufanego certyfikatu głównego, a następnie instalowane na każdym komputerze klienckim. Certyfikat klienta jest używany do uwierzytelniania klienta, gdy inicjuje on połączenie z siecią wirtualną. 

### <a name="getcer"></a>1. Uzyskiwanie pliku cer dla certyfikatu głównego

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Generowanie certyfikatu klienta

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Dodawanie puli adresów klienta

Pula adresów klienta to określony przez Ciebie zakres prywatnych adresów IP. Klienci łączący się przez połączenie VPN typu punkt-lokacja otrzymują adres IP z tego zakresu. Używaj zakresu prywatnych adresów IP nienakładającego się na lokalizację lokalną, z której się łączysz, ani na sieć wirtualną, z którą chcesz się łączyć.

1. Po utworzeniu bramy sieci wirtualnej przejdź do sekcji **Ustawienia** na stronie bramy sieci wirtualnej. W sekcji **Ustawienia** kliknij pozycję **Konfiguracja punktu do lokacji**, aby otworzyć stronę **Konfiguracja punktu do lokacji**.

  ![Strona połączenia typu punkt-lokacja](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. Na stronie **Konfiguracja punktu do lokacji** możesz usunąć automatycznie wypełniony zakres, a następnie dodać zakres prywatnych adresów IP, z którego chcesz korzystać. Kliknij pozycję **Zapisz**, aby zweryfikować i zapisać ustawienie.

  ![Pula adresów klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Przekazywanie danych certyfikatu publicznego dla certyfikatu głównego

Po utworzeniu bramy możesz przekazać do platformy Azure informacje o kluczu publicznym dla certyfikatu głównego. Po przekazaniu danych certyfikatu publicznego platforma Azure może używać go do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta wygenerowany na podstawie zaufanego certyfikatu głównego. Możesz przekazać łącznie do 20 zaufanych certyfikatów głównych.

1. Certyfikaty są dodawane na stronie **Konfiguracja punktu do lokacji** w sekcji **Certyfikat główny**.  
2. Upewnij się, że certyfikat główny został wyeksportowany jako plik X.509 (cer) zaszyfrowany z użyciem algorytmu Base-64. Certyfikat należy wyeksportować w takim formacie, aby możliwe było jego otwarcie za pomocą edytora tekstów.
3. Otwórz certyfikat za pomocą edytora tekstów, takiego jak Notatnik. Podczas kopiowania danych dotyczących certyfikatu upewnij się, że kopiujesz tekst jako jeden ciągły wiersz bez znaków powrotu karetki i wysuwu wiersza. Może zajść potrzeba zmodyfikowania widoku w edytorze tekstu na potrzeby pokazywania symboli lub pokazywania wszystkich znaków, aby wyświetlić znaki powrotu karetki i wysuwu wiersza. Skopiuj tylko następującą sekcję jako jeden ciągły wiersz:

  ![Dane dotyczące certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Wklej dane dotyczące certyfikatu w polu **Dane certyfikatu publicznego**. Korzystając z pola **Nazwa**, określ nazwę certyfikatu, a następnie kliknij przycisk **Zapisz**. Możesz dodać maksymalnie 20 zaufanych certyfikatów głównych.

  ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="installclientcert"></a>8. Instalowanie wyeksportowanego certyfikatu klienta

Jeśli chcesz utworzyć połączenie punkt-lokacja z komputerem klienckim innym niż użyty do wygenerowania certyfikatów klienta, należy zainstalować certyfikat klienta. Podczas instalowania certyfikatu klienta potrzebne jest hasło, które zostało utworzone w trakcie eksportowania certyfikatu klienta.

Upewnij się, że certyfikat klienta został wyeksportowany jako plik pfx wraz z całym łańcuchem certyfikatów (jest to ustawienie domyślne). W przeciwnym razie informacje o certyfikacie głównym nie będą dostępne na komputerze klienckim i klient nie będzie mógł się poprawnie uwierzytelnić.

Aby zapoznać się z krokami instalacji, zobacz [Install a client certificate](point-to-site-how-to-vpn-client-install-azure-cert.md) (Instalowanie certyfikatu klienta).

## <a name="clientconfig"></a>9. Generowanie i instalowanie pakietu konfiguracji klienta sieci VPN

Pliki konfiguracji klienta sieci VPN zawierają ustawienia do konfigurowania urządzeń pod kątem łączenia się z siecią wirtualną przez połączenie punkt-lokacja. Aby uzyskać instrukcje dotyczące generowania i instalowania plików konfiguracji klienta sieci VPN, zobacz [Create and install VPN client configuration files for native Azure certificate authentication P2S configurations](point-to-site-vpn-client-configuration-azure-cert.md) (Tworzenie i instalowanie plików konfiguracji klienta sieci VPN dla konfiguracji połączeń punkt-lokacja z natywnym uwierzytelnianiem certyfikatów platformy Azure).

## <a name="connect"></a>10. Nawiązywanie połączenia z usługą Azure

### <a name="to-connect-from-a-windows-vpn-client"></a>Aby połączyć się z klienta sieci VPN w systemie Windows

1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Kliknij przycisk **Kontynuuj**, aby skorzystać z podwyższonego poziomu uprawnień.

2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.

  ![Łączenie klienta sieci VPN z platformą Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Połączenie zostało ustanowione.

  ![Ustanowiono połączenie](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshoot-windows-p2s-connections"></a>Rozwiązywanie problemów z połączeniami punkt-lokacja w systemie Windows

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Aby połączyć się z klienta sieci VPN na komputerze Mac

W oknie dialogowym Sieć znajdź profil klienta, którego chcesz użyć, a następnie kliknij polecenie **Połącz**.

  ![Połączenie z komputerem Mac](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Aby zweryfikować połączenie

Te instrukcje dotyczą klientów w systemie Windows.

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

Te instrukcje dotyczą klientów w systemie Windows.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Dodawanie lub usuwanie zaufanych certyfikatów głównych

Zaufane certyfikaty główne można dodawać do platformy Azure lub z niej usuwać. Po usunięciu certyfikatu głównego klienci, którzy mają certyfikat wygenerowany na podstawie tego certyfikatu głównego, nie będą w stanie się uwierzytelnić i w związku z tym nie będą mogli nawiązywać połączeń. Jeśli chcesz, aby klienci mogli uwierzytelniać się i nawiązywać połączenia, musisz zainstalować nowy certyfikat klienta wygenerowany na podstawie certyfikatu głównego, który jest traktowany jako zaufany przez platformę Azure (przekazany na tę platformę).

### <a name="to-add-a-trusted-root-certificate"></a>Aby dodać zaufany certyfikat główny

Na platformie Azure można dodać maksymalnie 20 plików cer zaufanego certyfikatu głównego. Aby uzyskać instrukcje, zobacz sekcję [Przekazywanie zaufanego certyfikatu głównego](#uploadfile) w tym artykule.

### <a name="to-remove-a-trusted-root-certificate"></a>Usuwanie zaufanego certyfikatu głównego

1. Aby usunąć zaufany certyfikat główny, przejdź do strony **Konfiguracja punktu do lokacji** dla bramy Twojej sieci wirtualnej.
2. W sekcji **Certyfikat główny** na stronie znajdź certyfikat, który chcesz usunąć.
3. Kliknij wielokropek obok certyfikatu, a następnie kliknij pozycję „Usuń”.

## <a name="revokeclient"></a> Aby odwołać certyfikat klienta

Certyfikaty klienta można odwołać. Lista odwołania certyfikatów umożliwia dokonanie selektywnej odmowy połączenia punkt-lokacja w oparciu o indywidualne certyfikaty klienta. Różni się to od usuwania zaufanego certyfikatu głównego. Jeśli usuniesz plik cer zaufanego certyfikatu głównego z platformy Azure, spowoduje to odwołanie dostępu dla wszystkich certyfikatów klienta wygenerowanych lub podpisanych przez odwołany certyfikat główny. Odwołanie certyfikatu klienta zamiast certyfikatu głównego pozwala dalej używać innych certyfikatów wygenerowanych na podstawie certyfikatu głównego do uwierzytelniania.

Częstą praktyką jest użycie certyfikatu głównego do zarządzania dostępem na poziomach zespołu lub organizacji przy jednoczesnym korzystaniu z odwołanych certyfikatów klienta dla bardziej precyzyjnej kontroli dostępu poszczególnych użytkowników.

### <a name="revoke-a-client-certificate"></a>Odwołanie certyfikatu klienta

Certyfikat klienta można odwołać przez dodanie odcisku palca do listy odwołania.

1. Pobierz odcisk palca certyfikatu klienta. Aby uzyskać więcej informacji, zobacz [Jak pobrać odcisk palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Skopiuj informacje do edytora tekstu i usuń wszelkie spacje, tak aby powstał ciąg bez odstępów.
3. Przejdź do strony **Konfiguracja punktu do lokacji** bramy sieci wirtualnej. Korzystając z tej samej strony, [przekazano zaufany certyfikat główny](#uploadfile).
4. W sekcji **Odwołane certyfikaty** wprowadź przyjazną nazwę certyfikatu (nie musi to być nazwa pospolita certyfikatu).
5. Skopiuj i wklej ciąg odcisku palca do pola **Odcisk palca**.
6. Odcisk palca zostanie zweryfikowany i automatycznie dodany do listy odwołania. Na ekranie zobaczysz komunikat informujący o aktualizowaniu listy. 
7. Po zakończeniu aktualizowania nie będzie można już używać certyfikatu do nawiązywania połączeń. Klienci, którzy spróbują połączyć się za pomocą tego certyfikatu, zobaczą komunikat informujący o tym, że certyfikat nie jest już ważny.

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne). Aby dowiedzieć się więcej o sieci i maszynach wirtualnych, zobacz [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md) (Omówienie sieci maszyny wirtualnej z systemem Linux i platformy Azure).
