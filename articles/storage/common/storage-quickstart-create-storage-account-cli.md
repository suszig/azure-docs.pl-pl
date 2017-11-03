---
title: "Szybki Start Azure — Tworzenie konta magazynu przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się szybko utworzyć nowe konto magazynu przy użyciu wiersza polecenia platformy Azure."
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
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: b1fb2da4acf6e06219d790f2354cada4f1e34285
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Utwórz konto magazynu przy użyciu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. Szczegóły tego szybkiego startu Tworzenie konta usługi Azure Storage za pomocą wiersza polecenia platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Grupę zasobów platformy Azure można utworzyć za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. W tym przykładzie tworzy grupę zasobów o nazwie *myResourceGroup* w *eastus* regionu.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Jeśli masz pewności, który region, aby określić dla `--location` parametru, można pobrać listę obsługiwanych regionów dla subskrypcji z [konta az listy lokalizacje](/cli/azure/account#list) polecenia.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Tworzenie konta standardowe magazynu ogólnego przeznaczenia

Istnieje kilka typów kont magazynu są odpowiednie do użycia różnych scenariuszy, z których każdy obsługuje jeden lub więcej usług magazynowania (obiekty BLOB, plików, tabel lub kolejek). W poniższej tabeli przedstawiono typy kont magazynu dostępne.

|**Typ konta magazynu**|**Przeznaczenie ogólne w warstwie Standardowa**|**Przeznaczenie ogólne w warstwie Premium**|**Usługa Blob Storage w gorącej i chłodnej warstwie dostępu**|
|-----|-----|-----|-----|
|**Obsługiwane usługi**| Obiekt blob, plików, tabeli, kolejki usług | Usługa Obiekty Blob | Usługa Obiekty Blob|
|**Obsługiwane typy obiektów blob**|Blokowe i stronicowe obiekty BLOB, uzupełnialne. | Stronicowe obiekty blob | Blokowe obiekty blob i uzupełnialne obiekty blob|

Do utworzenia standardowego konta magazynu (ogólnego przeznaczenia) służy polecenie [az storage account create](/cli/azure/storage/account#create).

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie są już potrzebne zasobów w grupie zasobów, tym na koncie magazynu utworzonym w tego przewodnika Szybki Start, Usuń grupę zasobów z [az usunięcie grupy](/cli/azure/group#delete) polecenia.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tego przewodnika Szybki Start utworzyć grupę zasobów i konto magazynu ogólnego przeznaczenia standardowe. Aby dowiedzieć się, jak na przesyłanie danych do i z konta magazynu, nadal magazynu obiektów Blob Szybki Start.

> [!div class="nextstepaction"]
> [Obiekty transferu do i z magazynu obiektów Blob platformy Azure przy użyciu wiersza polecenia platformy Azure](../blobs/storage-quickstart-blobs-cli.md)