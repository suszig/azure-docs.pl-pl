---
title: "Przewodnik Szybki start platformy Azure — tworzenie konta magazynu za pomocą interfejsu wiersza polecenia platformy Azure | Microsoft Docs"
description: "Szybko naucz się, jak utworzyć nowe konto magazynu przy użyciu wiersza polecenia platformy Azure."
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
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 7186c5e2ce94d06b21d95a557e960b82e268cdce
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Tworzenie konta magazynu przy użyciu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. Ten przewodnik Szybki start szczegółowo omawia tworzenie konta w usłudze Azure Storage przy użyciu wiersza polecenia platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Grupę zasobów platformy Azure można utworzyć za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. W tym przykładzie tworzona jest grupa zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Jeśli nie masz pewności, który region wskazać dla parametru `--location`, za pomocą polecenia [az account list-locations](/cli/azure/account#list) możesz uzyskać listę obsługiwanych regionów dla swojej subskrypcji.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Tworzenie standardowego konta magazynu ogólnego przeznaczenia

Istnieje kilka typów kont magazynu odpowiednich dla różnych scenariuszy użycia. Każdy z nich obsługuje co najmniej jedną usługę magazynowania (obiekty blob, pliki, tabele lub kolejki). W poniższej tabeli przedstawiono dostępne typy kont magazynu.

|**Typ konta magazynu**|**Przeznaczenie ogólne w warstwie Standardowa**|**Przeznaczenie ogólne w warstwie Premium**|**Usługa Blob Storage w gorącej i chłodnej warstwie dostępu**|
|-----|-----|-----|-----|
|**Obsługiwane usługi**| Usługi Blob, File, Table i Queue | Blob service | Blob service|
|**Obsługiwane typy obiektów blob**|Blokowe obiekty blob, stronicowe obiekty blob, uzupełnialne obiekty blob | Stronicowe obiekty blob | Blokowe obiekty blob i uzupełnialne obiekty blob|

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

Jeśli nie są już potrzebne żadne zasoby z grupy zasobów, w tym konto magazynu utworzone w ramach tego przewodnika Szybki start, usuń grupę zasobów za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start została utworzona grupa zasobów i standardowe konto magazynu ogólnego przeznaczenia. Aby dowiedzieć się, jak przetransferować dane na konto magazynu i z niego, przejdź do przewodnika Szybki start dotyczącego magazynu obiektów blob.

> [!div class="nextstepaction"]
> [Transferowanie obiektów do usługi Azure Blob Storage i z niej za pomocą interfejsu wiersza polecenia platformy Azure](../blobs/storage-quickstart-blobs-cli.md)