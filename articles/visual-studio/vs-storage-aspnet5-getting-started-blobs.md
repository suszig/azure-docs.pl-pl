---
title: "Rozpoczynanie pracy z obiektu blob magazynu i Visual Studio połączone usługi (platformy ASP.NET Core) | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę przy użyciu magazynu obiektów Blob platformy Azure w projekcie programu Visual Studio platformy ASP.NET Core, po utworzeniu konta magazynu przy użyciu programu Visual Studio połączone usługi"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2e8060b44c395ad7c24e7778c0ef65148a3a45de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Wprowadzenie do obiektów Blob platformy Azure magazynu i Visual Studio połączone usługi (platformy ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie
W tym artykule opisano, jak rozpocząć pracę przy użyciu magazynu obiektów Blob platformy Azure w programie Visual Studio po utworzony lub odwołanie do konta magazynu Azure w projekcie platformy ASP.NET Core za pomocą okna dialogowego programu Visual Studio Dodaj połączenia usługi.

Magazyn obiektów Blob Azure to usługa do przechowywania dużych ilości danych bez struktury, którego mogą uzyskać dostęp z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. Pojedynczego obiektu blob może być dowolnym rozmiarze. Obiekty BLOB można np. obrazów, plików audio i wideo, nieprzetworzone dane i pliki dokumentów. W tym artykule opisano, jak rozpocząć pracę z magazynu obiektów blob, po utworzeniu konta magazynu platformy Azure przy użyciu programu Visual Studio **dodać usług połączonych** okna dialogowego w projekcie platformy ASP.NET Core.

Tak samo, jak żywe plików w folderach, na żywo magazynu obiektów blob w kontenerach. Po utworzeniu magazynu należy utworzyć kontenerach w magazynie. Na przykład magazynu o nazwie "Pamiętnik", można tworzyć kontenery w magazynie o nazwie "obrazy" do przechowywania obrazów i innej o nazwie "audio" do przechowywania plików audio. Po utworzeniu kontenerów, możesz przekazać pliki poszczególnych obiektów blob do nich. Zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure przy użyciu platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) uzyskać więcej informacji o programowo manipulowanie obiektów blob.

## <a name="access-blob-containers-in-code"></a>Dostęp do kontenerów obiektów blob w kodzie
Do uzyskania programowego dostępu do obiektów blob w projektów platformy ASP.NET Core, należy dodać następujące elementy, jeśli nie są one już istnieje.

1. Dodaj następujące deklaracje przestrzeni nazw kod na początku dowolnego pliku C# ma do uzyskania programowego dostępu do magazynu Azure.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Pobierz **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Poniższy kod umożliwia pobieranie parametrów połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi Azure.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Uwaga:** korzystać ze wszystkich powyższych kodu przed kod w następujących sekcjach.
3. Użyj **CloudBlobClient** obiekt, aby pobrać **CloudBlobContainer** odwołania do istniejącego kontenera na koncie magazynu.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Tworzenie kontenera w kodzie
Można również użyć **CloudBlobClient** do utworzenia kontenera na koncie magazynu. Wszystko co należy zrobić to dodanie wywołanie **CreateIfNotExistsAsync** zgodnie z poniższym kodem:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Uwaga:** interfejsów API służących do wykonywania wywołań do magazynu Azure w ASP.NET Core są asynchroniczne. Zobacz [programowanie asynchroniczne z Async i Await](http://msdn.microsoft.com/library/hh191443.aspx) Aby uzyskać więcej informacji. Poniższy kod przyjęto założenie, że są używane metody programowania asynchronicznego.

Aby udostępnić pliki w kontenerze dla wszystkich użytkowników, można ustawić kontener jako publiczny przy użyciu następującego kodu.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektu blob do kontenera
Aby przekazać plik obiektu blob do kontenera, Pobierz odwołanie do kontenera i użyj go, aby pobrać odwołanie do obiektu blob. Po utworzeniu odwołanie do obiektu blob możesz przekazać dowolny strumień danych do niej przez wywołanie metody **UploadFromStreamAsync** metody. Ta operacja tworzy obiektu blob, jeśli nie jest jeszcze określony lub zastąpiony, jeśli istnieje. W poniższym przykładzie przedstawiono, jak przekazać obiekt blob do kontenera, zakładając, że kontener został już utworzony.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze
Aby wyświetlić listę obiektów blob w kontenerze, należy najpierw uzyskać odwołanie do kontenera. Następnie można wywołać kontenera **ListBlobsSegmentedAsync** metodę, aby pobrać obiekty BLOB i/lub zawarte w nim katalogi. Aby uzyskać dostęp do bogatego zestawu właściwości i metod zwróconego obiektu **IListBlobItem**, należy rzutować go na obiekt **CloudBlockBlob**, **CloudPageBlob** lub **CloudBlobDirectory**. Jeśli nie znasz typu obiektu blob służy sprawdzanie typu umożliwia określenie, które rzutować obiekt. Poniższy kod przedstawia sposób pobierania i zwracania identyfikatora URI poszczególnych elementów w kontenerze.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Istnieją inne sposoby na wyświetlanie zawartości kontenera obiektów blob. Zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure przy użyciu platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) Aby uzyskać więcej informacji.

## <a name="download-a-blob"></a>Pobieranie obiektu blob
Aby pobrać obiektu blob, należy najpierw pobrać odwołanie do obiektu blob, a następnie wywołać **DownloadToStreamAsync** metody. W poniższym przykładzie użyto **DownloadToStreamAsync** metodę, aby przesłać zawartość obiektu blob do obiektu strumienia, który można zapisać jako plik lokalny.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Istnieją inne sposoby zapisywania obiektów blob jako plików. Zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure przy użyciu platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) Aby uzyskać więcej informacji.

## <a name="delete-a-blob"></a>Usuwanie obiektu blob
Aby usunąć obiekt blob, należy najpierw pobrać odwołanie do obiektu blob, a następnie wywołać **DeleteAsync** dla niego metodę.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

