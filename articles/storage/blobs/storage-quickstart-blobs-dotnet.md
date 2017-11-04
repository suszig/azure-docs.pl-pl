---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu platformy .NET | Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu platformy .NET"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/01/2017
ms.author: robinsh
ms.openlocfilehash: fdba4588fbb2c46efb3fc4de1a9e53414264444a
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu platformy .NET

W tym szybkiego startu Dowiedz się jak używać języka C# .NET przekazywanie, pobieranie i listy blokowych obiektów blob w kontenerze w magazynie obiektów Blob platformy Azure w systemie Windows.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* Zainstaluj [programu Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) z obciążeniem, następujące:
    - **Tworzenie aplikacji na platformie Azure**

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-storage-account-using-the-azure-portal"></a>Utwórz konto magazynu przy użyciu portalu Azure

Najpierw utwórz nowe konto magazynu ogólnego przeznaczenia na potrzeby tego przewodnika Szybki Start. 

1. Przejdź do [portalu Azure](https://portal.azure.com) i zaloguj się za pomocą konta platformy Azure. 
2. W menu Centrum wybierz **nowy** > **magazynu** > **konta magazynu — obiekt blob, plików, tabeli, kolejki**. 
3. Wprowadź nazwę konta magazynu. Nazwa musi mieć od 3 do 24 znaków i może zawierać tylko cyfry i małe litery. Również musi być unikatowa.
4. Ustaw `Deployment model` do **Menedżera zasobów**.
5. Ustaw `Account kind` do **ogólnego przeznaczenia**.
6. Ustaw `Performance` do **standardowe**. 
7. Ustaw `Replication` do **magazyn lokalnie nadmiarowy (LRS)**.
8. Ustaw `Storage service encryption` do **wyłączone**.
9. Ustaw `Secure transfer required` do **wyłączone**.
10. Wybierz subskrypcję. 
11. Aby uzyskać `resource group`, Utwórz nową i nadaj unikatową nazwę. 
12. Wybierz `Location` dla konta magazynu.
13. Sprawdź **Przypnij do pulpitu nawigacyjnego** i kliknij przycisk **Utwórz** można utworzyć konta magazynu. 

Po utworzeniu konta magazynu jest przypięta do pulpitu nawigacyjnego. Kliknij go, aby go otworzyć. W obszarze Ustawienia, kliknij przycisk **klucze dostępu**. Wybierz klucz i skopiuj parametry połączenia do Schowka, a następnie wklej go w edytorze tekstu do późniejszego użycia.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

Przykładowa aplikacja używana w tym szybkiego startu jest aplikacji konsoli podstawowe. 

Użyj [git](https://git-scm.com/) do pobrania kopii aplikacji w środowisku deweloperskim. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

To polecenie klonów repozytorium do folderu lokalnego git. Aby otworzyć rozwiązanie programu Visual Studio, wyszukaj folder magazynu — obiekty BLOB dotnet — Szybki Start, otwórz go, a następnie kliknij dwukrotnie na dotnet quickstart.sln, magazynu obiektów blob w —. 

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu

W aplikacji musisz podać parametry połączenia dla konta magazynu. Otwórz `app.config` plików w Eksploratorze rozwiązań w programie Visual Studio. Znajdź `StorageConnectionString` wpisu. Aby uzyskać **wartość**, Zastąp całą wartość parametrów połączenia zapisany w portalu Azure. Twoje `storageConnectionString` powinien wyglądać podobnie do następującego:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Uruchom próbki

Ten przykład tworzy plik testowy w folderze Moje dokumenty, przekazanie jej do magazynu obiektów Blob, zawiera listę obiektów blob w kontenerze, a następnie pobiera plik z nową nazwą, możesz porównać starych i nowych plików. 

Uruchom próbkę, naciskając klawisz F5. Przedstawia on dane wyjściowe w oknie konsoli, który jest podobny do następującego: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Naciśnij dowolny klawisz, aby kontynuować, usuwa się kontenera magazynu i plików. Przed kontynuowaniem sprawdź Moje dokumenty dwa pliki. Można je otworzyć i sprawdzić, czy są one takie same. Skopiuj adres URL obiektu blob poza okno konsoli i wklej go do przeglądarki, aby wyświetlić zawartość pliku w magazynie obiektów Blob.

Można również użyć narzędzia takie jak [Eksploratora usługi Storage Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) do wyświetlania plików w magazynie obiektów Blob. Eksplorator usługi Storage platformy Azure to bezpłatne narzędzie i platform umożliwiający dostęp do danych konta magazynu. 

Po zweryfikowaniu pliki naciśnij dowolny klawisz, aby zakończyć pokaz i usuwanie plików testowych. Teraz, znając prezentowanym przykładzie, otwórz plik Program.cs, aby przyjrzeć się kodu. 

## <a name="get-references-to-the-storage-objects"></a>Pobierz odwołania do obiektów magazynu

Najpierw musisz jest utworzyć odwołania do obiektów używane do uzyskania dostępu i zarządzania magazynem obiektów Blob. Te obiekty kompilacji na każdym z nich — są używane przez kolejnego na liście.

* Utwórz wystąpienie **CloudStorageAccount** obiektu wskazuje konta magazynu. 

* Utwórz wystąpienie **CloudBlobClient** obiektu, który wskazuje usługi obiektów Blob na koncie magazynu. 

* Utwórz wystąpienie **CloudBlobContainer** obiektu, który reprezentuje uzyskują dostęp do kontenera. Kontenery są używane do organizowania obiektów blob, jak używać folderów na komputerze do organizowania plików.

Po utworzeniu **CloudBlobContainer**, można utworzyć wystąpienia **CloudBlockBlob** obiekt, który wskazuje konkretnego obiektu blob, w którym interesuje i wykonaj przekazywanie, pobieranie kopiowania, itp. Operacja.

W tej sekcji utworzenia wystąpienia obiektów, Utwórz nowy kontener i następnie ustawić uprawnień dla kontenera obiektów blob są publiczne i jest możliwy tylko adres URL. Kontener jest nazywany **quickstartblobs**. 

W tym przykładzie użyto **CreateIfNotExists** ponieważ chcemy, aby utworzyć nowy kontener za każdym razem próbki jest uruchamiany. W środowisku produkcyjnym, której używasz tego samego kontenera w całej aplikacji, jest lepsze praktyką wywoływać tylko **CreateIfNotExists** po. Alternatywnie można utworzyć kontenera wcześniejsze, więc nie zostanie konieczne utworzenie go w kodzie.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

## <a name="upload-blobs-to-the-container"></a>Przekaż obiekty BLOB do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty BLOB są najczęściej używane i która jest używana w tym Szybki Start. 

Można przekazać pliku do obiektu blob, należy pobrać odwołanie do obiektu blob w kontenerze docelowej. Po uzyskaniu odwołania do obiektu blob możesz przekazać dane do niego przy użyciu [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Ta operacja tworzy obiektu blob, jeśli jeszcze nie istnieje lub zastąpiony, jeśli już istnieje.

Przykładowy kod tworzy plik lokalny do zastosowania w przypadku przekazywania i pobierania, przechowywanie plików do przekazania jako **fileAndPath** i nazwa obiektu blob w **LokalnaNazwaPliku**. Poniższy przykład powoduje przekazanie pliku z kontenera o nazwie **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

Istnieje kilka metod przekazywania, korzystających z magazynu obiektów Blob. Na przykład jeśli masz strumienia pamięci służy metoda UploadFromStreamAsync zamiast UploadFromFileAsync. 

Blokowe obiekty BLOB może być dowolnego typu text lub pliku binarnego. Stronicowe obiekty BLOB są głównie używane dla pliki VHD używane do tworzenia kopii maszyn wirtualnych IaaS. Dołącz obiekty BLOB są używane do logowania, takie jak kiedy zachodzi potrzeba zapisane do pliku i następnie dodać więcej informacji. Większość obiektów przechowywanych w magazynie obiektów Blob są blokowych obiektów blob.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Zostanie wyświetlona lista plików za pomocą kontenera [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Poniższy kod pobiera listę obiektów blob, a następnie w pętli, przedstawiający identyfikatorów URI obiektów blob znalezionych. Można skopiować identyfikator URI w oknie polecenia i wklej go w przeglądarce, aby wyświetlić plik.

Jeśli masz 5000 lub mniej obiektów blob w kontenerze, pobierane są wszystkie nazwy obiektów blob w jednym wywołaniu ListBlobsSegmentedAsync. Jeśli masz więcej niż 5000 obiekty BLOB w kontenerze, usługa pobiera listę w zestawach 5000, dopóki wszystkie nazwy obiektów blob zostały pobrane. Dlatego po raz pierwszy wywołać tego interfejsu API, zwraca pierwszy nazwy 5000 obiektów blob i token kontynuacji. Drugim, podaj token, usługa pobiera następnego zestawu nazwy obiektów blob i itd., dopóki token kontynuacji ma wartość null, co oznacza, że wszystkie nazwy obiektów blob zostały pobrane. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

## <a name="download-blobs"></a>Pobieranie obiektów blob

Pobieranie obiektów blob przy użyciu dysku lokalnym [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Poniższy kod pobiera blob przekazany w poprzedniej sekcji, dodanie sufiksu "_DOWNLOADED" Nazwa obiektu blob, dzięki czemu oba pliki na dysku lokalnym. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przekazane w tego przewodnika Szybki Start obiektów blob nie są już potrzebne, można usunąć za pomocą całego kontenera [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). To również usunięcie plików utworzony, jeśli nie są już potrzebne.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu przedstawiono sposób przesyłania plików między dyskiem lokalnym i magazynu obiektów Blob platformy Azure przy użyciu platformy .NET. Aby dowiedzieć się więcej na temat pracy z magazynem obiektów Blob, nadal do magazynu obiektów Blob, jak to zrobić.

> [!div class="nextstepaction"]
> [Porada operacje magazynu obiektów blob](storage-dotnet-how-to-use-blobs.md)

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zobacz [zasobami magazynu obiektów Blob platformy Azure zarządzać za pomocą Eksploratora usługi Storage](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
