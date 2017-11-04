---
title: "Tworzenie klastra sieci szkieletowej usług systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć klaster sieci szkieletowej usług Linux do istniejącej sieci wirtualnej platformy Azure przy użyciu wiersza polecenia platformy Azure."
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
ms.openlocfilehash: 983abcd103a58be63053e466c767015c0835eaba
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Wdrażanie klastra usługi sieć szkieletowa Linux do sieci wirtualnej platformy Azure
W tym samouczku wchodzi w jednej serii. Dowiesz się jak wdrażanie klastra usługi sieć szkieletowa usług systemu Linux w istniejącej sieci wirtualnej platformy Azure (VNET) i podrzędna net przy użyciu wiersza polecenia platformy Azure. Po zakończeniu, masz działającą w chmurze, które można wdrożyć aplikacji do klastra. Aby utworzyć klaster systemu Windows za pomocą programu PowerShell, zobacz [tworzenie bezpiecznej klastra systemu Windows na platformie Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci Wirtualnej na platformie Azure przy użyciu wiersza polecenia platformy Azure
> * Tworzenie bezpiecznej klastra sieci szkieletowej usług na platformie Azure przy użyciu wiersza polecenia platformy Azure
> * Zabezpieczanie klastra za pomocą certyfikatu X.509
> * Połącz się z klastrem przy użyciu interfejsu wiersza polecenia usługi sieci szkieletowej
> * Usuwanie klastra

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * Tworzenie bezpiecznej klastra na platformie Azure
> * [Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [usługi sieci szkieletowej interfejsu wiersza polecenia](service-fabric-cli.md)
- Zainstaluj [Azure CLI 2.0](/cli/azure/install-azure-cli)

Poniższe procedury tworzenia klastra usługi sieć szkieletowa pięcioma węzłami. Aby obliczyć kosztów ponoszonych przez uruchomienie klastra sieci szkieletowej usług Azure używana [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Logowanie do platformy Azure i wyboru subskrypcji
W tym przewodniku korzysta z wiersza polecenia platformy Azure. Po ponownym uruchomieniu nowej sesji, zaloguj się do konta platformy Azure i wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.
 
Uruchom następujący skrypt, aby zalogować się do konta platformy Azure Wybierz subskrypcję:

```azurecli
az login
az account set --subscription <guid>
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz nową grupę zasobów dla danego wdrożenia i nadaj mu nazwę i lokalizację.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
az group create --name $ResourceGroupName --location $Location
```

## <a name="deploy-the-network-topology"></a>Wdrażanie topologii sieci
Następnie skonfiguruj topologii sieci, do którego będzie można wdrożyć API Management i klaster sieci szkieletowej usług. [Network.json] [ network-arm] szablonu usługi Resource Manager jest skonfigurowany, aby utworzyć sieć wirtualną (VNET), a także grupy zabezpieczeń podsieci i sieci (NSG) dla sieci szkieletowej usług oraz podsieci i NSG dla interfejsu API zarządzania . Dowiedz się więcej o sieci wirtualnych, podsieci i grup NSG [tutaj](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] plik parametrów zawiera nazwy podsieci i grup NSG, wdrażających do interfejsu API zarządzania i sieci szkieletowej usług.  Zarządzanie interfejsami API zostało wdrożone w [następujących samouczek](service-fabric-tutorial-deploy-api-management.md). Ten przewodnik dla wartości parametrów, które nie muszą zostać zmienione. Szablony Menedżera zasobów sieci szkieletowej usług użyć tych wartości.  Jeśli wartości są modyfikowane w tym miejscu, należy je zmodyfikować w innych szablonów Menedżera zasobów używanych w tym samouczku i [samouczek wdrażania interfejsu API zarządzania](service-fabric-tutorial-deploy-api-management.md). 

Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
- [Network.JSON][network-arm]
- [Network.parameters.JSON][network-parameters-arm]

Użyj następującego skryptu wdrażania Menedżera zasobów plików szablonu i parametru konfiguracji sieci:

```azurecli
az group deployment create \
    --name VnetDeployment \
    --resource-group $ResourceGroupName \
    --template-file network.json \
    --parameters @network.parameters.json
```
<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Wdrażanie klastra sieci szkieletowej usług
Zasoby sieciowe zostały wykonane wdrażanie następnym krokiem jest wdrożenie klastra sieci szkieletowej usług w podsieci sieci Wirtualnej i NSG przeznaczone dla klastra sieci szkieletowej usług. Wdrażanie klastra do istniejącej sieci Wirtualnej i podsieci (wdrożone wcześniej w tym artykule) wymaga szablonu usługi Resource Manager.  Aby uzyskać więcej informacji, zobacz [Tworzenie klastra przy użyciu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Ten samouczek serii szablon jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim kroku.  

Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
- [linuxcluster.JSON][cluster-arm]
- [linuxcluster.parameters.JSON][cluster-parameters-arm]

Ten szablon umożliwia tworzenie bezpiecznej klastra.  Certyfikat klastra jest certyfikat X.509 używany do zabezpieczania komunikacji między węzłami i uwierzytelniania punktów końcowych klastra zarządzania do klienta zarządzania.  Certyfikat klastra umożliwia również SSL dla interfejsu API zarządzania HTTPS i Service Fabric Explorer, za pośrednictwem protokołu HTTPS. Usługa Azure Key Vault służy do zarządzania certyfikatami dla klastrów sieci szkieletowej usług platformy Azure.  Po wdrożeniu klastra na platformie Azure, odpowiedzialną za tworzenie klastrów sieci szkieletowej usług dostawcy zasobów platformy Azure pobiera certyfikaty z magazynu kluczy i instaluje je w klastrze maszyn wirtualnych. 

Można użyć certyfikatu od urzędu certyfikacji (CA), jako certyfikat klastra, lub do celów testowych, Utwórz certyfikat z podpisem własnym. Certyfikat klastra musi:

- zawiera klucz prywatny.
- można utworzyć na potrzeby wymiany kluczy, które można wyeksportować do pliku wymiany informacji osobistych (pfx).
- mieć nazwę podmiotu, która pasuje do domeny, który umożliwia dostęp do klastra usługi sieć szkieletowa usług. To dopasowanie jest wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikat od urzędu certyfikacji (CA) dla. cloudapp.azure.com domeny. Należy uzyskać niestandardowej nazwy domeny dla klastra. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używanego przez klaster.

Wypełnij puste **clusterName**, **adminUserName**, i **adminPassword** parametrów w *linuxcluster.parameters.json* pliku dla danego wdrożenia.  Pozostaw **certificateThumbprint**, **certificateUrlValue**, i **sourceVaultValue** parametry puste, aby utworzyć certyfikat z podpisem własnym.  Jeśli chcesz użyć istniejącego certyfikatu wcześniej przekazany do magazynu kluczy, wypełnij wartości tych parametrów.

Poniższy skrypt używa [tworzenia klastra rz az](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) polecenia i szablon do wdrożenia nowego klastra w systemie Azure. Polecenia cmdlet również tworzy nowy magazyn kluczy Azure dodaje nowy certyfikat z podpisem własnym do magazynu kluczy i pobiera lokalnie plik certyfikatu. Można określić za pomocą innych parametrów istniejącego certyfikatu i/lub magazynu kluczy [tworzenia klastra rz az](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) polecenia.

```azurecli
Password="q6D7nN%6ck@6"
Subject="mysfcluster.southcentralus.cloudapp.azure.com"
VaultName="linuxclusterkeyvault"
az group create --name $ResourceGroupName --location $Location

az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-output-folder . --certificate-password $Password --certificate-subject-name $Subject \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file linuxcluster.json --parameter-file linuxcluster.parameters.json

```

## <a name="connect-to-the-secure-cluster"></a>Połącz się z klastrem bezpieczne
Połącz się z klastrem przy użyciu interfejsu wiersza polecenia usługi sieć szkieletowa `sfctl cluster select` polecenia przy użyciu klucza.  Należy pamiętać, należy używać tylko **— Sprawdź nie** opcji dla certyfikatu z podpisem własnym.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Sprawdź, czy nawiązano połączenie i klastra jest w dobrej kondycji za pomocą `sfctl cluster health` polecenia.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Inne artykuły w tej serii samouczka użyj klastra, który został utworzony. Jeśli nie masz przenoszenie natychmiast do następnego artykułu, możesz usunąć klaster, aby uniknąć naliczania opłat. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów.

Logowanie do platformy Azure i wybierz identyfikator subskrypcji, z którą chcesz usunąć klaster.  Identyfikator subskrypcji można znaleźć po zalogowaniu się do [portalu Azure](http://portal.azure.com). Usuń grupę zasobów i zasobów klastra przy użyciu [usunięcie grupy az](/cli/azure/group?view=azure-cli-latest#az_group_delete) polecenia.

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci Wirtualnej na platformie Azure przy użyciu wiersza polecenia platformy Azure
> * Tworzenie bezpiecznej klastra sieci szkieletowej usług na platformie Azure przy użyciu wiersza polecenia platformy Azure
> * Zabezpieczanie klastra za pomocą certyfikatu X.509
> * Połącz się z klastrem przy użyciu interfejsu wiersza polecenia usługi sieci szkieletowej
> * Usuwanie klastra

Następnie przejdź do samouczka następujące informacje na temat wdrażania interfejsu API zarządzania za pomocą sieci szkieletowej usług.
> [!div class="nextstepaction"]
> [Wdrażanie Managment interfejsu API](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.parameters.json
