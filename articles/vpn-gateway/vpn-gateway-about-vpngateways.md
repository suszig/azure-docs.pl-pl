<properties 
   pageTitle="VPN Gateway — informacje | Microsoft Azure"
   description="Informacje na temat bramy VPN Gateway dla usługi Azure Virtual Network."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc" />

# VPN Gateway — informacje

Usługa VPN Gateway jest używana do wysyłania ruchu sieciowego między sieciami wirtualnymi i lokalizacjami lokalnymi. Umożliwia ona również wysyłanie ruchu między wieloma sieciami wirtualnymi na platformie Azure (sieć wirtualna-sieć wirtualna). W poniższych sekcjach omówiono zagadnienia dotyczące usługi VPN Gateway.

Instrukcje używane podczas tworzenia bramy sieci VPN będą zależeć od modelu wdrażania użytego w celu utworzenia sieci wirtualnej. Jeśli na przykład sieć wirtualna została utworzona przy użyciu klasycznego modelu wdrożenia, do tworzenia i konfigurowania bramy sieci VPN należy użyć wskazówek i instrukcji dotyczących klasycznego modelu wdrażania. Nie można utworzyć bramy sieci VPN usługi Resource Manager dla sieci wirtualnej z klasycznym modelem wdrażania. 

Aby uzyskać więcej informacji na temat modeli wdrażania, zobacz [Understanding Resource Manager and classic deployment models](../resource-manager-deployment-model.md) (Omówienie modelu wdrażania przy użyciu usługi Resource Manager oraz wdrażania klasycznego).


## <a name="gwsub"></a>Podsieć bramy

Aby skonfigurować bramę sieci VPN, najpierw musisz utworzyć podsieć bramy dla sieci wirtualnej. Aby podsieć bramy działała prawidłowo, musi nosić nazwę *GatewaySubnet*. Dzięki tej nazwie wiadomo, że dana podsieć powinna być używana na potrzeby bramy na platformie Azure.<BR>Jeśli używasz portalu klasycznego, podsieć bramy ma automatycznie nadawaną nazwę *Gateway* w interfejsie portalu. Taka sytuacja dotyczy tylko wyświetlania podsieci bramy w portalu klasycznym. W tym przypadku podsieć jest faktycznie tworzona na platformie Azure jako *GatewaySubnet* i może być wyświetlana w ten sposób w witrynie Azure Portal i w programie PowerShell.

Minimalny rozmiar podsieci bramy zależy tylko od konfiguracji, którą chcesz utworzyć. Chociaż w przypadku niektórych konfiguracji możesz utworzyć małą podsieć bramy o rozmiarze /29, zaleca się tworzenie podsieci bramy /28 i większych (/28, /27, /26 itp.). 

Tworzenie bramy o większym rozmiarze zapobiega przekraczaniu limitów rozmiaru bramy. Jeśli na przykład masz utworzoną bramę o rozmiarze podsieci bramy /29 i chcesz określić konfigurację współistnienia lokacja-lokacja/ExpressRoute, musisz usunąć bramę, usunąć podsieć bramy, utworzyć podsieć bramy o rozmiarze /28 lub większym i ponownie utworzyć bramę. 

Tworząc od początku większą podsieć bramy, możesz zaoszczędzić czas później, podczas dodawania nowych funkcji konfiguracji do środowiska sieci. 

W poniższym przykładzie przedstawiono podsieć bramy o nazwie GatewaySubnet. Zauważ, że notacja CIDR określa rozmiar /27, który umożliwia użycie wystarczającej liczby adresów IP dla większości istniejących obecnie konfiguracji.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

>[AZURE.IMPORTANT] Upewnij się, że dla podsieci GatewaySubnet nie zastosowano grupy zabezpieczeń sieci (NSG), ponieważ może to spowodować błędy połączeń.

## <a name="gwtype"></a>Typy bram

Typ bramy określa sposób nawiązywania przez nią połączenia i jest wymaganym ustawieniem konfiguracji w przypadku modelu wdrażania przy użyciu usługi Resource Manager. Nie należy mylić typu bramy z typem sieci VPN, który określa typ routingu dla sieci VPN. Dostępne wartości parametru `-GatewayType` to: 

- Vpn
- ExpressRoute


W tym przykładzie w modelu wdrażania przy użyciu usługi Resource Manager określono opcję -GatewayType jako *Vpn*. Podczas tworzenia bramy musisz upewnić się, że typ bramy jest prawidłowy dla danej konfiguracji. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## <a name="gwsku"></a>Jednostki SKU bramy

