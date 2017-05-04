---
title: "Łączenie sieci lokalnej sieci z siecią wirtualną platformy Azure: sieć VPN typu lokacja-lokacja: interfejs wiersza polecenia | Microsoft Docs"
description: "Kroki tworzenia połączenia IPsec z sieci lokalnej do sieci wirtualnej platformy Azure za pośrednictwem publicznego Internetu. Ta procedura jest pomocna podczas tworzenia połączenia bramy sieci VPN typu lokacja-lokacja obejmującego wiele lokalizacji za pomocą interfejsu wiersza polecenia."
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
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Tworzenie sieci wirtualnej z wykorzystaniem połączenia sieci VPN typu lokacja-lokacja przy użyciu interfejsu wiersza polecenia

Ten artykuł pokazuje, jak używać interfejsu wiersza polecenia platformy Azure do tworzenia połączenia bramy sieci VPN lokacja-lokacja z Twojej sieci lokalnej do sieci wirtualnej. Kroki podane w tym artykule mają zastosowanie do modelu wdrażania przy użyciu usługi Resource Manager. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager — interfejs wiersza polecenia](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Połączenie bramy sieci VPN typu lokacja-lokacja umożliwia łączenie sieci lokalnej z siecią wirtualną platformy Azure za pośrednictwem tunelu sieci VPN IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga lokalnego urządzenia sieci VPN z przypisanym publicznym adresem IP dostępnym z zewnątrz. Więcej informacji o bramach sieci VPN można znaleźć w artykule [Informacje dotyczące bram sieci VPN](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy są spełnione następujące kryteria:

* Upewnij się, że chcesz pracować z modelem wdrażania przy użyciu usługi Resource Manager. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Aby uzyskać więcej informacji o zgodnych urządzeniach sieci VPN i konfiguracji urządzeń, zobacz artykuł [Informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md).
* Dostępny zewnętrznie publiczny adres IPv4 urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Jeśli nie znasz zakresów adresów IP w konfiguracji swojej sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane. Tworząc tę konfigurację, musisz określić prefiksy zakresu adresów IP, które platforma Azure będzie kierować do Twojej lokalizacji lokalnej. Żadna z podsieci sieci lokalnej nie może się nakładać na podsieci sieci wirtualnej, z którymi chcesz nawiązać połączenie. 
* Najnowsza wersja poleceń interfejsu wiersza polecenia (interfejs wiersza polecenia w wersji 2.0 lub nowszy). Aby uzyskać informacje o instalowaniu poleceń interfejsu wiersza polecenia, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="example-values"></a>Przykładowe wartości

Następujących wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do tych wartości, aby lepiej zrozumieć przykłady w niniejszym artykule:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

Jeśli masz więcej niż jedną subskrypcję platformy Azure, wyświetl subskrypcje dla konta.

```azurecli
Az account list --all
```

Wskaż subskrypcję, której chcesz użyć.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Tworzenie grupy zasobów

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie „TestRG1” w lokalizacji „eastus”. Jeśli masz już grupę zasobów w regionie, w którym chcesz utworzyć swoją sieć wirtualną, możesz jej użyć zamiast tej.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Tworzenie sieci wirtualnej

Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Podczas tworzenia sieci wirtualnej upewnij się, że określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej. 

Poniższy przykład obejmuje tworzenie sieci wirtualnej o nazwie „TestVNet1” i podsieci „Subnet1”.

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Tworzenie podsieci bramy

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Dla tej konfiguracji potrzebna jest również podsieć bramy. Brama sieci wirtualnej korzysta z podsieci bramy, która zawiera adresy IP używane przez usługi bramy sieci VPN. Podczas tworzenia podsieci bramy musi ona otrzymać nazwę „GatewaySubnet”. Jeśli zostanie nadana inna nazwa, podsieć zostanie utworzona, ale platforma Azure nie będzie jej traktowała jako podsieci bramy.

Rozmiar określanej podsieci bramy zależy od konfiguracji bramy sieci VPN, którą chcesz utworzyć. Chociaż możliwe jest utworzenie podsieci bramy tak małej, jak /29, zaleca się wybranie większej podsieci /27 lub /28, aby zawierała więcej adresów. Zastosowanie większej podsieci bramy daje wystarczającą liczbę adresów IP, aby uwzględnić możliwe przyszłe konfiguracje.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Tworzenie bramy sieci lokalnej

Brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Nadaj lokacji nazwę, za pomocą której platforma Azure może odwołać się do niej, a następnie określ adres IP lokalnego urządzenia sieci VPN, z którym będzie tworzone połączenie. Określ również prefiksy adresów IP, które będą kierowane za pośrednictwem bramy sieci VPN do urządzenia sieci VPN. Określone prefiksy adresów są prefiksami znajdującymi się w Twojej sieci lokalnej. W przypadku zmian w sieci lokalnej prefiksy można łatwo zaktualizować.

Wprowadź następujące wartości:

* *--gateway-ip-address* to adres IP lokalnego urządzenia sieci VPN. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.
* *--local-address-prefixes* to Twoje lokalne przestrzenie adresowe.

Następujący przykład pokazuje, jak dodać bramę sieci lokalnej z wieloma prefiksami adresów:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Przesłanie żądania dotyczącego publicznego adresu IP

Prześlij żądanie przydzielenia publicznego adresu IP, który zostanie przypisany do Twojej bramy sieci VPN sieci wirtualnej. Jest to adres IP, do łączenia z którym jest konfigurowane Twoje urządzenie sieci VPN.

Brama sieci wirtualnej dla modelu wdrażania przy użyciu usługi Resource Manager obsługuje obecnie tylko publiczne adresy IP z wykorzystaniem metody dynamicznej alokacji. Nie oznacza to jednak, że adres IP się zmienia. Jedyną sytuacją, w której ma miejsce zmiana adresu IP bramy sieci VPN, jest usunięcie bramy i jej ponowne utworzenie. Publiczny adres IP bramy sieci wirtualnej nie zmienia się w przypadku zmiany rozmiaru, zresetowania ani w przypadku przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Tworzenie bramy sieci VPN

Utwórz bramę sieci VPN sieci wirtualnej. Tworzenie bramy sieci VPN może potrwać 45 minut lub więcej.

Wprowadź następujące wartości:

* Wartość *-gateway-type* dla konfiguracji lokacja-lokacja to *Vpn*. Typ bramy zawsze zależy od wdrażanej konfiguracji. Aby uzyskać więcej informacji, zobacz [Gateway types](vpn-gateway-about-vpn-gateway-settings.md#gwtype) (Typy bram)
* Dla pozycji *-vpn-type* określającej typ sieci VPN można wybrać opcję *RouteBased* (oparta na trasach; w dokumentacji używa się czasem określenia „brama dynamiczna”) lub *PolicyBased* (oparta na zasadach; w dokumentacji używa się czasem określenia „brama statyczna”). To ustawienie zależy od wymagań urządzenia, z którym nawiązujesz połączenie. Aby uzyskać więcej informacji o typach bram sieci VPN, zobacz [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md#vpntype) (Informacje o ustawieniach konfiguracji bramy sieci VPN).
* Dla opcji *-sku* można wybrać wartość Basic (Podstawowa), Standard (Standardowa) lub HighPerformance (Wysoka wydajność). Dla niektórych jednostek SKU istnieją ograniczenia konfiguracji. Aby uzyskać więcej informacji, zobacz [Gateway SKUs](vpn-gateway-about-vpngateways.md#gateway-skus) (Jednostki SKU bramy).

Po uruchomieniu tego polecenia nie zobaczysz żadnych informacji zwrotnych ani danych wyjściowych. Utworzenie bramy trwa około 45 minut.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Konfiguracja urządzenia sieci VPN

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
  Aby znaleźć publiczny adres IP bramy sieci wirtualnej, skorzystaj z poniższego przykładu, zastępując wartości własnymi. W celu poprawienia czytelności dane wyjściowe są sformatowane do wyświetlania listy publicznych adresów IP w formacie tabeli.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. Tworzenie połączenia sieci VPN

Utwórz połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej a lokalnym urządzeniem sieci VPN. Zwróć szczególną uwagę na wartość udostępnionego klucza, która musi być zgodna ze skonfigurowaną wartością klucza współużytkowanego dla Twojego urządzenia sieci VPN.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Po chwili zostanie nawiązane połączenie.

## <a name="toverify"></a>10. Sprawdzenie połączenia sieci VPN

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Jeśli chcesz użyć innej metody, aby sprawdzić swoje połączenie, zobacz [Weryfikowanie połączenia bramy sieci VPN](vpn-gateway-verify-connection-resource-manager.md).

## <a name="common-tasks"></a>Typowe zadania

### <a name="to-view-local-network-gateways"></a>Aby wyświetlić bramy sieci lokalnej

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej
Jeśli zajdzie potrzeba zmiany prefiksów bramy sieci lokalnej, należy wykonać czynności opisane poniżej. Po wprowadzeniu każdej zmiany należy określić całą listę prefiksów, a nie tylko prefiksy, które chcesz zmienić.

- **Jeśli masz określone połączenie**, użyj poniższego przykładu. Określ całą listę prefiksów zawierającą istniejące prefiksy i te, które chcesz dodać. W tym przykładzie już istnieją prefiksy 10.0.0.0/24 i 20.0.0.0/24. Dodamy prefiksy 30.0.0.0/24 i 40.0.0.0/24.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- **Jeśli nie masz określonego połączenia**, użyj tego samego polecenia co do tworzenia bramy sieci lokalnej. To polecenie umożliwia również zaktualizowanie adresu IP bramy dla urządzenia sieci VPN. Tego polecenia używaj tylko wtedy, gdy nie masz jeszcze połączenia. W tym przykładzie istnieją prefiksy 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 i 40.0.0.0/24. Określimy tylko prefiksy, które chcemy zachować. W tym przypadku są to 10.0.0.0/24 i 20.0.0.0/24.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>Aby zmodyfikować adres IP bramy dla bramy sieci lokalnej

Adres IP w tej konfiguracji to adres IP urządzenia sieci VPN, z którym tworzysz połączenie. Jeśli zmieni się adres IP urządzenia sieci VPN, możesz zmodyfikować tę wartość. Adres IP można zmienić nawet wtedy, gdy istnieje połączenie bramy.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Podczas przeglądania wyniku sprawdź, czy są uwzględnione prefiksy adresów IP.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>Aby wyświetlić publiczny adres IP bramy sieci wirtualnej

Aby znaleźć publiczny adres IP swojej bramy sieci wirtualnej, skorzystaj z poniższego przykładu. W celu poprawienia czytelności dane wyjściowe są sformatowane do wyświetlania listy publicznych adresów IP w formacie tabeli.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>Aby sprawdzić wartości klucza współużytkowanego

Sprawdź, czy wartość klucza współużytkowanego jest taka sama jak wartość użyta do konfiguracji urządzenia sieci VPN. Jeśli nie, ponownie uruchom połączenie przy użyciu wartości z urządzenia lub zaktualizuj urządzenie wartością ze zwracanych danych. Wartości muszą być zgodne.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Następne kroki

*  Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).
* Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).
* Aby uzyskać informacje o wymuszonym tunelowaniu, zobacz [Konfigurowanie wymuszonego tunelowania](vpn-gateway-forced-tunneling-rm.md).
* Aby zapoznać się z listą poleceń interfejsu wiersza polecenia platformy Azure dotyczących sieci, zobacz [Interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/network).
