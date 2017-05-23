---
title: "Różne sposoby tworzenia maszyny wirtualnej z systemem Linux na platformie Azure | Microsoft Azure"
description: "Informacje na temat różnych sposobów tworzenia maszyny wirtualnej z systemem Linux na platformie Azure oraz linki do narzędzi i samouczków dotyczących poszczególnych metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 756711abb014829971af126c5cb60c12e79c920e
ms.contentlocale: pl-pl
ms.lasthandoff: 05/11/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Różne sposoby tworzenia maszyny wirtualnej z systemem Linux
Platforma Azure umożliwia elastyczne tworzenie maszyn wirtualnych z systemem Linux przy użyciu dowolnych narzędzi i przepływów pracy. Ten artykuł zawiera podsumowanie różnych sposobów oraz przykłady tworzenia maszyn wirtualnych z systemem Linux, m.in. przy użyciu interfejsu wiersza polecenia platformy Azure 2.0. Możesz też wyświetlić informacje o opcjach tworzenia z użyciem [interfejsu wiersza polecenia platformy Azure 1.0](creation-choices-nodejs.md).

[Interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-az-cli2) jest dostępny na wielu platformach przy użyciu pakietów menedżera npm, pakietów dostępnych dla określonych dystrybucji lub kontenera Docker. Zainstaluj kompilację najbardziej odpowiednią dla danego środowiska i zaloguj się na koncie platformy Azure przy użyciu [identyfikatora logowania az](/cli/azure/#login)

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](quick-create-cli.md)
  
  * Za pomocą polecenia [az group create](/cli/azure/group#create) utwórz grupę zasobów o nazwie *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Za pomocą polecenia [az vm create](/cli/azure/vm#create) utwórz maszynę wirtualną o nazwie *myVM*, korzystając z obrazu systemu *UbuntuLTS*, i wygeneruj klucze SSH, jeśli już istnieją w katalogu *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu szablonu platformy Azure](create-ssh-secured-vm-from-template.md)
  
  * W poniższym przykładzie użyto polecenia [az group deployment create](/cli/azure/group/deployment#create) do utworzenia maszyny wirtualnej przy użyciu szablonu przechowywanego w witrynie GitHub:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Tworzenie maszyny wirtualnej z systemem Linux i dostosowywanie jej za pomocą skryptów cloud-init](tutorial-automate-vm-deployment.md)

* [Tworzenie aplikacji wysokiej dostępności ze zrównoważonym obciążeniem na wielu maszynach wirtualnych z systemem Linux](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure Portal
Witryna [Azure Portal](https://portal.azure.com) umożliwia szybkie tworzenie maszyn wirtualnych, ponieważ nie wymaga instalacji żadnych składników w systemie. Użyj witryny Azure Portal, aby utworzyć maszynę wirtualną:

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Wybór systemu operacyjnego i obrazu
Podczas tworzenia maszyny wirtualnej możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz również przekazać własny obraz (więcej informacji można znaleźć w [poniższej sekcji](#use-your-own-image)).

### <a name="azure-images"></a>Obrazy platformy Azure
Użyj polecenia [az vm image](/cli/azure/vm/image), aby wyświetlić dostępne obrazy według wydawcy, wersji dystrybucji i kompilacji.

Aby wyświetlić dostępnych wydawców:

```azurecli
az vm image list-publishers --location eastus
```

Aby wyświetlić dostępne produkty (oferty) dla wybranego wydawcy:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Aby wyświetlić dostępne jednostki SKU (wersje dystrybucji) dla danej oferty:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Aby wyświetlić dostępne obrazy dla danego wydania:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Aby uzyskać więcej przykładów związanych z przeglądaniem i wykorzystywaniem dostępnych obrazów, zobacz [Navigate and select Azure virtual machine images with the Azure CLI](cli-ps-findimage.md) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure).

Polecenie [az vm create](/cli/azure/vm#create) ma aliasy umożliwiające szybki dostęp do najpopularniejszych dystrybucji i ich najnowszych wersji. Użycie aliasów jest zwykle łatwiejsze niż określanie wydawcy, oferty, jednostki SKU i wersji za każdym razem podczas tworzenia maszyny wirtualnej:

| Alias | Wydawca | Oferta | SKU | Wersja |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |najnowsza |
| CoreOS |CoreOS |CoreOS |Stable |najnowsza |
| Debian |credativ |Debian |8 |najnowsza |
| openSUSE |SUSE |openSUSE |13.2 |najnowsza |
| RHEL |Redhat |RHEL |7.2 |najnowsza |
| SLES |SLES |SLES |12-SP1 |najnowsza |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |najnowsza |

### <a name="use-your-own-image"></a>Użycie własnego obrazu
Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez przechwycenie tej maszyny wirtualnej. Możesz również przekazać obraz utworzony lokalnie. Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

* [Dystrybucje zatwierdzone na platformie Azure](endorsed-distros.md)
* [Informacje dotyczące niezatwierdzonych dystrybucji](create-upload-generic.md)
* [Tworzenie obrazu na podstawie istniejącej maszyny wirtualnej platformy Azure](tutorial-custom-images.md)
  
  * Przykładowe gotowe polecenia umożliwiające utworzenie obrazu na podstawie istniejącej maszyny wirtualnej platformy Azure:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Następne kroki
* Utwórz maszynę wirtualną z systemem Linux przy użyciu [interfejsu wiersza polecenia](quick-create-cli.md) z poziomu [portalu](quick-create-portal.md) lub przy użyciu [szablonu usługi Azure Resource Manager](../windows/cli-deploy-templates.md).
* Po utworzeniu maszyny wirtualnej z systemem Linux [dowiedz się więcej o dyskach i magazynie platformy Azure](tutorial-manage-disks.md).
* Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](using-vmaccess-extension.md).

