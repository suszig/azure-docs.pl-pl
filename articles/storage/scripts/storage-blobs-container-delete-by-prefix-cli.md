---
title: "Azure CLI przykładowym skrypcie - Usuń kontenery według prefiksu | Dokumentacja firmy Microsoft"
description: "Usuń kontenerów obiektów blob magazynu Azure na podstawie prefiksu nazwy kontenera."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: marsma
ms.openlocfilehash: ff30037c7aba4e0e9c6b4a1829a0769093dce0ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Usuń kontenerów na podstawie prefiksu nazwy kontenera

Ten skrypt najpierw tworzy kilka przykładowych kontenerów w magazynie obiektów Blob platformy Azure, a następnie usuwa niektóre kontenerów na podstawie prefiksu nazwy kontenera.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, pozostałe kontenerów, i wszystkich powiązanych zasobów.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do usuwania kontenerów na podstawie prefiksu nazwy kontenera. Każdy element w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta magazynu az](/cli/azure/storage/account#create) | Utworzenie konta usługi Azure Storage w określonej grupy zasobów. |
| [Tworzenie kontenera magazynu az](/cli/azure/storage/container#create) | Tworzy kontener magazynu obiektów Blob platformy Azure. |
| [Lista kontenera magazynu az](/cli/azure/storage/container#list) | Wyświetla kontenery konta magazynu Azure. |
| [usunięcie kontenera magazynu az](/cli/azure/storage/container#delete) | Usuwa kontenery konta magazynu Azure. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Przykłady skryptów dodatkowego magazynu interfejsu wiersza polecenia można znaleźć w [próbki wiersza polecenia platformy Azure do usługi Azure Storage](../blobs/storage-samples-blobs-cli.md).
