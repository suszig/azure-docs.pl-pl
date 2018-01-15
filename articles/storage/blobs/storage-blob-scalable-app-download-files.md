---
title: "Pobierz dużych ilości losowe dane z usługi Azure Storage | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać zestawu Azure SDK do pobierania dużych ilości losowe dane z konta usługi Azure Storage"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 3842860acb1c0fdd9e07f6d2f678ac5d5304003b
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Pobierz dużych ilości losowe dane z usługi Azure storage

W tym samouczku jest częścią trzech serii. Ten samouczek pokazuje, jak pobrać dużych ilości danych z magazynu Azure.

W trzech części serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Aktualizowanie aplikacji
> * Uruchamianie aplikacji
> * Sprawdzanie poprawności liczby połączeń

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka, należy wykonać poprzednie samouczek magazynu: [przekazać dużych ilości danych losowych równolegle do magazynu Azure][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Zdalnego do maszyny wirtualnej

 Aby utworzyć sesję pulpitu zdalnego z maszyną wirtualną, wpisz następujące polecenie na komputerze lokalnym. Adres IP należy zastąpić publicznego adresu IP maszyny wirtualnej. Po wyświetleniu monitu wprowadź poświadczenia używane podczas tworzenia maszyny wirtualnej.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Aktualizowanie aplikacji

W samouczku poprzedniej tylko przekazany do konta magazynu plików. Otwórz `D:\git\storage-dotnet-perf-scale-app\Program.cs` w edytorze tekstów. Zastąp `Main` metodę z następującym przykładowym. Ten przykład komentarze out zadania przekazywania i uncomments zadań pobierania i zadań, aby usunąć zawartość na koncie magazynu po zakończeniu.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initialy
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Po zaktualizowaniu aplikacji, należy skompilować ponownie aplikację. Otwórz `Command Prompt` i przejdź do `D:\git\storage-dotnet-perf-scale-app`. Odbuduj aplikacji przez uruchomienie `dotnet build` jak pokazano w poniższym przykładzie:

```
dotnet build
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz, gdy aplikacja został przebudowany nadszedł czas na uruchamianie aplikacji z zaktualizowanego kodu. Jeśli nie już otwarty, otwórz `Command Prompt` i przejdź do `D:\git\storage-dotnet-perf-scale-app`.

Typ `dotnet run` do uruchomienia aplikacji.

```
dotnet run
```

Aplikacja odczytuje kontenery znajdujących się na koncie magazynu określony w **storageconnectionstring**. Iteruje on obiekty BLOB 10 czasu za pomocą [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) w kontenery i pliki do pobrania je do lokalnej komputera przy użyciu metody [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) metody.
W poniższej tabeli przedstawiono [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) które są definiowane dla każdego obiektu blob podczas pobierania.

|Właściwość|Wartość|Opis|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| prawda| Ta właściwość wyłącza sprawdzanie Skrót MD5 zawartość przesłana. Wyłączenie sprawdzania poprawności MD5 zapewnia szybsze transferu. Ale nie ma potwierdzenia ważności lub integralność przesyłania plików. |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| fałsz| Ta właściwość określa, czy skrót MD5 jest obliczany i przechowywane.   |

`DownloadFilesAsync` Zadań przedstawiono w poniższym przykładzie:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestionOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Sprawdzanie poprawności połączenia

Podczas pobierania plików do konta magazynu można sprawdzić liczbę równoczesnych połączeń. Otwórz `Command Prompt` i typu `netstat -a | find /c "blob:https"`. To polecenie przedstawia liczbę połączeń, które są obecnie otwarte przy użyciu `netstat`. Poniższy przykład przedstawia podobne dane wyjściowe, aby zobaczyć podczas uruchamiania samouczka samodzielnie. Jak widać w przykładzie powyżej 280 połączenia były otwarte podczas pobierania losowe pliki z konta magazynu.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Kolejne kroki

W trzech części serii wiesz o pobieraniu dużych ilości losowe dane z konta magazynu, np.:

> [!div class="checklist"]
> * Uruchamianie aplikacji
> * Sprawdzanie poprawności liczby połączeń

Przejście do czterech częścią serii, aby sprawdzić metryki przepustowości i opóźnień w portalu.

> [!div class="nextstepaction"]
> [Sprawdź metryki przepustowości i opóźnień w portalu](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md