---
title: "Bash w chmurze Azure powłoki (wersja zapoznawcza) — Szybki Start | Dokumentacja firmy Microsoft"
description: "Szybki Start dla Bash w powłoce chmury"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: bbad2735acf06ebefda020410731fb307c4ec614
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Szybki Start dla Bash w chmurze Azure powłoki

Ten dokument zawiera szczegóły dotyczące używania Bash w powłoce chmury Azure w [portalu Azure](https://ms.portal.azure.com/).

> [!NOTE]
> A [programu PowerShell w powłoce chmury Azure](quickstart-powershell.md) szybkiego startu jest również dostępna.

## <a name="start-cloud-shell"></a>Uruchom powłokę chmury
1. Uruchom **powłoki chmury** w górnym menu nawigacyjnym portalu Azure <br>
![](media/quickstart/shell-icon.png)
2. Wybierz subskrypcję, aby utworzyć konto magazynu i udziału plików na platformę Azure
3. Wybierz opcję "Utwórz magazyn"

> [!TIP]
> Azure CLI 2.0 są automatycznie uwierzytelniani w każdym sesssion.

### <a name="select-the-bash-environment"></a>Wybierz środowisko Bash
1. Wybierz środowisko rozwijanej z lewej strony okna powłoki <br>
![](media/quickstart/env-selector.png)
2. Wybierz Bash

### <a name="set-your-subscription"></a>Ustaw swoją subskrypcję
1. Subskrypcje listy, do których masz dostęp do: <br>
`az account list`
2. Ustaw preferowany subskrypcji: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Subskrypcja zostanie zapamiętany dla przyszłych sesji przy użyciu `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz nową grupę zasobów w WestUS o nazwie "MyRG": <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Tworzenie maszyny wirtualnej z systemem Linux
Tworzenie maszyny Wirtualnej systemu Ubuntu w nowej grupy zasobów. Azure CLI 2.0 tworzenie kluczy SSH, a następnie skonfigurować maszynę Wirtualną z nimi. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> Klucze publiczne i prywatne używane do uwierzytelniania maszyny Wirtualnej są umieszczane w `/User/.ssh/id_rsa` i `/User/.ssh/id_rsa.pub` przez Azure CLI 2.0 domyślnie. Folderu .ssh jest utrwalona w obrazie 5 GB na udział dołączonych plików na platformę Azure.

Nazwa użytkownika na tej maszynie Wirtualnej zostanie nazwy użytkownika używane w chmurze powłoki ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH do maszyny Wirtualnej systemu Linux
1. Wyszukaj nazwę maszyny Wirtualnej na pasku wyszukiwania portalu Azure
2. Kliknij przycisk "Połącz", a następnie uruchom:`ssh username@ipaddress`

![](media/quickstart/sshcmd-copy.png)

Podczas ustanawiania połączenia SSH, powinien zostać wyświetlony monit powitalnej Ubuntu. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Czyszczenie 
Usunięcie grupy zasobów i wszystkie zasoby w niej: <br>
Uruchom polecenie `az group delete -n MyRG`

## <a name="next-steps"></a>Następne kroki
[Więcej informacji na temat plików utrwalanie dla Bash w powłoce chmury](persisting-shell-storage.md) <br>
[Więcej informacji na temat usługi Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Więcej informacji na temat usługi Magazyn plików Azure](../storage/files/storage-files-introduction.md) <br>