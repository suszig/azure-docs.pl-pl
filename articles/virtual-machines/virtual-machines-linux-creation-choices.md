<properties
    pageTitle="Różne sposoby tworzenia maszyn wirtualnych systemu Linux | Microsoft Azure"
    description="Wyświetla listę różnych sposobów tworzenia maszyny wirtualnej systemu Linux na platformie Azure i zawiera linki do dalszych instrukcji"
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
    ms.date="04/12/2016"
    ms.author="iainfou"/>

# Różne sposoby tworzenia maszyny wirtualnej systemu Linux przy użyciu usługi Resource Manager

Platforma Azure oferuje różne sposoby tworzenia maszyn wirtualnych przy użyciu modelu wdrażania opartego na usłudze Resource Manager, które są dopasowane do różnych użytkowników i celów. Ten artykuł zawiera podsumowanie tych różnic oraz wyborów, których możesz dokonać podczas tworzenia maszyn wirtualnych systemu Linux.

## Wybory dotyczące narzędzi

### Powłoka poleceń: interfejs wiersza polecenia platformy Azure 

Z poziomu interfejsu wiersza polecenia użyj interfejsu wiersza polecenia platformy Azure. Możesz przeczytać więcej informacji o [instalowaniu interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) za pomocą pakietu npm, kontenera Docker lub skryptu instalacji. Następujące samouczki zawierają przykłady dotyczące używania interfejsu wiersza polecenia platformy Azure:

* [Tworzenie maszyny wirtualnej systemu Linux z poziomu interfejsu wiersza polecenia platformy Azure w celach programistycznych i testowych](virtual-machines-linux-quick-create-cli.md) 

* [Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

* [Tworzenie maszyny wirtualnej systemu Linux od podstaw przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-create-cli-complete.md)

### Graficzny interfejs użytkownika: Portal Azure

Graficzny interfejs użytkownika [Portalu Azure](https://portal.azure.com) zapewnia łatwy sposób na wypróbowanie maszyn wirtualnych, szczególnie w przypadku, gdy dopiero zaczynasz pracę z platformą Azure, ponieważ nie wymaga się instalowania czegokolwiek w systemie. Użyj Portalu Azure, aby utworzyć maszynę wirtualną:

* [Tworzenie maszyny wirtualnej z systemem Linux za pomocą Portalu Azure](virtual-machines-linux-quick-create-portal.md) 

## Wybór systemu operacyjnego i obrazu

Za pomocą obu metod możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz też przekazać jeden z własnych obrazów.

### Obrazy platformy Azure

We wszystkich powyższych artykułach możesz z łatwością użyć istniejącego obrazu platformy Azure, aby utworzyć maszynę wirtualną i skonfigurować ją pod kątem łączności sieciowej, równoważenia obciążenia i innych ustawień. Portal zawiera usługę Azure Marketplace do obsługi dostarczanych obrazów Azure. Możesz uzyskać podobne listy przy użyciu wiersza polecenia. Na przykład w interfejsie wiersza polecenia platformy Azure uruchom polecenie `azure vm image list`, aby uzyskać listę wszystkich dostępnych obrazów, zorganizowanych według lokalizacji i wydawcy. Zobacz [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure), aby uzyskać przykłady związane z przeglądaniem i wykorzystywaniem dostępnych obrazów.

### Użycie własnego obrazu

Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez *przechwycenie* tej maszyny wirtualnej lub przesłać własny obraz przechowywany na wirtualnym dysku twardym (VHD). Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

* [Dystrybucje zatwierdzone na platformie Azure](virtual-machines-linux-endorsed-distros.md)

* [Informacje dotyczące niezatwierdzonych dystrybucji](virtual-machines-linux-create-upload-generic.md)

* [How to capture a Linux virtual machine as a Resource Manager template](virtual-machines-linux-capture-image.md) (Jak przechwycić maszynę wirtualną systemu Linux jako szablon usługi Resource Manager). 

## Następne kroki

* Wypróbuj samouczki, aby utworzyć maszynę wirtualną systemu Linux przy użyciu [portalu](virtual-machines-linux-quick-create-portal.md), interfejsu [wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md) lub [szablonu](virtual-machines-linux-cli-deploy-templates.md) usługi Azure Resource Manager.

* Po utworzeniu maszyny wirtualnej systemu Linux możesz z łatwością [dodać dysk danych](virtual-machines-linux-add-disk.md).

* Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Jun16_HO2-->


