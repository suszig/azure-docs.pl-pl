---
title: "Tworzenie niestandardowych obrazów maszyn wirtualnych z wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Samouczek — Tworzenie niestandardowego obrazu maszyny Wirtualnej przy użyciu wiersza polecenia platformy Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e73494ff4827b74cbb42b2b0f1f9738c78960e23
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Tworzenie niestandardowego obrazu maszyny Wirtualnej platformy Azure przy użyciu interfejsu wiersza polecenia

Niestandardowe obrazy są takie jak obrazy marketplace, ale można je utworzyć samodzielnie. Niestandardowe obrazy może służyć do ładowania początkowego konfiguracje, takie jak wstępnego ładowania aplikacji, konfiguracji aplikacji i inne konfiguracje systemu operacyjnego. W tym samouczku utworzysz własnego niestandardowego obrazu maszyny wirtualnej platformy Azure. Omawiane kwestie:

> [!div class="checklist"]
> * Anulowanie zastrzeżenia i generalize maszyny wirtualne
> * Tworzenie obrazu niestandardowego
> * Utwórz maszynę Wirtualną z obrazu niestandardowego
> * Wyświetlanie listy wszystkich obrazów w ramach subskrypcji
> * Usuń obraz


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Przed rozpoczęciem

Poniższe kroki szczegółowo sposób pobrać istniejącej maszyny Wirtualnej i przekształcić ją do ponownego użycia niestandardowego obrazu używanego do utworzenia nowego wystąpienia maszyny Wirtualnej.

Aby ukończyć przykład, w tym samouczku, musi mieć istniejącej maszyny wirtualnej. Jeśli to konieczne, to [przykładowym skrypcie](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) można utworzyć. Podczas pracy nad samouczek, Zastąp maszyny Wirtualnej i grupy zasobów nazwy w razie potrzeby.

## <a name="create-a-custom-image"></a>Tworzenie obrazu niestandardowego

Aby utworzyć obraz maszyny wirtualnej, należy przygotować maszyny Wirtualnej anulowania obsługi, dealokowanie, a następnie oznaczenie źródłowej maszyny Wirtualnej, ponieważ uogólniony. Maszyny Wirtualnej został przygotowany, można tworzyć obrazu.

### <a name="deprovision-the-vm"></a>Anulowanie zastrzeżenia maszyny Wirtualnej 

Cofanie zastrzegania stanowi uogólnienie maszyny Wirtualnej przez usunięcie informacji na temat określonego komputera. Ta generalizacji sprawia, że można wdrożyć wiele maszyn wirtualnych za pomocą pojedynczego obrazu. Podczas anulowania obsługi, nazwa hosta jest resetowany do *localhost.localdomain*. Klucze hosta SSH, konfiguracje wskazują hasła głównego i buforowanych dzierżaw DHCP również są usuwane.

Aby anulowanie zastrzeżenia maszyny Wirtualnej, użyj agenta maszyny Wirtualnej platformy Azure (agenta waagent). Agent maszyny Wirtualnej platformy Azure jest zainstalowana na maszynie Wirtualnej i zarządza interakcji z kontrolerem sieci szkieletowej Azure i udostępniania. Aby uzyskać więcej informacji, zobacz [Podręcznik użytkownika agenta systemu Linux Azure](agent-user-guide.md).

Połączenie z maszyną Wirtualną przy użyciu protokołu SSH, a następnie uruchom polecenie Anulowanie zastrzeżenia maszyny Wirtualnej. Z `+user` argument, ostatnie konto użytkownika elastycznie i wszelkie powiązane dane, również zostaną usunięte. Zamień adres IP przykład publiczny adres IP maszyny Wirtualnej.

SSH z maszyną Wirtualną.
```bash
ssh azureuser@52.174.34.95
```
Anulowanie zastrzeżenia maszyny Wirtualnej.

```bash
sudo waagent -deprovision+user -force
```
Zamknij sesję SSH.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Cofnięcie przydziału i zostać oznaczone jako uogólniona maszyna wirtualna

Do utworzenia obrazu, musi można cofnąć przydziału maszyny Wirtualnej. Cofnięcie przydziału maszynę Wirtualną przy użyciu [deallocate wirtualna az](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Wreszcie, Ustaw stan maszyny Wirtualnej, ponieważ uogólniony z [generalize wirtualna az](/cli//azure/vm#generalize) będzie wówczas traktował platformy Azure, maszyny Wirtualnej został uogólniony. Można tworzyć tylko obraz z ogólnych maszyny Wirtualnej.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>Tworzenie obrazu

Teraz można utworzyć obrazu maszyny wirtualnej za pomocą [tworzenia obrazu az](/cli//azure/image#create). Poniższy przykład tworzy obraz o nazwie *myImage* z maszyny Wirtualnej o nazwie *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Tworzenie maszyn wirtualnych z obrazu

Teraz, gdy masz obrazu można utworzyć jeden lub więcej nowych maszyn wirtualnych z obrazu przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVMfromImage* z obrazu o nazwie *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Zarządzanie obrazami 

Oto kilka przykładów typowych zadań zarządzania obrazu i jak zakończyć je za pomocą wiersza polecenia platformy Azure.

Wyświetl listę wszystkich obrazów według nazwy w formacie tabeli.

```azurecli-interactive 
az image list \
    --resource-group myResourceGroup
```

Usuń obraz. W tym przykładzie usuwa obraz o nazwie *myOldImage* z *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzony niestandardowy obraz maszyny Wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Anulowanie zastrzeżenia i generalize maszyny wirtualne
> * Tworzenie obrazu niestandardowego
> * Utwórz maszynę Wirtualną z obrazu niestandardowego
> * Wyświetlanie listy wszystkich obrazów w ramach subskrypcji
> * Usuń obraz

Przejście do następnym samouczku, aby dowiedzieć się więcej o wysokiej dostępności maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie maszyn wirtualnych wysokiej dostępności](tutorial-availability-sets.md).

