---
title: "Azure skryptu interfejsu wiersza polecenia przykładowe — obliczania rozmiaru kontenera obiektu blob | Dokumentacja firmy Microsoft"
description: "Obliczania rozmiaru kontenera w magazynie obiektów Blob Azure przez sumowanie rozmiar obiektów blob w kontenerze."
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
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 86922fac2154d4084f34275e761addeafe565fc0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Obliczania rozmiaru kontenera magazynu obiektów Blob

Ten skrypt oblicza rozmiar kontenera w magazynie obiektów Blob Azure przez sumowanie rozmiar obiektów blob w kontenerze.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Ten skrypt interfejsu wiersza polecenia zawiera szacowany rozmiar kontenera i nie powinna być używana związanych z rozliczeniami obliczeń.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, kontenera i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do obliczania rozmiaru kontenera magazynu obiektów Blob. Każdy element w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Przekazywanie obiektu blob magazynu az](/cli/azure/storage/account#create) | Przekazuje lokalnych plików do kontenera magazynu obiektów Blob platformy Azure. |
| [AZ magazynu obiektów blob listy](/cli/azure/storage/account/keys#list) | Wyświetla listę obiektów blob w kontenerze magazynu obiektów Blob platformy Azure. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Przykłady skryptów dodatkowego magazynu interfejsu wiersza polecenia można znaleźć w [przykłady wiersza polecenia platformy Azure dla magazynu obiektów Blob Azure](../blobs/storage-samples-blobs-cli.md).
