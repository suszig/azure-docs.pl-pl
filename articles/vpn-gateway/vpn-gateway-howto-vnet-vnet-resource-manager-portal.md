---
title: "Łączenie sieci wirtualnej platformy Azure z inną siecią wirtualną: Portal | Microsoft Docs"
description: "Utwórz połączenie usługi bramy VPN Gateway między sieciami wirtualnymi przy użyciu usługi Resource Manager i witryny Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: cbbae06d4e5774d5fdf7bcce65dc7efddaa5f280
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Konfigurowanie połączenia bramy sieci VPN między sieciami wirtualnymi przy użyciu witryny Azure Portal

Ten artykuł pokazuje, jak utworzyć połączenie bramy sieci VPN między sieciami wirtualnymi. Sieci wirtualne mogą być zlokalizowane w tych samych lub różnych regionach i mogą funkcjonować w ramach tej samej lub różnych subskrypcji. W przypadku łączenia sieci wirtualnych z różnych subskrypcji subskrypcje nie muszą być skojarzone z tą samą dzierżawą usługi Active Directory. 

Kroki podane w tym artykule mają zastosowanie do modelu wdrażania przy użyciu usługi Resource Manager i użyto w nich witryny Azure Portal. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Portal Azure (klasyczny)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Łączenie różnych modeli wdrażania — witryna Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Łączenie różnych modeli wdrażania — program PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Diagram połączenia między sieciami wirtualnymi (v2v)](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Proces nawiązywania połączenia między dwiema sieciami wirtualnymi przebiega podobnie do procesu łączenia sieci wirtualnej z lokacją lokalną. Oba typy połączeń wykorzystują bramę sieci VPN, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE. Jeśli Twoje sieci wirtualne znajdują się w tym samym regionie, warto rozważyć połączenie ich za pomocą komunikacji równorzędnej sieci wirtualnych. W przypadku komunikacji równorzędnej sieci wirtualnych nie jest używana brama sieci VPN. Aby uzyskać więcej informacji, zobacz temat [Komunikacja równorzędna sieci wirtualnych](../virtual-network/virtual-network-peering-overview.md).

Komunikację między sieciami wirtualnymi można łączyć z konfiguracjami obejmującymi wiele lokacji. Pozwala to tworzyć topologie sieci, które łączą wdrożenia obejmujące wiele lokalizacji z połączeniami między sieciami wirtualnymi, jak pokazano na poniższym diagramie.

![Informacje o połączeniach](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informacje o połączeniach")

### <a name="why-connect-virtual-networks"></a>Dlaczego łączy się sieci wirtualne?

Sieci wirtualne można łączyć z następujących powodów:

* **Niezależna od regionu nadmiarowość i obecność geograficzna**
  
  * Można tworzyć własne replikacje geograficzne lub przeprowadzać synchronizację z bezpiecznym połączeniem bez przechodzenia przez punkty końcowe dostępne z Internetu.
  * Usługi Azure Traffic Manager i Load Balancer pozwalają skonfigurować obciążenie o wysokiej dostępności z nadmiarowością geograficzną w wielu regionach świadczenia usługi Azure. Istotnym przykładem jest konfiguracja ustawienia SQL Zawsze włączone, w przypadku którego grupy dostępności rozciągają się na wiele regionów świadczenia usługi Azure.
* **Regionalne aplikacje wielowarstwowe z izolacją lub granicami administracyjnymi**
  
  * W ramach jednego regionu można skonfigurować aplikacje wielowarstwowe z wielu połączonych ze sobą sieci wirtualnych, korzystając z izolacji lub wymagań administracyjnych.

Więcej informacji na temat połączeń między sieciami wirtualnymi znajduje się w sekcji [Często zadawane pytania dotyczące połączeń między sieciami wirtualnymi](#faq) na końcu tego artykułu. Pamiętaj, że jeśli Twoje sieci wirtualne należą do różnych subskrypcji, to nie można utworzyć połączenia w portalu. Można użyć programu [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) lub [interfejsu wiersza polecenia](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Przykładowe ustawienia

Korzystając z tych kroków w charakterze ćwiczenia, można użyć przykładowych wartości ustawień. W celach demonstracyjnych dla poszczególnych sieci wirtualnych używamy wielu przestrzeni adresowych. Konfiguracje połączeń między sieciami wirtualnymi nie wymagają jednak wielu przestrzeni adresowych.

**Wartości dla sieci TestVNet1:**

* Nazwa sieci wirtualnej: TestVNet1
* Przestrzeń adresowa: 10.11.0.0/16
  * Nazwa podsieci: FrontEnd
  * Zakres adresów podsieci: 10.11.0.0/24
* Grupa zasobów: TestRG1
* Lokalizacja: Wschodnie stany USA
* Przestrzeń adresowa: 10.12.0.0/16
  * Nazwa podsieci: BackEnd
  * Zakres adresów podsieci: 10.12.0.0/24
* Nazwa podsieci bramy: GatewaySubnet (zostanie automatycznie wypełniona w portalu)
  * Zakres adresów podsieci bramy: 10.11.255.0/27
* Serwer DNS: użyj adresu IP serwera DNS.
* Nazwa bramy sieci wirtualnej: TestVNet1GW
* Typ bramy: VPN
* Typ sieci VPN: oparta na trasach
* Jednostka SKU: wybierz jednostkę SKU bramy, która ma być używana.
* Publiczna nazwa adresu IP: TestVNet1GWIP
* Wartości połączenia:
  * Nazwa: TestVNet1toTestVNet4
  * Klucz współużytkowany: klucz współużytkowany można utworzyć samodzielnie. W tym przykładzie użyjemy wartości abc123. Ważne jest, aby podczas tworzenia połączenia między sieciami wirtualnymi, wartości były zgodne.

**Wartości dla sieci TestVNet4:**

* Nazwa sieci wirtualnej: TestVNet4
* Przestrzeń adresowa: 10.41.0.0/16
  * Nazwa podsieci: FrontEnd
  * Zakres adresów podsieci: 10.41.0.0/24
* Grupa zasobów: TestRG1
* Lokalizacja: West US
* Przestrzeń adresowa: 10.42.0.0/16
  * Nazwa podsieci: BackEnd
  * Zakres adresów podsieci: 10.42.0.0/24
* Nazwa podsieci bramy: GatewaySubnet (zostanie automatycznie wypełniona w portalu)
  * Zakres adresów podsieci GatewaySubnet: 10.41.255.0/27
* Serwer DNS: użyj adresu IP serwera DNS.
* Nazwa bramy sieci wirtualnej: TestVNet4GW
* Typ bramy: VPN
* Typ sieci VPN: oparta na trasach
* Jednostka SKU: wybierz jednostkę SKU bramy, która ma być używana.
* Nazwa publicznego adresu IP: TestVNet4GWIP
* Wartości połączenia:
  * Nazwa: TestVNet4toTestVNet1
  * Klucz współużytkowany: klucz współużytkowany można utworzyć samodzielnie. W tym przykładzie użyjemy wartości abc123. Ważne jest, aby podczas tworzenia połączenia między sieciami wirtualnymi, wartości były zgodne.

## <a name="CreatVNet"></a>1. Tworzenie i konfigurowanie sieci TestVNet1
Jeśli masz już sieć wirtualną, sprawdź, czy ustawienia są zgodne z projektem bramy sieci VPN. Zwróć szczególną uwagę na wszelkie podsieci, które mogą pokrywać się z innymi sieciami. Obecność nakładających się podsieci spowoduje, że połączenie nie będzie działać prawidłowo. Jeśli sieć wirtualną skonfigurowano poprawnie, można rozpocząć wykonywanie kroków z sekcji [Określanie serwera DNS](#dns).

### <a name="to-create-a-virtual-network"></a>Aby utworzyć sieć wirtualną
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Dodawanie dodatkowej przestrzeni adresowej i tworzenie podsieci
Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz utworzyć podsieci.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Tworzenie podsieci bramy
Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Jeśli to możliwe, najlepiej utworzyć podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj [przykładowych ustawień](#values) podczas tworzenia podsieci bramy.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Określanie serwera DNS (opcjonalne)
Serwer DNS nie jest wymagany w przypadku połączenia typu sieć wirtualna-sieć wirtualna. Jeśli jednak chcesz korzystać z funkcji rozpoznawania nazw dla zasobów, które zostały wdrożone w Twojej sieci wirtualnej, określ serwer DNS. To ustawienie umożliwia określenie serwera DNS, który ma być używany do rozpoznawania nazw dla tej sieci wirtualnej. Nie powoduje ono jednak utworzenia serwera DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Tworzenie bramy sieci wirtualnej
W tym kroku zostaje utworzona brama dla sieci wirtualnej użytkownika. Tworzenie bramy często może trwać 45 minut lub dłużej, w zależności od wybranej jednostki SKU bramy. W przypadku tworzenia tej konfiguracji w ramach ćwiczenia można korzystać z [przykładowych ustawień](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę sieci wirtualnej
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Tworzenie i konfigurowanie sieci TestVNet4
Po skonfigurowaniu sieci TestVNet1 utwórz sieć TestVNet4, powtarzając poprzednie kroki, używając wartości dla sieci TestVNet4. Przed skonfigurowaniem sieci TestVNet4 nie musisz czekać, aż tworzenie bramy sieci wirtualnej dla sieci TestVNet1 zostanie zakończone. Jeśli używasz własnych wartości, upewnij się, że przestrzenie adresowe nie pokrywają się z żadnymi sieciami wirtualnymi, z którymi chcesz się połączyć.

## <a name="TestVNet1Connection"></a>7. Konfigurowanie połączenia TestVNet1
Po zakończeniu tworzenia bram sieci wirtualnej (zarówno dla sieci TestVNet1, jak i TestVNet4) można utworzyć połączenia między bramami sieci wirtualnych. W tej sekcji opisano tworzenie połączenia z sieci VNet1 do sieci VNet4. Następujące kroki działają tylko w przypadku sieci wirtualnych należących do tej samej subskrypcji. Jeśli Twoje sieci wirtualne znajdują się w różnych subskrypcjach, do utworzenia połączenia musisz użyć programu PowerShell. Zobacz artykuł [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md).

1. W sekcji **Wszystkie zasoby** przejdź do bramy swojej sieci wirtualnej (np. **TestVNet1GW**). Kliknij pozycję **TestVNet1GW**, aby otworzyć blok bramy sieci wirtualnej.
   
    ![Blok połączeń](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Blok połączeń")
2. Kliknij przycisk **+Dodaj**, aby otworzyć blok **Dodaj połączenie**.
3. W bloku **Dodaj połączenie** wpisz nazwę połączenia w polu nazwy (np. **TestVNet1toTestVNet4**).
   
    ![Nazwa połączenia](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Nazwa połączenia")
4. Dla opcji **Typ połączenia** wybierz z listy rozwijanej pozycję **Sieć wirtualna-sieć wirtualna**.
5. Wartość pola **Pierwsza brama sieci wirtualnej** zostaje podana automatycznie, ponieważ połączenie jest tworzone z określonej bramy sieci wirtualnej.
6. Pole **Druga brama sieci wirtualnej** jest bramą sieci wirtualnej sieci wirtualnej, z którą chcesz utworzyć połączenie. Kliknij pozycję **Wybierz inną bramę sieci wirtualnej**, aby otworzyć blok **Wybieranie bramy sieci wirtualnej**.
   
    ![Dodawanie połączenia](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Dodawanie połączenia")
7. Wyświetl bramy sieci wirtualnej wymienione w tym bloku. Pamiętaj, że wyświetlane są tylko bramy sieci wirtualnej objęte subskrypcją. Jeśli chcesz połączyć się z bramą sieci wirtualnej, której nie ma w Twojej subskrypcji, zapoznaj się z [artykułem dotyczącym programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Kliknij bramę sieci wirtualnej, z którą chcesz nawiązać połączenie.
9. W polu **Klucz współużytkowany** wpisz klucz, który będzie używany dla tego połączenia. Klucz można wygenerować lub utworzyć samodzielnie. W przypadku połączenia lokacja-lokacja używany klucz jest dokładnie taki sam dla urządzenia lokalnego oraz połączenia bramy sieci wirtualnej. Koncepcja jest tutaj podobna, z tym że zamiast połączenia z urządzeniem sieci VPN następuje połączenie z inną bramą sieci wirtualnej.
   
    ![Klucz współużytkowany](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Klucz współużytkowany")
10. Kliknij przycisk **OK** na dole bloku, aby zapisać zmiany.

## <a name="TestVNet4Connection"></a>8. Konfigurowanie połączenia z siecią TestVNet4
Następnie utwórz połączenie z sieci TestVNet4 do sieci TestVNet1. Zastosuj tę samą metodę, która została użyta do utworzenia połączenia z sieci TestVNet1 do sieci TestVNet4. Pamiętaj, aby użyć tego samego klucza współużytkowanego.

## <a name="VerifyConnection"></a>9. Weryfikowanie połączenia
Zweryfikuj połączenie. Dla każdej bramy sieci wirtualnej wykonaj następujące czynności:

1. Znajdź blok bramy sieci wirtualnej (np. **TestVNet4GW**). 
2. W bloku bramy sieci wirtualnej kliknij opcję **Połączenia**, aby wyświetlić blok połączeń bramy sieci wirtualnej.

Wyświetl połączenia i zweryfikuj stan. Po utworzeniu połączenia zostanie wyświetlony stan z wartościami **Powodzenie** i **Połączono**.

![Powodzenie](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Powodzenie")

Możesz kliknąć dwukrotnie każde połączenie, aby wyświetlić więcej informacji.

![Podstawy](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Podstawy")

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń między sieciami wirtualnymi
Ta sekcja zawiera odpowiedzi na często zadawane pytań dotyczące połączeń między sieciami wirtualnymi.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Zobacz [dokumentację dotyczącą maszyn wirtualnych](https://docs.microsoft.com/azure/#pivot=services&panel=Compute), aby uzyskać więcej informacji.
