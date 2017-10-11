---
title: "Zarządzaj wygasaniem obiektów blob magazynu Azure w usłudze Azure CDN | Dokumentacja firmy Microsoft"
description: "Informacje na temat opcji kontroli time-to-live dla obiektów blob w pamięci podręcznej Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Zarządzaj wygasaniem obiektów blob magazynu Azure w usłudze Azure CDN
> [!div class="op_single_selector"]
> * [Usługi aplikacji/w chmurze Azure sieci Web, ASP.NET lub usług IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Usługa blob magazynu Azure](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Usługa blob](../storage/common/storage-introduction.md#blob-storage) w [usługi Azure Storage](../storage/common/storage-introduction.md) jest jedną z wielu źródeł opartych na platformie Azure zintegrowanych z usługą Azure CDN.  Zawartość obiektu blob publicznie mogą być buforowane w usłudze Azure CDN, dopóki nie upłynie jego czas wygaśnięcia (TTL).  Czas wygaśnięcia jest określany przez [ *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) w odpowiedzi HTTP z usługi Magazyn Azure.

> [!TIP]
> Można ustawić nie TTL dla obiektu blob.  W takim przypadku Azure CDN automatycznie stosuje domyślny czas wygaśnięcia wynosi siedem dni.
> 
> Aby uzyskać więcej informacji na temat działania usługi Azure CDN do Przyspieszanie dostępu do obiektów blob i innych plików, zobacz [Omówienie usługi Azure CDN](cdn-overview.md).
> 
> Więcej szczegółów usługi obiektów blob magazynu Azure, zobacz [pojęcia dotyczące usługi Blob](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

W tym samouczku przedstawiono kilka sposobów, w których czas TTL można ustawić dla obiektu blob w usłudze Azure Storage.  

## <a name="azure-powershell"></a>Azure PowerShell
[Program Azure PowerShell](/powershell/azure/overview) jest jednym ze sposobów najszybsze i najbardziej efektywny do administrowania usługami Azure.  Użyj `Get-AzureStorageBlob` polecenia cmdlet, aby pobrać odwołanie do obiektu blob, a następnie ustaw `.ICloudBlob.Properties.CacheControl` właściwości. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Można również użyć programu PowerShell do [Zarządzanie punktów końcowych i profile CDN](cdn-manage-powershell.md).
> 
> 

## <a name="azure-storage-client-library-for-net"></a>Biblioteka klienta usługi Azure Storage dla programu .NET
Aby ustawić czas wygaśnięcia obiektu blob przy użyciu platformy .NET, należy użyć [biblioteki klienta magazynu Azure dla platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) można ustawić [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) właściwości.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Są dostępne w wielu więcej przykładów kodu .NET [przykłady magazynu obiektów Blob platformy Azure dla platformy .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Inne metody
* [Interfejs wiersza polecenia platformy Azure](../cli-install-nodejs.md)
  
    Jeśli przekazywanie obiektu blob, należy skonfigurować *cacheControl* za pomocą właściwości `-p` przełącznika.  W tym przykładzie czas wygaśnięcia na godzinę (3600 sekund).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [Interfejs API REST usług Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Jawnie ustawiona *x-ms-blob-cache-control* właściwość [umieszczanie obiektu Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put zablokowanych](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), lub [Ustaw właściwości obiektów Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) żądania.
* Narzędzia do zarządzania magazynem innych firm
  
    Niektóre narzędzia do zarządzania magazynem Azure innych firm umożliwiają skonfigurowanie *CacheControl* właściwości obiektów blob. 

## <a name="testing-the-cache-control-header"></a>Testowanie *Cache-Control* nagłówka
Możesz łatwo sprawdzić czasu istnienia obiektów blob.  Za pomocą przeglądarki [narzędzia dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, który jest łącznie z obiektu blob *Cache-Control* nagłówka odpowiedzi.  Można także użyć narzędzia, takiego jak **wget**, [Postman](https://www.getpostman.com/), lub [Fiddler](http://www.telerik.com/fiddler) Aby sprawdzić nagłówki odpowiedzi.

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj informacje o *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Dowiedz się, jak zarządzać wygasaniem zawartości usługi w chmurze w usłudze Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

