---
title: "Tworzenie i zarządzanie nimi maszyn wirtualnych systemu Linux z interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Samouczek — tworzenie i zarządzanie nimi maszyn wirtualnych systemu Linux z interfejsu wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: bef7f6ef13f6d31c16d40deb46f168ae52a9e61b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a>Tworzenie i zarządzanie nimi maszyn wirtualnych systemu Linux z interfejsu wiersza polecenia platformy Azure

Maszyny wirtualne platformy Azure zawierają w pełni konfigurowalne i elastyczne środowiska komputerowego. Ten samouczek obejmuje elementów wdrożenia podstawowej maszyny wirtualnej platformy Azure, takich jak rozmiar maszyny Wirtualnej, wybierając obrazu maszyny Wirtualnej i wdrażanie maszyny Wirtualnej. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie i połączyć z maszyną wirtualną
> * Wybierz i używać obrazów maszyn wirtualnych
> * Wyświetlanie i używanie określonych rozmiarów maszyn wirtualnych
> * Zmienianie rozmiaru maszyny wirtualnej
> * Wyświetlanie i zrozumienie stanu maszyny Wirtualnej


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupy zasobów musi zostać utworzone przed maszyny wirtualnej. W tym przykładzie grupy zasobów o nazwie *myResourceGroupVM* jest tworzony w *eastus* regionu. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

Grupa zasobów jest określony, podczas tworzenia lub modyfikowania maszyn wirtualnych, które są widoczne w tym samouczku.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną z [tworzenia maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) polecenia. 

Podczas tworzenia maszyny wirtualnej, takich jak obraz systemu operacyjnego, dysku zmiany rozmiaru i administracyjnych poświadczeń jest kilka opcji. W tym przykładzie utworzono maszynę wirtualną o nazwie *myVM* systemem Ubuntu Server. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

Może upłynąć kilka minut, aby utworzyć maszynę Wirtualną. Po utworzeniu maszyny Wirtualnej, interfejsu wiersza polecenia Azure generuje informacje o maszynie Wirtualnej. Zwróć uwagę na `publicIpAddress`, adres ten umożliwia dostęp do maszyny wirtualnej. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a>Połączenie z maszyną Wirtualną

Teraz można połączyć się z maszyną Wirtualną przy użyciu protokołu SSH w powłoce chmury Azure lub z komputera lokalnego. Zastąp przykładowego adresu IP z `publicIpAddress` zauważono w poprzednim kroku.

```bash
ssh 52.174.34.95
```

Po zalogowaniu się do maszyny Wirtualnej, można zainstalować i skonfigurować aplikacje. Gdy skończysz, zamknięciu sesji SSH, jak zwykle:

```bash
exit
```

## <a name="understand-vm-images"></a>Zrozumienie obrazów maszyn wirtualnych

Azure marketplace zawiera wiele obrazów, które mogą służyć do tworzenia maszyn wirtualnych. W poprzednich krokach utworzono maszynę wirtualną przy użyciu obrazu Ubuntu. W tym kroku wiersza polecenia platformy Azure służy do wyszukiwania marketplace obrazu CentOS, który jest następnie używany do wdrażania drugiej maszyny wirtualnej.  

