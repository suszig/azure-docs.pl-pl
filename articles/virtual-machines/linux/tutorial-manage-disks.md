---
title: "Zarządzanie dyskami Azure z wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Samouczek — Zarządzanie dyskami Azure z wiersza polecenia platformy Azure"
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
ms.openlocfilehash: 5a7a58d4c402bcaf639bd255bb7c8b111694e548
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="manage-azure-disks-with-the-azure-cli"></a>Zarządzanie dyskami Azure z wiersza polecenia platformy Azure

Maszyny wirtualne platformy Azure dysków do przechowywania systemu operacyjnego, aplikacje i dane maszyn wirtualnych. Podczas tworzenia maszyny Wirtualnej jest ważne wybrać rozmiar dysku i odpowiednie do oczekiwanego obciążenia w konfiguracji. W tym samouczku opisano wdrażanie i Zarządzanie dyskami maszyny Wirtualnej. Informacje o:

> [!div class="checklist"]
> * Dyski systemu operacyjnego i tymczasowego
> * Dyski danych
> * Standard i dysków w warstwie Premium
> * Wydajność dysku
> * Dołączanie i przygotowywania dysków z danymi
> * Zmiana rozmiaru dysków
> * Migawki dysków


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Domyślna dysku systemu Azure

Po utworzeniu maszyny wirtualnej platformy Azure, dwa dyski są automatycznie dołączane do maszyny wirtualnej. 

**Dysk systemu operacyjnego** -dysków systemu operacyjnego może być o rozmiarze maksymalnie 1 terabajt oraz hostów maszyn wirtualnych systemu operacyjnego. Dysk systemu operacyjnego jest oznaczony */dev/sda* domyślnie. Dysk buforowanie konfiguracji dysku systemu operacyjnego jest zoptymalizowana pod kątem wydajności systemu operacyjnego. Z powodu tej konfiguracji dysku systemu operacyjnego **nie powinien** hosta aplikacji i danych. Dla aplikacji i danych użycia dysków danych, które opisano szczegółowo w dalszej części tego artykułu. 

**Dysku tymczasowym** -tymczasowego dysków używać dysków SSD, który znajduje się na tym samym hoście Azure co maszyna wirtualna. Tymczasowy dyski są wysokiej wydajności i mogą być używane do operacji, takich jak przetwarzanie danych tymczasowych. Jednak jeśli maszyna wirtualna zostanie przeniesiona do nowego hosta, zostaną usunięte wszystkie dane przechowywane na dysku tymczasowym. Rozmiar dysku tymczasowym zależy od rozmiaru maszyny Wirtualnej. Tymczasowe dyski są oznaczone jako */dev/sdb* i mieć punktu instalacji z *katalogu/mnt*.

### <a name="temporary-disk-sizes"></a>Rozmiary dysku tymczasowym

| Typ | Rozmiar maszyny Wirtualnej | Maksymalny rozmiar dysku tymczasowego (GB) |
|----|----|----|
| [Zastosowania ogólne](sizes-general.md) | A i serii D | 800 |
| [Optymalizacja pod kątem obliczeń](sizes-compute.md) | Seria F | 800 |
| [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md) | D i G serii | 6144 |
| [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md) | Seria L | 5630 |
| [Procesor GPU](sizes-gpu.md) | N serii | 1440 |
| [Wysoka wydajność](sizes-hpc.md) | A i serii H | 2000 |

## <a name="azure-data-disks"></a>Dyski danych Azure

Instalowanie aplikacji i przechowywania danych można dodać dodatkowego dysku z danymi. Dyski danych używanego w sytuacji, gdy wymagane jest trwałe i szybko reagowały pamięci masowej. Każdy dysk danych ma maksymalną pojemność 1 terabajt. Rozmiar maszyny wirtualnej Określa, jak wiele dysków z danymi może zostać dołączony do maszyny Wirtualnej. Dla każdej maszyny Wirtualnej vCPU można dołączyć danych dwóch dysków. 

### <a name="max-data-disks-per-vm"></a>Maksymalna liczba dysków z danymi dla maszyny Wirtualnej

| Typ | Rozmiar maszyny Wirtualnej | Maksymalna liczba dysków z danymi dla maszyny Wirtualnej |
|----|----|----|
| [Zastosowania ogólne](sizes-general.md) | A i serii D | 32 |
| [Optymalizacja pod kątem obliczeń](sizes-compute.md) | Seria F | 32 |
| [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md) | D i G serii | 64 |
| [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md) | Seria L | 64 |
| [Procesor GPU](sizes-gpu.md) | N serii | 48 |
| [Wysoka wydajność](sizes-hpc.md) | A i serii H | 32 |

## <a name="vm-disk-types"></a>Typy dysków maszyny Wirtualnej

Platforma Azure oferuje dwa rodzaje dysku.

### <a name="standard-disk"></a>Dysków w warstwie standardowa

