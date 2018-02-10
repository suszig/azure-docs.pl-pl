---
title: "Maszyn wirtualnych i typy węzłów sieci szkieletowej usług Azure skalowanie zestawów | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak ustawia węzeł sieci szkieletowej usług Azure, typy odnoszą się do skalowania maszyny wirtualnej, a na połączenie zdalne skali zestawu wystąpienia lub węzła klastra."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/5/2018
ms.author: chackdan
ms.openlocfilehash: 720bb83c9d8540549852ce78ee1709f8c8717348
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Maszyn wirtualnych i typy węzłów sieci szkieletowej usług Azure skalowanie zestawów
Zestawy skalowania maszyny wirtualnej są zasobami obliczeń platformy Azure. Zestawy skalowania służy do wdrażania i zarządzania nimi jako zestaw kolekcji maszyn wirtualnych. Skonfiguruj oddzielne skali, ustaw dla każdego typu węzła, który definiuje się w klastrze usługi sieć szkieletowa usług Azure. Można niezależnie skalować każdy typ węzła w górę lub w dół, mają różne zestawy otwartych portów i metryki pojemności różnych.

Na poniższej ilustracji przedstawiono klastra, która ma dwa typy węzłów, o nazwie frontonu i wewnętrznej bazy danych. Każdy typ węzła ma pięć węzłów.

