---
title: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza) | Microsoft Azure"
description: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)."
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
ms.date: 09/26/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 95b924257c64a115728c66956d5ea38eb8764a35
ms.openlocfilehash: 70592ac773aced0bfcec5c7418a6dc53555fab33


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview-azpy"></a>Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0 w wersji zapoznawczej (az.py)
Ten artykuł pokazuje, jak szybko wdrożyć maszynę wirtualną systemu Linux na platformie Azure przy użyciu polecenia [az vm create](/cli/azure/vm#create) za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza). 

> [!NOTE] 
> Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza) to nasza następna generacja wieloplatformowego interfejsu wiersza polecenia. [Wypróbuj ten produkt.](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2)
>
> W pozostałych naszych dokumentach jest używany istniejący interfejs wiersza polecenia platformy Azure. Aby utworzyć maszynę wirtualną przy użyciu istniejącego interfejsu wiersza polecenia platformy Azure 1.0, a nie interfejsu wiersza polecenia w wersji 2.0 (wersja zapoznawcza), zobacz [Create a VM with the Azure CLI](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Tworzenie maszyny wirtualnej za pomocą interfejsu wiersza polecenia platformy Azure).

Aby utworzyć maszynę wirtualną, potrzebne są: 

* konto platformy Azure ([skorzystaj z bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/))
* zainstalowany [interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)](/cli/azure/install-az-cli2)
* zalogowanie się do swojego konta platformy Azure (wpisz [az login](/cli/azure/#login))

(Szybkie wdrożenie maszyny wirtualnej z systemem Linux jest możliwe również przy użyciu witryny [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

Poniższy przykład pokazuje, jak wdrożyć maszynę wirtualną Debian i dołączyć klucz Secure Shell (SSH) (argumenty mogą się różnić, jeśli potrzebujesz innego obrazu, [możesz go poszukać](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Najpierw wpisz polecenie [az group create](/cli/azure/group#create), aby utworzyć grupę zasobów zawierającą wszystkie wdrożone zasoby:

```azurecli
az group create -n myResourceGroup -l westus
```

Dane wyjściowe wyglądają następująco (możesz wybrać inną opcję `--output`, jeśli chcesz):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-your-vm-using-the-latest-debian-image"></a>Tworzenie maszyny wirtualnej przy użyciu najnowszego obrazu systemu Debian

Teraz możesz utworzyć maszynę wirtualną i jej środowisko. Pamiętaj, aby zastąpić wartość `----public-ip-address-dns-name` wartością unikatową, ponieważ poniższa może już być zajęta.

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


Dane wyjściowe wyglądają jak poniżej. Zanotuj wartość `publicIpAddress` albo `fqdn` na potrzeby protokołu **ssh** maszyny wirtualnej.


```json
{
  "fqdn": "mydns.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-05-07",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.112.217.29",
  "resourceGroup": "myResourceGroup"
}
```

Zaloguj się do maszyny wirtualnej, używając publicznego adresu IP podanego w danych wyjściowych. Można również użyć podanej w pełni kwalifikowanej nazwy domeny (FQDN).

```bash
ssh ops@mydns.westus.cloudapp.azure.com
```

Powinno pojawić się coś podobnego do następujących wyników w zależności od wybranej dystrybucji:

```
The authenticity of host 'mydns.westus.cloudapp.azure.com (40.112.217.29)' can't be established.
RSA key fingerprint is SHA256:xbVC//lciRvKild64lvup2qIRimr/GB8C43j0tSHWnY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mydns.westus.cloudapp.azure.com,40.112.217.29' (RSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@mynewvm:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Następne kroki
Polecenie `az vm create` jest szybkim sposobem wdrożenia maszyny wirtualnej, dzięki któremu możesz zalogować się do powłoki bash i rozpocząć pracę. Jednak użycie polecenia `az vm create` nie zapewnia rozbudowanej kontroli ani nie umożliwia tworzenia bardziej złożonego środowiska.  Aby wdrożyć maszynę wirtualną z systemem Linux dostosowaną do swojej infrastruktury, możesz skorzystać z dowolnego z poniższych artykułów:

* [Tworzenie konkretnego wdrożenia za pomocą szablonu usługi Azure Resource Manager](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie niestandardowego środowiska dla maszyny wirtualnej z systemem Linux poprzez bezpośrednie użycie poleceń interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Możesz również [użyć sterownika platformy Azure `docker-machine` z różnymi poleceniami, aby szybko utworzyć maszynę wirtualną systemu Linux jako hosta docker](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), a jeśli korzystasz z języka Java, wypróbuj metodę [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine).




<!--HONumber=Jan17_HO1-->


