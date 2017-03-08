---
title: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0 | Microsoft Azure"
description: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0"
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
ms.assetid: 82005a05-053d-4f52-b0c2-9ae2e51f7a7e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 892e3c62a2ad4dc4fd0691874d46bb296e379524
ms.openlocfilehash: cc51b04c31c02aabf25c9efb1e9cd975077811a4
ms.lasthandoff: 02/27/2017


---

# <a name="create-a-linux-vm-using-the-azure-cli-20"></a>Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0
W tym artykule pokazano, jak szybko wdrożyć maszynę wirtualną z systemem Linux na platformie Azure przy użyciu polecenia [az vm create](/cli/azure/vm#create) za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 z użyciem zarówno dysków zarządzanych, jak i dysków w natywnych kontach magazynu. Czynności te można również wykonać przy użyciu [interfejsu wiersza polecenia platformy Azure w wersji 1.0](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aby utworzyć maszynę wirtualną, potrzebne są: 

* konto platformy Azure ([skorzystaj z bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/))
* zainstalowany [interfejs wiersza polecenia platformy Azure w wersji 2.0](/cli/azure/install-az-cli2)
* zalogowanie się do swojego konta platformy Azure (wpisz [az login](/cli/azure/#login))

(Wdrożenie maszyny wirtualnej z systemem Linux jest możliwe również przy użyciu witryny [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

Poniższy przykład pokazuje, jak wdrożyć maszynę wirtualną Debian i nawiązać z nią połączenie przy użyciu klucza Secure Shell (SSH). Argumenty mogą się różnić. Jeśli potrzebujesz innego obrazu, [możesz go poszukać](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="using-managed-disks"></a>Używanie dysków zarządzanych

Aby skorzystać z dysków zarządzanych platformy Azure, musisz użyć obsługującego je regionu. Najpierw wpisz polecenie [az group create](/cli/azure/group#create), aby utworzyć grupę zasobów zawierającą wszystkie wdrożone zasoby:

```azurecli
 az group create -n myResourceGroup -l westus
```

Dane wyjściowe wyglądają następująco (możesz określić inną opcję `--output`, jeśli chcesz zobaczyć inny format):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
### <a name="create-your-vm"></a>Tworzenie maszyny wirtualnej 
Teraz możesz utworzyć maszynę wirtualną i jej środowisko. Pamiętaj, aby zastąpić wartość `--public-ip-address-dns-name` wartością unikatową, ponieważ poniższa może już być zajęta.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Dane wyjściowe wyglądają jak poniżej. Zanotuj wartość `publicIpAddress` albo `fqdn` na potrzeby protokołu **ssh** maszyny wirtualnej.


```json
{
  "fqdn": "manageddisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-E9-41",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.42.127.53",
  "resourceGroup": "myResourceGroup"
}
```

Zaloguj się do maszyny wirtualnej, używając publicznego adresu IP lub w pełni kwalifikowanej nazwy domeny (FQDN) wymienionych w danych wyjściowych.

```bash
ssh ops@manageddisks.westus.cloudapp.azure.com
```

Powinno pojawić się coś podobnego do następujących wyników w zależności od wybranej dystrybucji:

```bash
The authenticity of host 'manageddisks.westus.cloudapp.azure.com (134.42.127.53)' can't be established.
RSA key fingerprint is c9:93:f5:21:9e:33:78:d0:15:5c:b2:1a:23:fa:85:ba.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'manageddisks.westus.cloudapp.azure.com' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan 13 14:44:21 2017 from net-37-117-240-123.cust.vodafonedsl.it
ops@myVM:~$ 
```

Aby poznać inne możliwości Twojej nowej maszyny wirtualnej korzystającej z dysków zarządzanych, zobacz sekcję [Następne kroki](#next-steps).

## <a name="using-unmanaged-disks"></a>Korzystanie z dysków niezarządzanych 

Maszyny wirtualne korzystające z niezarządzanych dysków magazynu mają niezarządzane konta magazynu. Najpierw wpisz polecenie [az group create](/cli/azure/group#create), aby utworzyć grupę zasobów przeznaczoną do przechowywania wszystkich wdrożonych zasobów:

```azurecli
az group create --name nativedisks --location westus
```

Dane wyjściowe wyglądają następująco (możesz wybrać inną opcję `--output`, jeśli chcesz):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks",
  "location": "westus",
  "managedBy": null,
  "name": "nativedisks",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### <a name="create-your-vm"></a>Tworzenie maszyny wirtualnej 

Teraz możesz utworzyć maszynę wirtualną i jej środowisko. Użyj flagi `--use-unmanaged-disk`, aby utworzyć maszynę wirtualną z dyskami niezarządzanymi. Tworzone jest także niezarządzane konto magazynu. Pamiętaj, aby zastąpić wartość `--public-ip-address-dns-name` wartością unikatową, ponieważ poniższa może już być zajęta.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name nativedisks \
--resource-group nativedisks \
--location westus \
--name myVM \
--use-unmanaged-disk
```

Dane wyjściowe wyglądają jak poniżej. Zanotuj wartość `publicIpAddress` albo `fqdn` na potrzeby protokołu **ssh** maszyny wirtualnej.

```json
{
  "fqdn": "nativedisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-33-24-3C",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.91.91.195",
  "resourceGroup": "nativedisks"
}
```

Zaloguj się do maszyny wirtualnej, używając publicznego adresu IP lub w pełni kwalifikowanej nazwy domeny (FQDN). Obie te wartości są podane w powyższych danych wyjściowych.

```bash
ssh ops@nativedisks.westus.cloudapp.azure.com
```

Powinno pojawić się coś podobnego do następujących wyników w zależności od wybranej dystrybucji:

```
The authenticity of host 'nativedisks.westus.cloudapp.azure.com (13.91.93.195)' can't be established.
RSA key fingerprint is 3f:65:22:b9:07:c9:ef:7f:8c:1b:be:65:1e:86:94:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'nativedisks.westus.cloudapp.azure.com,13.91.93.195' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@myVM:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Następne kroki
Polecenie `az vm create` jest szybkim sposobem wdrożenia maszyny wirtualnej, dzięki któremu możesz zalogować się do powłoki bash i rozpocząć pracę. Jednak użycie polecenia `az vm create` nie zapewnia rozbudowanej kontroli ani nie umożliwia tworzenia bardziej złożonego środowiska.  Aby wdrożyć maszynę wirtualną z systemem Linux dostosowaną do swojej infrastruktury, możesz skorzystać z dowolnego z poniższych artykułów:

* [Tworzenie konkretnego wdrożenia za pomocą szablonu usługi Azure Resource Manager](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie niestandardowego środowiska dla maszyny wirtualnej z systemem Linux poprzez bezpośrednie użycie poleceń interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Możesz również [użyć sterownika platformy Azure `docker-machine` z różnymi poleceniami, aby szybko utworzyć maszynę wirtualną systemu Linux jako hosta docker](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), a jeśli korzystasz z języka Java, wypróbuj metodę [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine).


