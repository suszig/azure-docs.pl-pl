---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu Node.js| Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu środowiska Node.js"
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
ms.openlocfilehash: 4c3c4ec341a0e5f4f0e7415128479f6448f7db6b
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu środowiska Node.js

Z tego przewodnika Szybki Start dowiesz się przekazywanie, pobieranie i listy blokowych obiektów blob w kontenerze w magazynie obiektów Blob platformy Azure przy użyciu środowiska Node.js.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* Zainstalować środowisko [Node.js](https://nodejs.org/en/).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

[Przykładowa aplikacja](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) używane w tym szybkiego startu jest aplikacji konsoli podstawowe. 

Użyj [git](https://git-scm.com/) do pobrania kopii aplikacji w środowisku deweloperskim.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

To polecenie klonów repozytorium do folderu lokalnego git. Aby otworzyć aplikacji Wyszukaj folder magazynu — obiekty BLOB — węzeł — Szybki Start, otwórz go i kliknij dwukrotnie index.js.

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu

W aplikacji musisz podać parametry połączenia dla konta magazynu. Otwórz `index.js` pliku, Znajdź `connectionString` zmiennej. Zastąp wartość całą wartość parametrów połączenia, który został zapisany w portalu Azure. Parametry połączenia magazynu powinien wyglądać podobnie do następującego:

```node
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Instalowanie wymaganych pakietów

W katalogu aplikacji, uruchom `npm install` z instalowaniem dowolnej wymagane pakiety wymienione w `package.json` pliku.

```node
npm install
```

## <a name="run-the-sample"></a>Uruchom próbki

Ten przykład tworzy plik testowy w folderze Moje dokumenty, przekazanie jej do magazynu obiektów Blob, zawiera listę obiektów blob w kontenerze, a następnie pobiera plik z nową nazwą, możesz porównać starych i nowych plików.

Uruchom próbkę wpisując `node index.js`. Następujące dane wyjściowe pochodzi z systemem Windows.  Jeśli przy użyciu systemu Linux można oczekiwać podobne dane wyjściowe ze ścieżkami odpowiedniego pliku.

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

Przed kontynuowaniem sprawdź Moje dokumenty dwa pliki. Można je otworzyć i sprawdzić, czy są one takie same.

Można również użyć narzędzia takie jak [Eksploratora usługi Storage Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) do wyświetlania plików w magazynie obiektów Blob. Eksplorator usługi Storage platformy Azure to bezpłatne narzędzie i platform umożliwiający dostęp do danych konta magazynu.

Po zweryfikowaniu pliki naciśnij dowolny klawisz, aby zakończyć pokaz i usuwanie plików testowych. Teraz, znając prezentowanym przykładzie, otwórz plik index.js, aby przyjrzeć się kodu. 

## <a name="get-references-to-the-storage-objects"></a>Pobierz odwołania do obiektów magazynu

Jest najpierw musisz utworzyć odwołania do `BlobService` używane do uzyskania dostępu i zarządzania magazynem obiektów Blob. Te obiekty kompilacji na każdym z nich — są używane przez kolejnego na liście.

* Utwórz wystąpienie  **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)**  obiektu, który wskazuje usługi obiektów Blob na koncie magazynu.

* Utwórz nowy kontener, a następnie ustaw uprawnienia w kontenerze obiektów blob są publiczne i jest możliwy tylko adres URL. Kontener, który rozpoczyna się od **quickstartcontainer -**.

W tym przykładzie użyto [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) ponieważ chcemy, aby utworzyć nowy kontener za każdym razem próbki jest uruchamiany. W środowisku produkcyjnym, której używasz tego samego kontenera w całej aplikacji, lepiej praktyką jest tylko jeden raz wywołać CreateIfNotExists. Można również utworzyć kontenera wcześniejsze, więc nie trzeba go utworzyć w kodzie.

```node
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

## <a name="upload-blobs-to-the-container"></a>Przekaż obiekty BLOB do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty BLOB są najczęściej używane. Doskonale nadają się do przechowywania tekstu i danych binarnych, czyli przyczyny, które są używane w tym Szybki Start.

Aby przekazać plik do obiektu blob, należy użyć [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) metody. Ta operacja tworzy obiektu blob, jeśli jeszcze nie istnieje lub zastąpiony, jeśli już istnieje.

Przykładowy kod tworzy plik lokalny do zastosowania w przypadku przekazywania i pobierania, przechowywanie plików do przekazania jako **localPath** i nazwa obiektu blob w **localFileToUpload**. Poniższy przykład powoduje przekazanie pliku z kontenera rozpoczynających się od **quickstartcontainer -**.

```node
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Istnieje kilka metod przekazywania, korzystających z magazynu obiektów Blob. Na przykład, jeśli masz strumienia pamięci, można użyć [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) metody zamiast [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Następnie aplikacja pobiera listę plików za pomocą kontenera [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Poniższy kod pobiera listę obiektów blob, a następnie w pętli, przedstawiający identyfikatorów URI obiektów blob znalezionych. Można skopiować identyfikator URI w oknie polecenia i wklej go w przeglądarce, aby wyświetlić plik.

Jeśli masz 5000 lub mniej obiektów blob w kontenerze, wszystkie nazwy obiektów blob są pobierane w jednym wywołaniu [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Jeśli masz więcej niż 5000 obiekty BLOB w kontenerze, usługa pobiera listę w zestawach 5000, dopóki wszystkie nazwy obiektów blob zostały pobrane. Dlatego po raz pierwszy wywołać tego interfejsu API, zwraca pierwszy nazwy 5000 obiektów blob i token kontynuacji. Drugim, podaj token, usługa pobiera następnego zestawu nazwy obiektów blob i itd., dopóki token kontynuacji ma wartość null, co oznacza, że wszystkie nazwy obiektów blob zostały pobrane.

```node
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

## <a name="download-blobs"></a>Pobieranie obiektów blob

Pobieranie obiektów blob przy użyciu dysku lokalnym [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

Poniższy kod pobiera blob przekazany w poprzedniej sekcji, dodanie sufiksu "_DOWNLOADED" Nazwa obiektu blob, dzięki czemu oba pliki na dysku lokalnym. 

```node
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przekazane w tego przewodnika Szybki Start obiektów blob nie są już potrzebne, można usunąć za pomocą całego kontenera [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) i [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). To również usunięcie plików utworzony, jeśli nie są już potrzebne. To jest poświęcony na obsługę aplikacji po naciśnięciu klawisza enter, aby zakończyć działanie aplikacji.

```node
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

W tym szybkiego startu przedstawiono sposób przesyłania plików między magazynem obiektów Blob platformy Azure przy użyciu środowiska Node.js i dysku lokalnego. Aby dowiedzieć się więcej na temat pracy z magazynem obiektów Blob, nadal do magazynu obiektów Blob, jak to zrobić.

> [!div class="nextstepaction"]
> [Porada operacje magazynu obiektów blob](storage-nodejs-how-to-use-blob-storage.md)

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zobacz [zasobami magazynu obiektów Blob platformy Azure zarządzać za pomocą Eksploratora usługi Storage](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
