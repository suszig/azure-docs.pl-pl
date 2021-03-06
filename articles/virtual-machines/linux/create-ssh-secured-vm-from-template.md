---
title: "Utworzyć Maszynę wirtualną systemu Linux na platformie Azure na podstawie szablonu | Dokumentacja firmy Microsoft"
description: "Sposób użycia 2.0 interfejsu wiersza polecenia Azure, aby utworzyć Maszynę wirtualną systemu Linux na podstawie szablonu usługi Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/18/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2750bed40707872bb120a7cb7130d8be01aabf7d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Tworzenie maszyny wirtualnej systemu Linux przy użyciu szablonów usługi Azure Resource Manager
W tym artykule przedstawiono szybki sposób wdrożenia maszyny wirtualnej systemu Linux (VM) do szablonów usługi Azure Resource Manager i 2.0 interfejsu wiersza polecenia platformy Azure. Czynności te można również wykonać przy użyciu [interfejsu wiersza polecenia platformy Azure w wersji 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Przegląd szablonów
Szablony usługi Azure Resource Manager są plikami JSON, definiujące infrastrukturze i konfiguracji rozwiązania Azure. Dzięki szablonowi można wielokrotnie wdrażać rozwiązanie w całym jego cyklu życia z gwarancją spójnego stanu zasobów po każdym wdrożeniu. Aby dowiedzieć się więcej o formacie szablonu i sposobu tworzenia, zobacz [Tworzenie pierwszego szablonu usługi Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). Aby wyświetlić składnię JSON dla typów zasobów, zobacz [Define resources in Azure Resource Manager templates](/azure/templates/) (Definiowanie zasobów w szablonach usługi Azure Resource Manager).


## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupy zasobów musi zostać utworzone przed maszyny wirtualnej. Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupVM* w *eastus* regionu:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
Poniższy przykład tworzy Maszynę wirtualną z [tego szablonu usługi Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) z [Utwórz wdrożenie grupy az](/cli/azure/group/deployment#az_group_deployment_create). Dozwolone jest tylko uwierzytelnianie SSH. Po wyświetleniu monitu podaj wartość własny klucz publiczny SSH, takie jak zawartość *~/.ssh/id_rsa.pub*. Jeśli musisz utworzyć parę kluczy SSH, zobacz [sposobu tworzenia i używania parę kluczy SSH dla maszyn wirtualnych systemu Linux na platformie Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

W poprzednim przykładzie wybrano szablon przechowywany w serwisie GitHub. Można również pobrać lub utworzyć szablon i określ ścieżkę lokalną z `--template-file` parametru.


## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną
Aby SSH do maszyny Wirtualnej, należy uzyskać publiczny adres IP z [az maszyny wirtualnej pokazu](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Można następnie SSH do maszyny Wirtualnej, jak zwykle. Podaj własny publiczny adres IP z poprzedniego polecenia:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Kolejne kroki
W tym przykładzie utworzono podstawowej maszyny Wirtualnej systemu Linux. Aby uzyskać więcej szablonów Menedżera zasobów, które obejmują struktur aplikacji lub utworzyć bardziej złożonych środowiskach, przejdź [galerię szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/).