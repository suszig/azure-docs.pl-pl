---
title: "Zarządzaj wygasaniem magazynu obiektów Blob platformy Azure w usłudze Azure Content Delivery Network | Dokumentacja firmy Microsoft"
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
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: 09d488a7600d85b6feab6a27cdd517240c95104b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Zarządzaj wygasaniem magazynu obiektów Blob platformy Azure w usłudze Azure Content Delivery Network
> [!div class="op_single_selector"]
> * [Zawartość sieci Web platformy Azure](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Obiektu Blob magazynu usługi](../storage/common/storage-introduction.md#blob-storage) w usłudze Azure Storage jest jedną z wielu źródeł opartych na platformie Azure zintegrowanych z Azure Content Delivery Network (CDN). Zawartość obiektu blob publicznie mogą być buforowane w usłudze Azure CDN, dopóki nie upłynie jego czas wygaśnięcia (TTL). Czas wygaśnięcia jest określany przez `Cache-Control` nagłówka odpowiedzi HTTP z serwera pochodzenia. W tym artykule opisano kilka metod, które można ustawić `Cache-Control` nagłówek obiektu blob w usłudze Azure Storage.

> [!TIP]
> Można ustawić nie TTL dla obiektu blob. W takim przypadku Azure CDN automatycznie stosuje domyślny czas wygaśnięcia wynosi siedem dni. To ustawienie domyślne TTL dotyczy tylko optymalizacji ogólne sieci web. Dla optymalizacji dużych plików domyślny czas wygaśnięcia wynosi jeden dzień, a dla multimediów strumieniowych optymalizacji, domyślny czas wygaśnięcia wynosi 1 rok.
> 
> Aby uzyskać więcej informacji na temat działania usługi Azure CDN do Przyspieszanie dostępu do obiektów blob i innych plików, zobacz [Omówienie usługi Azure Content Delivery Network](cdn-overview.md).
> 
> Aby uzyskać więcej informacji na temat magazynu obiektów Blob platformy Azure, zobacz [wprowadzenie do magazynu obiektów Blob](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Ustawienie nagłówki Cache-Control przy użyciu programu Azure PowerShell
[Program Azure PowerShell](/powershell/azure/overview) jest jednym ze sposobów najszybsze i najbardziej zaawansowanych do administrowania usługami Azure. Użyj `Get-AzureStorageBlob` polecenia cmdlet, aby pobrać odwołanie do obiektu blob, a następnie ustaw `.ICloudBlob.Properties.CacheControl` właściwości. 

Na przykład:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Można również użyć programu PowerShell do [Zarządzanie punktów końcowych i profile CDN](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Nagłówki Cache-Control ustawienie przy użyciu programu .NET
Aby ustawić obiektu blob `Cache-Control` nagłówka przy użyciu kodu platformy .NET, użyj [biblioteki klienta magazynu Azure dla platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) można ustawić [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) właściwości.

Na przykład:

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
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Więcej przykładów kodu .NET są dostępne w [przykłady magazynu obiektów Blob platformy Azure dla platformy .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Ustawienie nagłówki Cache-Control przy użyciu innych metod

### <a name="azure-storage-explorer"></a>Eksplorator usługi Azure Storage
Z [Eksploratora usługi Storage Azure](https://azure.microsoft.com/en-us/features/storage-explorer/), można wyświetlać i edytować zasobów magazynu obiektów blob, takie jak właściwości w tym *CacheControl* właściwości. 

Aby zaktualizować *CacheControl* właściwości obiektu blob z Eksploratora usługi Storage platformy Azure:
   1. Wybierz obiekt blob, a następnie wybierz **właściwości** z menu kontekstowego. 
   2. Przewiń w dół do *CacheControl* właściwości.
   3. Wprowadź wartość, a następnie kliknij przycisk **zapisać**.


![Właściwości Eksploratora usługi Storage platformy Azure](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Interfejs wiersza polecenia platformy Azure
Z [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) (CLI), mogą zarządzać zasobami obiektów blob platformy Azure z poziomu wiersza polecenia. Aby skonfigurować nagłówek cache-control, podczas ładowania obiektu blob z wiersza polecenia platformy Azure, ustawić *cacheControl* właściwości przy użyciu `-p` przełącznika. Poniższy przykład pokazuje, jak można ustawić czas wygaśnięcia na godzinę (3600 sekund):
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\test.txt myContainer test.txt
```

### <a name="azure-storage-services-rest-api"></a>Interfejs API REST usług magazynu Azure
Można użyć [interfejsu API REST usług magazynu Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx) jawnie ustaw *x-ms-blob-cache-control* właściwości przy użyciu następujących operacji na żądanie:
  
   - [Umieszczanie obiektu Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Umieść zablokowanych](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Ustaw właściwości obiektów Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Testowanie nagłówek Cache-Control
Można łatwo sprawdzić ustawienia TTL obiektów blob. Przy użyciu przeglądarki [narzędzia dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, który zawiera z obiektu blob `Cache-Control` nagłówka odpowiedzi. Można również użyć narzędzia takie jak [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), lub [Fiddler](http://www.telerik.com/fiddler) Aby sprawdzić nagłówki odpowiedzi.

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się, jak zarządzać wygasaniem zawartości usługi w chmurze w usłudze Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

