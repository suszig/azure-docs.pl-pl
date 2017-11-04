---
title: "Wprowadzenie do narzędzi programistycznych magazyn Azure stosu"
description: "Aby rozpocząć używanie narzędzi programistycznych magazyn Azure stosu"
services: azure-stack
author: xiaofmao
ms.author: xiaofmao
ms.date: 9/25/2017
ms.topic: get-started-article
ms.service: azure-stack
ms.openlocfilehash: 5b2898c64c0f1b5d804e63fa4e4e1218fa7a672c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Wprowadzenie do narzędzi programistycznych magazyn Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*


Microsoft Azure stosu zapewnia zbiór usług magazynowania, włącznie z magazynu obiektów Blob platformy Azure, tabel i kolejek.

Ten artykuł zawiera szybki wskazówki dotyczące sposobu uruchamiania przy użyciu narzędzi programistycznych magazyn Azure stosu. W odpowiednich samouczków usługi Azure Storage można znaleźć bardziej szczegółowe informacje, a przykładowy kod.

Istnieją znane różnice między usługi Azure Storage i Azure stosu magazynu, w tym niektóre określone wymagania dotyczące każdej platformy. Na przykład istnieją bibliotek określonego klienta i określonego końcowego sufiks wymagania dotyczące stosu Azure. Aby uzyskać więcej informacji, zobacz [usługi Azure Storage stosu: różnice i zagadnienia dotyczące](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Bibliotek klienta platformy Azure
Obsługiwana wersja interfejsu API REST usługi Azure Storage stosu jest 2015-04-05. Nie ma pełnej parzystości z najnowszą wersją interfejsu API REST magazynu Azure. Tak dla biblioteki klienta magazynu, należy mieć świadomość, wersji, który jest zgodny z REST API 2015-04-05.


|Biblioteka kliencka|Azure stosu obsługiwana wersja|Link|Specyfikacja punktu końcowego|
|---------|---------|---------|---------|
|.NET     |6.2.0|Pakiet Nuget:<br>[https://www.nuget.org/Packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>Wersja usługi GitHub:<br>[https://github.com/Azure/Azure-Storage-NET/Releases/tag/V6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|pliku App.config|
|Java|4.1.0|Pakiet maven:<br>[http://mvnrepository.com/Artifact/com.microsoft.Azure/Azure-Storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>Wersja usługi GitHub:<br> [https://github.com/Azure/Azure-Storage-Java/Releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|Ustawienia połączenia|
|Node.js     |1.1.0|Łącze NPM:<br>[https://www.npmjs.com/Package/Azure-Storage](https://www.npmjs.com/package/azure-storage)<br>(Uruchom:`npm install azure-storage@1.1.0)`<br><br>Wersja usługi Github:<br>[https://github.com/Azure/Azure-Storage-Node/Releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|Deklaracja wystąpienia usługi||C++|2.4.0|Pakiet Nuget:<br>[https://www.nuget.org/Packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>Wersja usługi GitHub:<br>[https://github.com/Azure/Azure-Storage-CPP/Releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Ustawienia połączenia|
|C++|2.4.0|Pakiet Nuget:<br>[https://www.nuget.org/Packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>Wersja usługi GitHub:<br>[https://github.com/Azure/Azure-Storage-CPP/Releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Ustawienia połączenia|
|PHP|0.15.0|Wersja usługi GitHub:<br>[https://github.com/Azure/Azure-Storage-PHP/Releases/tag/V0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>Zainstaluj za pośrednictwem Composer (Zobacz szczegóły poniżej)|Ustawienia połączenia|
|Python     |0.30.0|Pakiet PIP:<br> [https://pypi.Python.org/pypi/Azure-Storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>(Uruchom:`pip install -v azure-storage==0.30.0)`<br><br>Wersja usługi GitHub:<br> [https://github.com/Azure/Azure-Storage-Python/Releases/tag/V0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|Deklaracja wystąpienia usługi|
|Ruby|0.12.1<br>Wersja zapoznawcza|Pakiet RubyGems:<br> [https://rubygems.org/GEMS/Azure-Storage/Versions/0.12.1.Preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>Wersja usługi GitHub:<br> [https://github.com/Azure/Azure-Storage-Ruby/Releases/tag/V0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|Ustawienia połączenia|

> [!NOTE]
> Szczegóły PHP<br><br>
>Aby zainstalować za pomocą Composer:
>1. Utwórz plik o nazwie `composer.json` w folderze głównym projektu z następującym kodem:<br>
>
>   ```
>   {
>       "require":{
>           "Microsoft/azure-storage":"0.15.0"
>        }
>    }
>   ```
>
>2. Pobierz [composer.phar](http://getcomposer.org/composer.phar) w katalogu głównym projektu.
>3. Instalacja: `php composer.phar install`.
>


## <a name="endpoint-declaration"></a>Deklarację punktu końcowego
Punkt końcowy stosu Azure zawiera dwie części: Nazwa regionu i domena stosu Azure.
W zestawie Azure stosu Development Kit jest domyślny punkt końcowy **local.azurestack.external**.
Jeśli nie wiesz o punktu końcowego, skontaktuj się z administratorem chmury.

## <a name="examples"></a>Przykłady


### <a name="net"></a>.NET

Stosu Azure sufiks punkt końcowy jest określony w pliku app.config:

```
<add key="StorageConnectionString" 
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```
### <a name="java"></a>Java

Dla stosu Azure sufiks punktu końcowego określono w ustawieniach parametry połączenia:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Stosu Azure w deklaracji wystąpienia określono sufiks punktu końcowego:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```
### <a name="c"></a>C++

Dla stosu Azure sufiks punktu końcowego określono w ustawieniach parametry połączenia:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Dla stosu Azure sufiks punktu końcowego określono w ustawieniach parametry połączenia:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Stosu Azure w deklaracji wystąpienia określono sufiks punktu końcowego:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```
### <a name="ruby"></a>Ruby

Dla stosu Azure sufiks punktu końcowego określono w ustawieniach parametry połączenia:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob Storage

Następujące samouczki magazynu obiektów Blob platformy Azure są stosowane do stosu Azure. Należy zwrócić uwagę konieczność sufiks określonego końcowego stosu Azure opisane w poprzedniej [przykłady](#examples) sekcji.

* [Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Używanie usługi Blob Storage w języku Java](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Jak używać magazynu obiektów Blob w oprogramowaniu Node.js..] /.. / storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Jak używać magazynu obiektów Blob w języku C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Używanie usługi Blob Storage w języku PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Jak używać magazynu obiektów Blob platformy Azure w języku Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Używanie usługi Blob Storage w języku Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Queue Storage

Następujące samouczki magazynu kolejek Azure mają zastosowanie do stosu Azure. Należy zwrócić uwagę konieczność sufiks określonego końcowego stosu Azure opisane w poprzedniej [przykłady](#examples) sekcji.

* [Rozpoczynanie pracy z usługą Azure Queue Storage przy użyciu platformy .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Używanie usługi Queue Storage w języku Java](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Używanie usługi Queue Storage w oprogramowaniu Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Jak używać magazynu kolejek w języku C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Używanie usługi Queue Storage w języku PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Używanie usługi Queue Storage w języku Python](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Używanie usługi Queue Storage w języku Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)


## <a name="table-storage"></a>Magazyn tabel

Następujące samouczki magazynu tabel Azure mają zastosowanie do stosu Azure. Należy zwrócić uwagę konieczność sufiks określonego końcowego stosu Azure opisane w poprzedniej [przykłady](#examples) sekcji.

* [Rozpoczynanie pracy z usługą Azure Table Storage przy użyciu platformy .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Używanie usługi Table Storage w języku Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Jak używać magazynu tabel Azure w oprogramowaniu Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Jak używać magazynu tabel w języku C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Używanie usługi Table Storage w języku PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Jak używać magazynu tabel w języku Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Używanie usługi Table Storage w języku Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do usługi Microsoft Azure Storage](../../storage/common/storage-introduction.md)