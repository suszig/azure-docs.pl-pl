---
title: "Tworzenie i używanie pary kluczy SSH dla maszyn wirtualnych z systemem Linux na platformie Azure | Microsoft Docs"
description: "Jak utworzyć parę publicznych i prywatnych kluczy SSH dla maszyn wirtualnych z systemem Linux na platformie Azure i używać ich w celu zwiększenia bezpieczeństwa procesu uwierzytelniania."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: ecd3a01ee5591cb09140edb1b1290ff2d4510200
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Jak utworzyć parę publicznych i prywatnych kluczy SSH dla maszyn wirtualnych z systemem Linux i używać ich
Para kluczy Secure Shell (SSH) umożliwia tworzenie na platformie Azure maszyn wirtualnych, które używają kluczy SSH do uwierzytelniania, eliminując konieczność logowania przy użyciu haseł. W tym artykule przedstawiono sposób szybkiego generowania i używania pary plików prywatnych i publicznych kluczy RSA protokołu SSH w wersji 2 dla maszyn wirtualnych z systemem Linux. Można wykonać te czynności przy użyciu usługi Azure Cloud Shell, hosta z systemem macOS lub Linux albo podsystemu Windows dla systemu Linux. Aby uzyskać bardziej szczegółowe instrukcje i dodatkowe przykłady, zobacz [szczegółowe instrukcje dotyczące tworzenia par kluczy SSH i certyfikatów](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH
Użyj `ssh-keygen` polecenie, aby utworzyć SSH publicznego i prywatnego klucza pliki, które są domyślnie tworzone w `~/.ssh` katalogu. Można określić inną lokalizację i hasło dodatkowe (hasło dostępu do pliku klucza prywatnego) po wyświetleniu monitu. Jeśli istnieje parę kluczy SSH w bieżącej lokalizacji, zostaną zastąpione.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>Używanie pary kluczy SSH
Klucz publiczny umieszczony na maszynie wirtualnej z systemem Linux na platformie Azure jest domyślnie przechowywany w katalogu `~/.ssh/id_rsa.pub`, chyba że podczas tworzenia klucza zmieniono tę lokalizację. Jeśli tworzysz maszynę wirtualną za pomocą [interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure), określ lokalizację tego klucza publicznego, używając polecenia [az vm create](/cli/azure/vm#az_vm_create) z opcją `--ssh-key-path`. Pamiętaj, aby podczas kopiowania i wklejania zawartości klucza publicznego do użycia w witrynie Azure Portal lub szablonie usługi Resource Manager nie kopiować dodatkowych spacji. Jeśli na przykład używasz systemu operacyjnego OS X, możesz potokować plik klucza publicznego (domyślnie **~/.ssh/id_rsa.pub**) do programu **pbcopy**, aby skopiować jego zawartość (dostępne są inne programy dla systemu Linux, które robią to samo, na przykład `xclip`).

Jeśli nie znasz kluczy publicznych SSH, możesz wyświetlić swój klucz publiczny, uruchamiając polecenie `cat` w następujący sposób i zastępując ścieżkę `~/.ssh/id_rsa.pub` lokalizacją własnego pliku klucza publicznego:

```bash
cat ~/.ssh/id_rsa.pub
```

Mając klucz publiczny na maszynie wirtualnej platformy Azure, nawiąż połączenie z maszyną wirtualną przez protokół SSH przy użyciu adresu IP lub nazwy DNS tej maszyny (pamiętaj, aby zastąpić poniższe ciągi `azureuser` i `myvm.westus.cloudapp.azure.com` nazwą użytkownika administratora i w pełni kwalifikowaną nazwą domeny lub adresem IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Jeśli podczas tworzenia pary kluczy podano hasło, wprowadź je, gdy podczas logowania zostanie wyświetlony odpowiedni monit. (Serwer zostanie dodany do folderu `~/.ssh/known_hosts` i monit o ponowne połączenie nie zostanie wyświetlony, dopóki klucz publiczny na maszynie wirtualnej nie ulegnie zmianie lub nazwa serwera nie zostanie usunięta z folderu `~/.ssh/known_hosts`).

## <a name="next-steps"></a>Kolejne kroki

W domyślnej konfiguracji maszyn wirtualnych utworzonych przy użyciu kluczy SSH hasła są wyłączone, aby próby odgadnięcia hasła za pomocą ataków siłowych były znacznie bardziej kosztowne, a przez to również trudniejsze. W tym temacie opisano tworzenie prostej pary kluczy SSH do szybkiego używania. Jeśli potrzebujesz więcej pomocy przy tworzeniu pary kluczy SSH lub potrzebujesz dodatkowych certyfikatów, zobacz [Szczegółowe instrukcje dotyczące tworzenia par kluczy SSH i certyfikatów](create-ssh-keys-detailed.md).

Maszyny wirtualne korzystające z pary kluczy SSH możesz tworzyć za pomocą witryny Azure Portal, interfejsu wiersza polecenia oraz szablonów:

* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu witryny Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