![Klastra, która ma dwa typy węzłów][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Mapowanie wystąpień zestawu skalowania maszyn wirtualnych do węzłów
Jak pokazano na poprzedniej ilustracji, wystąpienia zestawu skali rozpoczęli wystąpienie 0, a dopiero potem zwiększyć jej przez 1. Numerowanie znajduje odzwierciedlenie w nazwach węzła. Na przykład węzeł BackEnd_0 jest wystąpienie 0 zestaw skali zaplecza. Ten zestaw skalowania określonego ma pięć wystąpień, o nazwie BackEnd_0, BackEnd_1 BackEnd_2, BackEnd_3 i BackEnd_4.

Skalowanie w górę zestaw skalowania utworzono nowe wystąpienie. Nazwa nowego wystąpienia zestawu skali jest zwykle zestawu skali nazwy oraz dalej liczby wystąpień. W naszym przykładzie jest BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Mapowania usługi równoważenia obciążenia zestaw skalowania typy węzła i skalowanie zestawów
Jeśli wdrażane klastrem w portalu Azure lub użyć przykładowego szablonu usługi Azure Resource Manager, są wyświetlane wszystkie zasoby w grupie zasobów. Widać usługi równoważenia obciążenia dla każdego typu zestawu lub węzeł skali. Nazwa modułu równoważenia obciążenia jest używany następujący format: **LB -&lt;Nazwa typu węzła&gt;**. Przykładem jest LB-sfcluster4doc-0, jak pokazano na poniższej ilustracji:

![Zasoby][Resources]
## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Połączenie zdalne wystąpienie zestawu skali maszyny wirtualnej lub węzła klastra
Skonfiguruj oddzielne skali, ustaw dla każdego typu węzła, który został zdefiniowany w klastrze. Typy węzłów można niezależnie skalować w górę lub w dół. Można też użyć innej jednostki SKU maszyny Wirtualnej. W przeciwieństwie do maszyn wirtualnych z jednego wystąpienia wystąpienia zestawu skali nie mają własne wirtualne adresy IP. To może być wyzwaniem, jeśli szukasz adres IP i port, który umożliwia zdalne połączenia z określonego wystąpienia.

Aby znaleźć adres IP i port, który umożliwia zdalne połączenia z konkretnym wystąpieniem, wykonaj następujące kroki.

**Krok 1**: znaleźć wirtualnego adresu IP dla typu węzła pobierając reguły NAT ruchu przychodzącego dla protokołu RDP (Remote Desktop).

Najpierw pobierz zdefiniowane jako część definicji zasobu dla wartości reguły ruchu przychodzącego translatora adresów Sieciowych `Microsoft.Network/loadBalancers`.

W portalu Azure, na stronie usługi równoważenia obciążenia, wybierz **ustawienia** > **reguł ruchu przychodzącego translatora adresów Sieciowych**. Zapewnia to adres IP i port, który umożliwia zdalne połączenia z pierwszej skali Ustaw wystąpienia. 

![Moduł równoważenia obciążenia][LBBlade]

Na poniższej ilustracji, adres IP i port są **104.42.106.156** i **3389**.

![Reguły translatora adresów sieciowych][NATRules]

**Krok 2**: Znajdź port, który umożliwia zdalne połączenia z wystąpienia zestawu skali określonych lub węzeł.

Skala ustawioną mapy wystąpień węzłów. Umożliwia określenie dokładnej port używany informacji zestaw skali.

Porty są przydzielane w kolejności rosnącej, odpowiadający wystąpienia zestawu skali. Na przykład wcześniej typu węzła serwera sieci Web w poniższej tabeli wymieniono portów dla wszystkich wystąpień pięcioma węzłami. Zastosowanie tego samego mapowania do Twojego wystąpienia zestawu skali.

| **Wystąpienia zestawu skalowania maszyn wirtualnych** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

**Krok 3**: zdalne połączenia z określoną wystąpienia zestawu.

Na poniższym rysunku pokazano, przy użyciu usługi Podłączanie pulpitu zdalnego w celu nawiązywania wystąpienia zestawu skali FrontEnd_1:

![Podłączanie pulpitu zdalnego][RDP]

## <a name="change-the-rdp-port-range-values"></a>Zmienianie wartości zakresu portów protokołu RDP

### <a name="before-cluster-deployment"></a>Przed wdrożeniem klastra
Po skonfigurowaniu klastra przy użyciu szablonu usługi Resource Manager, należy określić zakres w `inboundNatPools`.

Przejdź do definicji zasobu dla `Microsoft.Network/loadBalancers`. Znajdź opis `inboundNatPools`.  Zastąp `frontendPortRangeStart` i `frontendPortRangeEnd` wartości.

![wartości inboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Po wdrożeniu klastra
Zmiana wartości zakresu portów protokołu RDP po wdrożeniu klastra jest bardziej złożony. Aby upewnić się, że nie Odtwórz maszyn wirtualnych, przy użyciu programu Azure PowerShell Ustaw nowe wartości. 

> [!NOTE]
> Upewnij się, że masz Azure PowerShell 1.0 lub nowszej wersji zainstalowanej na komputerze. Jeśli nie masz programu Azure Powershell 1.0 lub nowszej wersji, firma Microsoft zaleca, skorzystaj z procedury opisanej w [jak instalowanie i konfigurowanie programu Azure PowerShell.](/powershell/azure/overview)

1. Zaloguj się do konta platformy Azure. Jeśli następującego polecenia programu PowerShell zakończy się niepowodzeniem, sprawdź, czy PowerShell poprawnie zainstalowane.

    ```
    Login-AzureRmAccount
    ```

2. Aby uzyskać szczegółowe informacje o przez moduł równoważenia obciążenia i aby zobaczyć wartości opisu `inboundNatPools`, uruchom następujący kod:

    ```
    Get-AzureRmResource -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
    ```

3. Ustaw `frontendPortRangeEnd` i `frontendPortRangeStart` na wartości, które mają.

    ```
    $PropertiesObject = @{
        #Property = value;
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name> -ApiVersion <use the API version that is returned> -Force
    ```

## <a name="change-the-rdp-user-name-and-password-for-nodes"></a>Zmień hasło dla węzłów oraz nazwa użytkownika pulpitu zdalnego

Aby zmienić hasło dla wszystkich węzłów typu określonego węzła, wykonaj następujące kroki. Te zmiany zostaną zastosowane do wszystkich aktualnych i przyszłych węzłów w zestawie skalowania.

1. Otwórz program PowerShell jako administrator. 
2. Aby zalogować się i wybierz subskrypcję dla sesji, uruchom następujące polecenia. Zmień `SUBSCRIPTIONID` parametr do Twojego identyfikatora subskrypcji. 

    ```powershell
    Login-AzureRmAccount
    Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
    ```

3. Uruchom poniższy skrypt. Użyj odpowiedniego `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME`, i `PASSWORD` wartości. `USERNAME` i `PASSWORD` wartości są nowe poświadczenia, że używasz w przyszłych sesji protokołu RDP. 

    ```powershell
    $nodeTypeName = 'NODETYPENAME'
    $resourceGroup = 'RESOURCEGROUP'
    $publicConfig = @{'UserName' = 'USERNAME'}
    $privateConfig = @{'Password' = 'PASSWORD'}
    $extName = 'VMAccessAgent'
    $publisher = 'Microsoft.Compute'
    $node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
    $node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

    Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
    ```

## <a name="next-steps"></a>Kolejne kroki
* Zobacz [omówienie funkcji "Wdrażanie z dowolnego miejsca" i porównanie z klastrami zarządzane Azure](service-fabric-deploy-anywhere.md).
* Dowiedz się więcej o [klastra zabezpieczeń](service-fabric-cluster-security.md).
* Dowiedz się więcej o [zestawu SDK usług sieci szkieletowej i wprowadzenie](service-fabric-get-started.md).

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
