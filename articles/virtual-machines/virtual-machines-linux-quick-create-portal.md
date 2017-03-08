---
title: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 1/17/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: beff4fb41ed46b016088734054e7a7897fed1a30
ms.openlocfilehash: 7287b87b1e50e28de06a5363a1f35bd7ac34d51c
ms.lasthandoff: 02/15/2017


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu portalu
W tym artykule przedstawiono sposób użycia [witryny Azure Portal](https://portal.azure.com/) do utworzenia maszyny wirtualnej z systemem Linux.

Wymagania są następujące:

* [Konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Pliki kluczy publicznych i prywatnych SSH](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sign-in"></a>Logowanie
Zaloguj się do witryny Azure Portal przy użyciu tożsamości konta platformy Azure. Kliknij pozycję **+ Nowy** w lewym górnym rogu:

![Tworzenie zasobu platformy Azure](./media/virtual-machines-linux-quick-create-portal/create_new_resource.png)

## <a name="choose-vm"></a>Wybieranie maszyny wirtualnej
Kliknij pozycję **Compute** w obszarze **Marketplace**, a następnie wybierz pozycję **Ubuntu Server 16.04 LTS** z listy obrazów **Polecane aplikacje**.  Sprawdź u dołu, czy model wdrażania to `Resource Manager`, a następnie kliknij przycisk **Utwórz**.

![Wybieranie obrazu maszyny wirtualnej w witrynie Azure Marketplace](./media/virtual-machines-linux-quick-create-portal/create_new_vm.png)

## <a name="enter-vm-options"></a>Wprowadzanie opcji maszyny wirtualnej
Na stronie **Podstawowe** wprowadź:

* nazwę maszyny wirtualnej,
* typ dysku maszyny wirtualnej (domyślny typ SSD albo HDD),
* nazwę użytkownika dla administratora,
* **Typ uwierzytelniania** o wartości **Klucz publiczny SSH**,
* klucz publiczny SSH w formie ciągu (z katalogu `~/.ssh/`),
* nazwę grupy zasobów lub wybierz istniejącą grupę zasobów,

a następnie kliknij przycisk **OK**, aby kontynuować. Blok powinien wyglądać podobnie jak na następującym zrzucie ekranu:

![Wprowadzanie podstawowych opcji maszyny wirtualnej platformy Azure](./media/virtual-machines-linux-quick-create-portal/enter_basic_vm_details.png)

## <a name="choose-vm-size"></a>Wybieranie rozmiaru maszyny wirtualnej
Wybierz rozmiar maszyny wirtualnej. W poniższych przykładach wybrano rozmiar **DS1_V2 Standardowa**, co spowoduje zainstalowanie systemu Ubuntu na dysku Premium SSD. Litera **S** w rozmiarze maszyny wirtualnej oznacza obsługę dysków SSD. Kliknij pozycję **Wybierz**, aby skonfigurować ustawienia.

![Wybieranie rozmiaru maszyny wirtualnej platformy Azure](./media/virtual-machines-linux-quick-create-portal/select_vm_size.png)

## <a name="storage-and-network"></a>Magazyn i sieć
W bloku **Ustawienia** można wybrać opcję użycia dysków Azure Managed Disks dla maszyny wirtualnej. Bieżące ustawienie domyślne to użycie dysków niezarządzanych. Dyski Azure Managed Disks są obsługiwane przez platformę Azure, a do ich przechowywania nie jest wymagane żadne przygotowanie ani lokalizacja. Aby uzyskać więcej informacji o dyskach Azure Managed Disks, zobacz [Omówienie usługi Azure Managed Disks](../storage/storage-managed-disks-overview.md). W przypadku dysków niezarządzanych musisz utworzyć lub wybrać konto magazynu dla wirtualnych dysków twardych:

![Wybieranie konta magazynu dla dysków niezarządzanych](./media/virtual-machines-linux-quick-create-portal/configure_non_managed_disks.png)

Jeśli wybierzesz dyski Azure Managed Disks, do skonfigurowania nie będzie żadnych dodatkowych opcji magazynu, tak jak to pokazano na poniższym przykładzie:

![Wybieranie opcji Azure Managed Disks w portalu](./media/virtual-machines-linux-quick-create-portal/select_managed_disks.png)

Dla reszty ustawień sieci pozostaw opcje domyślne.

## <a name="confirm-vm-settings-and-launch"></a>Potwierdzanie ustawień maszyny wirtualnej i jej uruchamianie
Potwierdź ustawienia dla nowej maszyny wirtualnej z systemem Ubuntu i kliknij przycisk **OK**.

![Przeglądanie ustawień maszyny wirtualnej platformy Azure i tworzenie maszyny wirtualnej](./media/virtual-machines-linux-quick-create-portal/review_final_vm_settings.png)

## <a name="select-the-vm-resource"></a>Wybieranie zasobu maszyny wirtualnej
Otwórz stronę główną witryny Azure Portal i wybierz pozycję **Grupy zasobów** z menu znajdującego się w lewym górnym rogu. W razie potrzeby kliknij trzy paski w górnej części menu, aby rozwinąć listę w następujący sposób:

![Otwieranie listy grup zasobów](./media/virtual-machines-linux-quick-create-portal/select_resource_group.png)

Wybierz grupę zasobów, a następnie kliknij nową maszynę wirtualną:

![Znajdowanie ustawień karty sieciowej maszyny wirtualnej platformy Azure](./media/virtual-machines-linux-quick-create-portal/select_vm_resource.png)

## <a name="find-the-public-ip"></a>Znajdowanie publicznego adresu IP
Wyświetl **Publiczny adres IP** przypisany do maszyny wirtualnej:

![Uzyskiwanie publicznego adresu IP maszyny wirtualnej platformy Azure](./media/virtual-machines-linux-quick-create-portal/view_public_ip_address.png)

## <a name="ssh-to-the-vm"></a>Łączenie przez protokół SSH z maszyną wirtualną
Nawiąż połączenie z publicznym adresem IP, korzystając z protokołu SSH oraz publicznego klucza SSH.  Na stacjach roboczych Mac lu z systemem Linux możesz połączyć się przez protokół SSH bezpośrednio z terminala. Jeśli pracujesz na stacji roboczej z systemem Windows, musisz użyć programu PuTTY, MobaXTerm lub Cygwin, aby połączyć się przez protokół SSH z systemem Linux.  Jeśli nie masz jeszcze odpowiedniego oprogramowania, oto dokument omawiający przygotowywanie komputera z systemem Windows do nawiązywania połączeń przez protokół SSH z systemem Linux.

[Jak używać kluczy protokołu SSH w systemie Windows na platformie Azure](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```
ssh -i ~/.ssh/azure_id_rsa ops@40.112.255.214
```

## <a name="next-steps"></a>Następne kroki
Szybko utworzono maszynę wirtualną z systemem Linux na potrzeby testowania lub demonstracji. Aby utworzyć maszynę wirtualną z systemem Linux dostosowaną do Twojej infrastruktury, możesz skorzystać z dowolnego z tych artykułów.

* [Create a Linux VM on Azure using Templates](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu szablonów)
* [Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Create a Linux VM using the Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure)


