---
title: "Przekaż dużych ilości danych losowych równolegle do usługi Azure Storage | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak użyć zestawu Azure SDK w celu przekazania dużych ilości danych losowych równolegle do konta usługi Azure Storage"
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
ms.openlocfilehash: 98f3f69c6025d61caac20e13b573651854952432
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/23/2017
---
# <a name="upload-large-amounts-of-random-data-in-parallel-to-azure-storage"></a>Przekaż dużych ilości danych losowych równolegle do magazynu Azure

W tym samouczku jest częścią dwóch serii. Ten samouczek pokazuje, że wdrożyć aplikację, która przekazuje dużej liczby losowe dane do konta magazynu platformy Azure.

W części dwóch serii dowiesz się, jak:

> [!div class="checklist"]
> * Konfigurowanie parametrów połączenia
> * Kompilowanie aplikacji
> * Uruchamianie aplikacji
> * Sprawdzanie poprawności liczby połączeń

Magazyn obiektów blob platformy Azure udostępnia usługę skalowalne do przechowywania danych. Aby upewnić się, że aplikacja jest jako wydajność, jak to możliwe, zrozumienia sposobu działania magazynu obiektów blob jest zalecane. Znajomość ograniczeń dla obiektów blob platformy Azure jest ważne, aby dowiedzieć się więcej na temat tych limitów odwiedź: [wartości docelowe skalowalności magazynu obiektu blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).

