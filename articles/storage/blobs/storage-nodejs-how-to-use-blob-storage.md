---
title: "Jak używać magazynu obiektów Blob w oprogramowaniu Node.js | Dokumentacja firmy Microsoft"
description: "Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
services: storage
documentationcenter: nodejs
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 52f1f1543fe0ef15cf71d2cf1f9a8bfeaae8933f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-use-blob-storage-from-nodejs"></a>Jak używać Magazynu obiektów Blob w oprogramowaniu Node.js
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Przegląd
W tym artykule przedstawiono sposób wykonywania typowych scenariuszy przy użyciu magazynu obiektów Blob. Przykłady są napisane przy użyciu interfejsu API środowiska Node.js. Omówione scenariusze obejmują jak przekazywanie, listy, pobieranie i usuwanie obiektów blob.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji w języku Node.js
Aby uzyskać instrukcje dotyczące tworzenia aplikacji Node.js, zobacz [tworzenie aplikacji sieci web Node.js w usłudze Azure App Service](../../app-service/app-service-web-get-started-nodejs.md), [tworzenia i wdrażania aplikacji Node.js do usługi w chmurze platformy Azure](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) — za pomocą środowiska Windows PowerShell lub [tworzenie i wdrażanie aplikacji sieci web Node.js na platformie Azure przy użyciu Web Matrix](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji dostęp do magazynu
Korzystanie z usługi Azure storage, wymaga zestawu SDK usługi Magazyn Azure dla środowiska Node.js, w tym zestaw wygody bibliotek, które komunikują się z magazynu usługi REST.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu węzeł Menedżera pakietów (NPM)
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **terminali** (Mac), lub **Bash** (Unix), aby przejść do folderu, w którym utworzono aplikację przykładową.
2. Typ **magazyn azure instalacji narzędzia npm** w oknie wiersza polecenia. Dane wyjściowe polecenia jest podobny do poniższego przykładu kodu.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Możesz ręcznie uruchomić **ls** polecenie, aby sprawdzić, czy **węzła\_modułów** folder został utworzony. W tym folderze, Znajdź **magazyn azure** pakiet, który zawiera bibliotek, które wymagają dostępu do magazynu.

### <a name="import-the-package"></a>Importowanie pakietu
Za pomocą Notatnika lub innego edytora tekstu, Dodaj następujący element do góry **server.js** pliku aplikacji, których zamierzasz używać magazynu:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Moduł Azure odczyta zmiennych środowiskowych `AZURE_STORAGE_ACCOUNT` i `AZURE_STORAGE_ACCESS_KEY`, lub `AZURE_STORAGE_CONNECTION_STRING`, aby uzyskać informacje wymagane do łączenia się z kontem magazynu platformy Azure. Jeśli te zmienne środowiskowe nie są skonfigurowane, należy określić informacje o koncie podczas wywoływania metody **createBlobService**.

## <a name="create-a-container"></a>Tworzenie kontenera
**BlobService** obiektu umożliwia pracę z kontenerów i obiektów blob. Poniższy kod tworzy **BlobService** obiektu. Dodaj następujący kod w górnej części **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Dostęp obiektu blob anonimowo za pomocą **createBlobServiceAnonymous** i podając adres hosta. Na przykład użyć `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Aby utworzyć nowy kontener, użyj **createContainerIfNotExists**. Poniższy przykład kodu tworzy nowy kontener o nazwie "mojkontener":

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Jeśli kontener jest nowo utworzony, `result.created` ma wartość true. Jeśli kontener już istnieje, `result.created` ma wartość false. `response`zawiera informacje na temat operacji, w tym informacje o ETag kontenera.

### <a name="container-security"></a>Zabezpieczenia kontenerów
Domyślnie nowe kontenery są prywatne i nie można uzyskać dostępu do anonimowo. Aby udostępnić kontenera, dzięki czemu można do niego dostęp anonimowo, można ustawić dostępu do kontenera poziom **obiektu blob** lub **kontenera**.

* **Obiekt blob** — umożliwia anonimowy dostęp do odczytu do zawartości obiektu blob i metadanych, w tym kontenerze, ale nie do kontenera metadane, takie jak wyświetlanie listy wszystkich obiektów blob w kontenerze
* **kontener** — umożliwia anonimowy dostęp do odczytu do zawartości obiektu blob i metadanych, tak jak metadanych kontenera

Poniższy przykład kodu pokazuje ustawienie poziom dostępu **obiektu blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

Alternatywnie można zmodyfikować poziom dostępu do kontenera za pomocą **setContainerAcl** określa poziom dostępu. Poniższy przykład kodu zmienia poziom dostępu do kontenera:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

Wynik zawiera informacje na temat operacji, łącznie z bieżącej **ETag** kontenera.

### <a name="filters"></a>Filtry
Opcjonalne operacjach filtrowania może dotyczyć operacje wykonywane przy użyciu **BlobService**. Filtrowanie operacje mogą obejmować rejestrowania, Automatyczne ponawianie próby itp. Obiekty, które implementują metodę o sygnaturze są następujące filtry:

```nodejs
function handle (requestOptions, next)
```

Po wykonaniu przetwarzanie wstępne opcje żądania, metoda musi wywołać "dalej", przekazywanie wywołania zwrotnego z następującą sygnaturą:

```nodejs
function (returnObject, finalCallback, next)
```

W tym wywołania zwrotnego, a po przetworzeniu returnObject (odpowiedź z żądania do serwera) wywołania zwrotnego musi wywołać obok, jeśli istnieje kontynuować przetwarzanie inne filtry lub po prostu Wywołaj finalCallback na koniec wywołania usługi.

Dwa filtry, które implementują logikę ponawiania wchodzą w skład zestawu Azure SDK dla środowiska Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. Tworzy następujące **BlobService** obiekt, który używa **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Istnieją trzy typy obiektów blob: blokowe obiekty BLOB, stronicowe i uzupełnialne. Blokowe obiekty BLOB zezwala na przekazywanie bardziej wydajne dużej ilości danych. Dołącz obiekty BLOB są zoptymalizowane pod kątem operacji dołączania. Stronicowe obiekty BLOB są zoptymalizowane pod kątem operacji odczytu/zapisu. Aby uzyskać więcej informacji, zobacz [opis blokowych obiektów blob, Dołącz obiektów blob i stronicowe obiekty BLOB](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Obiekty BLOB typu Block
Aby przekazać dane do blokowego obiektu blob, użyj następującego polecenia:

* **createBlockBlobFromLocalFile** — tworzy nowy obiekt blob blokowy i przekazuje zawartość pliku
* **createBlockBlobFromStream** — tworzy nowy obiekt blob blokowy i przekazuje zawartość strumienia
* **createBlockBlobFromText** — tworzy nowy obiekt blob blokowy i przekazuje zawartości ciągu
* **createWriteStreamToBlockBlob** — zawiera strumień zapisu do blokowego obiektu blob

Poniższy przykład kodu przekazuje zawartość **test.txt** pliku do **mojblob**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

`result` Zwrócony przy użyciu tej metody zawiera informacje na temat działania, takie jak **ETag** obiektu blob.

### <a name="append-blobs"></a>Uzupełnialnych obiektów blob
Aby przekazać dane do nowy uzupełnialny obiekt blob, użyj następującego polecenia:

* **createAppendBlobFromLocalFile** — tworzy nowy uzupełnialny obiekt blob i przekazuje zawartość pliku
* **createAppendBlobFromStream** — tworzy nowy uzupełnialny obiekt blob i przekazuje zawartość strumienia
* **createAppendBlobFromText** — tworzy nowy uzupełnialny obiekt blob i przekazuje zawartości ciągu
* **createWriteStreamToNewAppendBlob** — tworzy nowy uzupełnialny obiekt blob, a następnie oferuje strumienia do zapisu

Poniższy przykład kodu przekazuje zawartość **test.txt** pliku do **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Aby dołączyć blok do istniejących uzupełnialny obiekt blob, użyj następującego polecenia:

* **appendFromLocalFile** -dołącza zawartość pliku do istniejącej uzupełnialnego obiektu blob
* **appendFromStream** -Dołącz zawartość strumienia do istniejących uzupełnialnego obiektu blob
* **appendFromText** -Dołącz zawartość ciąg do istniejącego uzupełnialnego obiektu blob
* **appendBlockFromStream** -Dołącz zawartość strumienia do istniejących uzupełnialnego obiektu blob
* **appendBlockFromText** -Dołącz zawartość ciąg do istniejącego uzupełnialnego obiektu blob

> [!NOTE]
> appendFromXXX interfejsów API wykona niektórych po stronie klienta niepowodzenie sprawdzania poprawności szybko uniknąć niepotrzebnych serwera wywołania. nie będzie appendBlockFromXXX.
>
>

Poniższy przykład kodu przekazuje zawartość **test.txt** pliku do **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Stronicowe obiekty blob
Aby przekazać dane do stronicowych obiektów blob, użyj następującego polecenia:

* **createPageBlob** — tworzy nowy stronicowych obiektów blob o określonej długości
* **createPageBlobFromLocalFile** — tworzy nowy stronicowych obiektów blob i przekazuje zawartość pliku
* **createPageBlobFromStream** — tworzy nowy stronicowych obiektów blob i przekazuje zawartość strumienia
* **createWriteStreamToExistingPageBlob** — zawiera strumień zapisu do istniejącego obiektu blob strony
* **createWriteStreamToNewPageBlob** — tworzy nowy stronicowych obiektów blob, a następnie oferuje strumienia do zapisu

Poniższy przykład kodu przekazuje zawartość **test.txt** pliku do **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Stronicowe obiekty BLOB składać się z 512-bajtowych "stron". Podczas przekazywania danych o rozmiarze, który nie jest wielokrotnością 512, zostanie zwrócony błąd.
>
>

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, należy użyć **listBlobsSegmented** metody. Jeśli chcesz przywrócić obiekty BLOB z określonym prefiksem, użyj **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

`result` Zawiera `entries` kolekcji, która jest Tablica obiektów, które opisują każdy obiekt blob. Jeśli nie można zwrócić wszystkie obiekty BLOB, `result` udostępnia również `continuationToken`, którego można użyć jako drugiego parametru do pobrania dodatkowe wpisy.

## <a name="download-blobs"></a>Pobieranie obiektów blob
Aby pobrać dane z obiektu blob, użyj następującego polecenia:

* **getBlobToLocalFile** -zapisuje zawartość obiektu blob do pliku
* **getBlobToStream** -zapisuje zawartość obiektu blob do strumienia
* **getBlobToText** -zapisuje zawartość obiektu blob na ciąg
* **createReadStream** — zawiera strumień do odczytu z obiektu blob

Poniższy przykład kodu pokazuje, za pomocą **getBlobToStream** do pobrania zawartości **mojblob** obiektu blob i zapisze go do **output.txt** pliku przy użyciu strumienia:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

`result` Zawiera informacje dotyczące obiektów blob, łącznie z **ETag** informacji.

## <a name="delete-a-blob"></a>Usuwanie obiektu blob
Na koniec można usunąć obiektu blob, należy wywołać **deleteBlob**. Poniższy przykład kodu usuwa obiektu blob o nazwie **mojblob**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Współbieżny dostęp
Aby obsługiwać równoczesny dostęp do obiektu blob z wielu klientów lub wielu wystąpień procesu, można użyć **elementy etag** lub **dzierżawy**.

* **Element etag** — pozwala wykryć, że obiekt blob lub kontener został zmodyfikowany przez inny proces
* **Dzierżawy** — pozwala uzyskać wyłącznej, odnawialnymi, zapisu albo usuwania dostępu do obiektu blob w danym okresie czasu

### <a name="etag"></a>ETag
Elementy etag Użyj, jeśli konieczne jest zezwolenie na wielu klientów lub wystąpień do zapisu blokowych obiektów Blob lub strony Blob jednocześnie. Element ETag pozwala określić, czy kontener lub obiekt blob był modyfikowany od początku odczytu lub utworzone, dzięki czemu można uniknąć zastępowania zmian dokonanych przez innego klienta lub inny proces.

Można ustawić warunki ETag przy użyciu opcjonalnego `options.accessConditions` parametru. Poniższy kod przykładowy tylko przekazywanie **test.txt** pliku, jeśli obiekt blob już istnieje i ma wartość ETag zawarty w `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

Jeśli używasz elementy etag ogólne wzorzec jest:

1. Uzyskaj element ETag w wyniku tworzenia, lista lub operacji get.
2. Wykonaj akcję, sprawdzanie, czy wartość ETag nie zostały zmodyfikowane.

Jeśli wartość została zmieniona, to wskazuje, że innego klienta lub wystąpienia obiektu blob lub kontener ponieważ uzyskać wartość ETag.

### <a name="lease"></a>Dzierżawy
Możesz uzyskać nową dzierżawę za pomocą **acquireLease** metoda określania obiektu blob lub kontenera, który chcesz uzyskać dzierżawę na. Na przykład następujący kod uzyskuje dzierżawę na **mojblob**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Kolejne operacje na **mojblob** podać `options.leaseId` parametru. Identyfikator jest zwracana jako dzierżawy `result.id` z **acquireLease**.

> [!NOTE]
> Domyślnie czas trwania dzierżawy to nieskończoność. Można określić czas trwania — bez ograniczeń czasowych (od 15 do 60 sekund) podając `options.leaseDuration` parametru.
>
>

Aby usunąć dzierżawę, użyj **releaseLease**. Aby przerwać dzierżawy, ale uniemożliwić innym użytkownikom uzyskiwanie nową dzierżawę, dopóki wygasł czas trwania oryginalnej, użyj **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Praca z sygnatury dostępu współdzielonego
Sygnatury dostępu współdzielonego (SAS) to bezpieczny sposób zapewnienia szczegółowego dostępu do obiektów blob i kontenery bez podawania Twojej nazwy konta magazynu i klucze. Sygnatury dostępu współdzielonego są często używane w celu zapewnienia ograniczony dostęp do danych, na przykład pozwala aplikacji mobilnej, aby uzyskać dostępu do obiektów blob.

> [!NOTE]
> Gdy, możesz również zezwolić na anonimowy dostęp do obiektów blob, sygnatur dostępu współdzielonego umożliwiają zapewniają więcej kontrolowany dostęp, jak należy wygenerować sygnatury dostępu Współdzielonego.
>
>

Zaufanych aplikacji, takich jak jest usługą opartą na chmurze generuje sygnatur dostępu współdzielonego przy użyciu **generateSharedAccessSignature** z **BlobService**i udostępnia go do niezaufanych lub częściowo zaufanych aplikacji takich jak aplikacji mobilnej. Udostępnione dostępu podpisy są generowane przy użyciu zasad, które opisano rozpoczęcia i zakończenia dat, w których są prawidłowe sygnatury dostępu współdzielonego, jak również poziom dostępu przyznane posiadacz sygnatur dostępu współdzielonego.

Poniższy przykład kodu generuje nowe zasady dostępu współdzielonego, które umożliwia posiadacz sygnatur dostępu współdzielonego do wykonywania operacji odczytu na **mojblob** obiektu blob i wygasa 100 minut po jego utworzeniu.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Należy pamiętać, że informacji o hoście należy podać również, jak jest wymagany, gdy posiadacz sygnatur dostępu współdzielonego próbuje uzyskać dostępu do kontenera.

Następnie aplikacja kliencka używa sygnatur dostępu współdzielonego z **BlobServiceWithSAS** wykonywanie operacji względem obiektu blob. Następujące pobiera informacje **mojblob**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Ponieważ sygnatur dostępu współdzielonego zostały wygenerowane z dostępem tylko do odczytu, jeśli próby modyfikowania obiektu blob, zostanie zwrócony błąd.

### <a name="access-control-lists"></a>Listy kontroli dostępu
Listy kontroli dostępu (ACL) umożliwia także ustawić zasady dostępu dla sygnatury dostępu Współdzielonego. Jest to przydatne, jeśli chcesz umożliwić wielu klientom dostęp do kontenera, ale zapewnia różne zasady dostępu dla każdego klienta.

Listy ACL jest zaimplementowana przy użyciu tablicy zasad dostępu w usłudze identyfikator skojarzony z każdej zasady. Poniższy przykład kodu definiuje dwie zasady, jeden dla "Użytkownik1" i jeden dla "uzytkownik2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Poniższy przykładowy kod pobiera bieżącej listy ACL dla **mojkontener**, a następnie dodaje nowe zasady przy użyciu **setBlobAcl**. Takie podejście umożliwia:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Po ustawieniu listy ACL, następnie można utworzyć na podstawie Identyfikatora zasady sygnatury dostępu współdzielonego. Poniższy przykład kodu tworzy nowe sygnatury dostępu współdzielonego dla "uzytkownik2":

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji zobacz następujące zasoby.

* [Magazynu azure SDK dla węzła dokumentacja interfejsu API] [Magazynu azure SDK dla węzła dokumentacja interfejsu API]  
* [Blog zespołu usługi Magazyn azure] [Blog zespołu usługi Magazyn azure]  
* [Zestaw Azure SDK magazynu dla węzła] [ Azure Storage SDK for Node] repozytorium w witrynie GitHub  
* [Centrum deweloperów środowiska Node.js](https://azure.microsoft.com/develop/nodejs/)  
* [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)  

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node  

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/  
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx  
[Azure portal]: https://portal.azure.com  
[Tworzenie i wdrażanie aplikacji Node.js do usługi w chmurze platformy Azure](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)  
[Blog zespołu usługi Magazyn azure]: http://blogs.msdn.com/b/windowsazurestorage/  
[Magazynu azure SDK dla węzła dokumentacja interfejsu API]: http://dl.windowsazure.com/nodestoragedocs/index.html  
