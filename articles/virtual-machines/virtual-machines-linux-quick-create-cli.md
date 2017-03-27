---
title: "Azure: Szybki start — Tworzenie maszyn wirtualnych za pomocą interfejsu wiersza polecenia | Microsoft Docs"
description: "Szybka nauka tworzenia maszyn wirtualnych przy użyciu interfejsu wiersza polecenia platformy Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c1d7cfe614ab4e677e7fff989e79eb09acb3feed
ms.lasthandoff: 03/21/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Tworzenie maszyny wirtualnej z systemem Linux za pomocą interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku zawarto szczegółowe instrukcje korzystania z interfejsu wiersza polecenia platformy Azure w celu wdrożenia maszyny wirtualnej z systemem Ubuntu 16.04 LTS.

Przed rozpoczęciem upewnij się, że interfejs wiersza polecenia platformy Azure został zainstalowany. Aby uzyskać więcej informacji, zobacz [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli) (Przewodnik instalacji interfejsu wiersza polecenia platformy Azure). 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w lokalizacji `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#create). 

Następujący przykład umożliwia utworzenie maszyny wirtualnej o nazwie `myVM` i kluczy SSH, jeśli jeszcze nie istnieją w domyślnej lokalizacji kluczy. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Po utworzeniu maszyny wirtualnej w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej. Zanotuj publiczny adres IP. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Użyj następującego polecenia, aby utworzyć sesję SSH z maszyną wirtualną. Zastąp adres IP publicznym adresem IP Twojej maszyny wirtualnej.

```bash 
ssh <Public IP Address>
```

## <a name="delete-virtual-machine"></a>Usuwanie maszyny wirtualnej

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć za pomocą następującego polecenia.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

[Samouczek dotyczący tworzenia maszyn wirtualnych o wysokiej dostępności](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Przegląd przykładowych poleceń interfejsu wiersza polecenia umożliwiających wdrożenie maszyny wirtualnej](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

