---
title: "Azure Quickstart — przekazywanie, pobieranie i wyświetlanie listy obiektów blob w usłudze Azure Storage za pomocą platformy .NET | Microsoft Docs"
description: "W tym przewodniku Szybki start utworzysz konto magazynu i kontener. Następnie przy użyciu biblioteki klienta platformy .NET przekażesz obiekt blob do usługi Azure Storage, pobierzesz obiekt blob i wyświetlisz listę obiektów blob w kontenerze."
services: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: tamram
ms.openlocfilehash: 265691ff189c628156f234083645a4b2ca4b637b
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-net"></a>Szybki start: przekazywanie, pobieranie i wyświetlanie listy obiektów blob za pomocą platformy .NET

Dzięki temu przewodnikowi Szybki start dowiesz się, w jaki sposób za pomocą biblioteki klienckiej platformy .NET dla usługi Azure Storage przekazywać, pobierać i wyświetlać listę blokowych obiektów blob w kontenerze.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki start:

* Zainstaluj program .NET Core 2.0 dla systemu [Linux](/dotnet/core/linux-prerequisites?tabs=netcore2x) lub [Windows](/dotnet/core/windows-prerequisites?tabs=netcore2x)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

Przykładowa aplikacja używana w tym przewodniku Szybki start to podstawowa aplikacja konsoli. 

