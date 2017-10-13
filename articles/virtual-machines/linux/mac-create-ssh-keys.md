---
title: "Tworzenie i używanie pary kluczy SSH dla maszyn wirtualnych z systemem Linux na platformie Azure | Microsoft Docs"
description: "Jak utworzyć parę publicznych i prywatnych kluczy SSH dla maszyn wirtualnych z systemem Linux na platformie Azure i używać ich w celu zwiększenia bezpieczeństwa procesu uwierzytelniania."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 6fcdcc96c7762e2362aebf909ef25f4a5ab62f99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Jak utworzyć parę publicznych i prywatnych kluczy SSH dla maszyn wirtualnych z systemem Linux i używać ich
Para kluczy Secure Shell (SSH) umożliwia tworzenie na platformie Azure maszyn wirtualnych, które używają kluczy SSH do uwierzytelniania, eliminując konieczność logowania przy użyciu haseł. W tym artykule przedstawiono sposób szybkiego generowania i używania pary plików prywatnych i publicznych kluczy RSA protokołu SSH w wersji 2 dla maszyn wirtualnych z systemem Linux. Aby uzyskać bardziej szczegółowe instrukcje i dodatkowe przykłady, zobacz [szczegółowe instrukcje dotyczące tworzenia par kluczy SSH i certyfikatów](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH
Przy użyciu polecenia `ssh-keygen` utwórz pliki publicznych i prywatnych kluczy SSH. Pliki te są zazwyczaj tworzone w katalogu `~/.ssh`, ale gdy zostanie wyświetlony monit, możesz określić inną lokalizację oraz dodatkowe hasło (umożliwiające dostęp do pliku klucza prywatnego). Uruchom następujące polecenie z poziomu powłoki Bash, podając własne informacje w odpowiedzi na monity.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>Używanie pary kluczy SSH
Klucz publiczny umieszczony na maszynie wirtualnej z systemem Linux na platformie Azure jest domyślnie przechowywany w katalogu `~/.ssh/id_rsa.pub`, chyba że podczas tworzenia klucza zmieniono tę lokalizację. Jeśli tworzysz maszynę wirtualną za pomocą [interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure), określ lokalizację tego klucza publicznego, używając polecenia [az vm create](/cli/azure/vm#create) z opcją `--ssh-key-path`. Pamiętaj, aby podczas kopiowania i wklejania zawartości klucza publicznego do użycia w witrynie Azure Portal lub szablonie usługi Resource Manager nie kopiować dodatkowych spacji. Jeśli na przykład używasz systemu operacyjnego OS X, możesz potokować plik klucza publicznego (domyślnie **~/.ssh/id_rsa.pub**) do programu **pbcopy**, aby skopiować jego zawartość (dostępne są inne programy dla systemu Linux, które robią to samo, na przykład `xclip`).

Jeśli nie znasz kluczy publicznych SSH, możesz wyświetlić swój klucz publiczny, uruchamiając polecenie `cat` w następujący sposób i zastępując ścieżkę `~/.ssh/id_rsa.pub` lokalizacją własnego pliku klucza publicznego:

```bash
cat ~/.ssh/id_rsa.pub
```

Mając klucz publiczny na maszynie wirtualnej platformy Azure, nawiąż połączenie z maszyną wirtualną przez protokół SSH przy użyciu adresu IP lub nazwy DNS tej maszyny (pamiętaj, aby zastąpić poniższe ciągi `azureuser` i `myvm.westus.cloudapp.azure.com` nazwą użytkownika administratora i w pełni kwalifikowaną nazwą domeny lub adresem IP):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Jeśli podczas tworzenia pary kluczy podano hasło, wprowadź je, gdy podczas logowania zostanie wyświetlony odpowiedni monit. (Serwer zostanie dodany do folderu `~/.ssh/known_hosts` i monit o ponowne połączenie nie zostanie wyświetlony, dopóki klucz publiczny na maszynie wirtualnej nie ulegnie zmianie lub nazwa serwera nie zostanie usunięta z folderu `~/.ssh/known_hosts`).

## <a name="next-steps"></a>Następne kroki

W domyślnej konfiguracji maszyn wirtualnych utworzonych przy użyciu kluczy SSH hasła są wyłączone, aby próby odgadnięcia hasła za pomocą ataków siłowych były znacznie bardziej kosztowne, a przez to również trudniejsze. W tym temacie opisano tworzenie prostej pary kluczy SSH do szybkiego używania. Jeśli potrzebujesz więcej pomocy przy tworzeniu pary kluczy SSH lub potrzebujesz dodatkowych certyfikatów, zobacz [Szczegółowe instrukcje dotyczące tworzenia par kluczy SSH i certyfikatów](create-ssh-keys-detailed.md).

Maszyny wirtualne korzystające z pary kluczy SSH możesz tworzyć za pomocą witryny Azure Portal, interfejsu wiersza polecenia oraz szablonów:

* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu witryny Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
