---
title: "Azure CLI przykładowym skrypcie - kluczy dostępu do konta magazynu obracania | Dokumentacja firmy Microsoft"
description: "Tworzenie konta usługi Azure Storage, a następnie pobrać i Obróć klucze dostępu do swojego konta."
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
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: c8c99dd8663eab5b5c1c47d34f110f9dceaace6b
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Utwórz konto magazynu i obracania klucze dostępu do swojego konta

Ten skrypt tworzy konto usługi Azure Storage, wyświetla klucze dostępu do nowego konta magazynu, a następnie odnowieniu (obraca) kluczy.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, konto magazynu oraz wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do tworzenia konta magazynu i pobrać i Obróć jego klucze dostępu. Każdy element w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta magazynu az](/cli/azure/storage/account#create) | Utworzenie konta usługi Azure Storage w określonej grupy zasobów. |
| [Lista kluczy kont magazynu az](/cli/azure/storage/account/keys#list) | Wyświetla klucze dostępu do konta magazynu dla określonego konta. |
| [Odnów klucze konta magazynu az](/cli/azure/storage/account/keys#renew) | Generuje ponownie klucz dostępu do konta magazynu podstawowy lub pomocniczy. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Przykłady skryptów dodatkowego magazynu interfejsu wiersza polecenia można znaleźć w [przykłady wiersza polecenia platformy Azure dla magazynu obiektów Blob Azure](../blobs/storage-samples-blobs-cli.md).
