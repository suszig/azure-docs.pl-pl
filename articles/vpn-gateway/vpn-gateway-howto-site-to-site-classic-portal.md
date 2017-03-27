---
title: "Łączenie sieci lokalnej z siecią wirtualną platformy Azure: sieć VPN typu lokacja-lokacja (model klasyczny): portal | Microsoft Docs"
description: "Kroki tworzenia połączenia IPsec z sieci lokalnej do sieci wirtualnej platformy Azure za pośrednictwem publicznego Internetu. Kroki te są pomocne podczas tworzenia obejmującego wiele lokalizacji połączenia bramy sieci VPN typu lokacja-lokacja za pomocą portalu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 061226c76128122b61d2fe135fdfae0bfd7490d7
ms.lasthandoff: 03/17/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal-classic"></a>Tworzenie sieci wirtualnej za pomocą połączenia typu lokacja-lokacja przy użyciu witryny Azure Portal (model klasyczny)
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Model klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
>
>

W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia bramy sieci VPN typu lokacja-lokacja z siecią lokalną z użyciem klasycznego modelu wdrażania i witryny Azure Portal. 

Połączenie bramy sieci VPN typu lokacja-lokacja to połączenie nawiązywane za pośrednictwem tunelu sieci VPN wykorzystującego protokół IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga, aby urządzenie sieci VPN znajdowało się lokalnie, miało przypisany publiczny adres IP i nie znajdowało się za translatorem adresów sieciowych. Połączeń typu lokacja-lokacja możesz używać w konfiguracjach z wieloma lokalizacjami i konfiguracjach hybrydowych albo w celu łączenia sieci wirtualnych korzystających z różnych modeli wdrażania.

![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Metody i modele wdrażania używane w kontekście połączeń typu lokacja-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono metody i modele wdrażania aktualnie dostępne dla konfiguracji lokacja-lokacja. Jeśli jest dostępny artykuł z krokami konfiguracji, można przejść do niego bezpośrednio z tabeli.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowe konfiguracje
Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykułem [Configure a VNet-to-VNet connection](virtual-networks-configure-vnet-to-vnet-connection.md) (Konfigurowanie połączenia między sieciami wirtualnymi). Jeśli chcesz dodać połączenie lokacja-lokacja do sieci wirtualnej, z którą istnieje już połączenie, zobacz [Add a S2S connection to a VNet with an existing VPN gateway connection](vpn-gateway-multi-site.md) (Dodawanie połączenia lokacja-lokacja do sieci wirtualnej, z którą istnieje połączenie bramy sieci VPN).

## <a name="before-you-begin"></a>Przed rozpoczęciem
Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane.
* Dostępny zewnętrznie publiczny adres IPv4 urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial).
* Obecnie w celu określenia klucza wspólnego i utworzenia połączenia bramy sieci VPN jest wymagany program PowerShell. Zainstaluj najnowszą wersję poleceń cmdlet programu PowerShell usługi Azure Service Management (SM). Aby uzyskać więcej informacji, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs). Podczas pracy z programem PowerShell dla tej konfiguracji upewnij się, że działasz jako administrator. 

> [!NOTE]
> Podczas konfigurowania połączenia typu lokacja-lokacja wymagane jest użycie publicznego adresu IPv4 dla urządzenia sieci VPN.                                                                                                                                                                               
>
>

### <a name="values"></a>Przykładowe wartości konfiguracji dla tego ćwiczenia
Korzystając z tych kroków w charakterze ćwiczenia, można użyć przykładowych wartości konfiguracji:

* **Nazwa sieci wirtualnej:** TestVNet1
* **Przestrzeń adresowa:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (opcjonalnie na potrzeby tego ćwiczenia)
* **Podsieci:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (opcjonalnie na potrzeby tego ćwiczenia)
* **GatewaySubnet:** 10.11.255.0/27
* **Grupa zasobów:** TestRG1
* **Lokalizacja:** Wschodnie stany USA
* **Serwer DNS:** 8.8.8.8 (opcjonalnie na potrzeby tego ćwiczenia)
* **Nazwa lokacji lokalnej:** Lokacja2

