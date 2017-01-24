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
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/03/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 44c46fff9ccf9c7dba9ee380faf5f8213b58e3c3
ms.openlocfilehash: 4397d84ef4d97bdee387777a193ec0b969f2d5e1


---
# <a name="different-ways-to-create-a-linux-vm-including-azure-cli-20-preview"></a>Różne sposoby tworzenia maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)
Platforma Azure umożliwia elastyczne tworzenie maszyn wirtualnych z systemem Linux przy użyciu dowolnych narzędzi i przepływów pracy. Ten artykuł zawiera podsumowanie różnych sposobów oraz przykłady tworzenia maszyn wirtualnych z systemem Linux.

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

- Interfejs wiersza polecenia platformy Azure w wersji 1.0 — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami
- [Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)](../xplat-cli-install.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami

Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza) jest dostępny na wielu platformach przy użyciu pakietów menedżera npm, pakietów dostępnych dla określonych dystrybucji lub kontenera Docker. Pamiętaj, aby zalogować się przy użyciu **identyfikatora logowania az**.

Poniższe samouczki zawierają przykłady dotyczące używania interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza). Zapoznaj się z poszczególnymi artykułami, aby uzyskać więcej informacji na temat przedstawionych poleceń:

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W tym przykładzie tworzona jest grupa zasobów o nazwie myResourceGroup: 
    
    ```azurecli
    az group create -n myResourceGroup -l westus
    ```

  * W tym przykładzie w nowej grupie zasobów tworzona jest maszyna wirtualna przy użyciu najnowszego obrazu systemu Debian z kluczem publicznym o nazwie `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
    --admin-username ops \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name mydns \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

* [Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W poniższym przykładzie maszyna wirtualna jest tworzona przy użyciu szablonu przechowywanego w witrynie GitHub:
    
    ```azurecli
    az group deployment create -g myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Tworzenie kompletnego środowiska systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Obejmuje tworzenie modułu równoważenia obciążenia i wielu maszyn wirtualnych w zestawie dostępności.

* [Dodawanie dysku do maszyny wirtualnej z systemem Linux](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W poniższym przykładzie dodano dysk o rozmiarze 5 GB do istniejącej maszyny wirtualnej o nazwie `myVM`:
    
    ```azurecli
    az vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
      --disk-size 5 --vhd https://myStorage.blob.core.windows.net/vhds/myDataDisk1.vhd
    ```

## <a name="azure-portal"></a>Portal Azure
Witryna [Azure Portal](https://portal.azure.com) umożliwia szybkie tworzenie maszyn wirtualnych, ponieważ nie wymaga instalacji żadnych składników w systemie. Użyj witryny Azure Portal, aby utworzyć maszynę wirtualną:

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Dołączanie dysku przy użyciu witryny Azure Portal](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Wybór systemu operacyjnego i obrazu
Podczas tworzenia maszyny wirtualnej możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz również przekazać własny obraz (więcej informacji można znaleźć w [poniższej sekcji](#use-your-own-image)).

### <a name="azure-images"></a>Obrazy platformy Azure
Użyj poleceń `az vm image` interfejsu wiersza polecenia, aby wyświetlić dostępne obrazy według wydawcy, wersji dystrybucji i kompilacji.

Aby wyświetlić dostępnych wydawców:

```azurecli
az vm image list-publishers -l WestUS
```

Aby wyświetlić dostępne produkty (oferty) dla wybranego wydawcy:

```azurecli
az vm image list-offers --publisher-name Canonical -l WestUS
```

Aby wyświetlić dostępne jednostki SKU (wersje dystrybucji) dla danej oferty:

```azurecli
az vm image list-skus --publisher-name Canonical --offer UbuntuServer -l WestUS
```

Aby wyświetlić dostępne obrazy dla danego wydania:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS -l WestUS
```

Aby uzyskać więcej przykładów związanych z przeglądaniem i wykorzystywaniem dostępnych obrazów, zobacz [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure).

Polecenie `az vm create` ma aliasy umożliwiające szybki dostęp do najpopularniejszych dystrybucji i ich najnowszych wersji. Użycie aliasów jest zwykle łatwiejsze niż określanie wydawcy, oferty, jednostki SKU i wersji za każdym razem podczas tworzenia maszyny wirtualnej:

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
Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez *przechwycenie* tej maszyny wirtualnej. Możesz również przekazać obraz utworzony lokalnie. Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

* [Dystrybucje zatwierdzone na platformie Azure](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Informacje dotyczące niezatwierdzonych dystrybucji](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [How to capture a Linux virtual machine as a Resource Manager template](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Jak przechwycić maszynę wirtualną systemu Linux jako szablon usługi Resource Manager).
  
  * Przykładowe polecenia Szybki start umożliwiające przechwycenie istniejącej maszyny wirtualnej:
    
    ```azurecli
    az vm deallocate -g myResourceGroup -n myVM
    az vm generalize -g myResourceGroup -n myVM
    az vm capture -g myResourceGroup -n myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Następne kroki
* Utwórz maszynę wirtualną z systemem Linux przy użyciu [portalu](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), interfejsu [wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub [szablonu](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) usługi Azure Resource Manager.
* Po utworzeniu maszyny wirtualnej z systemem Linux [dodaj dysk danych](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Jan17_HO1-->