Magazyn Standard Storage bazuje na dyskach twardych (HDD) i stanowi ekonomiczne, chociaż wciąż wydajne rozwiązanie. Dyski standardowe idealnie nadają się do deweloperów ekonomiczne i testów obciążenia.

### <a name="premium-disk"></a>Dysku Premium

Dyski Premium bazują na dysk SSD dysku wysokiej wydajności i małych opóźnień. Idealny dla maszyn wirtualnych obciążeniu produkcji. Magazyn w warstwie Premium obsługuje serii DS, DSv2 serii GS-series i FS serii maszyn wirtualnych. Dyski w warstwie Premium są dostępne w trzech typów (P10, P20, P30), rozmiar dysku Określa typ dysku. Podczas wybierania, rozmiar dysku wartość jest zaokrąglana do dalej typu. Na przykład jeśli rozmiar dysku jest mniejszy niż 128 GB, typ dysku jest P10. Jeśli rozmiar dysku wynosi 129 GB i 512, rozmiar jest P20. Niczego ponad 512 GB, rozmiar jest P30.

### <a name="premium-disk-performance"></a>Wydajność dysku Premium

|Typ dysku magazynu w warstwie Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Rozmiar dysku (zaokrąglony) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Maksymalna liczba operacji wejścia/wyjścia na sekundę na dysk | 500 | 2,300 | 5,000 |
Przepływność na dysk | 100 MB/s | 150 MB/s | 200 MB/s |

Gdy powyższej tabeli określa maksymalną liczbę IOPS dla każdego dysku, można osiągnąć wyższy poziom wydajności przez stosowanie wielu dysków z danymi. Na przykład Standard_GS5 maszyny Wirtualnej można osiągnąć maksymalnie 80 000 IOPS. Aby uzyskać szczegółowe informacje o maksymalnej IOPS dla maszyny Wirtualnej, zobacz [rozmiarów maszyn wirtualnych systemu Linux](sizes.md).

## <a name="create-and-attach-disks"></a>Utwórz i Dołącz dysków

Dyski danych można tworzyć i dołączone w czasie tworzenia maszyny Wirtualnej lub do istniejącej maszyny Wirtualnej.

### <a name="attach-disk-at-vm-creation"></a>Dołączenie dysku podczas tworzenia maszyny Wirtualnej

