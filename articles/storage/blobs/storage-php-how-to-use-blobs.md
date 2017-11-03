---
title: "Jak używać magazynu obiektów blob (obiekt magazynu) za pomocą języka PHP | Dokumentacja firmy Microsoft"
description: "Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
documentationcenter: php
services: storage
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 9de2f7e81d75669267fe6448030c118d06b3f88a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-php"></a>Jak używać magazynu obiektów blob w języku PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie
Magazyn obiektów blob Azure jest usługą służącą do przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

W tym przewodniku przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi obiektów blob platformy Azure. Przykłady są napisane w PHP i użyj [biblioteki klienta magazynu Azure dla programu PHP][download]. Omówione scenariusze obejmują **przekazywania**, **wyświetlania**, **pobieranie**, i **usuwanie** obiektów blob. Aby uzyskać więcej informacji dotyczących obiektów blob, zobacz [następne kroki](#next-steps) sekcji.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP
Jedynym wymaganiem dla tworzenia aplikacji PHP, który uzyskuje dostęp do usługi obiektów blob platformy Azure jest odwołanie do klasy w [biblioteki klienta magazynu Azure dla programu PHP] [ download] od w kodzie. Narzędzia do programowania służy do tworzenia aplikacji, łącznie z Notatnika.

W tym przewodniku należy użyć funkcji usługi magazynu obiektów Blob, które można wywołać w ramach aplikacji PHP lokalnie lub w kodzie działających w roli sieci web platformy Azure, roli procesu roboczego lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Pobierz bibliotek klienta platformy Azure
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurowanie aplikacji do uzyskania dostępu do usługi blob
Aby korzystać z usługi Azure blob interfejsów API, musisz:

1. Odwołanie przy użyciu pliku automatycznej ładowarki [require_once] instrukcji, i
2. Odwoływać się do wszystkich klas, których może używać.

Poniższy przykład pokazuje, jak dołączyć plik automatycznej ładowarki i odwołanie **ServicesBuilder** klasy.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Common\ServicesBuilder;
```

W poniższych przykładach `require_once` instrukcji jest zawsze wyświetlany, ale odwołuje się tylko do klas, które są konieczne na przykład do wykonania.

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Można utworzyć klienta usługi obiektów blob platformy Azure, najpierw musi mieć prawidłowe parametry połączenia. Format dla parametrów połączenia usługi obiektów blob jest:

Aby uzyskać dostęp do usługi na żywo:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Aby uzyskać dostęp do emulatora magazynu:

```php
UseDevelopmentStorage=true
```

Aby utworzyć dowolnego klienta usługi Azure, musisz użyć **ServicesBuilder** klasy. Możesz:

* Przekaż parametry połączenia do niego bezpośrednio lub
* W aplikacji sieci Web można używać zmiennych środowiskowych, aby zapisać parametry połączenia. Zobacz [ustawienia konfiguracji aplikacji sieci web platformy Azure](../../app-service/web-sites-configure.md) dokumentu do konfigurowania parametrów połączenia.

Przykłady przedstawione w tym miejscu ciąg połączenia jest przekazywany bezpośrednio.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Tworzenie kontenera
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** obiektu umożliwia tworzenie kontenera obiektów blob z **tworzony kontener** metody. Podczas tworzenia kontenera, można ustawić opcje w kontenerze, ale spowoduje to nie jest wymagane. (Poniższy przykład przedstawia sposób ustawiania listy kontroli dostępu do kontenera (ACL) i metadanych kontenera).

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Wywoływanie **setPublicAccess (PublicAccessType::CONTAINER\_i\_obiektów blob)** powoduje, że dane i kontener obiektów blob jest dostępny za pomocą żądań anonimowych. Wywoływanie **setPublicAccess(PublicAccessType::BLOBS_ONLY)** sprawia, że tylko obiektu blob danych dostępny za pomocą żądań anonimowych. Aby uzyskać więcej informacji na temat kontenera listy kontroli dostępu, zobacz [kontenera zestawu list kontroli dostępu (interfejsu API REST)][container-acl].

Aby uzyskać więcej informacji o kodach błędów usługi obiektów Blob, zobacz [kody błędów usługi Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Aby przekazać plik jako obiekt blob, użyj **BlobRestProxy -> createBlockBlob** metody. Ta operacja tworzy obiektu blob nie istnieje lub zastąpiony, jeśli istnieje. Poniższy przykład kodu zakłada, że kontener został już utworzony i używa [fopen —] [ fopen] można otworzyć pliku jako strumień.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Należy pamiętać, że powyższego przykładu przekazywanie obiektu blob jako strumień. Jednak obiektu blob mogą również być przekazywane jako przy użyciu ciągu, na przykład [pliku\_uzyskać\_zawartość] [ file_get_contents] funkcji. Aby to zrobić przy użyciu powyższego przykładu, zmień `$content = fopen("c:\myfile.txt", "r");` do `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, należy użyć **BlobRestProxy -> listBlobs** metody z **foreach** pętlę do pętli za pośrednictwem wyniku. Poniższy kod wyświetla nazwę każdego obiektu blob jako dane wyjściowe w kontenerze oraz jego identyfikatora URI do przeglądarki.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Pobieranie obiektu blob
Aby pobrać obiektu blob, należy wywołać **BlobRestProxy -> getBlob** metody, następnie wywołaj **getContentStream** metoda powstałe w ten sposób **GetBlobResult** obiektu.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Należy pamiętać, że w powyższym przykładzie pobiera obiektu blob jako zasób strumienia (domyślnie). Można jednak użyć [strumienia\_uzyskać\_zawartość] [ stream-get-contents] funkcji konwersji na ciąg zwrócony strumień.

## <a name="delete-a-blob"></a>Usuwanie obiektu blob
Aby usunąć obiekt blob, Przekaż nazwa kontenera i nazwa obiektu blob do **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Usunięcie kontenera obiektów blob
Na koniec można usunąć kontenera obiektów blob, przekaż nazwę kontenera, aby **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy usługi Azure blob, skorzystaj z poniższych linków, aby dowiedzieć się więcej o bardziej skomplikowanych zadaniach magazynu.

* Odwiedź stronę [dokumentacja interfejsu API dla biblioteki klienta magazynu Azure PHP](http://azure.github.io/azure-storage-php/)
* Zobacz [przykład zaawansowane Blob](https://github.com/Azure/azure-storage-php/blob/master/samples/BlobSamples.php).

Aby uzyskać więcej informacji, zobacz też [Centrum deweloperów języka PHP](/develop/php/).

[download]: https://github.com/Azure/azure-storage-php
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
