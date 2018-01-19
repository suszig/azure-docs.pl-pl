---
title: "Usługa Azure sieci szkieletowej interfejsu wiersza polecenia skryptu wdrażania próbki"
description: "Tworzenie bezpiecznej klastra usługi sieć szkieletowa Linux na platformie Azure przy użyciu interfejsu wiersza polecenia Azure Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: d49383b4f7b3b13beb9ea36ae725938e17ef1456
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Tworzenie bezpiecznej klastra usługi sieć szkieletowa Linux na platformie Azure

To polecenie tworzy certyfikat z podpisem własnym, dodaje go do magazynu kluczy i pobranie certyfikatu lokalnie.  Nowy certyfikat jest używany do zabezpieczania klastra podczas wdrażania.  Umożliwia także istniejącego certyfikatu zamiast tworzenia nowej.  W obu przypadkach nazwa podmiotu certyfikatu musi odpowiadać domeny, który umożliwia dostęp do klastra usługi sieć szkieletowa usług. To dopasowanie jest wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikatu SSL z urzędu certyfikacji dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klastra. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używanego przez klaster.

Jeśli to konieczne, zainstaluj [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Przykładowy skrypt

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowym skrypcie następującego polecenia można usunąć grupy zasobów klastra i wszystkie powiązane zasoby.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie klastra rz az](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Tworzy nowy klaster sieci szkieletowej usług.  |

## <a name="next-steps"></a>Następne kroki

Dodatkowe przykłady interfejsu wiersza polecenia usługi sieci szkieletowej dla sieci szkieletowej usług Azure można znaleźć w [przykłady interfejsu wiersza polecenia usługi sieć szkieletowa](../samples-cli.md).
