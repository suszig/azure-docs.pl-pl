---
title: "Tworzenie sieci wirtualnej z połączeniem bramy sieci VPN typu lokacja-lokacja przy użyciu klasycznej witryny Azure Portal | Microsoft Docs"
description: "Tworzenie sieci wirtualnej z połączeniem bramy sieci VPN S2S dla konfiguracji hybrydowych i obejmujących wiele lokalizacji przy użyciu klasycznego modelu wdrażania."
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: d269d9a76ff4ccd973eee70d2d5b54a7262383ef
ms.openlocfilehash: b45801d4e6deb25ca8c6af5c3b46de741d4993e0


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Tworzenie sieci wirtualnej za pomocą połączenia typu lokacja-lokacja przy użyciu klasycznej witryny Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Model klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
> 

W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia bramy sieci VPN typu lokacja-lokacja z siecią lokalną z wykorzystaniem klasycznego modelu wdrażania i portalu klasycznego. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.

![Diagram połączenia typu lokacja-lokacja](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Metody i modele wdrażania używane w kontekście połączeń typu lokacja-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono metody i modele wdrażania aktualnie dostępne dla konfiguracji lokacja-lokacja. Jeśli jest dostępny artykuł z krokami konfiguracji, można przejść do niego bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowe konfiguracje
Jeśli chcesz połączyć ze sobą sieci wirtualne, zobacz temat [Configure a VNet-to-VNet connection for the classic deployment model](virtual-networks-configure-vnet-to-vnet-connection.md) (Konfiguracja połączenia między sieciami wirtualnymi dla klasycznego modelu wdrażania). Jeśli chcesz dodać połączenie lokacja-lokacja do sieci wirtualnej, z którą istnieje już połączenie, zobacz [Add a S2S connection to a VNet with an existing VPN gateway connection](vpn-gateway-multi-site.md) (Dodawanie połączenia lokacja-lokacja do sieci wirtualnej, z którą istnieje połączenie bramy sieci VPN).

## <a name="before-you-begin"></a>Przed rozpoczęciem
Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane.
* Dostępny zewnętrznie publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).

## <a name="a-namecreatevnetacreate-your-virtual-network"></a><a name="CreateVNet"></a>Tworzenie sieci wirtualnej
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com/).
2. W lewym dolnym rogu ekranu kliknij opcję **Nowe**. W okienku nawigacji kliknij opcję **Usługi sieciowe**, a następnie opcję **Sieć wirtualna**. Kliknij przycisk **Tworzenie niestandardowe**, aby rozpocząć korzystanie z kreatora konfiguracji.
3. Aby utworzyć sieć wirtualną, wprowadź ustawienia konfiguracji na następujących stronach:

## <a name="a-namedetailsavirtual-network-details-page"></a><a name="Details"></a>Strona szczegółów sieci wirtualnej
Wprowadź następujące informacje:

* **Nazwa**: nadaj nazwę sieci wirtualnej. Na przykład: *EastUSVNet*. Ta nazwa sieci wirtualnej będzie używana podczas wdrażania wystąpień maszyn wirtualnych i usług PaaS, dlatego nie powinna być zbyt skomplikowana.
* **Lokalizacja**: omawiana lokalizacja jest bezpośrednio związana z lokalizacją fizyczną (regionem), w której mają się znajdować zasoby (maszyny wirtualne). Jeśli na przykład maszyny wirtualne wdrażane w ramach tej sieci wirtualnej mają znajdować się we wschodnim regionie USA, należy wybrać tę właśnie lokalizację (*Wschodnie stany USA*). Po utworzeniu sieci wirtualnej nie można zmienić powiązanego z nią regionu.

## <a name="a-namednsadns-servers-and-vpn-connectivity-page"></a><a name="DNS"></a>Strona serwerów DNS i łączności VPN
Wprowadź następujące informacje, a następnie kliknij strzałkę widoczną w prawym dolnym rogu, aby przejść dalej.

* **Serwery DNS**: wprowadź nazwę serwera DNS i jego adres IP lub wybierz zarejestrowany wcześniej serwer DNS z menu skrótów. To ustawienie nie powoduje jednak utworzenia serwera DNS. Umożliwia natomiast określenie serwerów DNS, które mają być używane do rozpoznawania nazw dla tej sieci wirtualnej.
* **Konfiguracja sieci VPN typu lokacja-lokacja**: należy zaznaczyć pole wyboru **Konfiguracja sieci VPN typu lokacja-lokacja**.
* **Sieć lokalna**: wartość w tym polu określa fizyczną lokalizację sieci lokalnej. Możesz wybrać wcześniej utworzoną sieć lokalną lub utworzyć nową. W przypadku wybrania utworzonej wcześniej sieci lokalnej należy przejść na stronę konfiguracji **Sieci lokalne** i upewnić się, że adres IP urządzenia sieci VPN (publiczny adres IPv4) dla używanego urządzenia sieci VPN jest prawidłowo wprowadzony.

