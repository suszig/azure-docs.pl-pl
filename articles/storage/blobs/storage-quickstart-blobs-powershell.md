---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu programu PowerShell"
services: storage
documentationcenter: storage
author: robinsh
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
ms.author: robinsh
ms.openlocfilehash: 1a9941b21b92c70dd0a46ce2e4c75142e1786650
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu programu Azure PowerShell

Moduł Azure PowerShell umożliwia tworzenie zasobów platformy Azure i zarządzanie nimi za pomocą wiersza polecenia programu PowerShell lub skryptów. Szczegóły ten przewodnik transferu plików między dysku lokalnym i magazynu obiektów Blob platformy Azure przy użyciu programu PowerShell.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Dla tego przewodnika Szybki start jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty BLOB są zawsze przesyłane do kontenera. Dzięki temu można organizować grupy obiektów blob, takie jak organizowanie plików na komputerze w folderach.

Ustaw nazwę kontenera, a następnie utworzyć przy użyciu kontenera [AzureStorageContainer nowy](/powershell/module/azure.storage/new-azurestoragecontainer), ustawianie uprawnień do blob, aby zezwolić na publiczny dostęp do plików. Nazwa kontenera, w tym przykładzie jest *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Przekaż obiekty BLOB do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Pliki VHD używane do tworzenia kopii maszyn wirtualnych IaaS są stronicowych obiektów blob. Dołącz obiekty BLOB są używane do logowania, takie jak kiedy zachodzi potrzeba zapisane do pliku i następnie dodać więcej informacji. Większość plików przechowywanych w magazynie obiektów Blob są blokowych obiektów blob. 

Aby przekazać plik do blokowego obiektu blob, Pobierz odwołanie do kontenera, a następnie pobrać odwołanie do blokowych obiektów blob w tym kontenerze. Po uzyskaniu odwołania do obiektu blob możesz przekazać dane do niego przy użyciu [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Ta operacja tworzy obiektu blob, jeśli jeszcze nie istnieje lub zastąpiony, jeśli już istnieje.

Poniższe przykłady Przekaż rysunek001.jpg i Image002.png od D:\\_TestImages folderu na dysku lokalnym w kontenerze, który został utworzony.

```powershell
# upload a file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Przekazywanie plików tyle jak przed kontynuowaniem.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Pobierz listę obiektów blob w kontenerze, za pomocą [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob). W tym przykładzie są wyświetlane tylko nazwy obiektów blob przekazany.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Pobieranie obiektów blob

Pobieranie obiektów blob na lokalnym dysku twardym. Dla każdego obiektu blob do pobrania, należy ustawić nazwę i wywołanie [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) można pobrać obiektu blob.

W tym przykładzie pobiera obiekty BLOB do D:\\_TestImages\Downloads na dysku lokalnym. 

```powershell
# download first blob
Get-AzureStorageBlobContent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
Get-AzureStorageBlobContent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 
```

## <a name="data-transfer-with-azcopy"></a>Transfer danych z narzędzia AzCopy

[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) narzędzie jest inną opcją w przypadku transferu danych za pomocą skryptów wysokiej wydajności dla usługi Azure Storage. Narzędzia AzCopy można użyć do transferu danych do i z magazynu obiektów Blob, plików i tabeli.

Jako przykład szybki, Oto polecenia AzCopy przekazywanie pliku o nazwie *mojplik.txt* do *mystoragecontainer* kontenera z okna programu PowerShell.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Usuń wszystkie zasoby, które zostały utworzone. Najprostszym sposobem, w tym celu jest można usunąć grupy zasobów. Spowoduje to również usunięcie wszystkie zasoby zawarte w obrębie grupy. W takim przypadku spowoduje usunięcie konta magazynu i grupy zasobów, do samej siebie.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym szybki start przedstawiono sposób przesyłania plików między magazynu obiektów Blob platformy Azure i dysku lokalnego. Aby dowiedzieć się więcej na temat pracy z magazynem obiektów Blob, nadal do magazynu obiektów Blob, jak to zrobić.

> [!div class="nextstepaction"]
> [Porada operacje magazynu obiektów blob](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Dokumentacja poleceń cmdlet usługi Magazyn Microsoft Azure PowerShell
* [Polecenia cmdlet programu PowerShell magazynu](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.