Użyj narzędzia [git](https://git-scm.com/), aby pobrać kopię tej aplikacji do swojego środowiska projektowego. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

To polecenie klonuje repozytorium do lokalnego folderu git. Aby otworzyć rozwiązanie programu Visual Studio, wyszukaj folder storage-dotnet-quickstart, otwórz go i kliknij dwukrotnie pozycję storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu

W aplikacji należy wprowadzić parametry połączenia konta magazynu. Zaleca się do przechowywanie tych parametrów połączenia w zmiennej środowiskowej na maszynie lokalnej, na której uruchomiona jest aplikacja. Postępuj zgodnie z jednym z poniższych przykładów w zależności od używanego systemu operacyjnego, aby utworzyć zmienną środowiskową.  Zastąp element \<yourconnectionstring\> własnymi parametrami połączenia.

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```
### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

Ta aplikacja przykładowa tworzy plik testowy w folderze Moje dokumenty, przesyła go do usługi Blob Storage, tworzy listę obiektów blob w kontenerze, a następnie pobiera plik z nową nazwą, tak aby można było porównać stary i nowy plik. 

Przejdź do katalogu aplikacji, a następnie uruchom aplikację za pomocą polecenia `dotnet run`.

```
dotnet run
```

Wyświetlone dane wyjściowe są podobne do poniższego przykładu:

```
Azure Blob storage quick start sample
Temp file = /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Uploading to Blob storage as blob 'QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt'
List blobs in container.
https://mystorageaccount.blob.core.windows.net/quickstartblobs/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Downloading blob to /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374_DOWNLOADED.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

Po naciśnięciu dowolnego klawisza w celu kontynuowania kontener magazynu i pliki zostaną usunięte. Przed kontynuowaniem sprawdź folder Moje dokumenty pod kątem tych dwóch plików. Możesz je otworzyć i sprawdzić, czy są identyczne. Skopiuj adres URL dla obiektu blob z okna konsoli i wklej go do przeglądarki, aby wyświetlić zawartość pliku w usłudze Blob Storage.

Możesz również wyświetlić pliki w usłudze Blob Storage za pomocą narzędzia takiego jak [Eksplorator usługi Azure Storage](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Eksplorator usługi Azure Storage to darmowe narzędzie międzyplatformowe, które umożliwia dostęp do informacji na koncie magazynu. 

Po zweryfikowaniu plików naciśnij dowolny klawisz, aby zakończyć demonstrację i usunąć pliki testowe. Wiesz już, jak działa aplikacja przykładowa. Otwórz plik Program.cs i spójrz na kod. 

## <a name="understand-the-sample-code"></a>Omówienie przykładowego kodu

W kolejnej części omówimy przykładowy kod, aby wyjaśnić, w jaki sposób działa.

### <a name="get-references-to-the-storage-objects"></a>Pobieranie odwołań do obiektów magazynu

Najpierw należy utworzyć odwołania do obiektów używane w celu uzyskania dostępu do usługi Blob Storage i zarządzania nią. Te obiekty są powiązane — każdy obiekt jest używany przez kolejny na liście.

* Utwórz wystąpienie obiektu [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet) wskazujące konto magazynu.

* Utwórz wystąpienie obiektu [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet), które wskazuje na usługę Blob Service na koncie magazynu.

* Utwórz wystąpienie obiektu [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet) reprezentujące kontener, do którego uzyskujesz dostęp. Kontenery są używane do porządkowania obiektów blob w ten sam sposób, w jaki foldery na komputerze są używane do porządkowania plików.

Gdy już masz obiekt [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet), możesz tworzyć odwołanie do obiektu [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet) wskazującego konkretny obiekt blob, który Cię interesuje, i wykonywać operacje przekazywania, pobierania, kopiowania itp.

> [!IMPORTANT]
> Nazwy kontenerów muszą być zapisane małymi literami. Aby uzyskać dodatkowe informacje o regułach nazewnictwa kontenerów i obiektów blob, zobacz [Nazewnictwo i odwoływanie się do kontenerów, obiektów blob i metadanych](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Ta sekcja poświęcona jest tworzeniu wystąpień obiektów, tworzeniu nowego kontenera, a następnie konfigurowaniu uprawnień w kontenerze, tak aby obiekty blob były publiczne i można było do nich uzyskać dostęp za pomocą samego adresu URL. Kontener nazywa się **quickstartblobs**.

W tym przykładzie użyto metody [CreateIfNotExists](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexists?view=azure-dotnet), ponieważ chcemy utworzyć nowy kontener za każdym razem, gdy jest uruchamiana aplikacja przykładowa. W środowisku produkcyjnym, w którym korzystasz z tego samego kontenera w całej aplikacji, lepiej jest wywołać metodę **CreateIfNotExists** tylko raz. Możesz również utworzyć kontener wcześniej, aby nie było konieczne tworzenie go w kodzie.

```csharp
// Load the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");
if (storageConnectionString == null)
{
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add a environment variable name 'storageconnectionstring' with the actual storage " +
        "connection string as a value.");
}
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConnectionString);

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Przekazywanie obiektów blob do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty blob są używane najczęściej i dlatego zostały użyte w tym przewodniku Szybki start.

Aby przekazać plik do obiektu blob, pobierz odwołanie do obiektu blob w kontenerze docelowym. Po uzyskaniu odwołania do obiektu blob możesz przekazać do niego dane przy użyciu polecenia [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Ta operacja tworzy obiekt blob, jeśli jeszcze nie istnieje, lub zastępuje go, jeśli już istnieje.

Przykładowy kod tworzy plik lokalny do zastosowania w przypadku przekazywania i pobierania, przechowujący plik do przekazania jako **fileAndPath** i nazwę obiektu blob w **localFileName**. Następujący kod przykładowy przekazuje plik do kontenera o nazwie **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the location where the blob is going to go, then upload the file.
// Upload the file you created, use localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
```

Istnieje kilka metod przekazywania, których można użyć z usługą Blob Storage. Na przykład jeśli masz strumień pamięci, możesz użyć metody UploadFromStreamAsync zamiast UploadFromFileAsync.

Blokowe obiekty blob mogą mieć formę dowolnego typu pliku tekstowego lub binarnego. Stronicowe obiekty blob są używane głównie do tworzenia plików VHD służących do obsługi maszyn wirtualnych IaaS. Uzupełnialne obiekty blob są używane do rejestrowania, na przykład w sytuacji, w której konieczny jest zapis do pliku, a następnie dodawanie kolejnych informacji. Większość obiektów przechowywanych w usłudze Blob Storage to blokowe obiekty blob.

### <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Możesz uzyskać listę plików w kontenerze za pomocą polecenia [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Poniższy kod umożliwia pobranie listy obiektów blob, a następnie przetwarza je w pętli, wyświetlając identyfikatory URI odnalezionych obiektów blob. Możesz skopiować identyfikator URI z okna polecenia i wkleić go do przeglądarki, aby wyświetlić plik.

Jeśli kontener zawiera 5000 lub mniej obiektów blob, wszystkie nazwy obiektów blob są pobierane w jednym wywołaniu ListBlobsSegmentedAsync. Jeśli kontener zawiera więcej niż 5000 obiektów blob, usługa pobiera listę w zestawach po 5000 do momentu pobrania wszystkich nazw obiektów blob. Dlatego pierwsze wywołanie tego interfejsu API zwraca 5000 pierwszych nazw obiektów blob i token kontynuacji. Za drugim razem należy podać token, a usługa pobierze następny zestaw nazw obiektów blob i tak dalej, aż do momentu, gdy wartość tokenu kontynuacji wyniesie null, co oznacza pobranie wszystkich nazw obiektów blob.

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
    blobContinuationToken = results.ContinuationToken;
} while (blobContinuationToken != null);

```

### <a name="download-blobs"></a>Pobieranie obiektów blob

Można pobierać obiekty blob na dysk lokalny przy użyciu polecenia [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Następujący kod pobiera obiekty blob przekazane w poprzedniej sekcji, dodając sufiks „_DOWNLOADED” do nazwy obiektu blob, tak aby oba pliki były widoczne na dysku lokalnym.

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie potrzebujesz już obiektów blob przekazanych podczas pracy z tym przewodnikiem Szybki start, możesz usunąć cały kontener, korzystając z polecenia [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Usuń także utworzone pliki, jeśli nie są już potrzebne.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the sample files, example container, and exit the application.");
Console.ReadLine();
// Clean up resources. This includes the container and the two temp files.
Console.WriteLine("Deleting the container");
if (cloudBlobContainer != null)
{
    await cloudBlobContainer.DeleteIfExistsAsync();
}
Console.WriteLine("Deleting the source, and downloaded files");
File.Delete(sourceFile);
File.Delete(destinationFile);
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start przedstawiono metodę transferowania plików między dyskiem lokalnym i usługą Azure Blob Storage przy użyciu platformy .NET. Aby dowiedzieć się więcej na temat pracy z usługą Blob Storage, przejdź do instrukcji dotyczących magazynu obiektów blob.

> [!div class="nextstepaction"]
> [Instrukcje: Operacje wykonywane w usłudze Blob Storage](storage-dotnet-how-to-use-blobs.md)

Nasza lista [przykładów usługi Azure Storage korzystających z platformy .NET](../common/storage-samples-dotnet.md) zawiera dodatkowe przykłady kodu usługi Azure Storage, które można pobierać i uruchamiać.

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zapoznaj się artykułem [Manage Azure Blob storage resources with Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Zarządzanie zasobami usługi Azure Blob Storage za pomocą Eksploratora usługi Storage).
