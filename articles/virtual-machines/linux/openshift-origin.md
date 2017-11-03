---
title: "Wdrażanie pochodzenia OpenShift na platformie Azure | Dokumentacja firmy Microsoft"
description: "Wdróż pochodzenia OpenShift na platformie Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Wdrażanie pochodzenia OpenShift na platformie Azure

Istnieje wiele sposobów wdrażania pochodzenia OpenShift na platformie Azure. Można ręcznie wdrożyć wszystkie składniki wymagane infrastruktury platformy Azure, a następnie postępuj zgodnie ze źródłem OpenShift [dokumentacji](https://docs.openshift.org/3.6/welcome/index.html).
Umożliwia także istniejącego szablonu usługi Resource Manager upraszcza wdrażanie klastra OpenShift pochodzenia. Po umieszczeniu takie szablonu [tutaj](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>Wdrażanie przy użyciu szablonu OpenShift źródła

Użyj `appId` wartości z nazwy głównej usługi utworzony wcześniej dla `aadClientId` parametru.

Poniższy przykład tworzy plik parametrów o nazwie **azuredeploy.parameters.json** z wszystkich wymaganych danych wejściowych.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-using-azure-cli"></a>Wdrażanie przy użyciu wiersza polecenia platformy Azure


> [!NOTE] 
> Polecenie wymaga interfejsu wiersza polecenia Azure 2.0.8 lub nowszym. Az interfejsu wiersza polecenia można sprawdzić wersji z `az --version` polecenia. Aby zaktualizować wersję interfejsu wiersza polecenia, zobacz [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli).

Poniższy przykład wdraża klaster OpenShift i wszystkie powiązane zasoby w grupie zasobów o nazwie myResourceGroup o nazwie myOpenShiftCluster wdrożenia. Szablon jest przywoływany bezpośrednio z repozytorium github i pliku parametrów lokalnego o nazwie **azuredeploy.parameters.json** plik jest używany.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Wdrożenie może zająć co najmniej 25 minut w zależności od całkowitej liczby węzłów wdrożone. Adres URL konsoli OpenShift i nazwa DNS wzorca OpenShift drukowania na terminalu po zakończeniu wdrożenia.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Połącz się z klastrem OpenShift

Po zakończeniu wdrożenia łączenie z konsolą OpenShift przy użyciu przeglądarki `OpenShift Console Uri`. Alternatywnie możesz nawiązać połączenie wzorca OpenShift za pomocą następującego polecenia:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenia, aby usunąć grupę zasobów, OpenShift klastra, a wszystkie powiązane zasoby.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

- [Zadania wdrażania po](./openshift-post-deployment.md)
- [Rozwiązywanie problemów z wdrażaniem OpenShift](./openshift-troubleshooting.md)
- [Wprowadzenie do korzystania z OpenShift źródła](https://docs.openshift.org/latest/getting_started/index.html)
