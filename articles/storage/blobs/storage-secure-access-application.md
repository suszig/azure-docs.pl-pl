---
title: "Bezpieczny dostęp do danych aplikacji w chmurze za pomocą usługi Azure Storage | Dokumentacja firmy Microsoft"
description: "Użyj tokeny sygnatury dostępu Współdzielonego, szyfrowania i protokołu HTTPS, aby zabezpieczyć dane aplikacji w chmurze"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: c43165e230a00b6a4408637fd2290a21800d07b9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Bezpieczny dostęp do danych aplikacji w chmurze

W tym samouczku jest częścią trzech serii. Jak zabezpieczyć dostęp do konta magazynu. 

W trzech części serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Umożliwia dostęp do obrazów miniatur tokeny sygnatury dostępu Współdzielonego
> * Włącz szyfrowanie po stronie serwera
> * Włącz transportu tylko do protokołu HTTPS

[Magazyn obiektów blob platformy Azure](../common/storage-introduction.md#blob-storage) to niezawodna usługa do przechowywania plików dla aplikacji. W tym samouczku rozszerza [poprzedniego tematu] [ previous-tutorial] do pokazują, jak bezpieczny dostęp do konta magazynu z aplikacji sieci web. Po zakończeniu obrazy są szyfrowane i aplikacji sieci web używa bezpiecznych tokeny sygnatury dostępu Współdzielonego do uzyskiwania dostępu obrazy miniatur.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka należy wykonać poprzednie samouczek magazynu: [zmiana rozmiaru Automatyzacja przekazać obrazów za pomocą zdarzeń siatki][previous-tutorial]. 

## <a name="set-container-public-access"></a>Dostęp do publicznego kontenera zestawu

W tej części samouczka serii tokeny sygnatury dostępu Współdzielonego są używane do uzyskiwania dostępu do miniatury. W tym kroku należy ustawić publiczny dostęp do _Kciuki_ kontener, aby `off`.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Skonfiguruj tokeny sygnatury dostępu Współdzielonego dla miniatur

W części tego samouczka serii aplikacji sieci web został wyświetlanie obrazów z publicznego kontenera. W tej części serii, użyj [podpisu bezpiecznego dostępu (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) tokenów można pobrać obrazów miniatur. Tokeny sygnatury dostępu Współdzielonego umożliwiają dostęp ograniczony kontener obiektów blob na podstawie adresu IP, protokół, interwał czasu lub praw dozwolone.

W tym przykładzie korzysta z repozytorium kodu źródłowego `sasTokens` gałęzi, który został zaktualizowany kod przykładowy. Usuń istniejące wdrożenie GitHub z [usuwania źródła wdrożenia aplikacji sieci Web az](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). Skonfiguruj GitHub wdrożenia w aplikacji sieci web przy użyciu [konfiguracji źródła wdrożenia aplikacji sieci Web az](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) polecenia.  

W poniższym poleceniu `<web-app>` to nazwa aplikacji sieci web.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

`sasTokens` Gałęzi w repozytorium aktualizacji `StorageHelper.cs` pliku. Zastępuje `GetThumbNailUrls` zadań z tego przykładu kodu. Zaktualizowano zadanie pobiera miniatury adresów URL przez ustawienie [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) Aby określić czas rozpoczęcia, czas wygaśnięcia i uprawnienia dla tokenu sygnatury dostępu Współdzielonego. Po wdrożeniu aplikacji sieci web pobiera teraz miniatur przy użyciu adresu URL za pomocą tokenu sygnatury dostępu Współdzielonego. Zaktualizowano zadanie pokazano w poniższym przykładzie:
    
```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create blob client
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the container
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ThumbnailContainer);

    // Set the permission of the container to public
    await container.SetPermissionsAsync(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

    BlobContinuationToken continuationToken = null;

    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);

        foreach (var blobItem in resultSegment.Results)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            //Set the expiry time and permissions for the blob.
            //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
            //The shared access signature will be valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

            sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);

            sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);

            sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

            //Generate the shared access signature on the blob, setting the constraints directly on the signature.
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(blob.Uri + sasBlobToken);

        }

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }

    while (continuationToken != null);

    return await Task.FromResult(thumbnailUrls);
}
```

Poniższe klasy, właściwości i metody są używane w poprzednich zadań:

|Klasa  |Właściwości| Metody  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Wyniki](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Uprawnienia](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Szyfrowanie po stronie serwera

[Szyfrowanie usługi Azure magazynu (SSE)](../common/storage-service-encryption.md) pomaga chronić i ochrony danych. SSE szyfruje dane przechowywane, obsługa szyfrowania, odszyfrowywania i zarządzania kluczami. Wszystkie dane są szyfrowane przy użyciu 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego bloku najwyższy szyfrów dostępne.

W poniższym przykładzie pozwalają włączyć szyfrowanie dla obiektów blob. Istniejące obiekty BLOB utworzone przed włączeniem szyfrowania nie są szyfrowane. `x-ms-server-encrypted` Nagłówka żądania dla obiekt blob Wyświetla stan szyfrowania obiektu blob.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Teraz, gdy jest włączone szyfrowanie, Przekaż nowy obraz aplikacji sieci web.

Przy użyciu `curl` z przełącznikiem `-I` można pobrać tylko nagłówki, Zastąp własne wartości `<storage-account-name>`, `<container>`, i `<blob-name>`.  

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<blob-name>?$sasToken -I
```

W odpowiedzi, należy zwrócić uwagę `x-ms-server-encrypted` pokazuje nagłówka `true`. Ten nagłówek identyfikuje teraz szyfrowanie danych z SSE.

```
HTTP/1.1 200 OK
Content-Length: 209489
Content-Type: image/png
Last-Modified: Mon, 11 Sep 2017 19:27:42 GMT
Accept-Ranges: bytes
ETag: "0x8D4F94B2BE76D45"
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 57047db3-001e-0050-3e34-2ba769000000
x-ms-version: 2017-04-17
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
x-ms-server-encrypted: true
Date: Mon, 11 Sep 2017 19:27:46 GMT
```

## <a name="enable-https-only"></a>Włącz tylko protokół HTTPS

Aby upewnić się, że żądania danych do i z konta magazynu są bezpieczne, można ograniczyć tylko żądania HTTPS. Zaktualizuj protokół wymagane konto magazynu przy użyciu [aktualizacja konta magazynu az](/cli/azure/storage/account#az_storage_account_update) polecenia.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Testowanie połączenia za pomocą `curl` przy użyciu `HTTP` protokołu.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Teraz, wymagana jest zapewnienia bezpiecznego transferu, pojawi się następujący komunikat:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Kolejne kroki

W części trzech serii przedstawiono sposób bezpieczny dostęp do konta magazynu, np.:

> [!div class="checklist"]
> * Umożliwia dostęp do obrazów miniatur tokeny sygnatury dostępu Współdzielonego
> * Włącz szyfrowanie po stronie serwera
> * Włącz transportu tylko do protokołu HTTPS

Przejście do czterech częścią serii, aby dowiedzieć się, jak monitorowanie i rozwiązywanie problemów aplikacji magazynu w chmurze.

> [!div class="nextstepaction"]
> [Monitorowanie i rozwiązywanie problemów z magazynem aplikacji aplikacji w chmurze](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json