[Nadawanie nazw partycji](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#subheading47) jest innym ważnym czynnikiem podczas projektowania aplikacji wysokiej wydajności, za pomocą obiektów blob. Usługa Azure storage wykorzystuje schemat partycjonowania opartej na zakresie skala i wielkość obciążenia równowagi. Ta konfiguracja oznacza, że pliki o podobnej konwencji nazewnictwa lub prefiksy, przejdź do tej samej partycji. Istotą takiej logiki zawiera nazwę kontenera, w którym pliki są przekazywane do. W tym samouczku użyjesz pliki, których identyfikatory GUID w nazwach jako zawartość również jako losowo wygenerowany. Są następnie przekazywane do pięciu różnych kontenerów o nazwach losowych.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka, należy wykonać poprzednie samouczek magazynu: [tworzenia maszyny wirtualnej i konto magazynu dla skalowalnych aplikacji][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Zdalnego do maszyny wirtualnej

Następujące polecenie na komputerze lokalnym do utworzenia sesji usług pulpitu zdalnego z maszyną wirtualną. Adres IP należy zastąpić publicznego adresu IP maszyny wirtualnej. Po wyświetleniu monitu wprowadź poświadczenia, które zostały użyte podczas tworzenia maszyny wirtualnej.

```
mstsc /v:<publicIpAddress>
```

## <a name="configure-the-connection-string"></a>Konfigurowanie parametrów połączenia

W portalu Azure przejdź do swojego konta magazynu. Wybierz **klucze dostępu** w obszarze **ustawienia** na koncie magazynu. Kopiuj **ciąg połączenia** z klucz podstawowy lub pomocniczy. Zaloguj się do maszyny wirtualnej utworzonej w poprzednim samouczka. Otwórz **wiersza polecenia** z uprawnieniami administratora i uruchom `setx` z `/m` przełącznika, to polecenie zapisuje zmienną środowiskową ustawienie maszyny. Zmienna środowiskowa nie jest dostępny do momentu ponownego załadowania **wiersza polecenia**. Zastąp  **\<storageConnectionString\>**  w następującym przykładzie:

```
setx storageconnectionstring "<storageConnectionString>" /m
```

Po zakończeniu operacji otwarcia innego **wiersza polecenia**, przejdź do `D:\git\storage-dotnet-perf-scale-app` i typ `dotnet build` do skompilowania aplikacji.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Przejdź do `D:\git\storage-dotnet-perf-scale-app`.

Typ `dotnet run` do uruchomienia aplikacji. Przy pierwszym uruchomieniu `dotnet` wypełnia pamięć podręczną pakiet lokalny, w celu zwiększenia szybkości przywracania i Włącz dostęp w trybie offline. To polecenie zajmuje na minutę do wykonania i odbywa się tylko raz.

```
dotnet run
```

Aplikacja tworzy pięć kontenery losowo nazwanego i rozpocznie się przekazywanie plików w katalogu przemieszczania na konto magazynu. Aplikacja Ustawia minimalną wątków do 100 i [DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(v=vs.110).aspx) do 100, aby upewnić się, że duża liczba równoczesnych połączeń są dozwolone podczas uruchamiania aplikacji.

Oprócz skonfigurowania ustawień limitu wątków i połączenia [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) dla [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) — metoda jest skonfigurowany do używania równoległości i Wyłącz sprawdzanie poprawności wyznaczania wartości skrótu MD5. Pliki są przekazywane w blokach 100 mb, ta konfiguracja zapewnia lepszą wydajność, ale może być kosztowne, jeśli za pomocą nieprawidłowo wykonywania sieci tak, jakby cały blok 100 mb awarii próba zostanie ponowiona.

|Właściwość|Wartość|Opis|
|---|---|---|
|[ParallelOperationThreadCount](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.paralleloperationthreadcount?view=azure-dotnet)| 8| Ustawienie dzieli obiektu blob na bloki podczas przekazywania. Na najwyższym poziomie wydajności ta wartość powinna być 8 x liczba rdzeni. |
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| prawda| Ta właściwość wyłącza sprawdzanie Skrót MD5 zawartość przesłana. Wyłączenie sprawdzania poprawności MD5 zapewnia szybsze transferu. Ale nie ma potwierdzenia ważności lub integralność przesyłania plików.   |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| fałsz| Ta właściwość określa, czy skrót MD5 jest obliczany i przechowywane w pliku.   |
| [RetryPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.retrypolicy?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_RetryPolicy)| 2 sekundy wycofywania wskutek ponowienia max 10 |Określa zasady ponawiania żądań. Liczba błędów połączenia są zwalniane, w tym przykładzie [ExponentialRetry](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.exponentialretry?view=azure-dotnet) zasad jest skonfigurowane przy użyciu wycofywania 2 sekundy i maksymalnej liczby ponownych prób 10. To ustawienie jest ważne, gdy aplikacja pobiera bliski naciśnięcie [wartości docelowe skalowalności magazynu obiektu blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).  |

`UploadFilesAsync` Zadań przedstawiono w poniższym przykładzie:

```csharp
private static async Task UploadFilesAsync()
{
    // Create random 5 characters containers to upload files to.
    CloudBlobContainer[] containers = await GetRandomContainersAsync();
    var currentdir = System.IO.Directory.GetCurrentDirectory();

    // path to the directory to upload
    string uploadPath = currentdir + "\\upload";
    Stopwatch time = Stopwatch.StartNew();
    try
    {
        Console.WriteLine("Iterating in directory: {0}", uploadPath);
        int count = 0;
        int max_outstanding = 100;
        int completed_count = 0;

        // Define the BlobRequestionOptions on the upload.
        // This includes defining an exponential retry policy to ensure that failed connections are retried with a backoff policy. As multiple large files are being uploaded
        // large block sizes this can cause an issue if an exponential retry policy is not defined.  Additionally parallel operations are enabled with a thread count of 8
        // This could be should be multiple of the number of cores that the machine has. Lastly MD5 hash validation is disabled for this example, this improves the upload speed.
        BlobRequestOptions options = new BlobRequestOptions
        {
            ParallelOperationThreadCount = 8,
            DisableContentMD5Validation = true,
            StoreBlobContentMD5 = false
        };
        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        List<Task> tasks = new List<Task>();
        Console.WriteLine("Found {0} file(s)", Directory.GetFiles(uploadPath).Count());

        // Iterate through the files
        foreach (string path in Directory.GetFiles(uploadPath))
        {
            // Create random file names and set the block size that is used for the upload.
            var container = containers[count % 5];
            string fileName = Path.GetFileName(path);
            Console.WriteLine("Uploading {0} to container {1}.", path, container.Name);
            CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

            // Set block size to 100MB.
            blockBlob.StreamWriteSizeInBytes = 100 * 1024 * 1024;
            await sem.WaitAsync();

            // Create tasks for each file that is uploaded. This is added to a collection that executes them all asyncronously.  
            tasks.Add(blockBlob.UploadFromFileAsync(path, null, options, null).ContinueWith((t) =>
            {
                sem.Release();
                Interlocked.Increment(ref completed_count);
            }));
            count++;
        }

        // Creates an asynchronous task that completes when all the uploads complete.
        await Task.WhenAll(tasks);

        time.Stop();

        Console.WriteLine("Upload has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());

        Console.ReadLine();
    }
    catch (DirectoryNotFoundException ex)
    {
        Console.WriteLine("Error parsing files in the directory: {0}", ex.Message);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

Poniższy przykład jest wyjściem skróconą aplikacji działający w systemie Windows.

```
Created container https://mystorageaccount.blob.core.windows.net/9efa7ecb-2b24-49ff-8e5b-1d25e5481076
Created container https://mystorageaccount.blob.core.windows.net/bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b
Created container https://mystorageaccount.blob.core.windows.net/9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f
Created container https://mystorageaccount.blob.core.windows.net/47646f1a-c498-40cd-9dae-840f46072180
Created container https://mystorageaccount.blob.core.windows.net/38b2cdab-45fa-4cf9-94e7-d533837365aa
Iterating in directory: D:\git\storage-dotnet-perf-scale-app\upload
Found 50 file(s)
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\1d596d16-f6de-4c4c-8058-50ebd8141e4d.txt to container 9efa7ecb-2b24-49ff-8e5b-1d25e5481076.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\242ff392-78be-41fb-b9d4-aee8152a6279.txt to container bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\38d4d7e2-acb4-4efc-ba39-f9611d0d55ef.txt to container 9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\45930d63-b0d0-425f-a766-cda27ff00d32.txt to container 47646f1a-c498-40cd-9dae-840f46072180.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\5129b385-5781-43be-8bac-e2fbb7d2bd82.txt to container 38b2cdab-45fa-4cf9-94e7-d533837365aa.
...
Upload has been completed in 142.0429536 seconds. Press any key to continue
```

### <a name="validate-the-connections"></a>Sprawdzanie poprawności połączenia

Podczas przekazywania plików, można sprawdzić liczbę równoczesnych połączeń, do konta magazynu. Otwórz **wiersza polecenia** i typu `netstat -a | find /c "blob:https"`. To polecenie przedstawia liczbę połączeń, które są obecnie otwarte przy użyciu `netstat`. Poniższy przykład przedstawia podobne dane wyjściowe, aby zobaczyć podczas uruchamiania samouczka samodzielnie. Jak widać w przykładzie 800 połączenia były otwarte podczas przekazywania plików losowe konta magazynu. Ta wartość zmienia się w całym systemem przekazywania. Przekazując w bloku równoległego fragmentów, ilość czasu wymagane do transferu zawartości jest znacznie ograniczone.

```
C:\>netstat -a | find /c "blob:https"
800

C:\>
```

## <a name="next-steps"></a>Kolejne kroki

W części dwóch serii poznanie przesyłanie dużych ilości danych losowych na konto magazynu równolegle, np.:

> [!div class="checklist"]
> * Konfigurowanie parametrów połączenia
> * Kompilowanie aplikacji
> * Uruchamianie aplikacji
> * Sprawdzanie poprawności liczby połączeń

Przejście do trzech częścią serii, aby pobrać dużych ilości danych z konta magazynu.

> [!div class="nextstepaction"]
> [Przekaż dużych ilości dużych plików równolegle na konto magazynu](storage-blob-scalable-app-download-files.md)

[previous-tutorial]: storage-blob-scalable-app-create-vm.md
