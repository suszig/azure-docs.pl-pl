---
title: Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal | Microsoft Docs
description: Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: v-livech

---
# Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu portalu
W tym artykule przedstawiono sposób użycia [portalu Azure](https://portal.azure.com/) do szybkiego utworzenia maszyny wirtualnej z systemem Linux. Jedynymi wymogami są: [konto Azure](https://azure.microsoft.com/pricing/free-trial/) i [pliki publicznego i prywatnego klucza SSH](virtual-machines-linux-mac-create-ssh-keys.md).

1. Po zalogowaniu w portalu Azure za pomocą tożsamości konta Azure kliknij przycisk **+ Nowy** w lewym górnym rogu:
   
    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)
2. Kliknij pozycję **Virtual Machines** w obszarze **Marketplace**, a następnie wybierz pozycję **Ubuntu Server 14.04 LTS** z listy obrazów **Polecane aplikacje**.  Sprawdź u dołu, czy model wdrażania to `Resource Manager`, a następnie kliknij przycisk **Utwórz**.
   
    ![ekran2](../media/virtual-machines-linux-quick-create-portal/screen2.png)
3. Na stronie **Podstawowe** wprowadź:
   
   * nazwę maszyny wirtualnej,
   * nazwę użytkownika dla administratora,
   * typ uwierzytelniania **Klucz publiczny SSH**,
   * klucz publiczny SSH w formie ciągu (z katalogu `~/.ssh/`),
   * nazwę grupy zasobów lub wybierz istniejącą grupę
     
     Następnie kliknij przycisk **OK**, aby kontynuować i wybierz rozmiar maszyny wirtualnej. Powinno to wyglądać podobnie do poniższego obrazu:
     
     ![ekran3](../media/virtual-machines-linux-quick-create-portal/screen3.png)
4. Wybierz rozmiar **DS1**, co spowoduje zainstalowanie systemu Ubuntu na dysku Premium SSD, a następnie kliknij przycisk **Wybierz**, aby skonfigurować ustawienia.
   
    ![ekran4](../media/virtual-machines-linux-quick-create-portal/screen4.png)
5. W sekcji **Ustawienia** pozostaw wartości domyślne dla opcji Usługa Storage i Sieć, a następnie kliknij przycisk **OK**, aby wyświetlić podsumowanie.  Zauważ, że typ dysku został ustawiony na Premium SSD w związku z wyborem opcji DS1. Litera **S** oznacza dysk SSD.
   
    ![ekran5](../media/virtual-machines-linux-quick-create-portal/screen5.png)
6. Potwierdź ustawienia dla nowej maszyny wirtualnej z systemem Ubuntu i kliknij przycisk **OK**.
   
    ![ekran6](../media/virtual-machines-linux-quick-create-portal/screen6.png)
7. Otwórz pulpit nawigacyjny portalu i w sekcji **Interfejsy sieciowe** wybierz swoją kartę sieciową.
   
    ![ekran7](../media/virtual-machines-linux-quick-create-portal/screen7.png)
8. Otwórz menu Publiczne adresy IP w ustawieniach karty sieciowej.
   
    ![ekran8](../media/virtual-machines-linux-quick-create-portal/screen8.png)
9. Nawiąż połączenie z publicznym adresem IP, korzystając z protokołu SSH oraz publicznego klucza SSH.

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## Następne kroki
Szybko utworzono maszynę wirtualną z systemem Linux na potrzeby testowania lub demonstracji. Aby utworzyć maszynę wirtualną z systemem Linux dostosowaną do Twojej infrastruktury, możesz skorzystać z dowolnego z tych artykułów.

* [Create a Linux VM on Azure using Templates (Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu szablonów)](virtual-machines-linux-cli-deploy-templates.md)
* [Create an SSH Secured Linux VM on Azure using Templates (Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów)](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Create a Linux VM using the Azure CLI (Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure)](virtual-machines-linux-create-cli-complete.md)

<!--HONumber=Sep16_HO3-->


