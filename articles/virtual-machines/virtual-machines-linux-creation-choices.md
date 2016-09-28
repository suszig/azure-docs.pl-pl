<properties
    pageTitle="Różne sposoby tworzenia maszyn wirtualnych systemu Linux | Microsoft Azure"
    description="Wyświetla listę różnych sposobów tworzenia maszyny wirtualnej systemu Linux na platformie Azure i zawiera linki do narzędzi i samouczków dotyczących poszczególnych metod."
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
    ms.date="07/06/2016"
    ms.author="iainfou"/>


# Różne sposoby tworzenia maszyny wirtualnej systemu Linux przy użyciu usługi Resource Manager

Platforma Azure oferuje różne sposoby tworzenia maszyn wirtualnych przy użyciu modelu wdrażania opartego na usłudze Resource Manager, które są dopasowane do różnych użytkowników i celów. Ten artykuł zawiera podsumowanie tych różnic oraz wyborów, których możesz dokonać podczas tworzenia maszyn wirtualnych systemu Linux.

## Interfejs wiersza polecenia platformy Azure 

Interfejs wiersza polecenia platformy Azure jest dostępny na wielu platformach przy użyciu pakietów menedżera npm, pakietów dostępnych dla określonych dystrybucji lub kontenera Docker. Jeśli chcesz, możesz uzyskać więcej informacji na temat [sposobu instalowania i konfigurowania interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md). Poniższe samouczki zawierają przykłady dotyczące używania interfejsu wiersza polecenia platformy Azure. Zapoznaj się z poszczególnymi artykułami, aby uzyskać więcej informacji na temat przedstawionych poleceń Szybki start interfejsu wiersza polecenia:

* [Tworzenie maszyny wirtualnej systemu Linux z poziomu interfejsu wiersza polecenia platformy Azure w celach programistycznych i testowych](virtual-machines-linux-quick-create-cli.md)

    ```bash
    azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
    ```

* [Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

* [Tworzenie maszyny wirtualnej systemu Linux od podstaw przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md)

* [Dodawanie dysku do maszyny wirtualnej z systemem Linux](virtual-machines-linux-add-disk.md)

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM <size-in-GB>
    ```

## Azure Portal

Graficzny interfejs użytkownika witryny [Azure Portal](https://portal.azure.com) zapewnia łatwy sposób na wypróbowanie maszyn wirtualnych, szczególnie w przypadku, gdy dopiero zaczynasz pracę z platformą Azure, ponieważ nie wymaga się instalowania czegokolwiek w systemie. Użyj witryny Azure Portal, aby utworzyć maszynę wirtualną:

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu portalu Azure](virtual-machines-linux-quick-create-portal.md) 
* [Dołączanie dysku przy użyciu portalu Azure](virtual-machines-linux-attach-disk-portal.md)

## Wybór systemu operacyjnego i obrazu
Podczas tworzenia maszyny wirtualnej możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz też przekazać jeden z własnych obrazów (patrz poniżej).

### Obrazy platformy Azure
Za pomocą poleceń `azure vm image` interfejsu wiersza polecenia można wyświetlić dostępne obrazy według wydawcy, wersji dystrybucji i kompilacji.

Aby wyświetlić dostępnych wydawców:

```bash
azure vm image list-publishers --location WestUS
```

Aby wyświetlić dostępne produkty (oferty) dla wybranego wydawcy:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Aby wyświetlić dostępne jednostki SKU (wersje dystrybucji) dla danej oferty:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Aby wyświetlić dostępne obrazy dla danego wydania:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Zobacz [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure), aby uzyskać więcej przykładów związanych z przeglądaniem i wykorzystywaniem dostępnych obrazów.

Polecenia `azure vm quick-create` i `azure vm create` mają również aliasy umożliwiające szybki dostęp do najpopularniejszych dystrybucji i ich najnowszych wersji. Jest to łatwiejsze niż określanie wydawcy, oferty, jednostki SKU i wersji za każdym razem podczas tworzenia maszyny wirtualnej:

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

Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez *przechwycenie* tej maszyny wirtualnej lub przesłać własny obraz przechowywany na wirtualnym dysku twardym (VHD). Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

* [Dystrybucje zatwierdzone na platformie Azure](virtual-machines-linux-endorsed-distros.md)

* [Informacje dotyczące niezatwierdzonych dystrybucji](virtual-machines-linux-create-upload-generic.md)

* [How to capture a Linux virtual machine as a Resource Manager template](virtual-machines-linux-capture-image.md) (Jak przechwycić maszynę wirtualną systemu Linux jako szablon usługi Resource Manager). Polecenia Szybki start:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## Następne kroki

* Wypróbuj samouczki, aby utworzyć maszynę wirtualną systemu Linux przy użyciu [portalu](virtual-machines-linux-quick-create-portal.md), interfejsu [wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md) lub [szablonu](virtual-machines-linux-cli-deploy-templates.md) usługi Azure Resource Manager.

* Po utworzeniu maszyny wirtualnej systemu Linux możesz z łatwością [dodać dysk danych](virtual-machines-linux-add-disk.md).

* Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Sep16_HO3-->


