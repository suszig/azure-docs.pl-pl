---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu języka Java | Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu języka Java"
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: b096b9d79c049d8659a4171a0cbb42a99e245776
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu języka Java

W tym szybkiego startu Dowiedz się jak przekazywanie, pobieranie i listy blokowych obiektów blob w kontenerze w magazynie obiektów Blob Azure za pomocą języka Java.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* Zainstaluj IDE, do którego ma integracji Maven

* Można również zainstalować i skonfigurować Maven do pracy z wiersza polecenia

W tym samouczku używana [Eclipse](http://www.eclipse.org/downloads/) z konfiguracją "Eclipse IDE dla języka Java deweloperów".

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

[Przykładowa aplikacja](https://github.com/Azure-Samples/storage-blobs-java-quickstart) używane w tym szybkiego startu jest aplikacji konsoli podstawowe. 

Użyj [git](https://git-scm.com/) do pobrania kopii aplikacji w środowisku deweloperskim. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

To polecenie klonów repozytorium do folderu lokalnego git. Aby otworzyć projekt, uruchom środowisko Eclipse i zamknąć ekran powitalny. Wybierz **pliku** następnie **otwierania projektów z systemu plików...** . Upewnij się, że **wykrywanie i konfigurowanie projektu natures** jest zaznaczony. Wybierz **katalogu** następnie przejdź do przechowywania sklonowanego repozytorium w nim wybierz **javaBlobsQuickstart** folderu. Upewnij się, że **javaBlobsQuickstarts** projekt jest wyświetlany jako projektu Eclipse, następnie wybierz opcję **Zakończ**.

Po zakończeniu importowania projektu otwórz **AzureApp.java** (znajdujące się w **blobQuickstart.blobAzureApp** wewnątrz **src/main/java**) i Zastąp `accountname`i `accountkey` wewnątrz `storageConnectionString` ciągu. Następnie uruchom aplikację.
     

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu
    
W aplikacji musisz podać parametry połączenia dla konta magazynu. Otwórz **AzureApp.Java** pliku. Znajdź `storageConnectionString` zmiennej. Zastąp `AccountName` i `AccountKey` wartości w parametrach połączenia przy użyciu wartości zapisane w portalu Azure. Twoje `storageConnectionString` powinien wyglądać podobnie do następującego:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Uruchom próbki

Ten przykład tworzy plik testowy w katalogu domyślnym (Moje dokumenty, użytkownikom systemu windows), przekazanie jej do magazynu obiektów Blob, zawiera listę obiektów blob w kontenerze, a następnie pobiera plik z nową nazwą, możesz porównać starych i nowych plików. 

Uruchom próbkę naciskając **klawisze Ctrl + F11** w środowisku Eclipse.

Jeśli chcesz uruchomić przykład za pomocą programu Maven w wierszu polecenia, Otwórz powłokę i przejdź do **blobAzureApp** wewnątrz sklonowany katalogu. Następnie wprowadź `mvn compile exec:java`.

Oto przykład danych wyjściowych, jakby był aplikacji dla systemu Windows.

```
Location of file: C:\Users\<user>\Documents\results.txt
File has been written
URI of blob is: http://myexamplesacct.blob.core.windows.net/quickstartblobs/results.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

 Przed kontynuowaniem sprawdź domyślny katalog (Moje dokumenty, użytkownikom systemu windows) dla dwóch plików. Można je otworzyć i sprawdzić, czy są one takie same. Skopiuj adres URL obiektu blob poza okno konsoli i wklej go do przeglądarki, aby wyświetlić zawartość pliku w magazynie obiektów Blob. Po naciśnięciu klawisza enter usuwa kontenera magazynu i plików.

Można również użyć narzędzia takie jak [Eksploratora usługi Storage Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) do wyświetlania plików w magazynie obiektów Blob. Eksplorator usługi Storage platformy Azure to bezpłatne narzędzie i platform umożliwiający dostęp do danych konta magazynu. 

Po zweryfikowaniu pliki, naciśnij klawisz enter, aby zakończyć pokaz i usuwanie plików testowych. Teraz, znając prezentowanym przykładzie, otwórz **AzureApp.java** pliku w kodzie. 

## <a name="understand-the-sample-code"></a>Zrozumienie przykładowy kod

Najpierw musisz jest utworzyć odwołania do obiektów używane do uzyskania dostępu i zarządzania magazynem obiektów Blob. Te obiekty kompilacji na każdym z nich — są używane przez kolejnego na liście.

* Utwórz wystąpienie **CloudStorageAccount** obiekt wskazujący [konta magazynu](/java/api/com.microsoft.azure.management.storage._storage_account).

**CloudStorageAccount** obiekt jest reprezentację konta magazynu i pozwala na ustawianie i programowy dostęp właściwości konta magazynu. Przy użyciu **CloudStorageAccount** można utworzyć wystąpienia obiektu **CloudBlobClient**, która jest niezbędne do uzyskania dostępu do usługi blob.

* Utwórz wystąpienie **CloudBlobClient** obiektu, który wskazuje [usługa Blob](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) na koncie magazynu.

**CloudBlobClient** zapewnia punkt dostępu do usługi obiektów blob, umożliwiając ustawić i programowy dostęp właściwości do magazynu obiektów blob. Przy użyciu **CloudBlobClient** można utworzyć wystąpienia **CloudBlobContainer** obiektu, które są niezbędne do utworzenia kontenerów.

* Utwórz wystąpienie **CloudBlobContainer** obiektu, który reprezentuje [kontenera](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) uzyskują dostęp do. Kontenery są używane do organizowania obiektów blob, jak używać folderów na komputerze do organizowania plików.    

Po utworzeniu **CloudBlobContainer**, można utworzyć wystąpienia **CloudBlockBlob** obiekt wskazujący na konkretnym [obiektu blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) , w której jesteś zainteresowany, i Wykonaj przekazywanie, pobieranie, kopiowania, operacji itp.

> [!IMPORTANT]
> Nazwy kontenerów muszą mieć małe litery. Zobacz [nazewnictwa i odwołuje się do kontenerów, obiektów blob i metadanych](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) uzyskać więcej informacji o nazwach i kontener obiektów blob.

### <a name="create-a-container"></a>Tworzenie kontenera 

W tej sekcji utworzenia wystąpienia obiektów, Utwórz nowy kontener i następnie ustawić uprawnień dla kontenera obiektów blob są publiczne i jest możliwy tylko adres URL. Kontener jest nazywany **quickstartblobs**. 

W tym przykładzie użyto **CreateIfNotExists** ponieważ chcemy, aby utworzyć nowy kontener za każdym razem próbki jest uruchamiany. W środowisku produkcyjnym, której używasz tego samego kontenera w całej aplikacji, jest lepsze praktyką wywoływać tylko **CreateIfNotExists** po. Można również utworzyć kontenera wcześniejsze, więc nie trzeba go utworzyć w kodzie.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Przekaż obiekty BLOB do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty BLOB są najczęściej używane i która jest używana w tym Szybki Start. 

Można przekazać pliku do obiektu blob, należy pobrać odwołanie do obiektu blob w kontenerze docelowej. Po uzyskaniu odwołania do obiektu blob możesz przekazać dane do niego przy użyciu [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Ta operacja tworzy obiektu blob, jeśli jeszcze nie istnieje lub zastąpiony, jeśli już istnieje.

Przykładowy kod tworzy plik lokalny do służyć do przekazywania i pobierania. przechowywanie plików do przekazania jako **źródła** i nazwa obiektu blob w **obiektu blob**. Poniższy przykład powoduje przekazanie pliku z kontenera o nazwie **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Istnieje kilka [Przekaż metody](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) korzystające z magazynu obiektów Blob. Na przykład jeśli ciąg, używając metody UploadText zamiast metody przesyłania. 

Blokowe obiekty BLOB może być dowolnego typu text lub pliku binarnego. Stronicowe obiekty BLOB są głównie używane dla pliki VHD używane do tworzenia kopii maszyn wirtualnych IaaS. Dołącz obiekty BLOB są używane do logowania, takie jak kiedy zachodzi potrzeba zapisane do pliku i następnie dodać więcej informacji. Większość obiektów przechowywanych w magazynie obiektów Blob są blokowych obiektów blob.

### <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Zostanie wyświetlona lista plików za pomocą kontenera [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). Poniższy kod pobiera listę obiektów blob, a następnie w pętli, przedstawiający identyfikatorów URI obiektów blob znalezionych. Można skopiować identyfikator URI w oknie polecenia i wklej go w przeglądarce, aby wyświetlić plik.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Pobieranie obiektów blob

Pobieranie obiektów blob przy użyciu dysku lokalnym [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

Poniższy kod pobiera blob przekazany w poprzedniej sekcji, dodanie sufiksu "_DOWNLOADED" Nazwa obiektu blob, dzięki czemu oba pliki na dysku lokalnym. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przekazane w tego przewodnika Szybki Start obiektów blob nie są już potrzebne, można usunąć za pomocą całego kontenera [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Spowoduje to również usunięcie plików w kontenerze.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu przedstawiono sposób przesyłania plików między magazynu obiektów Blob platformy Azure przy użyciu języka Java i dysku lokalnego. Aby dowiedzieć się więcej na temat pracy z magazynem obiektów Blob, nadal do magazynu obiektów Blob, jak to zrobić.

> [!div class="nextstepaction"]
> [Porada operacje magazynu obiektów blob](storage-java-how-to-use-blob-storage.md)

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zobacz [zasobami magazynu obiektów Blob platformy Azure zarządzać za pomocą Eksploratora usługi Storage](../../vs-azure-tools-storage-explorer-blobs.md).