Podczas tworzenia bramy sieci VPN musisz wybrać jednostkę SKU bramy do użycia. Istnieją 3 jednostki SKU bramy sieci VPN:

- Podstawowa (Basic)
- Standardowa (Standard)
- Wysoka wydajność (HighPerformance)

W poniższym przykładzie opcja `-GatewaySku` ma wartość *Standard*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard -GatewayType Vpn -VpnType RouteBased

###  <a name="aggthroughput"></a>Szacowana agregowana przepływność według jednostki SKU i typu bramy


W poniższej tabeli przedstawiono typy bram i szacowaną agregowaną przepływność. Ceny różnych jednostek SKU bramy są różne. Aby uzyskać informacje o cenach, zobacz stronę [Brama VPN — cennik](https://azure.microsoft.com/pricing/details/vpn-gateway/) Ta tabela ma zastosowanie w obu modelach wdrażania — przy użyciu usługi Resource Manager i klasycznym.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="vpntype"></a>Typy sieci VPN

Każda konfiguracja do działania wymaga określonego typu sieci VPN. W przypadku łączenia dwóch konfiguracji, na przykład tworzenia połączenia lokacja-lokacja i połączenia punkt-lokacja w tej samej sieci wirtualnej, należy użyć typu sieci VPN, który spełnia wymagania obu połączeń. 

W przypadku współistniejących połączeń punkt-lokacja i lokacja-lokacja podczas pracy z modelem wdrażania przy użyciu usługi Resource Manager należy użyć typu sieci VPN opartego na trasie, a podczas pracy z klasycznym trybem wdrażania — bramy dynamicznej.

Tworząc konfigurację, należy wybrać typ sieci VPN wymagany przez połączenie. 

Istnieją dwa typy sieci VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

W tym przykładzie w modelu wdrażania przy użyciu usługi Resource Manager określono opcję `-VpnType` jako *RouteBased*. Podczas tworzenia bramy musisz upewnić się, że typ -VpnType jest prawidłowy dla danej konfiguracji. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## <a name="connectiontype"></a>Typy połączeń

Każda konfiguracja wymaga określonego typu połączenia. Dostępne wartości opcji `-ConnectionType` w programie PowerShell w usłudze Resource Manager to:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

W poniższym przykładzie tworzymy połączenie lokacja-lokacja, które wymaga typu połączenia „IPsec”.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="lng"></a>Bramy sieci lokalnej

Brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. W klasycznym modelu wdrażania brama sieci była określana jako lokacja lokalna. Użytkownik przydziela bramie sieci lokalnej nazwę i publiczny adres IP lokalnego urządzenia sieci VPN, a także określa prefiksy adresów, które znajdują się w lokalizacji lokalnej. Na platformie Azure prefiksy adresów docelowych zostaną przeanalizowane pod kątem ruchu sieciowego, nastąpi porównanie z konfiguracją wybraną dla bramy sieci lokalnej i pakiety zostaną odpowiednio przekierowane. W razie potrzeby te prefiksy adresów można zmodyfikować.



### Modyfikowanie prefiksów adresów — Resource Manager

Procedura modyfikowania prefiksów adresów różni się w zależności od tego, czy brama sieci VPN została już utworzona. Zobacz temat [Modyfikowanie prefiksów adresów dla bramy sieci lokalnej](vpn-gateway-create-site-to-site-rm-powershell.md#modify).

W poniższym przykładzie definiujemy bramę sieci lokalnej o nazwie MyOnPremiseWest, która będzie mieć dwa prefiksy adresów IP.

    New-AzureRmLocalNetworkGateway -Name MyOnPremisesWest -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24') 

### Modyfikowanie prefiksów adresów — wdrożenie klasyczne

Jeśli musisz zmodyfikować lokacje lokalne w przypadku korzystania z klasycznego modelu wdrażania, możesz użyć strony konfiguracji Sieci lokalne w portalu klasycznym lub bezpośrednio zmodyfikować plik konfiguracji sieci o nazwie NETCFG.XML.


##  <a name="devices"></a> Urządzenia sieci VPN

Musisz się upewnić, że urządzenie sieci VPN, którego chcesz użyć, obsługuje typ sieci VPN wymagany dla danej konfiguracji. Aby uzyskać więcej informacji o zgodnych urządzeniach sieci VPN, zobacz artykuł [Informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md)

##  <a name="requirements"></a>Wymagania dotyczące bramy


[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## Następne kroki

Aby uzyskać dalsze informacje przed kontynuowaniem planowania i projektowania konfiguracji, zobacz artykuł [Usługa VPN Gateway — często zadawane pytania](vpn-gateway-vpn-faq.md).





 



<!--HONumber=jun16_HO2-->


