---
title: "Szybki start platformy Azure — Transferowanie obiektów do i z usługi Azure Blob Storage za pomocą środowiska Node.js | Microsoft Docs"
description: "Skrócona instrukcja transferowania obiektów do i z usługi Azure Blob Storage za pomocą środowiska Node.js"
services: storage
documentationcenter: storage
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: gwallace
ms.openlocfilehash: dd4d3abf082767c40760d020c0997b365452e769
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Transferowanie obiektów do i z usługi Azure Blob Storage za pomocą środowiska Node.js

Dzięki temu przewodnikowi Szybki start dowiesz się, w jaki sposób za pomocą środowiska Node.js przekazywać, pobierać i wyświetlać listę blokowych obiektów blob w kontenerze usługi Azure Blob Storage.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* Zainstalować środowisko [Node.js](https://nodejs.org/en/).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

[Przykładowa aplikacja](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) używana w tym przewodniku Szybki start to podstawowa aplikacja konsoli. 

Użyj narzędzia [git](https://git-scm.com/), aby pobrać kopię tej aplikacji do swojego środowiska projektowego.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

To polecenie klonuje repozytorium do lokalnego folderu git. Aby otworzyć tę aplikację, wyszukaj folder storage-blobs-node-quickstart, otwórz go i kliknij dwukrotnie plik index.js.

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu

W aplikacji należy wprowadzić parametry połączenia konta magazynu. Otwórz plik `index.js` i znajdź zmienną `connectionString`. Zastąp jej wartość, obejmującą całą wartość parametru połączenia, wartością skopiowaną z witryny Azure Portal. Parametry połączenia magazynu powinny wyglądać podobnie do poniższych:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Instalowanie wymaganych pakietów

W katalogu aplikacji uruchom polecenie `npm install`, aby zainstalować wszystkie wymagane pakiety wymienione w pliku `package.json`.

```javascript
npm install
```

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

Ta aplikacja przykładowa tworzy plik testowy w folderze Moje dokumenty, przesyła go do usługi Blob Storage, tworzy listę obiektów blob w kontenerze, a następnie pobiera plik z nową nazwą, tak aby można było porównać stary i nowy plik.

Uruchom aplikację przykładową, wpisując ciąg `node index.js`. Poniższe dane wyjściowe pochodzą z systemu Windows.  W przypadku używania systemu Linux można spodziewać się podobnych danych wyjściowych z odpowiednimi ścieżkami plików.

```
Azure Storage Node.js Client Library Blobs Quick Start

1. Creating a container with public access: quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d

2. Creating a file in ~/Documents folder to test the upload and download

   Local File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

3. Uploading BlockBlob: quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

   Uploaded Blob URL: https://mystorageaccount.blob.core.windows.net/quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d/quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

4. Listing blobs in container

   - quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt (type: BlockBlob)


5. Downloading blob

   Downloaded File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d_DOWNLOADED.txt

Sample finished running. When you hit <ENTER> key, the temporary files will be deleted and the sample application will exit.
```

Przed kontynuowaniem sprawdź folder Moje dokumenty pod kątem tych dwóch plików. Możesz je otworzyć i sprawdzić, czy są identyczne.

Możesz również wyświetlić pliki w usłudze Blob Storage za pomocą narzędzia takiego jak [Eksplorator usługi Azure Storage](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Eksplorator usługi Azure Storage to darmowe narzędzie międzyplatformowe, które umożliwia dostęp do informacji na koncie magazynu.

Po zweryfikowaniu plików naciśnij dowolny klawisz, aby zakończyć demonstrację i usunąć pliki testowe. Teraz, gdy wiesz już, jak działa aplikacja przykładowa, otwórz plik index.js i przyjrzyj się kodowi. 

## <a name="understand-the-sample-code"></a>Omówienie przykładowego kodu

W kolejnej części omówimy przykładowy kod, aby wyjaśnić, w jaki sposób działa.

### <a name="get-references-to-the-storage-objects"></a>Pobieranie odwołań do obiektów magazynu

Najpierw należy utworzyć odwołanie do obiektu `BlobService` używane w celu uzyskania dostępu do usługi Blob Storage i zarządzania nią. Te obiekty są powiązane — każdy obiekt jest używany przez kolejny na liście.

* Utwórz wystąpienie obiektu **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)**, które wskazuje na usługę Blob service na koncie magazynu.

* Utwórz nowy kontener, a następnie skonfiguruj uprawnienia w kontenerze, tak aby obiekty blob były publiczne i można było do nich uzyskać dostęp za pomocą samego adresu URL. Kontener zaczyna się od ciągu **quickstartcontainer -**.

W tym przykładzie użyto metody [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists), ponieważ chcemy utworzyć nowy kontener za każdym razem, gdy jest uruchamiana aplikacja przykładowa. W środowisku produkcyjnym, w którym korzystasz z tego samego kontenera w całej aplikacji, lepiej jest wywołać metodę CreateIfNotExists tylko raz. Możesz również utworzyć kontener wcześniej, aby nie było konieczne tworzenie go w kodzie.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>Przekazywanie obiektów blob do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty blob są używane najczęściej. Doskonale nadają się do przechowywania tekstu i danych binarnych, dlatego zostały użyte w tym przewodniku Szybki start.

Aby przekazać plik do obiektu blob, należy użyć metody [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile). Ta operacja tworzy obiekt blob, jeśli jeszcze nie istnieje, lub zastępuje go, jeśli już istnieje.

Przykładowy kod tworzy plik lokalny do zastosowania w przypadku przekazywania i pobierania, przechowujący plik do przekazania jako **localPath** i nazwę obiektu blob w **localFileToUpload**. Następujący kod przykładowy przekazuje plik do kontenera, którego nazwa zaczyna się od ciągu **quickstartcontainer-**.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Istnieje kilka metod przekazywania, których można użyć z usługą Blob Storage. Na przykład jeśli masz strumień pamięci, możesz użyć metody [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) zamiast metody [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

### <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Następnie aplikacja pobiera listę plików w kontenerze za pomocą metody [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Poniższy kod umożliwia pobranie listy obiektów blob, a następnie przetwarza je w pętli, wyświetlając identyfikatory URI odnalezionych obiektów blob. Możesz skopiować identyfikator URI z okna polecenia i wkleić go do przeglądarki, aby wyświetlić plik.

Jeśli kontener zawiera 5000 lub mniej obiektów blob, wszystkie nazwy obiektów blob są pobierane w jednym wywołaniu metody [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Jeśli kontener zawiera więcej niż 5000 obiektów blob, usługa pobiera listę w zestawach po 5000 do momentu pobrania wszystkich nazw obiektów blob. Dlatego pierwsze wywołanie tego interfejsu API zwraca 5000 pierwszych nazw obiektów blob i token kontynuacji. Za drugim razem należy podać token, a usługa pobierze następny zestaw nazw obiektów blob i tak dalej, aż do momentu, gdy wartość tokenu kontynuacji wyniesie null, co oznacza pobranie wszystkich nazw obiektów blob.

```javascript
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

### <a name="download-blobs"></a>Pobieranie obiektów blob

Pobierz obiekty blob na dysk lokalny, używając metody [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

Następujący kod pobiera obiekty blob przekazane w poprzedniej sekcji, dodając sufiks „_DOWNLOADED” do nazwy obiektu blob, tak aby oba pliki były widoczne na dysku lokalnym. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie potrzebujesz już obiektów blob przekazanych podczas pracy z tym przewodnikiem Szybki start, możesz usunąć cały kontener, korzystając z metod [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) i [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). Usuń także utworzone pliki, jeśli nie są już potrzebne. Te czynności są wykonywane w aplikacji po naciśnięciu klawisza Enter w celu zakończenia działania aplikacji.

```javascript
console.log('6. Deleting block Blob\n');
    blobService.deleteBlobIfExists(CONTAINER_NAME, BLOCK_BLOB_NAME, function (error) {
        handleError(error);

    console.log('7. Deleting container\n');
    blobService.deleteContainerIfExists(CONTAINER_NAME, function (error) {
        handleError(error);
            
        fs.unlinkSync(LOCAL_FILE_PATH);
        fs.unlinkSync(DOWNLOADED_FILE_PATH);
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start przedstawiono metodę transferowania plików między dyskiem lokalnym i usługą Azure Blob Storage przy użyciu środowiska Node.js. Aby dowiedzieć się więcej na temat pracy z usługą Blob Storage, przejdź do instrukcji dotyczących magazynu obiektów blob.

> [!div class="nextstepaction"]
> [Instrukcje: Operacje wykonywane w usłudze Blob Storage](storage-nodejs-how-to-use-blob-storage.md)

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zapoznaj się artykułem [Manage Azure Blob storage resources with Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Zarządzanie zasobami usługi Azure Blob Storage za pomocą Eksploratora usługi Microsoft Azure Storage).