Utwórz grupę zasobów za pomocą polecenia [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Utwórz maszynę Wirtualną przy użyciu [tworzenia maszyny wirtualnej az]( /cli/azure/vm#create) polecenia. `--datadisk-sizes-gb` Argument jest używany do określenia, czy dodatkowy dysk należy utworzyć i podłączony do maszyny wirtualnej. Aby utworzyć i dołączyć więcej niż jeden dysk, użyj rozdzieloną spacjami listę wartości rozmiaru dysku. W poniższym przykładzie maszyna wirtualna zostanie utworzona z dwóch dysków, zarówno 128 GB. Ponieważ rozmiary dysków wynoszą 128 GB, te dyski obydwa są skonfigurowane jako P10s, które zapewniają maksymalną 500 IOPS dla każdego dysku.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Dołączanie dysku do istniejącej maszyny Wirtualnej

Aby utworzyć i dołączyć nowego dysku do istniejącej maszyny wirtualnej, użyj [dołączyć dysku maszyny wirtualnej az](/cli/azure/vm/disk#attach) polecenia. Poniższy przykład tworzy dysk premium 128 GB w rozmiarze i dołącza go do maszyny Wirtualnej utworzone w ostatnim kroku.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Przygotowanie dysków z danymi

Po dysk został dołączony do maszyny wirtualnej, musi być skonfigurowana do używania dysku systemu operacyjnego. Poniższy przykład pokazuje, jak ręcznie skonfigurować dysk. Ten proces można również zautomatyzować, używając inicjowaniem chmury, które są objęte w [nowsze samouczek](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Konfiguracja ręczna

Utwórz połączenie SSH z maszyną wirtualną. Zamień adres IP przykład publiczny adres IP maszyny wirtualnej.

```azurecli-interactive 
ssh 52.174.34.95
```

Partycji dysku z `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Zapis systemu plików do partycji przy użyciu `mkfs` polecenia.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Zainstaluj nowy dysk, aby była dostępna w systemie operacyjnym.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Dysk jest teraz dostępna za pośrednictwem *datadrive* punktu instalacji, który można zweryfikować, uruchamiając `df -h` polecenia. 

```bash
df -h
```

Dane wyjściowe zawierają nowy dysk zainstalowany na */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Aby upewnić się, że dysk jest ponownej instalacji po ponownym uruchomieniu, musi zostać dodany do */etc/fstab* pliku. Aby to zrobić, należy uzyskać identyfikator UUID dysku o `blkid` narzędzia.

```bash
sudo -i blkid
```

Identyfikator UUID dysku, wyświetla dane wyjściowe `/dev/sdc1` w takim przypadku.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Dodaj wiersz podobny do następującego do */etc/fstab* pliku. Również należy pamiętać, że zapis bariery można wyłączyć za pomocą *bariery = 0*, ta konfiguracja może poprawić wydajność dysku. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Teraz, dysk został skonfigurowany, Zamknij sesję SSH.

```bash
exit
```

## <a name="resize-vm-disk"></a>Zmiana rozmiaru dysku maszyny Wirtualnej

Po wdrożeniu maszyny Wirtualnej, dysk systemu operacyjnego lub żadnych dysków dołączonych danych można zwiększyć rozmiar. Zwiększenie rozmiaru dysku jest przydatne, gdy wymagające więcej miejsca do magazynowania lub wyższego poziomu wydajności (P10 P20, P30). Zauważ, że dyski nie można zmniejszyć rozmiar.

Przed zwiększeniem rozmiaru dysku, wymagany jest identyfikator lub nazwę dysku. Użyj [Lista dysków az](/cli/azure/disk#az_disk_list) polecenia, aby zwrócić wszystkie dyski w grupie zasobów. Zanotuj nazwę dysku, który chcesz zmienić rozmiar.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

Maszyna wirtualna musi również być alokację. Użyj [deallocate wirtualna az]( /cli/azure/vm#deallocate) polecenie, aby zatrzymać i cofnięcia przydzielenia Maszynie wirtualnej.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Użyj [aktualizacja dysku az](/cli/azure/vm/disk#update) polecenia zmiany rozmiaru dysku. W tym przykładzie zmienia rozmiar dysku o nazwie *myDataDisk* do 1 terabajt.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Po ukończeniu operacji zmiany rozmiaru, uruchom maszynę Wirtualną.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Jeśli dysk systemu operacyjnego został zmieniony, jest automatycznie rozszerzana partycji. Jeśli zmieniono dysk z danymi, wszystkie partycje bieżącego musi zostać rozszerzony w systemie operacyjnym maszyny wirtualne.

## <a name="snapshot-azure-disks"></a>Migawki dysków Azure

Tworzenie migawki dysku tworzy odczytu tylko, w momencie kopii dysku. Migawki maszyny Wirtualnej platformy Azure są przydatne do szybkiego zapisywania stanu maszyny wirtualnej przed wprowadzeniem zmian w konfiguracji. W przypadku zmiany konfiguracji okazać się niepożądane, można przywrócić stan maszyny Wirtualnej za pomocą migawki. Gdy maszyna wirtualna ma więcej niż jeden dysk, migawki każdego dysku, niezależnie od innych. Do wykonywania kopii zapasowych spójności aplikacji, należy wziąć pod uwagę zatrzymanie maszyny Wirtualnej przed wykonaniem migawki dysków. Można również użyć [usługi Kopia zapasowa Azure](/azure/backup/), który służy do wykonywania automatycznych kopii zapasowych uruchomionej maszyny Wirtualnej.

### <a name="create-snapshot"></a>Tworzenie migawki

Przed utworzeniem migawek dysków maszyny wirtualnej, wymagany jest identyfikator lub nazwę dysku. Użyj [az maszyny wirtualnej pokazu](https://docs.microsoft.com/en-us/cli/azure/vm#az_vm_show) poleceniu, aby uzyskać identyfikator dysku. W tym przykładzie identyfikator dysku jest przechowywana w zmiennej, dzięki czemu mogą być używane w kolejnym kroku.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Teraz, gdy masz identyfikator dysku maszyny wirtualnej, polecenie tworzy migawkę dysku.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Utwórz dysk z migawki

Następnie można przekonwertować tej migawki na dysk, którego można użyć w celu ponownego utworzenia maszyny wirtualnej.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Przywracanie z migawki maszyny wirtualnej

Aby zademonstrować odzyskiwanie maszyny wirtualnej, usuń istniejącą maszynę wirtualną. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Utwórz nową maszynę wirtualną na podstawie dysku migawki.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Podłącz dysk danych

Wszystkie dyski danych muszą zostać ponownie nałożona do maszyny wirtualnej.

Najpierw znaleźć, używając nazwy dysku danych [Lista dysków az](https://docs.microsoft.com/cli/azure/disk#az_disk_list) polecenia. W tym przykładzie umieszcza nazwę dysku w zmiennej o nazwie *datadisk*, która jest używana w następnym kroku.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Użyj [dołączyć dysku maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) polecenie, aby podłączyć dysk.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono tematy dysków maszyny Wirtualnej takich jak:

> [!div class="checklist"]
> * Dyski systemu operacyjnego i tymczasowego
> * Dyski danych
> * Standard i dysków w warstwie Premium
> * Wydajność dysku
> * Dołączanie i przygotowywania dysków z danymi
> * Zmiana rozmiaru dysków
> * Migawki dysków

Przejdź do następnego w samouczku więcej informacji na temat automatyzacji konfiguracji maszyny Wirtualnej.

> [!div class="nextstepaction"]
> [Automatyzowanie konfiguracji maszyny wirtualnej](./tutorial-automate-vm-deployment.md)