## <a name="CreatVNet"></a>1. Tworzenie sieci wirtualnej

Podczas tworzenia sieci wirtualnej na potrzeby połączenia typu lokacja-lokacja upewnij się, że określone przestrzenie adresowe nie nakładają się na żadne przestrzenie adresowe klientów dla lokacji lokalnych, z którymi chcesz nawiązywać połączenia. Obecność nakładających się podsieci spowoduje, że połączenie nie będzie działać prawidłowo.

* Jeśli masz już sieć wirtualną, sprawdź, czy ustawienia są zgodne z projektem bramy sieci VPN. Zwróć szczególną uwagę na wszelkie podsieci, które mogą pokrywać się z innymi sieciami. 

* Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi.

### <a name="to-create-a-virtual-network"></a>Aby utworzyć sieć wirtualną

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

    ![Wyszukiwanie bloku Sieć wirtualna](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Klasyczny**, a następnie kliknij przycisk **Utwórz**.

    ![Wybieranie modelu wdrożenia](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku dodaj pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Tworzenie bloku sieci wirtualnej")
5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.
8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.

    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "Przypnij do pulpitu nawigacyjnego")
9. Po kliknięciu przycisku „Utwórz” na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek Tworzenie sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Tworzenie sieci wirtualnej")

Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal pojawi się wartość **Utworzona**.

## <a name="additionaladdress"></a>2. Dodawanie kolejnej przestrzeni adresowej

Po utworzeniu sieci wirtualnej możesz dodać kolejną przestrzeń adresową. Dodanie kolejnej przestrzeni adresowej nie jest wymagane w przypadku konfiguracji typu lokacja-lokacja, ale jeśli potrzebujesz wielu przestrzeni adresowych, wykonaj następujące kroki:

1. Znajdź sieci wirtualne w portalu.
2. W bloku Twojej sieci wirtualnej, w sekcji **Ustawienia** kliknij pozycję **Przestrzeń adresowa**.
3. W bloku Przestrzeń adresowa kliknij pozycję **+Dodaj** i wprowadź dodatkową przestrzeń adresową.
 
## <a name="dns"></a>3. Określanie serwera DNS
Ustawienia DNS nie są wymagane w przypadku konfiguracji typu lokacja-lokacja, ale serwer DNS jest konieczny, aby korzystać z rozpoznawania nazw.

Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Otwórz ustawienia dla sieci wirtualnej, kliknij pozycję Serwery DNS i dodaj adres IP serwera DNS, który ma być używany do rozpoznawania nazw. To ustawienie nie powoduje jednak utworzenia serwera DNS. W ustawieniach przykładowych używamy publicznego serwera DNS. Zazwyczaj zalecane jest użycie prywatnego serwera DNS. Należy pamiętać o dodaniu serwera DNS, z którym zasoby mogą się komunikować.

1. Znajdź sieci wirtualne w portalu.
2. W bloku Twojej sieci wirtualnej, w sekcji **Ustawienia** kliknij pozycję **Serwery DNS**.
3. Dodaj serwer DNS.
4. Kliknij przycisk **Zapisz** znajdujący się u góry strony, aby zapisać ustawienia.
 
## <a name="localsite"></a>4. Konfigurowanie lokacji lokalnej

Lokacja lokalna zazwyczaj oznacza lokalizację lokalną. Zawiera ona adres IP urządzenia sieci VPN, z którym będzie tworzone połączenie, oraz zakresy adresów IP, które będą kierowane za pośrednictwem bramy sieci VPN do tego urządzenia sieci VPN.

1. W witrynie portalu przejdź do sieci wirtualnej, dla której chcesz utworzyć bramę.
2. W bloku **Przegląd** Twojej sieci wirtualnej, w sekcji Połączenia VPN kliknij pozycję **Brama**, aby otworzyć blok **Nowe połączenie VPN**.

    ![Kliknij, aby skonfigurować ustawienia bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Kliknij, aby skonfigurować ustawienia bramy")
3. W bloku **Nowe połączenie VPN** wybierz pozycję **Lokacja-lokacja**.

    ![Kliknij pozycję Lokacja-lokacja](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site.png "Lokacja-lokacja")
4. Kliknij pozycję **Lokacja lokalna — Skonfiguruj wymagane ustawienia**, aby otworzyć blok **Lokacja lokalna**. Skonfiguruj ustawienia, a następnie kliknij przycisk **OK**, aby je zapisać.
    - **Nazwa:** Utwórz nazwę lokacji lokalnej, aby ułatwić jej identyfikację.
    - **Adres IP bramy sieci VPN:** Publiczny adres IP urządzenia sieci VPN w sieci lokalnej. Urządzenie sieci VPN wymaga publicznego adresu IPv4. Określ prawidłowy publiczny adres IP dla urządzenia sieci VPN, z którym chcesz się połączyć. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.
    - **Przestrzeń adresowa klienta:** Podaj listę zakresów adresów IP, które mają być kierowane do sieci lokalnej za pośrednictwem tej bramy. Można dodać wiele zakresów przestrzeni adresów. Upewnij się, że określone w tym miejscu zakresy nie pokrywają się z zakresami innych sieci, z którymi łączy się Twoja sieć wirtualna, ani z zakresami adresów samej sieci wirtualnej.

    ![Lokacja lokalna](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurowanie lokacji lokalnej")

## <a name="gatewaysubnet"></a>5. Konfigurowanie podsieci bramy

Musisz utworzyć podsieć dla bramy sieci VPN. Podsieć bramy zawiera adresy IP, z których będą korzystać usługi bramy sieci VPN.

1. W bloku **Nowe połączenie VPN** zaznacz pole wyboru **Utwórz bramę natychmiast**. Zostanie wyświetlony blok „Opcjonalna konfiguracja bramy”. Jeśli nie zaznaczysz pola wyboru, blok umożliwiający skonfigurowanie podsieci bramy nie zostanie wyświetlony.

    ![Konfiguracja bramy — Podsieć, rozmiar, typ routingu](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Konfiguracja bramy — Podsieć, rozmiar, typ routingu")
2. Kliknij pozycję **Opcjonalna konfiguracja bramy — Podsieć, rozmiar i typ routingu**, aby otworzyć blok **Konfiguracja bramy**.
3. W bloku **Konfiguracja bramy** kliknij pozycję **Podsieć — Skonfiguruj wymagane ustawienia**, aby otworzyć blok **Dodawanie podsieci**.

    ![Konfiguracja bramy — Podsieć bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Konfiguracja bramy — Podsieć bramy")
4. W bloku **Dodawanie podsieci** dodaj podsieć bramy. Jeśli to możliwe, podczas dodawania podsieci bramy najlepiej utworzyć ją przy użyciu bloku CIDR /28 lub /27. Daje to pewność, że nie braknie adresów IP w celu spełnienia dodatkowych wymagań konfiguracji w przyszłości.  Kliknij pozycję **OK**, aby zapisać ustawienia.

    ![Dodawanie podsieci bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Dodawanie podsieci bramy")

## <a name="sku"></a>6. Określ typ jednostki SKU i sieci VPN
1. Wybierz **rozmiar** bramy. To jest jednostka SKU bramy, która będzie używana do tworzenia bramy sieci wirtualnej. W portalu domyślna jednostka SKU to **Podstawowa**. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Informacje o ustawieniach bramy VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Wybieranie typu jednostki SKU i sieci VPN](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Wybieranie typu jednostki SKU i sieci VPN")
2. Wybierz **Typ routingu** dla bramy. Jest on również nazywany typem sieci VPN. Wybranie poprawnego typu bramy jest bardzo ważne, ponieważ nie można konwertować bramy na inny typ. Urządzenie sieci VPN musi być zgodne z wybranym typem routingu. Więcej informacji o typie sieci VPN można znaleźć w artykule [Informacje na temat ustawień usługi VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype). W niektórych artykułach mogą znajdować się odwołania do typów sieci VPN „RouteBased” i „PolicyBased”. Typ „Dynamiczny” odpowiada typowi „RouteBased”, a „Statyczny” — typowi „PolicyBased”.
3. Kliknij pozycję **OK**, aby zapisać ustawienia.
4. W bloku **Nowe połączenie VPN** kliknij przycisk **OK** u dołu bloku, aby rozpocząć tworzenie bramy sieci wirtualnej. Może to potrwać do 45 minut.

## <a name="vpndevice"></a>7. Konfiguracja urządzenia sieci VPN

Uzyskaj określone informacje dotyczące konfiguracji od producenta urządzenia i skonfiguruj urządzenie. Zapoznaj się z tematem [Urządzenia sieci VPN](vpn-gateway-about-vpn-devices.md), aby uzyskać więcej informacji o urządzeniach sieci VPN, które dobrze współdziałają z platformą Azure. Sprawdź również [Znane problemy dotyczące zgodności urządzeń](vpn-gateway-about-vpn-devices.md#known) dla urządzenia sieci VPN, które ma być używane. 

Podczas konfigurowania urządzenia sieci VPN jest potrzebny adres IP utworzonej bramy sieci VPN. Aby go sprawdzić, przejdź do bloku **Przegląd** sieci wirtualnej.

## <a name="CreateConnection"></a>8. Tworzenie połączenia
W tym kroku należy ustawić klucz wspólny i utworzyć połączenie. Ustawiony klucz musi być tym samym kluczem, którego użyto do skonfigurowania urządzenia sieci VPN.

> [!NOTE]
> Obecnie tego kroku nie można wykonać w witrynie Azure Portal. Musisz użyć wersji poleceń cmdlet programu Azure PowerShell pochodzącej z usługi Service Management (SM).                                                                                                                                                                             
>
>

### <a name="step-1-connect-to-your-azure-account"></a>Krok 1. Nawiąż połączenie z kontem platformy Azure

1. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

        Login-AzureRmAccount
2. Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription
3. Jeśli masz więcej niż jedną subskrypcję, wybierz tę, której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
4. Dodaj wersję poleceń cmdlet programu PowerShell pochodzącą z usługi SM.

        Add-AzureAccount

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>Krok 2. Ustaw klucz wspólny i utwórz połączenie

Podczas pracy z programem PowerShell w klasycznym modelu wdrażania czasami nazwy zasobów w portalu nie są nazwami, których platforma Azure oczekuje przy korzystaniu z programu PowerShell. Poniższe kroki pomogą Ci wyeksportować plik konfiguracji sieci, aby uzyskać dokładne wartości nazw.

1. Utwórz katalog na komputerze, a następnie wyeksportuj plik konfiguracji sieci do tego katalogu. W tym przykładzie plik konfiguracji sieci zostanie wyeksportowany do katalogu C:\AzureNet.

         Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
2. Otwórz plik konfiguracji sieci przy użyciu edytora xml, a następnie sprawdź wartości właściwości „LocalNetworkSite name” i „VirtualNetworkSite name”. Zmodyfikuj przykład tak, aby odzwierciedlał te wartości. W przypadku podawania nazwy, która zawiera spacje, umieść wartość w apostrofach.

3. Ustaw klucz wspólny i utwórz połączenie. Wartość „-SharedKey” jest generowana i określana przez Ciebie. W tym przykładzie użyliśmy wartości „abc123”, ale można (i należy) wygenerować bardziej złożoną wartość. Pamiętaj o tym, że wartość podana w tym miejscu musi być taka sama, jak wartość podana podczas konfigurowania urządzenia sieci VPN.

        Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
        -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
Wynik po utworzeniu połączenia: **Stan: Powodzenie**.

## <a name="verify"></a>9. Weryfikowanie połączenia

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).


