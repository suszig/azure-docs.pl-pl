<properties
    pageTitle="Różne sposoby tworzenia maszyn wirtualnych systemu Linux | Microsoft Azure"
    description="Informacje na temat różnych sposobów tworzenia maszyny wirtualnej z systemem Linux na platformie Azure oraz linki do narzędzi i samouczków dotyczących poszczególnych metod."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>


# Różne sposoby tworzenia maszyny wirtualnej z systemem Linux na platformie Azure

Platforma Azure umożliwia elastyczne tworzenie maszyn wirtualnych z systemem Linux przy użyciu dowolnych narzędzi i przepływów pracy. Ten artykuł zawiera podsumowanie różnych sposobów oraz przykłady tworzenia maszyn wirtualnych z systemem Linux.


## Interfejs wiersza polecenia platformy Azure 

Interfejs wiersza polecenia platformy Azure jest dostępny na wielu platformach przy użyciu pakietów menedżera npm, pakietów dostępnych dla określonych dystrybucji lub kontenera Docker. Jeśli chcesz, możesz uzyskać więcej informacji na temat [sposobu instalowania i konfigurowania interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md). Poniższe samouczki zawierają przykłady dotyczące używania interfejsu wiersza polecenia platformy Azure. Zapoznaj się z poszczególnymi artykułami, aby uzyskać więcej informacji na temat przedstawionych poleceń Szybki start interfejsu wiersza polecenia:

- [Tworzenie maszyny wirtualnej systemu Linux z poziomu interfejsu wiersza polecenia platformy Azure w celach programistycznych i testowych](virtual-machines-linux-quick-create-cli.md)
    - W poniższym przykładzie maszyna wirtualna z systemem CoreOS jest tworzona przy użyciu klucza publicznego o nazwie `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - W poniższym przykładzie maszyna wirtualna jest tworzona przy użyciu szablonu przechowywanego w witrynie GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Tworzenie kompletnego środowiska systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md)
    - Obejmuje tworzenie modułu równoważenia obciążenia i wielu maszyn wirtualnych w zestawie dostępności.

- [Dodawanie dysku do maszyny wirtualnej z systemem Linux](virtual-machines-linux-add-disk.md)
    - W poniższym przykładzie dodano dysk o rozmiarze 5 GB do istniejącej maszyny wirtualnej o nazwie `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## Portal Azure

Witryna [Azure Portal](https://portal.azure.com) umożliwia szybkie tworzenie maszyn wirtualnych, ponieważ nie wymaga instalacji żadnych składników w systemie. Użyj witryny Azure Portal, aby utworzyć maszynę wirtualną:

- [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu portalu Azure](virtual-machines-linux-quick-create-portal.md) 
- [Dołączanie dysku przy użyciu portalu Azure](virtual-machines-linux-attach-disk-portal.md)


## Wybór systemu operacyjnego i obrazu
Podczas tworzenia maszyny wirtualnej możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz również przekazać własny obraz (więcej informacji można znaleźć w [poniższej sekcji](#use-your-own-image)).

### Obrazy platformy Azure
Użyj poleceń `azure vm image` interfejsu wiersza polecenia, aby wyświetlić dostępne obrazy według wydawcy, wersji dystrybucji i kompilacji.

Wyświetl dostępnych wydawców w następujący sposób:

```bash
azure vm image list-publishers --location WestUS
```

Wyświetl dostępne produkty (oferty) w następujący sposób:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Wyświetl dostępne jednostki SKU (wersje dystrybucji) dla danej oferty w następujący sposób:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Wyświetl dostępne obrazy dla danego wydania w następujący sposób:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Aby uzyskać więcej przykładów związanych z przeglądaniem i wykorzystywaniem dostępnych obrazów, zobacz [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure).

Polecenia `azure vm quick-create` i `azure vm create` mają aliasy umożliwiające szybki dostęp do najpopularniejszych dystrybucji i ich najnowszych wersji. Użycie aliasów jest zwykle łatwiejsze niż określanie wydawcy, oferty, jednostki SKU i wersji za każdym razem podczas tworzenia maszyny wirtualnej:

| Alias     | Wydawca | Oferta        | SKU         | Wersja |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | najnowsza  |
| CoreOS    | CoreOS    | CoreOS       | Stable      | najnowsza  |
| Debian    | credativ  | Debian       | 8           | najnowsza  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnowsza  |
| RHEL      | Redhat    | RHEL         | 7.2         | najnowsza  |
| SLES      | SLES      | SLES         | 12-SP1      | najnowsza  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | najnowsza  |

### Użycie własnego obrazu

Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez *przechwycenie* tej maszyny wirtualnej. Możesz również przekazać obraz utworzony lokalnie. Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

- [Dystrybucje zatwierdzone na platformie Azure](virtual-machines-linux-endorsed-distros.md)

- [Informacje dotyczące niezatwierdzonych dystrybucji](virtual-machines-linux-create-upload-generic.md)

- [How to capture a Linux virtual machine as a Resource Manager template](virtual-machines-linux-capture-image.md) (Jak przechwycić maszynę wirtualną systemu Linux jako szablon usługi Resource Manager).
    - Przykładowe polecenia Szybki start umożliwiające przechwycenie istniejącej maszyny wirtualnej:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## Następne kroki

- Utwórz maszynę wirtualną z systemem Linux przy użyciu [portalu](virtual-machines-linux-quick-create-portal.md), interfejsu [wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md) lub [szablonu](virtual-machines-linux-cli-deploy-templates.md) usługi Azure Resource Manager.

- Po utworzeniu maszyny wirtualnej z systemem Linux [dodaj dysk danych](virtual-machines-linux-add-disk.md).

- Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Sep16_HO4-->


