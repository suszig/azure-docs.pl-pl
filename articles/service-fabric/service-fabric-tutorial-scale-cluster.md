---
title: "Skalowanie klastra usługi sieć szkieletowa usług Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak szybko skalować klastra usługi sieć szkieletowa usług."
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2017
ms.author: adegeo
ms.openlocfilehash: d203580f676d55acbad4936160982a40592af1d0
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="scale-a-service-fabric-cluster"></a>Skalowanie klastra sieci szkieletowej usług

W tym samouczku jest częścią dwóch serii i pokazuje, jak skalować istniejący klaster, a w. Po wprowadzeniu gotowe, będzie wiadomo, jak skalować klastra i sposób wyczyścić wszystkie zasoby przedziale.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Liczba węzłów klastra do odczytu
> * Dodaj węzły klastra (skalowanie w poziomie)
> * Usuń węzły klastra (skali w)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub wyższej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) lub [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Tworzenie bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure
- W przypadku wdrażania klastra systemu Windows, konfigurowanie środowiska projektowego systemu Windows. Zainstaluj [programu Visual Studio 2017](http://www.visualstudio.com) i **Azure programowanie**, **ASP.NET i sieć web development**, i **aplikacji dla wielu platform .NET Core**obciążeń.  Następnie skonfiguruj [środowiska programowania .NET](service-fabric-get-started.md).
- Jeśli w przypadku wdrażania klastra Linux należy skonfigurować środowisko projektowe Java na [Linux](service-fabric-get-started-linux.md) lub [MacOS](service-fabric-get-started-mac.md).  Zainstaluj [usługi sieci szkieletowej interfejsu wiersza polecenia](service-fabric-cli.md). 

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure
Zaloguj się na konto Azure Wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Do pomyślnego ukończenia tej części samouczka zostały wykonane, należy nawiązać zarówno klastra sieci szkieletowej usług, jak i zestawu skali maszyny wirtualnej (obsługującego klastra). Zestaw skali maszyny wirtualnej jest zasobem platformy Azure, który jest hostem usługi sieć szkieletowa usług Azure.

Po podłączeniu do klastra, możesz go zapytania informacji. Aby dowiedzieć się więcej o jakie węzłów klastra jest znane, można użyć klastra. Łączenie z klastrem w poniższym kodzie używa tego samego certyfikatu, który został utworzony w pierwszej części tej serii. Upewnij się, że ustawisz `$endpoint` i `$thumbprint` zmienne do wartości.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Teraz, gdy masz połączenie, służy polecenie można uzyskać stanu każdego węzła w klastrze. Dla programu PowerShell, użyj `Get-ServiceFabricClusterHealth` polecenia oraz **sfctl** Użyj "polecenia.

## <a name="scale-out"></a>Skalowanie w poziomie

Skalowanie w poziomie, możesz dodać więcej wystąpień maszyny wirtualnej w zestawie skali. Wystąpienia te staną się węzły, które korzysta z sieci szkieletowej usług. Sieć szkieletowa usług wie, kiedy zestaw skali ma więcej wystąpień dodane (przez skalowanie w poziomie) i reaguje automatycznie. Poniższy kod pobiera skalę ustawić według nazwy i zwiększa **pojemności** skali ustawione przez 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Ten kod ustawia pojemność 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Skalowanie w

Skalowanie w jest taka sama jak skalowanie w poziomie, z wyjątkiem użycia dolna **pojemności** wartość. Skalowanie w zestawie skalowania, należy usunąć wystąpień maszyn wirtualnych z zestawu skalowania. Zazwyczaj usługi sieć szkieletowa nie zna co miało miejsce, a sądzi, że węzeł przeszedł Brak. Sieć szkieletowa usług następnie zgłasza Stan nieprawidłowości dla klastra. Aby uniknąć tego nieprawidłowego stanu, należy poinformować usługi sieć szkieletowa spodziewać się węzeł, który ma zniknąć.

### <a name="remove-the-service-fabric-node"></a>Usuń węzeł sieci szkieletowej usług

> [!NOTE]
> Ta część dotyczy tylko *brązowa* warstwa trwałości. Aby uzyskać więcej informacji o trwałości, zobacz [planowania pojemności klastra usługi sieć szkieletowa][durability].

Skalowanie w zestawie skalowania maszyn wirtualnych skali (w większości przypadków) usuwa wystąpienie maszyny wirtualnej, utworzonej ostatnio. Dlatego należy znaleźć dopasowanie, ostatniego utworzenia, węzeł sieci szkieletowej usług. Możesz znaleźć tego ostatniego węzła za pośrednictwem powiązanych punktów sprzedaży sprawdzanie `NodeInstanceId` wartości właściwości w węzłach sieci szkieletowej usług. Przykłady kodu poniżej sortowania przez węzeł wystąpienia i zwracają szczegółowe informacje o wystąpieniu o największej wartości identyfikatora. 

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

```azurecli
`sfctl node list --query "sort_by(items[*], &instanceId)[-1]"`
```

Klastra sieci szkieletowej usług musi wiedzieć, że ten węzeł ma zostać usunięte. Istnieją trzy kroki, które należy wykonać:

1. Wyłącz węzeł, aby była już replikacja danych.  
Środowiska PowerShell:`Disable-ServiceFabricNode`  
sfcli:`sfctl node disable`

2. Zatrzymanie węzła, tak aby środowisko uruchomieniowe usługi sieć szkieletowa przebiega prawidłowo, a aplikacja pobiera żądanie przerwania.  
Środowiska PowerShell:`Start-ServiceFabricNodeTransition -Stop`  
sfcli:`sfctl node transition --node-transition-type Stop`

2. Usuń węzeł z klastra.  
Środowiska PowerShell:`Remove-ServiceFabricNodeState`  
sfcli:`sfctl node remove-state`

Gdy te trzy kroki zostały zastosowane do węzła, można usunąć z zestawu skalowania. Jeśli używasz dowolnego warstwa trwałości oprócz [brązowa][durability], te kroki są wykonywane, gdy wystąpienia zestawu skalowania powoduje usunięcie.

Poniższy blok kodu pobiera ostatni węzeł utworzony, wyłącza zatrzymuje i usuwa węzeł z klastra.

```powershell
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300
    
    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }
    
    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

W **sfctl** kodu poniżej, następujące polecenie służy do pobierania **nazwa węzła** i **węzła wystąpienia identyfikatora** wartości utworzone ostatniego węzła:`sfctl node list --query "sort_by(items[*], &instanceId)[-1].[instanceId,name]"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Należy użyć następującego **sfctl** zapytania, aby sprawdzić stan każdego kroku
>
> **Sprawdź stan dezaktywację.**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].nodeDeactivationInfo"`
>
> **Sprawdź stan zatrzymania**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].isStopped"`
>


### <a name="scale-in-the-scale-set"></a>Skalowanie w zestawie skalowania

Teraz, gdy węzeł sieci szkieletowej usług został usunięty z klastra, zestaw skali maszyny wirtualnej może być skalowana w. W poniższym przykładzie zostanie zmniejszona pojemność zestawu skalowania 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Ten kod ustawia pojemność 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Liczba węzłów klastra do odczytu
> * Dodaj węzły klastra (skalowanie w poziomie)
> * Usuń węzły klastra (skali w)


Następnie przejdź do następujących samouczkiem, aby dowiedzieć się, jak wdrożyć aplikację i używać usługi API management.
> [!div class="nextstepaction"]
> [Wdrażanie usługi API Management](service-fabric-tutorial-deploy-api-management.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
