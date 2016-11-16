---
title: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/28/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b1446cd8892e14988ff428eaa03233f8e9aefb8a


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu portalu
W tym artykule przedstawiono sposób użycia [witryny Azure Portal](https://portal.azure.com/) do utworzenia maszyny wirtualnej z systemem Linux.

Wymagania są następujące:

* [Konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Pliki kluczy publicznych i prywatnych SSH](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="sign-in"></a>Logowanie
Po zalogowaniu w portalu Azure za pomocą tożsamości konta Azure kliknij przycisk **+ Nowy** w lewym górnym rogu:

![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

## <a name="choose-vm"></a>Wybieranie maszyny wirtualnej
Kliknij pozycję **Virtual Machines** w obszarze **Marketplace**, a następnie wybierz pozycję **Ubuntu Server 14.04 LTS** z listy obrazów **Polecane aplikacje**.  Sprawdź u dołu, czy model wdrażania to `Resource Manager`, a następnie kliknij przycisk **Utwórz**.

![ekran2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

## <a name="enter-vm-options"></a>Wprowadzanie opcji maszyny wirtualnej
Na stronie **Podstawowe** wprowadź:

* nazwę maszyny wirtualnej,
* nazwę użytkownika dla administratora,
* typ uwierzytelniania **Klucz publiczny SSH**,
* klucz publiczny SSH w formie ciągu (z katalogu `~/.ssh/`),
* nazwę grupy zasobów lub wybierz istniejącą grupę

i kliknij przycisk **OK**, aby kontynuować, a następnie wybierz rozmiar maszyny wirtualnej. Powinno to wyglądać podobnie do poniższego zrzutu ekranu:

![ekran3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

## <a name="choose-vm-size"></a>Wybieranie rozmiaru maszyny wirtualnej
Wybierz rozmiar **DS1**, co spowoduje zainstalowanie systemu Ubuntu na dysku Premium SSD, a następnie kliknij przycisk **Wybierz**, aby skonfigurować ustawienia.

![ekran4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

## <a name="storage-and-network"></a>Magazyn i sieć
W sekcji **Ustawienia** pozostaw wartości domyślne dla opcji Usługa Storage i Sieć, a następnie kliknij przycisk **OK**, aby wyświetlić podsumowanie.  Zauważ, że typ dysku został ustawiony na Premium SSD w związku z wyborem opcji DS1. Litera **S** oznacza dysk SSD.

![ekran5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

## <a name="confirm-vm-settings-and-launch"></a>Potwierdzanie ustawień maszyny wirtualnej i jej uruchamianie
Potwierdź ustawienia dla nowej maszyny wirtualnej z systemem Ubuntu i kliknij przycisk **OK**.

![ekran6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

## <a name="find-the-vm-nic"></a>Znajdowanie karty sieciowej maszyny wirtualnej
Otwórz pulpit nawigacyjny portalu i w sekcji **Interfejsy sieciowe** wybierz swoją kartę sieciową.

![ekran7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

## <a name="find-the-public-ip"></a>Znajdowanie publicznego adresu IP
Otwórz menu Publiczne adresy IP w ustawieniach karty sieciowej.

![ekran8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

## <a name="ssh-to-the-vm"></a>Łączenie przez protokół SSH z maszyną wirtualną
Nawiąż połączenie z publicznym adresem IP, korzystając z protokołu SSH oraz publicznego klucza SSH.  Na stacjach roboczych Mac lu z systemem Linux możesz połączyć się przez protokół SSH bezpośrednio z terminala. Jeśli pracujesz na stacji roboczej z systemem Windows, musisz użyć programu PuTTY, MobaXTerm lub Cygwin, aby połączyć się przez protokół SSH z systemem Linux.  Jeśli nie masz jeszcze odpowiedniego oprogramowania, oto dokument omawiający przygotowywanie komputera z systemem Windows do nawiązywania połączeń przez protokół SSH z systemem Linux.

[Jak używać kluczy protokołu SSH w systemie Windows na platformie Azure](virtual-machines-linux-ssh-from-windows.md)

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Następne kroki
Szybko utworzono maszynę wirtualną z systemem Linux na potrzeby testowania lub demonstracji. Aby utworzyć maszynę wirtualną z systemem Linux dostosowaną do Twojej infrastruktury, możesz skorzystać z dowolnego z tych artykułów.

* [Create a Linux VM on Azure using Templates](virtual-machines-linux-cli-deploy-templates.md) (Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu szablonów)
* [Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Create a Linux VM using the Azure CLI](virtual-machines-linux-create-cli-complete.md) (Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure)




<!--HONumber=Nov16_HO2-->