## <a name="a-nameconnectivityasite-to-site-connectivity-page"></a><a name="Connectivity"></a>Strona łączności lokacja-lokacja
W toku procedury tworzenia nowej sieci lokalnej zostanie wyświetlona strona **Łączność typu „lokacja do lokacji”** . W przypadku wyboru opcji użycia wcześniej utworzonej sieci lokalnej ta strona nie zostanie wyświetlona w ramach kreatora i będzie można przejść bezpośrednio do następnej sekcji.

Wprowadź następujące informacje, a następnie kliknij strzałkę, aby przejść dalej.

* **Nazwa**: nazwa, jaką chcesz nadać lokalizacji w sieci lokalnej.
* **Adres IP urządzenia sieci VPN**: jest to publiczny adres IPv4 urządzenia lokalnej sieci VPN, który jest używany do łączenia z platformą Azure. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.
* **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz wartość CIDR (liczbę adresów). W tym obszarze należy określić zakresy adresów, które mają zostać wysłane za pośrednictwem bramy sieci wirtualnej do lokalizacji w ramach sieci lokalnej. Jeśli docelowy adres IP mieści się w zakresach określonych w tym obszarze, zostanie skierowany przez bramę sieci wirtualnej.
* **Dodaj przestrzeń adresową**: chcąc wysyłać za pośrednictwem bramy sieci wirtualnej wiele zakresów adresów, należy określić w tym obszarze wszystkie dodatkowe zakresy. Zakresy można dodawać i usuwać w późniejszym czasie na stronie **Sieć lokalna**.

## <a name="a-nameaddressavirtual-network-address-spaces-page"></a><a name="Address"></a>Strona przestrzeni adresowych sieci wirtualnej
Określ zakres adresów, który ma zostać użyty dla sieci wirtualnej. Są to dynamiczne adresy IP (adresy DIP), które zostaną przypisane maszynom wirtualnym i innym wystąpieniom roli wdrażanym w ramach tej sieci wirtualnej.

Szczególnie istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. Skontaktuj się z administratorem sieci. Może się okazać niezbędne wydzielenie przez administratora sieci z przestrzeni adresowej sieci lokalnej zakresu adresów IP do użycia dla sieci wirtualnej.

Wprowadź następujące informacje, a następnie kliknij znacznik wyboru widoczny w prawym dolnym rogu, aby skonfigurować sieć.

* **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz liczbę adresów. Sprawdź, czy określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej.
* **Dodaj podsieć**: należy uwzględnić początkowy adres IP oraz liczbę adresów. Dodatkowe podsieci nie są wymagane, można jednak utworzyć osobne podsieci dla maszyn wirtualnych, które będą miały statyczne adresy DIP. Maszyny wirtualne mogą także tworzyć podsieci niezależne od innych wystąpień roli.
* **Dodaj podsieć bramy**: kliknij, aby dodać podsieć bramy. Podsieć bramy jest używana tylko dla bramy sieci wirtualnej i jest wymagana dla tej konfiguracji.

Kliknij znacznik wyboru na dole strony. Spowoduje to rozpoczęcie procesu tworzenia sieci wirtualnej. Po jego zakończeniu w obszarze **Stan** na stronie **Sieci** w klasycznym portalu Azure pojawi się wartość **Utworzona**. Po utworzeniu sieci wirtualnej można skonfigurować jej bramę.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="a-namevnetgatewayaconfigure-your-virtual-network-gateway"></a><a name="VNetGateway"></a>Konfigurowanie bramy sieci wirtualnej
Następnie konieczne będzie skonfigurowanie bramy sieci wirtualnej w celu utworzenia bezpiecznego połączenia typu lokacja-lokacja. Zobacz artykuł [Configure a virtual network gateway in the Azure classic portal](vpn-gateway-configure-vpn-gateway-mp.md) (Konfigurowanie bramy sieci wirtualnej w klasycznym portalu Azure).

## <a name="next-steps"></a>Następne kroki
 Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).




<!--HONumber=Nov16_HO4-->


