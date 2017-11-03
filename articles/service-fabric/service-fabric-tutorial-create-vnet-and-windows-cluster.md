---
title: "Tworzenie klastra sieci szkieletowej usług systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć klaster sieci szkieletowej usług systemu Windows do istniejącej sieci wirtualnej platformy Azure przy użyciu programu PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: b3bab57f5ca6627b4532284376a9809d5ab543f2
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Wdrażanie klastra usługi Windows Fabric w sieci wirtualnej platformy Azure
W tym samouczku wchodzi w jednej serii. Dowiesz się jak wdrażanie klastra usługi sieć szkieletowa usług systemu Windows w istniejącej sieci wirtualnej platformy Azure (VNET) i podrzędna net przy użyciu programu PowerShell. Po zakończeniu, masz działającą w chmurze, które można wdrożyć aplikacji do klastra.  Aby utworzyć klaster systemu Linux za pomocą interfejsu wiersza polecenia Azure, zobacz [tworzenia bezpiecznych klastrów systemu Linux na platformie Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci Wirtualnej na platformie Azure przy użyciu programu PowerShell
> * Tworzenie magazynu kluczy i przekazywanie certyfikatu
> * Tworzenie bezpiecznej klastra sieci szkieletowej usług w programie Azure PowerShell
> * Zabezpieczanie klastra za pomocą certyfikatu X.509
> * Nawiązywanie połączenia z klastrem przy użyciu programu PowerShell
> * Usuwanie klastra

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * Tworzenie bezpiecznej klastra na platformie Azure
> * [Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [modułu zestawu SDK usług sieci szkieletowej i programu PowerShell](service-fabric-get-started.md)
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub nowszej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Poniższe procedury tworzenia klastra usługi sieć szkieletowa pięcioma węzłami. Aby obliczyć kosztów ponoszonych przez uruchomienie klastra sieci szkieletowej usług Azure używana [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Logowanie do platformy Azure i wyboru subskrypcji
W tym przewodniku korzysta z programu Azure PowerShell. Po ponownym uruchomieniu nowej sesji programu PowerShell, zaloguj się do konta platformy Azure i wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.
 
Uruchom następujący skrypt, aby zalogować się do konta platformy Azure Wybierz subskrypcję:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz nową grupę zasobów dla danego wdrożenia i nadaj mu nazwę i lokalizację.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc
```

## <a name="deploy-the-network-topology"></a>Wdrażanie topologii sieci
Następnie skonfiguruj topologii sieci, do którego będzie można wdrożyć API Management i klaster sieci szkieletowej usług. [Network.json] [ network-arm] szablonu usługi Resource Manager jest skonfigurowany, aby utworzyć sieć wirtualną (VNET), a także grupy zabezpieczeń podsieci i sieci (NSG) dla sieci szkieletowej usług oraz podsieci i NSG dla interfejsu API zarządzania . Dowiedz się więcej o sieci wirtualnych, podsieci i grup NSG [tutaj](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] plik parametrów zawiera nazwy podsieci i grup NSG, wdrażających do interfejsu API zarządzania i sieci szkieletowej usług.  Zarządzanie interfejsami API zostało wdrożone w [następujących samouczek](service-fabric-tutorial-deploy-api-management.md). Ten przewodnik dla wartości parametrów, które nie muszą zostać zmienione. Szablony Menedżera zasobów sieci szkieletowej usług użyć tych wartości.  Jeśli wartości są modyfikowane w tym miejscu, należy je zmodyfikować w innych szablonów Menedżera zasobów używanych w tym samouczku i [samouczek wdrażania interfejsu API zarządzania](service-fabric-tutorial-deploy-api-management.md). 

Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
- [Network.JSON][network-arm]
- [Network.parameters.JSON][network-parameters-arm]

Do wdrażania Menedżera zasobów plików szablonu i parametru konfiguracji sieci, należy użyć następującego polecenia programu PowerShell:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Wdrażanie klastra sieci szkieletowej usług
Zasoby sieciowe zostały wykonane wdrażanie następnym krokiem jest wdrożenie klastra sieci szkieletowej usług w podsieci sieci Wirtualnej i NSG przeznaczone dla klastra sieci szkieletowej usług. Wdrażanie klastra do istniejącej sieci Wirtualnej i podsieci (wdrożone wcześniej w tym artykule) wymaga szablonu usługi Resource Manager.  Aby uzyskać więcej informacji, zobacz [Tworzenie klastra przy użyciu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Ten samouczek serii szablon jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim kroku.  Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
- [cluster.JSON][cluster-arm]
- [cluster.parameters.JSON][cluster-parameters-arm]

Certyfikat jest używany do zabezpieczania komunikacji między węzłami klastra oraz zarządzanie dostępem użytkowników do klastra usługi sieć szkieletowa usług. Zarządzanie interfejsami API również używa tego certyfikatu do dostępu do usługi nazewnictwa sieci szkieletowej usług dla potrzeb odnajdywania usług. 

Poniższy skrypt używa [AzureRmServiceFabricCluster nowy](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) polecenia cmdlet, aby wdrożyć nowy klaster na platformie Azure. Polecenia cmdlet również tworzy magazyn kluczy Azure tworzy certyfikat z podpisem własnym i magazyn kluczy i pobiera lokalnie plik certyfikatu.   

```powershell
# Certificate variables.
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"

# Variables for VM admin.
$adminuser="vmadmin"
$adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

# Variables for common values
$clustername = "mysfcluster"
$vmsku = "Standard_D2_v2"
$vaultname = "clusterkeyvault"
$vaultgroupname="clusterkeyvaultgroup"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. Possible values: 1, 3-99
$clustersize=5 

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname -KeyVaultResouceGroupName $vaultgroupname `
-TemplateFile .\cluster.json -ParameterFile .\cluster.parameters.json
```

## <a name="connect-to-the-secure-cluster"></a>Połącz się z klastrem bezpieczne
Połącz z klastrem przy użyciu modułu programu PowerShell usługi Service Fabric zainstalowany przy użyciu zestawu SDK sieci szkieletowej usług.  Najpierw zainstaluj certyfikat do magazynu osobistego (Mój) bieżącego użytkownika na komputerze.  Uruchom następujące polecenie programu PowerShell:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Teraz możesz przystąpić do nawiązywania połączenia z zabezpieczonym klastrem.

**Sieci szkieletowej usług** modułu programu PowerShell zawiera wiele poleceń cmdlet do zarządzania klastrami, aplikacji i usług sieci szkieletowej usług.  Użyj [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) polecenia cmdlet, aby połączyć się z klastrem bezpieczne. Szczegóły punktu końcowego połączenia i odcisk palca certyfikatu można znaleźć w danych wyjściowych z poprzedniego kroku.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Sprawdź, czy nawiązano połączenie i klastra jest w dobrej kondycji za pomocą [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) polecenia cmdlet.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Inne artykuły w tej serii samouczka użyj klastra, który został utworzony. Jeśli nie masz przenoszenie natychmiast do następnego artykułu, możesz usunąć klaster i magazyn kluczy, aby uniknąć naliczania opłat. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów.

Usuń grupę zasobów i zasobów klastra przy użyciu [polecenia cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).  Również usunąć grupę zasobów, zawierającą magazyn kluczy.

```powershell
Remove-AzureRmResourceGroup -Name $groupname -Force
Remove-AzureRmResourceGroup -Name $vaultgroupname -Force
```

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci Wirtualnej na platformie Azure przy użyciu programu PowerShell
> * Tworzenie magazynu kluczy i przekazywanie certyfikatu
> * Tworzenie bezpiecznej klastra sieci szkieletowej usług na platformie Azure przy użyciu programu PowerShell
> * Zabezpieczanie klastra za pomocą certyfikatu X.509
> * Nawiązywanie połączenia z klastrem przy użyciu programu PowerShell
> * Usuwanie klastra

Następnie przejdź do samouczka następujące informacje na temat wdrażania interfejsu API zarządzania za pomocą sieci szkieletowej usług.
> [!div class="nextstepaction"]
> [Wdrażanie Managment interfejsu API](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
