---
title: "Przekazywanie obrazu danych w chmurze za pomocą usługi Azure Storage | Dokumentacja firmy Microsoft"
description: "Użyj magazynu obiektów blob platformy Azure z aplikacji sieci web do przechowywania danych aplikacji"
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
ms.openlocfilehash: a204498016ff837c5247009eaaffbd4f79285d0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Przekazywanie obrazu danych w chmurze za pomocą usługi Azure Storage

W tym samouczku wchodzi w jednej serii. Ten samouczek przedstawia sposób wdrażania aplikacji sieci web, która używa biblioteki klienta magazynu Azure do przekazania na konto magazynu obrazów. Po zakończeniu, masz aplikacji sieci web, przechowywania i wyświetlanie obrazów z usługi Azure storage.

![Widok kontener obrazów](media/storage-upload-process-images/figure2.png)

W części jednej serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Utwórz kontener i ustaw uprawnienia
> * Pobierz klucz dostępu
> * Konfigurowanie ustawień aplikacji
> * Wdrażanie aplikacji sieci Web na platformie Azure
> * Współdziałanie z aplikacją sieci web

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów 

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.
 
Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup`.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu
 
Próbka przekazywać obrazy do kontenera obiektów blob na koncie magazynu Azure. Konto magazynu zapewnia unikatową przestrzeń nazw do przechowywania i udostępniania obiektów danych usługi Azure storage. Utwórz konto magazynu w utworzonej grupie zasobów przy użyciu polecenia [az storage account create](/cli/azure/storage/account#create). 

> [!IMPORTANT] 
> W części 2 samouczka użyjesz subskrypcji zdarzeń dla magazynu obiektów blob. Subskrypcja zdarzeń obecnie są obsługiwane tylko w przypadku kont magazynu obiektów Blob w zachodnie centralnej nam i zachodnie stany USA 2. Z powodu tego ograniczenia należy utworzyć konta magazynu obiektów Blob, używanego przez przykładowej aplikacji do przechowywania obrazów i miniatur.   

W poniższym poleceniu zastąp własne globalnie unikatowej nazwy dla konta magazynu obiektów Blob, w którym można zobaczyć `<blob_storage_account>` symbolu zastępczego.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Utwórz kontenery magazynu obiektów blob
 
Aplikacja używa dwóch kontenery w ramach konta magazynu obiektów Blob. Kontenery są podobne do folderów i są używane do przechowywania obiektów blob. _Obrazów_ kontenera jest, gdzie aplikacja przekazuje obrazy pełnej rozdzielczości. W późniejszym częścią serii, aplikację Azure funkcja przekazuje po zmianie rozmiaru obrazów miniatur _Kciuki_ kontenera. 

Pobierz klucz konta magazynu przy użyciu [listy kluczy konta magazynu az](/cli/azure/storage/account/keys#list) polecenia. Następnie użyj tego klucza do utworzenia dwóch kontenerów przy użyciu [utworzyć kontenera magazynu az](/cli/azure/storage/container#create) polecenia.  
 
W takim przypadku `<blob_storage_account>` jest nazwą utworzonego konta magazynu obiektów Blob. _Obrazów_ ustawiono dostępu publicznego kontenery `off`, _Kciuki_ ustawiono dostępu publicznego kontenery `container`. `container` Ustawienia dostępu publicznego pozwalają na miniaturę, aby być widoczny dla osób, które odwiedź stronę sieci web.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbs --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Zanotuj nazwę konta magazynu obiektów blob i klucza. Przykładowa aplikacja używa tych ustawień do nawiązania połączenia konta magazynu, aby przekazać obrazów. 

## <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service 

[Plan usługi App Service](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) określa lokalizację, rozmiar i funkcje farmy serwerów sieci Web, która jest hostem aplikacji. 

Utwórz plan usługi App Service za pomocą polecenia [az appservice plan create](/cli/azure/appservice/plan#create). 

W poniższym przykładzie jest tworzony plan usługi App Service o nazwie `myAppServicePlan` przy użyciu warstwy cenowej **Bezpłatna**: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web 

Aplikacja sieci web miejsce na wpisanie hostingu jest wdrażany z repozytorium GitHub próbki kodu przykładowej aplikacji. Utwórz [aplikację sieci Web](../../app-service/app-service-web-overview.md) w `myAppServicePlan`planie usługi App Service za pomocą polecenia [az webapp create](/cli/azure/webapp#create).  
 
W poniższym poleceniu zastąp `<web_app>` o unikatowej nazwie (prawidłowe znaki to `a-z`, `0-9`, i `-`). Jeśli `<web_app>` jest nie jest unikatowy, otrzymasz komunikat o błędzie: _witryny sieci Web o podanej nazwie `<web_app>` już istnieje._ Domyślnym adresem URL aplikacji sieci Web jest `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Wdrażanie przykładowej aplikacji z repozytorium GitHub 

