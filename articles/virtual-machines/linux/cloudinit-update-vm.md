---
title: "Zaktualizować i zainstaluj pakiety w Maszynę wirtualną systemu Linux na platformie Azure za pomocą init chmurze | Dokumentacja firmy Microsoft"
description: "Jak używać init chmury do aktualizacji, a następnie zainstaluj pakiety w Maszynę wirtualną systemu Linux podczas tworzenia 2.0 interfejsu wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 4209bc270a6d255c8512dd6ccd5551b556da5a6b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Zaktualizować i zainstaluj pakiety w Maszynę wirtualną systemu Linux na platformie Azure za pomocą init chmury
W tym artykule przedstawiono sposób użycia [init chmury](https://cloudinit.readthedocs.io) aktualizację pakietów w systemie Linux maszyny wirtualnej (VM) lub skalowania maszyny wirtualnej ustawia (VMSS) na inicjowanie obsługi administracyjnej czas na platformie Azure. Skrypty te init chmury są uruchamiane po pierwszym uruchomieniu komputera po zasoby zostały udostępnione przez platformę Azure. Aby uzyskać więcej informacji na temat chmury inicjowania działania natywnie Azure i obsługiwanych dystrybucjach systemu Linux, zobacz [init chmury — omówienie](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Aktualizacja maszyny Wirtualnej z inicjowaniem chmury
Ze względów bezpieczeństwa można skonfigurować Maszynę wirtualną do zastosowania najnowszych aktualizacji po pierwszym uruchomieniu komputera. Jak chmury init działa za pośrednictwem różnych dystrybucjach systemu Linux, nie istnieje potrzeba do określenia `apt` lub `yum` Menedżera pakietów. Zamiast tego należy zdefiniować `package_upgrade` i pozwolić, aby ustalić odpowiedni mechanizm distro używany proces init chmury. Ten przepływ pracy umożliwia przy użyciu tego samego skrypty chmurze init między dystrybucjach.

Aby wyświetlić procesu uaktualniania w akcji, należy utworzyć plik w bieżącym powłoki o nazwie *cloud_init_upgrade.txt* i wklej następującą konfigurację. Na przykład można utworzyć pliku w powłoce chmury nie na komputerze lokalnym. Można użyć dowolnego edytora, którego chcesz. Wprowadź `sensible-editor cloud_init_upgrade.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Wybierz #1, aby użyć **nano** edytora. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszego wiersza.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Przed wdrożeniem tego obrazu, należy utworzyć nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create) polecenia. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Teraz, utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) i określ plik init chmury z `--custom-data cloud_init_upgrade.txt` w następujący sposób:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH jako publiczny adres IP wyświetlany w danych wyjściowych z poprzedniego polecenia maszyny wirtualnej. Wprowadź własne **publicznego adresu IP** w następujący sposób:

```bash
ssh <publicIpAddress>
```

Uruchom pakiet narzędzia do zarządzania i sprawdzają aktualizacje. W poniższym przykładzie użyto `apt-get` na maszynie Wirtualnej systemu Ubuntu:

```bash
sudo apt-get upgrade
```

Ponieważ init chmury sprawdzane pod kątem i zainstalować aktualizacje na rozruchu, powinny być żadnych aktualizacji do zastosowania, jak pokazano w poniższym przykładzie danych wyjściowych:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

Można również wyświetlić który `httpd` został zainstalowany, uruchamiając `yum history` i przejrzyj dane wyjściowe odwołujące się do `httpd`. 

## <a name="next-steps"></a>Następne kroki
Przykłady dodatkowe init chmury zmian konfiguracji zobacz następujące tematy:
 
- [Dodaj dodatkowe użytkownika w systemie Linux na maszynie Wirtualnej](cloudinit-add-user.md)
- [Uruchom Menedżera pakietów, aby zaktualizować istniejące pakiety po pierwszym uruchomieniu komputera](cloudinit-update-vm.md)
- [Zmień lokalną nazwą hosta maszyny Wirtualnej](cloudinit-update-vm-hostname.md) 
- [Zainstaluj pakiet aplikacji, zaktualizuj pliki konfiguracji i wstrzyknąć kluczy](tutorial-automate-vm-deployment.md)