---
title: "Wdrażanie OpenShift platformy kontenera na platformie Azure | Dokumentacja firmy Microsoft"
description: "Wdróż OpenShift platformy kontenera na platformie Azure."
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
ms.openlocfilehash: c91b7232b2f87e0b4b5e659126b96a6ef8b4202c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Wdrażanie OpenShift platformy kontenera na platformie Azure

Istnieje wiele sposobów wdrażania platformy kontenera OpenShift na platformie Azure. Można ręcznie wdrożyć wszystkie składniki wymagane infrastruktury platformy Azure, a następnie postępuj zgodnie z OpenShift kontenera platformy [dokumentacji](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
Można również użyć istniejącego szablonu usługi Resource Manager, który upraszcza wdrażanie klastra OpenShift kontenera platformy. Po umieszczeniu takie szablonu [tutaj](https://github.com/Microsoft/openshift-container-platform/).

Inną opcją jest użycie [Azure Marketplace oferują](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Dla obu opcji wymagana jest subskrypcja Red Hat. Podczas wdrażania wystąpienia RHEL jest zarejestrowany do subskrypcji Red Hat i powiązany identyfikator puli, zawierający uprawnień OpenShift kontenera platformy.
Upewnij się, że masz prawidłowe nazwy Red Hat subskrypcji Menedżera użytkownika, hasło i identyfikator puli (RHSM nazwę użytkownika, hasło RHSM i identyfikator puli). Informacje można sprawdzić, logując się do https://access.redhat.com.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Wdrażanie przy użyciu szablonu usługi Resource Manager OpenShift kontenera platformy

Aby wdrożyć przy użyciu szablonu usługi Resource Manager, pliku parametrów służy umożliwiają określanie wartości parametrów wejściowych. Jeśli chcesz dostosować elementów wdrożenia, które nie są obsługiwane przy użyciu parametrów wejściowych rozwidlenia repozytorium github i zmień odpowiednie elementy.

Niektóre typowe opcje dostosowywania obejmują (między innymi):

- Sieć wirtualna CIDR [zmiennej w azuredeploy.json]
- Rozmiar maszyny Wirtualnej bastionu [zmiennej w azuredeploy.json]
- Konwencje nazewnictwa [zmienne w azuredeploy.json]
- Szczegóły klastra OpenShift - zmodyfikowany za pomocą pliku hosts [deployOpenShift.sh]

### <a name="configure-parameters-file"></a>Konfigurowanie pliku parametrów

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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
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
        "enableAzure": {
            "value": "true"
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

Zastąp ujęta w {} z informacjami o odpowiednie elementy.

### <a name="deploy-using-azure-cli"></a>Wdrażanie przy użyciu wiersza polecenia platformy Azure

> [!NOTE] 
> Polecenie wymaga interfejsu wiersza polecenia Azure 2.0.8 lub nowszym. Az interfejsu wiersza polecenia można sprawdzić wersji z `az --version` polecenia. Aby zaktualizować wersję interfejsu wiersza polecenia, zobacz [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli).

Poniższy przykład wdraża klaster OpenShift i wszystkie powiązane zasoby w grupie zasobów o nazwie myResourceGroup o nazwie myOpenShiftCluster wdrożenia. Szablon jest przywoływany bezpośrednio z repozytorium github i pliku parametrów lokalnego o nazwie **azuredeploy.parameters.json** plik jest używany.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Wdrożenie ma co najmniej 30 minut w zależności od całkowitej liczby węzłów wdrożone. Adres URL konsoli OpenShift i nazwa DNS wzorca OpenShift drukowania na terminalu po zakończeniu wdrożenia.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-using-openshift-container-platform-marketplace-offer"></a>Wdrażanie przy użyciu OpenShift kontenera platformy oferta portalu marketplace

Najprostszym sposobem, aby wdrożyć OpenShift kontenera platformy na platformie Azure jest użycie [Azure Marketplace oferują](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Ta opcja jest to najprostsza, ale również ma ograniczone możliwości dostosowywania. Oferta obejmuje trzy opcje konfiguracji:

- Mała liczba godzin: Wdraża bez-HA klaster z jednym węźle głównym, jednym z węzłów infrastruktury dwa węzły aplikacji i jeden węzeł bastionu. Wszystkie węzły są rozmiary standardowe DS2v2 maszyn wirtualnych. Ten klaster wymaga 10 całkowita liczba rdzeni i jest niezwykle przydatna przy testowaniu małą skalę.
- Średnia liczba godzin: Wdraża klaster wysokiej dostępności z trzech węzłów Master, dwóch węzłów infrastruktury cztery węzły aplikacji i jeden węzeł bastionu. Wszystkie węzły z wyjątkiem bastionu są rozmiary standardowe DS3v2 maszyn wirtualnych. Węzeł bastionu jest DS2v2 standardowa. Ten klaster wymaga 38 rdzeni.
- Duże: Wdraża HA klaster z trzech węzłów Master, dwóch węzłów infrastruktury sześciu węzłów aplikacji i jeden węzeł bastionu. Master i węzłów infrastruktury są rozmiary standardowe DS3v2 maszyn wirtualnych, węzłów aplikacji są rozmiary standardowe DS4v2 maszyn wirtualnych i węzeł bastionu jest DS2v2 standardowa. Ten klaster wymaga 70 rdzeni.

Konfiguracja dostawcy chmury Azure jest opcjonalna, średni i duży rozmiar klastra. Rozmiar klastra małych nie daje opcja konfigurowania dostawcy chmury Azure.

## <a name="connect-to-the-openshift-cluster"></a>Połącz się z klastrem OpenShift

Po zakończeniu wdrożenia łączenie z konsolą OpenShift przy użyciu przeglądarki `OpenShift Console Uri`. Alternatywnie możesz nawiązać połączenie wzorca OpenShift za pomocą następującego polecenia:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenia, aby usunąć grupę zasobów, OpenShift klastra, a wszystkie powiązane zasoby.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

- [Zadania wdrażania po](./openshift-post-deployment.md)
- [Rozwiązywanie problemów z wdrażaniem OpenShift](./openshift-troubleshooting.md)
- [Rozpoczynanie pracy z platformą kontenera OpenShift](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
