<properties
    pageTitle="Rozpoczynanie pracy z Magazynem obiektów blob Azure przy użyciu programu .NET | Microsoft Azure"
    description="Przechowuj dane niestrukturalne w chmurze za pomocą Magazynu obiektów blob Azure."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="tamram"/>


# Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## Omówienie

Magazyn obiektów blob Azure jest usługą służącą do przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

### Informacje o tym samouczku

W tym samouczku pokazano, jak napisać kod .NET dla niektórych typowych scenariuszy przy użyciu Magazynu obiektów blob Azure. Przedstawione scenariusze obejmują przekazywanie, pobieranie i usuwanie obiektów blob oraz wyświetlanie ich listy.

**Szacowany czas trwania:** 45 minut

**Wymagania wstępne:**

- [Program Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Biblioteka klienta usługi Azure Storage dla programu .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Menedżer konfiguracji Azure dla programu .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Konto usługi Azure Storage](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### Więcej przykładów

Dodatkowe przykłady użycia usługi Blob Storage znajdziesz w temacie [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) (Rozpoczynanie pracy z usługą Azure Blob Storage w programie .NET). Możesz pobrać przykładową aplikację i uruchomić ją lub przejrzeć kod w witrynie GitHub. 


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### Dodawanie deklaracji przestrzeni nazw

Dodaj następujące instrukcje `using` na początku pliku `program.cs`:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### Analizowanie parametrów połączenia

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### Tworzenie klienta usługi Blob

Klasa **CloudBlobClient** umożliwia pobieranie kontenerów i obiektów blob przechowywanych w usłudze Blob Storage. Oto jeden ze sposobów tworzenia klienta usługi:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Teraz możesz przystąpić do pisania kodu, który będzie odczytywać dane z Magazynu obiektów blob i zapisywać je w nim.

## Tworzenie kontenera

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

W tym przykładzie pokazano, jak utworzyć kontener, jeśli jeszcze nie istnieje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Domyślnie nowy kontener jest prywatny, co oznacza, że musisz podać klucz dostępu do magazynu, aby pobierać obiekty blob z tego kontenera. Jeśli chcesz, aby pliki w kontenerze były dostępne dla wszystkich, możesz ustawić kontener jako publiczny przy użyciu następującego kodu:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Wszyscy użytkownicy Internetu mogą wyświetlać obiekty blob w kontenerze publicznym, ale można je modyfikować lub usuwać tylko przy użyciu odpowiedniego klucza dostępu do konta lub sygnatury dostępu współdzielonego.

## Przekazywanie obiektu blob do kontenera

Azure Blob Storage obsługuje blokowe i stronicowe obiekty blob.  W większości przypadków zalecane jest użycie blokowych obiektów blob.

Aby przekazać plik do blokowego obiektu blob, pobierz odwołanie do kontenera i uzyskaj za jego pomocą odwołanie do blokowego obiektu blob. Po uzyskaniu odwołania do obiektu blob możesz przekazać do niego dowolny strumień danych, wywołując metodę **UploadFromStream** . Ta operacja spowoduje utworzenie obiektu blob, jeśli jeszcze nie istnieje, lub jego zastąpienie, jeśli już istnieje.

W poniższym przykładzie przedstawiono, jak przekazać obiekt blob do kontenera, zakładając, że kontener został już utworzony.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## Wyświetlanie listy obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, należy najpierw uzyskać odwołanie do kontenera. Następnie można użyć metody **ListBlobs** kontenera, aby pobrać obiekty blob i/lub zawarte w nim katalogi. Aby uzyskać dostęp do bogatego zestawu właściwości i metod zwróconego obiektu **IListBlobItem**, należy rzutować go na obiekt **CloudBlockBlob**, **CloudPageBlob** lub **CloudBlobDirectory**.  Jeśli typ jest nieznany, można zastosować sprawdzanie typu, aby określić, do którego obiektu rzutować obiekt.  Poniższy kod przedstawia sposób pobierania i zwracania identyfikatora URI poszczególnych elementów w kontenerze `photos`:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Jak przedstawiono powyżej, obiektom blob można nadawać nazwy zawierające informacje o ścieżce. Powoduje to utworzenie wirtualnej struktury katalogów, które można organizować i przechodzić między nimi tak jak w przypadku tradycyjnego systemu plików. Należy pamiętać, że struktura katalogów jest wyłącznie wirtualna — jedyne zasoby dostępne w Magazynie obiektów blob to kontenery i obiekty blob. Jednak biblioteka klienta magazynu zapewnia obiekt **CloudBlobDirectory** służący do odwoływania się do katalogów wirtualnych i ułatwiający pracę z obiektami blob zorganizowanymi w ten sposób.

Rozważmy na przykład następujący zestaw blokowych obiektów blob w kontenerze o nazwie `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Wywołanie metody **ListBlobs** w kontenerze „photos” (jak w powyższym przykładzie) powoduje zwrócenie listy hierarchicznej. Zawiera ona obiekty **CloudBlobDirectory** i **CloudBlockBlob** reprezentujące odpowiednio katalogi i obiekty blob w kontenerze. Dane wyjściowe wyglądają następująco:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Opcjonalnie można ustawić parametr **UseFlatBlobListing** metody **ListBlobs** na wartość **true**. W takim przypadku każdy obiekt blob w kontenerze będzie zwracany jako obiekt **CloudBlockBlob**. Wywołanie metody **ListBlobs** w celu zwrócenia listy niezhierarchizowanej wygląda następująco:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

Wyniki wyglądają następująco:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## Pobieranie obiektów blob

Aby pobrać obiekty blob, należy najpierw pobrać odwołanie do obiektu blob, a następnie wywołać metodę **DownloadToStream**. W poniższym przykładzie użyto metody **DownloadToStream**, aby przesłać zawartość obiektu blob do obiektu strumienia, który można następnie zachować w pliku lokalnym.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Można również użyć metody **DownloadToStream**, aby pobrać zawartość obiektu blob jako ciąg tekstowy.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## Usuwanie obiektów blob

Aby usunąć obiekt blob, należy najpierw pobrać odwołanie do obiektu blob, a następnie wywołać dla niego metodę **Delete**.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## Asynchroniczne wyświetlanie obiektów blob na stronach

Jeśli chcesz wyświetlić dużą liczbę obiektów blob lub kontrolować liczbę wyników zwracanych przez jedną operację wyświetlania listy, możesz wyświetlić obiekty blob na stronach wyników. W tym przykładzie przedstawiono sposób asynchronicznego zwracania wyników na stronach, dzięki czemu wykonanie nie jest blokowane podczas oczekiwania na zwrócenie dużych zestawów wyników.

W tym przykładzie przedstawiono niezhierarchizowaną listę obiektów blob, ale można również uzyskać listę hierarchiczną, ustawiając parametr `useFlatBlobListing` metody **ListBlobsSegmentedAsync** na wartość `false`.

Ze względu na to, że przykładowa metoda wywołuje metodę asynchroniczną, musi być poprzedzona słowem kluczowym `async` i zwracać obiekt **Task**. Słowo kluczowe await określone dla metody **ListBlobsSegmentedAsync** wstrzymuje wykonywanie przykładowej metody do momentu ukończenia zadania wyświetlania listy.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## Zapisywanie do uzupełnialnego obiektu blob

Uzupełnialny obiekt blob jest nowym typem obiektu blob wprowadzonym w wersji 5.x biblioteki klienta magazynu Azure dla programu .NET. Uzupełnialny obiekt blob jest zoptymalizowany pod kątem operacji dołączania, takich jak rejestrowanie. Podobnie jak blokowy obiekt blob, uzupełnialny obiekt blob jest złożony z bloków, ale nowy blok dodany do uzupełnialnego obiektu blob jest zawsze dołączany na końcu obiektu blob. Nie można zaktualizować lub usunąć istniejącego bloku w uzupełnialnym obiekcie blob. Identyfikatory bloków w uzupełnialnym obiekcie blob nie są widoczne, jak w przypadku blokowego obiektu blob.

Każdy blok w uzupełnialnym obiekcie blob może różnić się rozmiarem, który może wynosić maksymalnie 4 MB, a uzupełnialny obiekt blob może zawierać do 50 000 bloków. Z tego względu maksymalny rozmiar uzupełnialnego obiektu blob nieznacznie przekracza 195 GB (4 MB X 50 000 bloków).

W poniższym przykładzie utworzono nowy uzupełnialny obiekt blob i dołączono do niego określone dane, symulując prostą operację rejestrowania.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Aby uzyskać więcej informacji o różnicach między tymi trzema typami obiektów blob, zobacz [Understanding Block Blobs, Page Blobs, and Append Blobs](https://msdn.microsoft.com/library/azure/ee691964.aspx) (Omówienie blokowych i stronicowych obiektów blob oraz uzupełnialnych obiektów blob).

## Zarządzanie zabezpieczeniami obiektów blob

Domyślnie usługa Azure Storage chroni dane, umożliwiając dostęp wyłącznie właścicielowi konta, który ma klucze dostępu do konta. W przypadku potrzeby udostępnienia danych obiektów blob na koncie magazynu ważne jest, aby zrobić to bez uszczerbku dla bezpieczeństwa kluczy dostępu do konta. Dodatkowo można zaszyfrować dane obiektu blob, aby zapewnić ich bezpieczeństwo podczas przesyłania przez sieć oraz w usłudze Azure Storage.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### Kontrolowanie dostępu do danych obiektów blob

Domyślnie dane obiektów blob na koncie magazynu są dostępne tylko dla właściciela konta magazynu. Domyślnie do uwierzytelniania żądań dotyczących Magazynu obiektów blob jest wymagany klucz dostępu do konta. Możesz jednak udostępnić określone dane obiektów blob innym użytkownikom. Dostępne są dwie opcje:

- **Dostęp anonimowy:** możesz ustawić kontener lub zawarte w nim obiekty blob jako dostępne publicznie, aby umożliwić dostęp anonimowy. Aby uzyskać więcej informacji, zobacz [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md) (Zarządzanie dostępem anonimowym do kontenerów i obiektów blob).
- **Sygnatury dostępu współdzielonego:** możesz zapewnić klientom sygnatury dostępu współdzielonego (SAS), które umożliwiają dostęp delegowany do zasobu na koncie magazynu z uprawnieniami i w przedziale czasu, który określisz. Aby uzyskać więcej informacji, zobacz [Shared Access Signatures: Understanding the SAS Model](storage-dotnet-shared-access-signature-part-1.md) (Sygnatury dostępu współdzielonego: opis modelu sygnatur dostępu współdzielonego).

### Szyfrowanie danych obiektów blob

Usługa Azure Storage obsługuje szyfrowanie danych obiektów blob na kliencie i na serwerze:

- **Szyfrowanie po stronie klienta:** biblioteka klienta usługi Storage dla programu .NET obsługuje szyfrowanie danych w aplikacjach klienckich przed ich przekazaniem do usługi Azure Storage oraz odszyfrowywanie danych przed pobraniem do klienta. Biblioteka obsługuje również integrację z magazynem kluczy Azure do zarządzania kluczami konta magazynu. Aby uzyskać więcej informacji, zobacz [Client-Side Encryption with .NET for Microsoft Azure Storage](storage-client-side-encryption.md) (Szyfrowanie po stronie klienta przy użyciu programu .NET dla usługi Microsoft Azure Storage). Zobacz również [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md) (Samouczek: szyfrowanie i odszyfrowywanie obiektów blob w usłudze Microsoft Azure Storage przy użyciu usługi Azure Key Vault).
- **Szyfrowanie po stronie serwera**: usługa Azure Storage obsługuje teraz szyfrowanie po stronie serwera. Zobacz [Azure Storage Service Encryption for Data at Rest (Preview)](storage-service-encryption.md) (Szyfrowanie usługi Azure Storage dla danych magazynowanych (wersja zapoznawcza)). 

## Następne kroki

Teraz, kiedy znasz już podstawy usługi Blob Storage, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

### Microsoft Azure Storage Explorer
- [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest darmową aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia pracę poglądową z danymi w usłudze Azure Storage w systemie Windows, OS X i Linux.

### Przykłady użycia usługi Blob Storage

- [Rozpoczynanie pracy z usługą Azure Blob Storage w programie .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### Informacje o usłudze Blob Storage

- [Dokumentacja biblioteki klienta usługi Storage dla programu .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Informacje o interfejsie API REST](http://msdn.microsoft.com/library/azure/dd179355)

### Przewodniki koncepcyjne

- [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
- [Wprowadzenie do Magazynu plików dla programu .NET](storage-dotnet-how-to-use-files.md)
- [Jak korzystać z Magazynu obiektów blob platformy Azure przy użyciu zestawu SDK WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Blog zespołu odpowiedzialnego za usługę Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
  [Konfigurowanie parametrów połączenia]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [Informacje o bibliotece klienta .NET]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Informacje o interfejsie API REST]: http://msdn.microsoft.com/library/azure/dd179355



<!--HONumber=sep16_HO1-->