Usługa aplikacji obsługuje kilka metod wdrażania zawartości w aplikacji sieci web. W tym samouczku, wdrażanie aplikacji sieci web z publicznego repozytorium przykładowej GitHub: [https://github.com/Azure-Samples/storage-blob-upload-from-webapp](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). Konfigurowanie wdrożenia GitHub w aplikacji sieci web przy użyciu [konfiguracji źródła wdrożenia aplikacji sieci Web az](/cli/azure/webapp/deployment/source#config) polecenia. Zastąp `<web_app>` o nazwie aplikacji sieci web został utworzony w poprzednim kroku.

Przykładowy projekt zawiera [ASP.NET MVC](https://www.asp.net/mvc) aplikację, która akceptuje obrazu, zapisuje go na konto magazynu i wyświetla obrazy miniatur kontenera. Aplikacja sieci web używa [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet)i [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) przestrzenie nazw z usługi Azure storage biblioteki klienta do interakcji z usługą Azure storage. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Konfiguruj ustawienia aplikacji sieci web 

Przykładowe zastosowania aplikacji sieci web [biblioteki klienta magazynu Azure](/dotnet/api/overview/azure/storage?view=azure-dotnet) do żądania tokenów dostępu, które są używane do przekazywania obrazów. Używana przez zestaw SDK magazynu poświadczeń konta magazynu są ustawiane w ustawieniach aplikacji dla aplikacji sieci web. Dodawanie ustawienia aplikacji wdrożonej aplikacji przy użyciu [az aplikacji sieci Web config appsettings zestaw](/cli/azure/webapp/config/appsettings#set) polecenia. 

W poniższym poleceniu `<blob_storage_account>` jest nazwą konta magazynu obiektów Blob i `<blob_storage_key>` jest skojarzony klucz. Zastąp `<web_app>` o nazwie aplikacji sieci web został utworzony w poprzednim kroku.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbs \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Po wdrożeniu i skonfigurować aplikacji sieci web można przetestować funkcji przekazywania obrazu w aplikacji.   

## <a name="upload-an-image"></a>Przekazywanie obrazu 

Aby przetestować aplikację sieci web, przejdź do adresu URL opublikowanej aplikacji. Domyślnym adresem URL aplikacji sieci Web jest `https://<web_app>.azurewebsites.net`. Wybierz **przekazać zdjęć** obszaru Wybierz i przekazanie pliku lub przeciągnij i upuść plik na region. Obraz znika, jeśli pomyślnie przekazany.

![ImageResizer aplikacji](media/storage-upload-process-images/figure1.png)

W przykładowym kodzie `UploadFiletoStorage` zadań w `Storagehelper.cs` plik jest używany do przekazywania obrazów do `images` kontenera w przy użyciu konta magazynu [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) metody. Poniższy przykładowy kod zawiera `UploadFiletoStorage` zadań. 

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

Następujące klasy i metody są używane w poprzednich zadań:

|Klasy  |Metoda  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Sprawdź, czy obraz jest wyświetlany w obszarze konta magazynu

Zaloguj się do https://portal.azure.com. Wybierz z menu po lewej stronie **kont magazynu**, następnie wybierz nazwę konta magazynu. W obszarze **omówienie**, wybierz pozycję **obrazów** kontenera.

Sprawdź, czy obraz jest wyświetlany w kontenerze.

![Widok kontener obrazów](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Wyświetlanie miniatur testu

Aby przetestować wyświetlania miniatur, będzie przekazywanie obrazu w kontenerze miniatur, aby upewnić się, że aplikacja może odczytywać miniatur kontenera.

Zaloguj się do https://portal.azure.com. Wybierz z menu po lewej stronie **kont magazynu**, następnie wybierz nazwę konta magazynu. Wybierz **kontenery** w obszarze **usługa Blob** i wybierz **Kciuki** kontenera. Wybierz **przekazać** otworzyć **przekazywanie obiektu blob** okienka.

Wybierz plik za pomocą selektora plików **przekazać**.

Przejdź z powrotem do aplikacji w taki sposób, aby sprawdzić, czy obraz przesłany do **Kciuki** kontenera jest widoczny.

![Widok kontener obrazów](media/storage-upload-process-images/figure2.png)

W **Kciuki** kontenera w portalu Azure, wybierz obraz przekazany i wybierz **usunąć** można usunąć obrazu. W części dwóch serii automatyzacji tworzenia obrazów miniatur, więc ten obraz testu nie jest potrzebny.

CDN można włączyć buforowanie zawartości z kontem magazynu platformy Azure. Podczas nie opisano w tym samouczku, aby dowiedzieć się, jak włączyć CDN z kontem magazynu platformy Azure możesz odwiedzić: [integracji konta magazynu platformy Azure z usługą Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Następne kroki

W części jednej serii wiesz o konfigurowaniu aplikacji sieci web, interakcji z magazynem, np.:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Utwórz kontener i ustaw uprawnienia
> * Pobierz klucz dostępu
> * Konfigurowanie ustawień aplikacji
> * Wdrażanie aplikacji sieci Web na platformie Azure
> * Współdziałanie z aplikacją sieci web

Przejdź do części dwóch serii, aby dowiedzieć się więcej o używaniu siatki zdarzeń do wyzwolenia Azure funkcji zmiany rozmiaru obrazu.

> [!div class="nextstepaction"]
> [Użyj siatki zdarzeń do wyzwolenia funkcji platformy Azure, aby zmienić rozmiar załadowanego obrazu](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
