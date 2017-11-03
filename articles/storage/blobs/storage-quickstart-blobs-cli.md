---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu wiersza polecenia platformy Azure"
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
ms.date: 07/19/2017
ms.author: marsma
ms.openlocfilehash: c9b7e7a1fbc6b67821183ce31bdf2527de490c92
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. Szczegóły tego szybkiego startu przekazywania i pobierania danych do i z magazynu obiektów Blob platformy Azure przy użyciu wiersza polecenia platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty BLOB są zawsze przesyłane do kontenera. Kontenery umożliwiają organizować grupy obiektów blob, np. zorganizować plików w katalogach na tym komputerze.

Do tworzenia kontenera do przechowywania obiektów blob służy polecenie [az storage container create](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Przekazywanie obiektu blob

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Większość plików przechowywanych w magazynie obiektów Blob są przechowywane jako blokowych obiektów blob. Dołącz obiekty BLOB są używane podczas danych należy dodać do istniejącego obiektu blob bez modyfikowania jej istniejącej zawartości, takie jak rejestrowanie. Stronicowe obiekty blob obsługują pliki VHD maszyn wirtualnych IaaS.

W tym przykładzie mamy przekazywanie obiektu blob do kontenera utworzyliśmy w ostatnim kroku z [az magazynu obiektów blob przekazywania](/cli/azure/storage/blob#upload) polecenia.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Jeśli obiekt blob nie istnieje, zostanie utworzony. W przeciwnym razie zostanie zastąpiony. Przekazywanie plików tyle jak przed kontynuowaniem.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Do wyświetlania listy obiektów blob w kontenerze służy polecenie [az storage blob list](/cli/azure/storage/blob#list).

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Pobieranie obiektu blob

Użyj [az magazynu obiektów blob pobierania](/cli/azure/storage/blob#download) polecenie, aby pobrać obiektu blob możesz przekazać wcześniej.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Transfer danych z narzędzia AzCopy

[AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) narzędzie jest inną opcją w przypadku transferu danych za pomocą skryptów wysokiej wydajności dla usługi Azure Storage. Narzędzia AzCopy można użyć do transferu danych do i z magazynu obiektów Blob, plików i tabeli.

Jako przykład szybki, Oto polecenia AzCopy przekazywanie pliku o nazwie *mojplik.txt* do *mystoragecontainer* kontenera.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie są już potrzebne zasobów w grupie zasobów, tym na koncie magazynu utworzonym w tego przewodnika Szybki Start, Usuń grupę zasobów z [az usunięcie grupy](/cli/azure/group#delete) polecenia.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu przedstawiono sposób przesyłania plików między kontenera w magazynie obiektów Blob platformy Azure i dysku lokalnego. Aby dowiedzieć się więcej na temat pracy z obiektami BLOB w usłudze Azure Storage, nadal samouczek do pracy z magazynem obiektów Blob Azure.

> [!div class="nextstepaction"]
> [Porady: obiektu Blob magazynu operacje z wiersza polecenia platformy Azure](storage-how-to-use-blobs-cli.md)
