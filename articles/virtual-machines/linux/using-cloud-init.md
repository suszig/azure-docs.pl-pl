---
title: "Init chmury umożliwia dostosowywanie Maszynę wirtualną systemu Linux | Dokumentacja firmy Microsoft"
description: "Dostosowywanie maszyny Wirtualnej systemu Linux podczas tworzenia 2.0 interfejsu wiersza polecenia platformy Azure przy użyciu init chmury"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/03/2017
ms.author: iainfou
ms.openlocfilehash: 5559f258f5c29b07edb5e61be4755d67173019e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Init chmury umożliwia dostosowywanie Maszynę wirtualną systemu Linux na platformie Azure
W tym artykule przedstawiono sposób użycia [init chmury](https://cloudinit.readthedocs.io) można ustawić nazwę hosta, pakietów aktualizacji i zarządzanie kontami użytkowników na maszynie wirtualnej (VM) na platformie Azure. Skrypty te init chmury są uruchamiane przy rozruchu podczas tworzenia maszyny Wirtualnej z 2.0 interfejsu wiersza polecenia platformy Azure. Więcej informacji na temat Omówienie na sposobu instalowania aplikacji, zapisywać pliki konfiguracji i wstrzyknąć kluczy z usługi Key Vault, zobacz [w tym samouczku](tutorial-automate-vm-deployment.md). Czynności te można również wykonać przy użyciu [interfejsu wiersza polecenia platformy Azure w wersji 1.0](using-cloud-init-nodejs.md).


## <a name="cloud-init-overview"></a>Init chmury — omówienie
[Init chmury](https://cloudinit.readthedocs.io) jest powszechnie używaną podejście, aby dostosować Maszynę wirtualną systemu Linux, ponieważ jest on uruchamiany po raz pierwszy. Init chmury można użyć, aby zainstalować pakiety i zapisywać pliki, lub aby skonfigurować użytkowników i zabezpieczeń. Podczas inicjowania chmury działania podczas początkowego procesu rozruchu, nie ma, nie dodatkowe kroki lub agentów wymaganych do zastosowania konfiguracji.

Init chmury działa także w dystrybucji. Na przykład nie używaj **instalacji stanie get** lub **yum zainstalować** do zainstalowania pakietu. Zamiast tego można zdefiniować listę pakietów do zainstalowania. Init chmury automatycznie używa narzędzia do zarządzania natywnego pakietu dla distro, którą wybierzesz.

Pracujemy nad z naszych partnerów uzyskanie init chmury uwzględnione i Praca w obrazach, zapewniające na platformie Azure. W poniższej tabeli przedstawiono bieżącej dostępności init chmury na obrazy platformy Azure:

| Alias | Wydawca | Oferta | SKU | Wersja |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |najnowsza |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |najnowsza |
| CoreOS |CoreOS |CoreOS |Stable |najnowsza |


## <a name="set-the-hostname-with-cloud-init"></a>Ustaw nazwę hosta z inicjowaniem chmury
Init chmury pliki są zapisywane [yaml programu](http://www.yaml.org). Do uruchomienia skryptu chmury init, podczas tworzenia maszyny Wirtualnej na platformie Azure z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create), określ plik init chmury z `--custom-data` przełącznika. Oto kilka przykładów co można skonfigurować przy użyciu pliku init chmury. Typowy scenariusz jest nazwa hosta maszyny wirtualnej. Domyślnie nazwa hosta jest taka sama jak nazwa maszyny Wirtualnej. 

Najpierw utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroup* w *eastus* lokalizacji:

```azurecli
az group create --name myResourceGroup --location eastus
```

W bieżącym powłoki, Utwórz plik o nazwie *cloud_init_hostname.txt* i wklej następującą konfigurację. Na przykład utworzyć plik, w powłoce chmury nie na komputerze lokalnym. Można użyć dowolnego edytora, którego chcesz. W powłoce chmury wprowadź `sensible-editor cloud_init_hostname.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

```yaml
#cloud-config
hostname: myhostname
```

Teraz, utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) i określ plik init chmury z `--custom-data cloud_init_hostname.txt` w następujący sposób:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMHostname \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_hostname.txt
```

Po utworzeniu interfejsu wiersza polecenia Azure zawiera informacje o maszynie Wirtualnej. Użyj `publicIpAddress` do SSH do maszyny Wirtualnej. Wprowadź własne adres e-mail w następujący sposób:

```bash
ssh azureuser@publicIpAddress
```

Aby wyświetlić nazwę maszyny Wirtualnej, użyj `hostname` polecenia w następujący sposób:

```bash
hostname
```

Maszyna wirtualna Zgłoś nazwa hosta jako tego wartość ustawiona w pliku chmury init, jak pokazano w poniższym przykładzie danych wyjściowych:

```bash
myhostname
```

## <a name="update-a-vm-with-cloud-init"></a>Aktualizacja maszyny Wirtualnej z inicjowaniem chmury
Ze względów bezpieczeństwa można skonfigurować Maszynę wirtualną do zastosowania najnowszych aktualizacji po pierwszym uruchomieniu komputera. Jak chmury init działa za pośrednictwem różnych dystrybucjach systemu Linux, nie istnieje potrzeba do określenia `apt` lub `yum` Menedżera pakietów. Zamiast tego należy zdefiniować `package_upgrade` i pozwolić, aby ustalić odpowiedni mechanizm distro używany proces init chmury. Ten przepływ pracy umożliwia przy użyciu tego samego skrypty chmurze init między dystrybucjach.

Aby zobaczyć procesu uaktualniania w akcji, należy utworzyć plik init chmury o nazwie *cloud_init_upgrade.txt* i wklej następującą konfigurację:

```yaml
#cloud-config
package_upgrade: true
```

Teraz, utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) i określ plik init chmury z `--custom-data cloud_init_upgrade.txt` w następujący sposób:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUpgrade \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_upgrade.txt
```

SSH jako publiczny adres IP wyświetlany w danych wyjściowych z poprzedniego polecenia maszyny wirtualnej. Wprowadź własne publicznego adresu IP w następujący sposób:

```bash
ssh azureuser@publicIpAddress
```

Uruchom pakiet narzędzia do zarządzania i sprawdzają aktualizacje. W poniższym przykładzie użyto `apt-get` na maszynie Wirtualnej systemu Ubuntu:

```bash
sudo apt-get upgrade
```

Ponieważ init chmury sprawdzane pod kątem i zainstalować aktualizacje na rozruchu, nie ma żadnych aktualizacji do zastosowania, jak pokazano w poniższym przykładzie danych wyjściowych:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Dodawanie użytkownika do maszyny Wirtualnej z inicjowaniem chmury
Jedno z pierwszym zadań na nowej maszyny Wirtualnej z systemem Linux jest dodanie użytkownika dla siebie Unikaj używania *głównego*. Najlepszym rozwiązaniem w zakresie bezpieczeństwa i użyteczność są klucze SSH. Klucze są dodawane do *~/.ssh/authorized_keys* plików za pomocą tego skryptu init chmury.

Aby dodać użytkownika do maszyny Wirtualnej systemu Linux, Utwórz plik init chmury o nazwie *cloud_init_add_user.txt* i wklej następującą konfigurację. Podaj klucza publicznego (takie jak zawartość *~/.ssh/id_rsa.pub*) dla *ssh autoryzowany kluczy*:

```yaml
#cloud-config
users:
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```

Teraz, utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) i określ plik init chmury z `--custom-data cloud_init_add_user.txt` w następujący sposób:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUser \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_add_user.txt
```

SSH jako publiczny adres IP wyświetlany w danych wyjściowych z poprzedniego polecenia maszyny wirtualnej. Wprowadź własne publicznego adresu IP w następujący sposób:

```bash
ssh myadminuser@publicIpAddress
```

Aby upewnić się, że użytkownik został dodany do maszyny Wirtualnej i określonych grup, Wyświetl zawartość */etc/grupy* plików w następujący sposób:

```bash
cat /etc/group
```

Następujące przykładowe dane wyjściowe wyglądają użytkownika z *cloud_init_add_user.txt* plik został dodany do maszyny Wirtualnej i odpowiednie grupy:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Następne kroki
Init chmury jest jednym z standardowe sposoby modyfikowania maszyn wirtualnych systemu Linux na rozruchu. Na platformie Azure umożliwia także rozszerzeń maszyny Wirtualnej można zmodyfikować maszyny Wirtualnej systemu Linux przy rozruchu lub po jej uruchomieniu. Na przykład służy rozszerzenia maszyny Wirtualnej platformy Azure do uruchomienia skryptu na uruchomionej maszynie Wirtualnej, nie tylko na najpierw uruchomić. Aby uzyskać więcej informacji na temat rozszerzeń maszyny Wirtualnej, zobacz [wirtualna rozszerzeń i funkcji](extensions-features.md), lub przykłady dotyczące sposobu używania rozszerzenia można znaleźć [Zarządzanie użytkownikami, SSH, sprawdź i napraw dyski na maszynach wirtualnych systemu Linux platformy Azure przy użyciu rozszerzenia VMAccess](using-vmaccess-extension.md).