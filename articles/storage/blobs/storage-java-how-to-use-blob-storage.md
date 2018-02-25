---
title: "Jak używać magazynu obiektów Blob platformy Azure (obiekt magazynu) w języku Java | Dokumentacja firmy Microsoft"
description: "Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 4b243aa7a4e9aa3028259258292d1271867d28f4
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="how-to-use-blob-storage-from-java"></a>Jak używać Magazynu obiektów Blob w języku Java
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Przegląd
Magazyn obiektów blob Azure jest usługą służącą do przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

W tym artykule opisano sposób wykonywania typowych scenariuszy przy użyciu magazynu obiektów Blob Microsoft Azure. Przykłady są napisane w języku Java i użyj [Azure Storage SDK for Java][Azure Storage SDK for Java]. Omówione scenariusze obejmują **przekazywania**, **wyświetlania**, **pobieranie**, i **usuwanie** obiektów blob. Aby uzyskać więcej informacji dotyczących obiektów blob, zobacz [następne kroki](#Next-Steps) sekcji.

> [!NOTE]
> Zestaw SDK jest dostępny dla deweloperów, którzy korzystają z usługi Azure Storage na urządzeniach z systemem Android. Aby uzyskać więcej informacji, zobacz [zestawu SDK usługi Magazyn Azure dla systemu Android][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java
W tym artykule użyje funkcji magazynu, które mogą być uruchamiane w ramach aplikacji Java lokalnie lub w kodzie działających w roli sieci web lub roli proces roboczy na platformie Azure.

Aby to zrobić, należy zainstalować zestaw Java Development Kit (JDK) i utworzyć konto magazynu Azure w ramach subskrypcji platformy Azure. Po zostało to zrobione, należy sprawdzić, czy platformie programistycznej spełnia minimalne wymagania i zależności, które są wymienione w [Azure Storage SDK for Java] [ Azure Storage SDK for Java] repozytorium w witrynie GitHub. Jeżeli system spełnia te wymagania, należy wykonać instrukcje dotyczące pobierania i instalowania biblioteki magazynu Azure dla języka Java w systemie z tego repozytorium. Po wykonaniu tych zadań, można utworzyć aplikacji Java, który używa przykłady w tym artykule.

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurowanie aplikacji dostęp do magazynu obiektów Blob
Dodaj następujące instrukcje import u góry pliku języka Java, którym chcesz uzyskać dostępu do obiektów blob przy użyciu interfejsów API magazynu Azure.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu Azure
Klient usługi Azure Storage używa parametrów połączenia magazynu do przechowywania punktów końcowych i poświadczeń do uzyskiwania dostępu do danych usługi zarządzania. Podczas uruchamiania w aplikacji klienckiej, należy podać parametry połączenia magazynu w następującym formacie, przy użyciu nazwy konta magazynu i podstawowy klucz dostępu dla konta magazynu na liście [portalu Azure](https://portal.azure.com) dla *AccountName* i *AccountKey* wartości. W poniższym przykładzie pokazano, jak można zadeklarować pola statycznego do przechowywania parametrów połączenia.

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

W aplikacji działającej w ramach roli w systemie Microsoft Azure, te parametry mogą być przechowywane w pliku konfiguracji usługi *pliku ServiceConfiguration.cscfg*i jest dostępny w wyniku wywołania **RoleEnvironment.getConfigurationSettings** metody. Poniższy przykład pobiera parametry połączenia z **ustawienie** elementu o nazwie *StorageConnectionString* w pliku konfiguracji usługi.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Poniższe przykłady założono użycie jednej z tych dwóch metod można pobrać parametry połączenia magazynu.

## <a name="create-a-container"></a>Tworzenie kontenera
A **CloudBlobClient** obiektu pozwala pobierać obiekty odwołań dla kontenerów i obiektów blob. Poniższy kod tworzy **CloudBlobClient** obiektu.

> [!NOTE]
> Istnieją dodatkowe sposoby tworzenia **CloudStorageAccount** obiektów; Aby uzyskać więcej informacji, zobacz **CloudStorageAccount** w [odwołania do zestawu SDK klienta magazynu Azure].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Użyj **CloudBlobClient** obiekt, aby pobrać odwołanie do kontenera, którego chcesz użyć. Kontener można utworzyć, jeśli nie istnieje z **createIfNotExists** metody, które w przeciwnym razie zwraca istniejącego kontenera. Domyślnie nowy kontener jest prywatny, dlatego należy określić klucz dostępu do magazynu (tak samo jak wcześniej) aby pobierać obiekty BLOB z tego kontenera.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a>Opcjonalnie: Konfigurowanie kontener dla dostępu publicznego
Kontener uprawnienia są domyślnie skonfigurowane dla dostępu prywatnego, ale można łatwo skonfigurować kontenera uprawnień umożliwiających dostęp do publicznego, tylko do odczytu dla wszystkich użytkowników w Internecie:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Aby przekazać pliku do obiektu blob, Pobierz odwołanie do kontenera i użyj go, aby pobrać odwołanie do obiektu blob. Po utworzeniu odwołanie do obiektu blob możesz przekazać strumieniem, wywołując przekazywania na odwołanie do obiektu blob. Ta operacja spowoduje utworzenie obiektu blob, jeśli nie istnieje lub jego zastąpienie, jeśli istnieje. Poniższy przykład kodu pokazuje i przyjęto założenie, że kontener został już utworzony.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, należy najpierw pobrać odwołanie do kontenera, jak przekazać obiekt blob. Korzystając z kontenera **listBlobs** metody z **dla** pętli. Poniższy kod wyświetla identyfikator Uri każdy obiekt blob w kontenerze do konsoli.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Należy pamiętać, nazwę obiekty BLOB zawierające informacje o ścieżce ich nazw. Powoduje to utworzenie wirtualnej struktury katalogów, które można organizować i przechodzić między nimi tak jak w przypadku tradycyjnego systemu plików. Należy pamiętać, że struktura katalogów jest wyłącznie wirtualna — jedyne zasoby dostępne w Magazynie obiektów blob to kontenery i obiekty blob. Jednak biblioteka klienta oferuje **CloudBlobDirectory** obiekt, aby odwoływać się do katalogu wirtualnego i uprościć proces Praca z obiektami blob zorganizowanymi w ten sposób.

Na przykład można mieć kontener o nazwie "zdjęć", w których może przekazać obiekty BLOB o nazwie "rootphoto1", "2010/photo1", "2010/photo2" i "2011/photo1". Spowoduje to utworzenie katalogi wirtualne "2010" i "2011" w kontenerze "zdjęć". Podczas wywoływania **listBlobs** w kontenerze "fotografie" Kolekcja zwracana będzie zawierać **CloudBlobDirectory** i **CloudBlob** obiekty reprezentujące katalogów i obiekty BLOB zawarte na najwyższym poziomie. W takim przypadku katalogi "2010" i "2011", a także photo "rootphoto1" zostałaby zwrócona. Można użyć **instanceof** operatora, aby odróżnić tych obiektów.

Opcjonalnie można przekazać parametry **listBlobs** metody z **useFlatBlobListing** parametr ma wartość true. Spowoduje to każdy obiekt blob zostały zwrócone, niezależnie od tego katalogu. Aby uzyskać więcej informacji, zobacz **CloudBlobContainer.listBlobs** w [odwołania do zestawu SDK klienta magazynu Azure].

## <a name="download-a-blob"></a>Pobieranie obiektu blob
Aby pobrać obiekty BLOB, wykonaj te same czynności, ponieważ została ona przekazywanie obiektu blob, aby pobrać odwołanie do obiektu blob. W przykładzie przekazywania przekazywania jest wywoływana dla obiektu blob. W poniższym przykładzie, wywołanie pobierania, aby przesłać zawartość obiektu blob do obiektu strumienia, takich jak **FileOutputStream** można utrwalić obiektu blob do pliku lokalnego.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a>Usuwanie obiektu blob
Aby usunąć obiekt blob, Pobierz odwołanie do obiektu blob i wywołanie **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a>Usunięcie kontenera obiektów blob
Na koniec można usunąć kontenera obiektów blob, Pobierz obiekt blob odwołanie do kontenera i wywołanie **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Kolejne kroki
Teraz, kiedy znasz już podstawy magazynu obiektów Blob, skorzystaj z poniższych linków, aby dowiedzieć się więcej o bardziej skomplikowanych zadaniach magazynu.

* [Magazyn Azure SDK dla języka Java][Azure Storage SDK for Java]
* [Odwołanie do zestawu SDK klienta usługi Azure Storage][odwołania do zestawu SDK klienta magazynu Azure]
* [Interfejs API REST usługi Azure Storage][Azure Storage REST API]
* [Azure Storage Team Blog][Azure Storage Team Blog]

Aby uzyskać więcej informacji, zobacz też [Azure dla deweloperów języka Java](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[odwołania do zestawu SDK klienta magazynu Azure]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
