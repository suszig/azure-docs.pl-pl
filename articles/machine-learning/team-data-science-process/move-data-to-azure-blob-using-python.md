---
title: "Przenoszenie danych do i z magazynu obiektów Blob platformy Azure przy użyciu języka Python | Dokumentacja firmy Microsoft"
description: "Przenoszenie danych do oraz z usługi Azure Blob Storage za pomocą języka Python"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: f6e325f70a37200552832b9c0ac38f6c56471f2c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Przenoszenie danych do i z magazynu obiektów Blob platformy Azure przy użyciu języka Python
W tym temacie opisano, jak na liście, przekazywanie i pobieranie obiektów blob przy użyciu interfejsu API języka Python. Za pomocą interfejsu API języka Python, udostępniane w zestawie Azure SDK można:

* Tworzenie kontenera
* Przekazywanie obiektu blob do kontenera
* Pobieranie obiektów blob
* Wyświetlanie listy obiektów blob w kontenerze
* Usuwanie obiektu blob

Aby uzyskać więcej informacji dotyczących korzystania z interfejsu API języka Python, zobacz [jak używać usługi magazynu obiektów Blob w języku Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Jeśli używasz maszyny Wirtualnej, który został skonfigurowany za pomocą skryptów dostarczonych przez [maszyn wirtualnych nauki danych na platformie Azure](virtual-machines.md), następnie AzCopy jest już zainstalowana na maszynie Wirtualnej.
> 
> [!NOTE]
> Pełne wprowadzenie do magazynu obiektów blob platformy Azure, można znaleźć w temacie [podstawy obiektów Blob Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) i [usługi obiektów Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
Tym dokumencie przyjęto założenie, że masz subskrypcję platformy Azure, konta magazynu i odpowiedniego klucza magazynu dla tego konta. Przed przekazaniem/pobieranie danych, musisz znać Azure klucz konta magazynu nazwy i konta.

* Aby skonfigurować subskrypcję platformy Azure, zobacz [bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).
* Aby uzyskać instrukcje dotyczące tworzenia konta magazynu oraz uzyskiwanie konta i informacje o kluczu, zobacz [kont magazynu Azure o](../../storage/common/storage-create-storage-account.md).

## <a name="upload-data-to-blob"></a>Przekazywanie danych do obiektu Blob
Dodaj poniższy fragment u góry żadnego kodu języka Python, w którym chcesz uzyskania programowego dostępu do magazynu Azure:

    from azure.storage.blob import BlobService

**BlobService** obiektu umożliwia pracę z kontenerów i obiektów blob. Poniższy kod tworzy obiekt BlobService przy użyciu klucza nazwy i konta konta magazynu. Zamień na nazwę konta i klucz konta swój rzeczywistych konta i klucz.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Aby przekazać dane do obiektu blob, należy użyć następujących metod:

1. Umieść\_bloku\_obiektu blob\_z\_ścieżki (przekazuje zawartość pliku z określonej ścieżki)
2. Umieść\_block_blob\_z\_pliku (przekazuje zawartość z otwartego pliku/strumienia)
3. Umieść\_bloku\_obiektu blob\_z\_bajtów (przekazywanie jako tablicę bajtów)
4. Umieść\_bloku\_obiektu blob\_z\_tekst (przekazuje wartość określony tekst przy użyciu określonego kodowania)

Następujący przykładowy kod przekazuje plik lokalny do kontenera:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Następujący przykładowy kod przekazuje wszystkie pliki (z wyjątkiem katalogów) w katalogu lokalnego do magazynu obiektów blob:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Pobierz dane z obiektu Blob
Podczas pobierania danych z obiektu blob, należy użyć następujących metod:

1. Pobierz\_obiektu blob\_do\_ścieżki
2. Pobierz\_obiektu blob\_do\_pliku
3. Pobierz\_obiektu blob\_do\_bajtów
4. Pobierz\_obiektu blob\_do\_tekstu

Te metody, wykonujących podziału niezbędne, gdy rozmiar danych przekroczy 64 MB.

Następujący przykładowy kod pobiera zawartość obiektu blob w kontenerze do pliku lokalnego:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Następujący przykładowy kod pobiera wszystkie obiekty BLOB z kontenera. Używa listy\_obiektów blob, aby wyświetlić listę dostępnych obiektów blob w kontenerze i pobiera je do katalogu lokalnego.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
