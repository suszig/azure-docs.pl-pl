---
title: "Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — obliczanie rozmiaru kontenera obiektów blob | Microsoft Docs"
description: "Obliczanie rozmiaru kontenera w usłudze Azure Blob Storage przez zsumowanie rozmiaru obiektów blob w tym kontenerze."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: c38a49e82a71a23fdf621f5ac350c4242ffc2f8f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Obliczanie rozmiaru kontenera w usłudze Blob Storage

Ten skrypt oblicza rozmiar kontenera w usłudze Azure Blob Storage przez zsumowanie rozmiaru obiektów blob w tym kontenerze.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Ten skrypt interfejsu wiersza polecenia określa szacunkowy rozmiar kontenera i nie należy go używać w obliczeniach na potrzeby rozliczeń.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, kontener i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia, służące do obliczenia rozmiaru kontenera w usłudze Blob Storage. Każda pozycja w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az storage blob upload](/cli/azure/storage/account#az_storage_account_create) | Przekazuje pliki lokalne do kontenera w usłudze Azure Blob Storage. |
| [az storage blob list](/cli/azure/storage/account/keys#az_storage_account_keys_list) | Zwraca listę obiektów blob w kontenerze usługi Azure Blob Storage. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](/cli/azure).

Więcej przykładowych skryptów interfejsu wiersza polecenia dla usługi Storage można znaleźć na stronie [Azure CLI samples for Azure Blob Storage (Przykłady skryptów interfejsu wiersza polecenia platformy Azure dla usługi Azure Blob Storage)](../blobs/storage-samples-blobs-cli.md).
