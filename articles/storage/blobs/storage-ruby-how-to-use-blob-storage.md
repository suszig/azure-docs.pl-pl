---
title: "Jak używać magazynu obiektów Blob (obiekt magazynu) w języku Ruby | Dokumentacja firmy Microsoft"
description: "Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 01/18/2018
ms.author: tamram
ms.openlocfilehash: c4c6d47511acdae7afaf4a535c24c6fcc7e389b1
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-blob-storage-from-ruby"></a>Jak używać Magazynu obiektów Blob w języku Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Przegląd
Magazyn obiektów blob Azure jest usługą służącą do przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

W tym przewodniku opisano sposób wykonywania typowych scenariuszy przy użyciu magazynu obiektów Blob. Przykłady są napisane przy użyciu interfejsu API Ruby. Omówione scenariusze obejmują **przekazywania, wyświetlanie, pobieranie,** i **usuwanie** obiektów blob.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji Ruby
Utwórz aplikację dopisków fonetycznych. Aby uzyskać instrukcje, zobacz [tworzenie Ruby aplikacji w usłudze App Service w systemie Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).


## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji dostęp do magazynu
Aby korzystać z usługi Azure Storage, konieczne pobranie i użycie dopisków fonetycznych pakiet azure zawiera zestaw wygody bibliotek, które komunikują się z magazynu usługi REST.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **terminali** (Mac), lub **Bash** (Unix).
2. Wpisz "gem zainstalować obiektu blob magazynu azure" w oknie wiersza polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu
Za pomocą edytora tekstu, Dodaj następujący element do góry pliku dopisków fonetycznych, których zamierzasz używać magazynu:

```ruby
require "azure/storage/blob"
```

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Moduł azure odczyta zmiennych środowiskowych **AZURE\_MAGAZYNU\_konta** i **AZURE\_MAGAZYNU\_ACCESS_KEY** informacji wymagane do łączenia się z kontem magazynu platformy Azure. Jeśli te zmienne środowiskowe nie są skonfigurowane, należy określić przy użyciu informacji o koncie **Azure::Blob::BlobService:: Utwórz** następującym kodem:

```ruby
blob_client = Azure::Storage::Blob::BlobService.create(
    storage_account_name: account_name,
    storage_access_key: account_key
    )
```

Aby uzyskać te wartości z klasyczny lub Menedżera zasobów konta magazynu w portalu Azure:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Przejdź do konta magazynu, którego chcesz użyć.
3. W bloku ustawienia po prawej stronie, kliknij przycisk **klucze dostępu**.
4. W bloku klucze dostępu pojawia się zostanie wyświetlony klucz dostępu 1 i klucz dostępu 2. Można użyć jednego z tych.
5. Kliknij ikonę kopiowania, aby skopiować klucz do Schowka.

## <a name="create-a-container"></a>Tworzenie kontenera
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

**Azure::Storage::Blob::BlobService** obiektu umożliwia pracę z kontenerów i obiektów blob. Aby utworzyć kontener, użyj **utworzyć\_container()** metody.

Poniższy przykład kodu tworzy kontener lub drukuje ten błąd, jeśli istnieje.

```ruby
azure_blob_service = Azure::Storage::Blob::BlobService.create_from_env
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Jeśli chcesz udostępnić pliki w kontenerze, możesz ustawić uprawnienia do kontenera.

Można zmodyfikować <strong>utworzyć\_container()</strong> wywołania do przekazania **: publiczny\_dostępu\_poziom** opcji:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Prawidłowymi wartościami dla **: publiczny\_dostępu\_poziom** są opcji:

* **Obiekt blob:** Określa publiczny dostęp do odczytu obiektów blob. Mogą być odczytywane dane obiektów blob w tym kontenerze za pomocą żądania od użytkowników anonimowych, ale nie są dostępne dane kontenera. Klienci nie można wyliczyć obiektów blob w kontenerze, za pomocą żądania od użytkowników anonimowych.
* **kontener:** określa pełnej publiczny dostęp do odczytu obiektów blob i kontenera danych. Klientów można wyliczyć obiektów blob w kontenerze, za pomocą żądania od użytkowników anonimowych, ale nie można wyliczyć kontenery w ramach konta magazynu.

Alternatywnie można zmodyfikować poziomu dostępu publicznego kontenera za pomocą **ustawić\_kontenera\_acl()** metodę, aby określić poziom dostępu publicznego.

Poniższy przykład kodu zmienia poziom dostępu publicznego do **kontenera**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Aby przekazać zawartości do obiektu blob, użyj **utworzyć\_bloku\_blob()** metodę w celu utworzenia obiektu blob, użyj pliku lub ciągu jako zawartość obiektu blob.

Poniższy kod powoduje przekazanie pliku **test.png** jako nowy obiekt blob o nazwie "— obiekt blob obrazu" w kontenerze.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę kontenery, użyj **list_containers()** metody.
Aby wyświetlić listę obiektów blob w kontenerze, należy użyć **listy\_blobs()** metody. Aby wyświetlić listę wszystkich obiektów blob w kontenerze, należy wykonaj token kontynuacji zwrócony przez usługę i kontynuować list_blobs z tym tokenem. Zobacz [interfejsu API REST obiekty BLOB listy](https://docs.microsoft.com/rest/api/storageservices/list-blobs) szczegółowe informacje.

Poniższy kod wyświetla wszystkie obiekty BLOB w kontenerze.

```ruby
nextMarker = nil
loop do
    blobs = azure_blob_service.list_blobs(container_name, { marker: nextMarker })
    blobs.each do |blob|
        puts "\tBlob name #{blob.name}"
    end
    nextMarker = blobs.continuation_token
    break unless nextMarker && !nextMarker.empty?
end
```

## <a name="download-blobs"></a>Pobieranie obiektów blob
Aby pobrać obiekty BLOB, użyj **uzyskać\_blob()** metody, aby pobrać zawartość.

Poniższy przykład kodu pokazuje, za pomocą **uzyskać\_blob()** do pobrania zawartości "obiekt blob obrazu" i zapisz je w pliku lokalnym.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Usuwanie obiektu Blob
Na koniec, aby usunąć obiekt blob, użyj **usunąć\_blob()** metody. Poniższy przykład kodu pokazuje, jak można usunąć obiektu blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o bardziej skomplikowanych zadaniach magazynu, skorzystaj z poniższych linków:

* [Blog zespołu odpowiedzialnego za usługę Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure Storage SDK for Ruby](https://github.com/azure/azure-storage-ruby) repozytorium w witrynie GitHub
* [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

