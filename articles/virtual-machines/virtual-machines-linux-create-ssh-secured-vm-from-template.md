---
title: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu szablonu platformy Azure | Microsoft Docs"
description: "Tworzenie maszyny wirtualnej systemu Linux na platformie Azure przy użyciu szablonu usługi Azure Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ff7ea65c329a37acf8b2febb52fd140954d81e97


---
# <a name="create-a-linux-vm-using-an-azure-template"></a>Tworzenie maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure
W tym artykule przedstawiono szybki sposób wdrożenia maszyny wirtualnej systemu Linux na platformie Azure przy użyciu szablonu Azure.  Wykonanie czynności opisanych w tym artykule wymaga:

* konta platformy Azure ([skorzystaj z bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/));
* dostępu do [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) (po zalogowaniu przy użyciu `azure login`).
* Interfejs wiersza polecenia platformy Azure *musi działać w* trybie usługi Azure Resource Manager`azure config mode arm`.

Szybkie wdrożenie szablonu maszyny wirtualnej z systemem Linux jest możliwe również przy użyciu witryny [Azure Portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Szybkie polecenia — podsumowanie
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Szczegółowy przewodnik
Szablony umożliwiają tworzenie maszyn wirtualnych na platformie Azure przy użyciu ustawień, które można dostosować podczas uruchomienia, np. nazw użytkowników i nazw hostów. W tym artykule szablon platformy Azure został użyty do utworzenia maszyny wirtualnej z systemem Ubuntu oraz grupy zabezpieczeń sieci z otwartym portem 22 dla usługi SSH.

Szablony programu Azure Resource Manager są plikami JSON, za pomocą których można wykonywać proste, jednorazowe zadania, takie jak uruchamianie maszyny wirtualnej z systemem Ubuntu przedstawione w tym artykule.  Szablony platformy Azure mogą być również używane do tworzenia złożonych konfiguracji całych środowisk na platformie Azure, takich jak stos wdrażania do testowania, programowania lub produkcji.

## <a name="create-the-linux-vm"></a>Tworzenie maszyny wirtualnej systemu Linux
Poniższy przykład kodu pokazuje sposób wywoływania polecenia `azure group create` w celu jednoczesnego utworzenia grupy zasobów i wdrożenia maszyny wirtualnej systemu Linux zabezpieczonej przez protokół SSH za pomocą [szablonu usługi Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Należy pamiętać, że w tym przykładzie trzeba użyć nazw, które będą unikatowe w danym środowisku. Ten przykład wykorzystuje `myResourceGroup` jako nazwę grupy zasobów oraz `myVM` jako nazwę maszyny wirtualnej.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Dane wyjściowe powinny wyglądać podobnie do następującego bloku:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

W tym przykładzie wdrożono maszynę wirtualną przy użyciu parametru `--template-uri`.  Możesz również pobrać bądź utworzyć szablon lokalnie i przekazać go przy użyciu parametru `--template-file` ze ścieżką do pliku szablonu jako argumentem. Interfejs wiersza polecenia platformy Azure monituje o parametry wymagane przez szablon.

## <a name="next-steps"></a>Następne kroki
Przeszukaj [galerię szablonów](https://azure.microsoft.com/documentation/templates/), aby odkryć kolejne platformy aplikacji do wdrożenia.




<!--HONumber=Nov16_HO2-->