Aby wyświetlić listę najczęściej najczęściej używanych obrazów, użyj [listy obrazów maszyny wirtualnej az](/cli/azure/vm/image#list) polecenia.

```azurecli-interactive 
az vm image list --output table
```

Dane wyjściowe polecenia zwraca najbardziej popularnych obrazów maszyn wirtualnych na platformie Azure.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Pełna lista, można wyświetlić, dodając `--all` argumentu. Listy obrazów można również filtrować według `--publisher` lub `–-offer`. W tym przykładzie lista jest przefiltrowana pod kątem wszystkich obrazów z ofertą odpowiadający *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Częściowe dane wyjściowe:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

Aby wdrożyć Maszynę wirtualną przy użyciu określonego obrazu, zanotuj wartość w *Urn* kolumny. Podczas określania obrazu, numer wersji obrazu można zastąpić "najnowszej", który wybiera najnowszą wersję dystrybucji. W tym przykładzie `--image` argument jest używany do określenia najnowszą wersję obrazu CentOS 6.5.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>Zrozumienie rozmiarów maszyn wirtualnych

Rozmiar maszyny wirtualnej określa zasoby obliczeniowe np. Procesora GPU i pamięci, które są dostępne do maszyny wirtualnej. Maszyny wirtualne muszą być rozmiary oczekiwanego obciążenia. Jeśli zwiększa obciążenie, można zmienić rozmiar istniejącej maszyny wirtualnej.

### <a name="vm-sizes"></a>Rozmiary maszyn wirtualnych

Poniższa tabela kategoryzuje rozmiary do przypadków użycia.  

| Typ                     | Rozmiary           |    Opis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Zastosowania ogólne](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7| Zrównoważonym Procesora do pamięci. Nadaje się doskonale dla deweloperów i testowania i w małych, średnich rozwiązania aplikacji i danych.  |
| [Optymalizacja pod kątem obliczeń](sizes-compute.md)   | FS, F             | Wysoka Procesora do pamięci. Nadaje się do aplikacji średnia ruchu, urządzeń sieciowych i procesów wsadowych.        |
| [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Wysoka pamięci do core. Doskonałe rozwiązanie dla relacyjnych baz danych, średnich i dużych pamięci podręcznych i analiza w pamięci.                 |
| [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md)      | Ls                | Wysoka przepływność dysku i operacje we/wy. Idealne rozwiązanie w przypadku danych big data oraz baz danych SQL i NoSQL.                                                         |
| [Procesor GPU](sizes-gpu.md)          | WIRTUALIZACJĄ SIECI, NC            | Celem duże Renderowanie grafiki i wideo edycji specjalne maszyn wirtualnych.       |
| [Wysoka wydajność](sizes-hpc.md) | H-A8 11          | Nasze najbardziej zaawansowanych Procesora maszyny wirtualne z interfejsami opcjonalne wysokiej przepustowości sieci (RDMA). 


### <a name="find-available-vm-sizes"></a>Znajdowanie dostępnych rozmiarów maszyny Wirtualnej

Aby wyświetlić listę dostępnych rozmiarów maszyny Wirtualnej w regionie, w szczególności, użyj [listy rozmiarów maszyn wirtualnych az](/cli/azure/vm#list-sizes) polecenia. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Częściowe dane wyjściowe:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Tworzenie maszyny Wirtualnej o określonym rozmiarze

W poprzednim przykładzie tworzenia maszyny Wirtualnej o rozmiarze nie podano, której wynikiem jest domyślny rozmiar. Można wybrać rozmiar maszyny Wirtualnej za pomocą czasu tworzenia [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) i `--size` argumentu. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>Zmienianie rozmiaru maszyny wirtualnej

Po wdrożeniu maszyny Wirtualnej, można zmienić rozmiaru można zwiększyć lub zmniejszyć alokacji zasobów. Możesz wyświetlić bieżący rozmiar maszyny Wirtualnej z [az maszyny wirtualnej pokazu](/cli/azure/vm#show):

```azurecli-interactive
az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize
```

Przed zmianą rozmiaru maszyny Wirtualnej, sprawdź, czy żądany rozmiar jest dostępny na bieżący klaster platformy Azure. [Az maszyny wirtualnej-vm — zmiany rozmiaru — opcje listy](/cli/azure/vm#list-vm-resize-options) polecenie zwraca listę rozmiarów. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
Jeśli żądany rozmiar jest dostępny, maszyny Wirtualnej można zmienić rozmiar ze stanu zasilania na, jednak podczas operacji ponownego rozruchu. Użyj [Zmień rozmiar maszyny wirtualnej az]( /cli/azure/vm#resize) polecenie do wykonania rozmiar.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

Jeśli na bieżący klaster nie ma żądanego rozmiaru, maszyna wirtualna musi alokację, zanim nastąpi operacji zmiany rozmiaru. Użyj [deallocate wirtualna az]( /cli/azure/vm#deallocate) polecenie, aby zatrzymać i cofnięcia przydzielenia Maszynie wirtualnej. Uwaga, kiedy maszyna wirtualna jest włączona ponownie, mogą zostać usunięte wszystkie dane na dysku tymczasowym. Publiczny adres IP także zmianę, chyba że używana jest statyczny adres IP. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Po cofnięciu przydziału, może wystąpić rozmiar. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Po zmiany rozmiaru można uruchomić maszyny Wirtualnej.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>Stany zasilania maszyny Wirtualnej

Maszyny Wirtualnej platformy Azure może mieć jedną z wielu stany zasilania. Ten stan reprezentuje bieżący stan maszyny wirtualnej z punktu widzenia funkcji hypervisor. 

### <a name="power-states"></a>Stany zasilania

| Stan zasilania | Opis
|----|----|
| Uruchamianie | Wskazuje, że maszyna wirtualna zostanie uruchomiona. |
| Działanie | Wskazuje, że maszyna wirtualna jest uruchomiona. |
| Zatrzymywanie | Wskazuje, że maszyna wirtualna jest zatrzymywana. | 
| Zatrzymane | Wskazuje, czy maszyna wirtualna jest zatrzymana. Maszyny wirtualne w stanie zatrzymania nadal naliczenie opłat za obliczenia.  |
| Cofanie przydziału | Wskazuje, że cofana jest maszyny wirtualnej. |
| Cofnięcie przydziału | Wskazuje, że maszyna wirtualna jest usuwane z funkcji hypervisor, ale nadal dostępne w płaszczyźnie formantu. Maszyny wirtualne w stanie Deallocated nie naliczenie opłat za obliczenia. |
| - | Wskazuje, że stan zasilania maszyny wirtualnej jest nieznany. |

### <a name="find-power-state"></a>Znajdź stan zasilania

Aby pobrać stan określonej maszyny Wirtualnej, użyj [wirtualna az Pobierz widok wystąpienia](/cli/azure/vm#get-instance-view) polecenia. Należy określić prawidłową nazwę maszyny wirtualnej i grupy zasobów. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Dane wyjściowe:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Zadania zarządzania

Podczas cyklu życia maszyny wirtualnej można uruchomić zadania zarządzania, takie jak uruchamianie, zatrzymywanie lub usuwanie maszyny wirtualnej. Ponadto można tworzyć skrypty służące do automatyzowania zadań powtarzających się lub złożonych. Przy użyciu wiersza polecenia platformy Azure, wiele typowych zadań zarządzania można uruchomić z wiersza polecenia lub w skryptach. 

### <a name="get-ip-address"></a>Pobierz adres IP

To polecenie zwraca prywatnych i publicznych adresów IP maszyny wirtualnej.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Zatrzymaj maszynę wirtualną

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Uruchom maszynę wirtualną

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Usuwanie grupy zasobów

Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów zawartych w, takich jak maszyny Wirtualnej, sieci wirtualnej i dysku. `--no-wait` Parametr zwraca sterowania do wiersza polecenia bez oczekiwania na ukończenie tej operacji. `--yes` Parametr potwierdza, że chcesz usunąć zasoby bez dodatkowych wiersza, aby to zrobić.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku poznanie podstawowych tworzenia maszyny Wirtualnej i zarządzania, np.:

> [!div class="checklist"]
> * Tworzenie i połączyć z maszyną wirtualną
> * Wybierz i używać obrazów maszyn wirtualnych
> * Wyświetlanie i używanie określonych rozmiarów maszyn wirtualnych
> * Zmienianie rozmiaru maszyny wirtualnej
> * Wyświetlanie i zrozumienie stanu maszyny Wirtualnej

Przejście do następnym samouczku, aby dowiedzieć się więcej o dyski maszyny Wirtualnej.  

> [!div class="nextstepaction"]
> [Utwórz i Zarządzaj maszyny Wirtualnej dyski](./tutorial-manage-disks.md)
