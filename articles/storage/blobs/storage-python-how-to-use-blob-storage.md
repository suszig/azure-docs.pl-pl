---
title: "Jak używać magazynu obiektów Blob platformy Azure (obiekt magazynu) w języku Python | Dokumentacja firmy Microsoft"
description: "Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: tamram
ms.openlocfilehash: ae5ad68929a6779ed4944de82a609321a5c4b5ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a>Jak używać magazynu obiektów Blob platformy Azure w języku Python
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie
Magazyn obiektów blob Azure jest usługą służącą do przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

W tym artykule opisano sposób wykonywania typowych scenariuszy przy użyciu magazynu obiektów Blob. Próbki są napisane w języku Python i użyj [Microsoft Azure magazynu SDK dla języka Python]. Omówione scenariusze obejmują przekazywanie, wyświetlanie, pobieranie i usuwanie obiektów blob.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Pobierz i zainstaluj magazynu Azure SDK dla języka Python

Azure Storage SDK for Python wymaga Python 2.7, 3.3, 3.4, 3.5 lub 3,6 i składa się z 4 pakietów różnych: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` i `azure-storage-queue`. W tym samouczku zamierzamy użyj `azure-storage-blob` pakietu.
 
### <a name="install-via-pypi"></a>Zainstaluj za pośrednictwem PyPi

Aby zainstalować za pomocą indeksu pakietów języka Python (PyPI), wpisz:

```bash
pip install azure-storage-blob
```


> [!NOTE]
> Jeśli uaktualniasz z zestawu SDK usługi Magazyn Azure dla języka Python w wersji 0.36 lub starszej, najpierw musisz ją odinstalować przy użyciu `pip uninstall azure-storage` jako już nie udostępnimy zestawu SDK usługi Magazyn dla języka Python w jednym pakiecie.
> 
> 

Dla metod instalacji alternatywny, odwiedź stronę [zestawu SDK usługi Magazyn Azure dla języka Python w usłudze Github](https://github.com/Azure/azure-storage-python/).

## <a name="create-a-container"></a>Tworzenie kontenera
Utwórz na podstawie typu obiektu blob, które chcesz użyć, **BlockBlobService**, **AppendBlobService**, lub **PageBlobService** obiektu. Poniższy kod używa **BlockBlobService** obiektu. Dodaj następujący kod w górnej części każdego pliku Python, w którym chcesz uzyskania programowego dostępu do magazynu obiektów Blob Azure bloku.

```python
from azure.storage.blob import BlockBlobService
```

Poniższy kod tworzy **BlockBlobService** przy użyciu klucza nazwy i konta konta magazynu.  Zamień "myaccount" i "klucze" Nazwa konta i klucz.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

W poniższym przykładzie kodu, można użyć **BlockBlobService** obiekt, aby utworzyć kontener, jeśli nie istnieje.

```python
block_blob_service.create_container('mycontainer')
```

Domyślnie nowy kontener jest prywatny, dlatego należy określić klucz dostępu do magazynu (tak samo jak wcześniej) aby pobierać obiekty BLOB z tego kontenera. Jeśli chcesz udostępnić wszystkim obiekty BLOB w kontenerze, można utworzyć kontenera i przekazać poziom dostępu publicznego, używając następującego kodu.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Alternatywnie można modyfikować kontenera po utworzeniu go przy użyciu następującego kodu.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Po tej zmianie wszyscy użytkownicy Internetu mogą wyświetlać obiekty BLOB w kontenerze publicznym, ale tylko wtedy można zmodyfikować lub usunąć je.

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Aby utworzyć blokowych obiektów blob i przekazywanie danych, użyj **utworzyć\_obiektów blob\_z\_ścieżki**, **utworzyć\_obiektów blob\_z\_strumienia**, **utworzyć\_obiektu blob\_z\_bajtów** lub **utworzyć\_obiektu blob\_z\_tekst** metody. Są one wysokiego poziomu metodach podziału niezbędne, gdy rozmiar danych przekroczy 64 MB.

**Utwórz\_obiektu blob\_z\_ścieżki** wysyła zawartość pliku z określonej ścieżki i **utworzyć\_obiektu blob\_z\_strumienia** przekazuje zawartość z otwartego pliku/strumienia. **Utwórz\_obiektu blob\_z\_bajtów** przekazuje tablicę bajtów, i **utworzyć\_obiektu blob\_z\_tekst** przekazuje wartość określony tekst przy użyciu określonego kodowania (wartość domyślna to UTF-8).

Poniższy przykład przekazuje zawartość **sunset.png** pliku do **myblockblob** obiektu blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, należy użyć **listy\_obiekty BLOB** metody. Ta metoda zwraca generator. Poniższy kod wyjścia **nazwa** każdego obiektu blob w kontenerze do konsoli.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Pobieranie obiektów blob
Podczas pobierania danych z obiektu blob, użyj **uzyskać\_obiektów blob\_do\_ścieżki**, **uzyskać\_obiektów blob\_do\_strumienia**, **uzyskać\_obiektu blob\_do\_bajtów**, lub **uzyskać\_obiektu blob\_do\_tekstu**. Są one wysokiego poziomu metodach podziału niezbędne, gdy rozmiar danych przekroczy 64 MB.

W poniższym przykładzie pokazano, za pomocą **uzyskać\_obiektu blob\_do\_ścieżki** do pobrania zawartości **myblockblob** obiektu blob i zapisze go do **sunset.png poza** pliku.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Usuwanie obiektu blob
Na koniec można usunąć obiektu blob, należy wywołać **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a>Zapisywanie do uzupełnialnego obiektu blob
Uzupełnialny obiekt blob jest zoptymalizowany pod kątem operacji dołączania, takich jak rejestrowanie. Podobnie jak blokowy obiekt blob, uzupełnialny obiekt blob jest złożony z bloków, ale nowy blok dodany do uzupełnialnego obiektu blob jest zawsze dołączany na końcu obiektu blob. Nie można zaktualizować lub usunąć istniejącego bloku w uzupełnialnym obiekcie blob. Identyfikatory bloków w uzupełnialnym obiekcie blob nie są widoczne, jak w przypadku blokowego obiektu blob.

Każdy blok w uzupełnialnym obiekcie blob może różnić się rozmiarem, który może wynosić maksymalnie 4 MB, a uzupełnialny obiekt blob może zawierać do 50 000 bloków. Z tego względu maksymalny rozmiar uzupełnialnego obiektu blob nieznacznie przekracza 195 GB (4 MB X 50 000 bloków).

W poniższym przykładzie utworzono nowy uzupełnialny obiekt blob i dołączono do niego określone dane, symulując prostą operację rejestrowania.

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy usługi Blob Storage, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

* [Centrum deweloperów języka Python](https://azure.microsoft.com/develop/python/)
* [Interfejs API REST usług Azure Storage](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog zespołu odpowiedzialnego za usługę Azure Storage]
* [Microsoft Azure magazynu SDK dla języka Python]

[Blog zespołu odpowiedzialnego za usługę Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure magazynu SDK dla języka Python]: https://github.com/Azure/azure-storage-python
