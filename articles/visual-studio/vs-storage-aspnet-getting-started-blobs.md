---
title: "Wprowadzenie do magazynu obiektów blob platformy Azure i programu Visual Studio połączone usługi (ASP.NET) | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę przy użyciu magazynu obiektów blob platformy Azure w projekcie platformy ASP.NET w programie Visual Studio po połączeniu z kontem magazynu przy użyciu programu Visual Studio usług połączonych"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: e953c7978705379a28581213e8f1c665473ddd60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Wprowadzenie do magazynu obiektów blob platformy Azure i programu Visual Studio połączone usługi (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów blob platformy Azure to usługa, która przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

Ten samouczek pokazuje, jak napisać kod ASP.NET dla niektórych typowych scenariuszy przy użyciu magazynu obiektów blob platformy Azure. Scenariusze obejmują tworzenie kontenera obiektów blob i przekazywanie, wyświetlanie, pobieranie i usuwanie obiektów blob.

##<a name="prerequisites"></a>Wymagania wstępne

* [Program Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Konto usługi Azure Storage](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Tworzenie kontrolera MVC 

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów**i z menu kontekstowego wybierz **kontrolera -> Dodaj**.

    ![Dodawanie kontrolera do aplikacji ASP.NET MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Na **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 — pusty**i wybierz **Dodaj**.

    ![Określ typ kontrolera MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Na **Dodaj kontroler** okna dialogowego, nazwy kontrolera *BlobsController*i wybierz **Dodaj**.

    ![Nazwa kontrolera MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Dodaj następujące *przy użyciu* dyrektywy `BlobsController.cs` pliku:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Tworzenie kontenera obiektów blob

Kontener obiektów blob jest hierarchią zagnieżdżone obiekty BLOB i folderów. Poniższe kroki przedstawiają sposób tworzenia kontenera obiektów blob:

> [!NOTE]
> 
> Kod w tej sekcji założono, że zostały wykonane kroki opisane w sekcji [Konfigurowanie środowiska programowania](#set-up-the-development-environment). 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **CreateBlobContainer** zwracającą **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. W ramach **CreateBlobContainer** metody get **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu, aby pobrać parametry połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi Azure. (Zmień  *&lt;nazwy konta magazynu >* do nazwy konta magazynu Azure uzyskujesz dostęp do.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Pobierz **CloudBlobClient** obiekt reprezentuje klienta usługi blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pobierz **CloudBlobContainer** obiekt, który reprezentuje odwołanie do nazwy kontenera żądanego obiektu blob. **CloudBlobClient.GetContainerReference** — metoda nie powoduje żądań dotyczących magazynu obiektów blob. Zwracane jest odwołanie, lub nie istnieje w kontenerze obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Wywołanie **CloudBlobContainer.CreateIfNotExists** metody, aby utworzyć kontener, jeśli jeszcze nie istnieje. **CloudBlobContainer.CreateIfNotExists** metoda zwraca **true** Jeśli kontener nie istnieje i został utworzony pomyślnie. W przeciwnym razie **false** jest zwracany.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Aktualizacja **obiekt ViewBag** o nazwie kontenera obiektów blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folderu, kliknij prawym przyciskiem myszy **obiekty BLOB**, a z menu kontekstowego wybierz **Dodaj -> Widok**.

1. Na **Dodaj widok** okna dialogowego, wprowadź **CreateBlobContainer** dla nazwy widoku i wybierz **Dodaj**.

1. Otwórz `CreateBlobContainer.cshtml`i zmodyfikuj go, aby wygląda jak poniższy fragment kodu:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego **Html.ActionLink**, Dodaj następujący **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Uruchom aplikację i wybierz **Tworzenie kontenera obiektów Blob** aby zobaczyć wyniki, podobnie jak na poniższym zrzucie ekranu:
  
    ![Tworzenie kontenera obiektów blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Jak wspomniano wcześniej, **CloudBlobContainer.CreateIfNotExists** metoda zwraca **true** tylko, gdy kontener nie istnieje i zostanie utworzony. W związku z tym po uruchomieniu aplikacji, gdy istnieje w kontenerze, metoda zwraca **false**. Do uruchomienia aplikacji wiele razy, można usunąć kontenera przed ponownym uruchomieniem aplikacji. Usunięcie kontenera może odbywać się za pośrednictwem **CloudBlobContainer.Delete** metody. Możesz także usunąć za pomocą kontenera [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) lub [Eksploratora usługi Microsoft Azure Storage](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Przekazywanie obiektu blob do kontenera obiektów blob

Po wprowadzeniu [utworzono kontener obiektów blob](#create-a-blob-container), możesz przekazać pliki do tego kontenera. Ta sekcja przeprowadzi Cię przez przekazanie pliku lokalnego do kontenera obiektów blob. W krokach założono po utworzeniu kontenera obiektów blob o nazwie *blobcontainer testu*. 

> [!NOTE]
> 
> Kod w tej sekcji założono, że zostały wykonane kroki opisane w sekcji [Konfigurowanie środowiska programowania](#set-up-the-development-environment). 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **uploadblob dla platformy** zwracającą **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. W ramach **uploadblob dla platformy** metody get **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi platformy Azure: (zmiany  *&lt;nazwy konta magazynu >* do nazwy konta magazynu Azure uzyskujesz dostęp do.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Pobierz **CloudBlobClient** obiekt reprezentuje klienta usługi blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pobierz **CloudBlobContainer** obiekt, który reprezentuje odwołanie do nazwy kontenera obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Jak opisano wcześniej, usługa Azure storage obsługuje typy inny obiektu blob. Aby pobrać odwołanie do obiektu blob strony, należy wywołać **CloudBlobContainer.GetPageBlobReference** metody. Aby pobrać odwołanie do blokowego obiektu blob, należy wywołać **CloudBlobContainer.GetBlockBlobReference** metody. Zwykle blokowych obiektów blob jest zalecany typ do użycia. (Zmień < nazwa obiektu blob > * na nazwę, którą chcesz nadać raz przekazać obiektu blob.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Po utworzeniu odwołanie do obiektu blob, dowolny strumień danych można przekazać do niej przez wywołanie obiektu blob odwołania **UploadFromStream** metody. **UploadFromStream** metoda tworzy obiekt blob, jeśli nie istnieje lub zastąpiony, jeśli istnieje. (Zmień  *&lt;przekazywania pliku >* do w pełni kwalifikowana ścieżka do pliku, który chcesz przekazać.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folderu, kliknij prawym przyciskiem myszy **obiekty BLOB**, a z menu kontekstowego wybierz **Dodaj -> Widok**.

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego **Html.ActionLink**, Dodaj następujący **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uruchom aplikację i wybierz **przekazywanie obiektu blob**.  
  
Sekcja - [listę obiektów blob w kontenerze obiektów blob](#list-the-blobs-in-a-blob-container) -ilustruje sposób wyświetlić listę obiektów blob w kontenerze obiektów blob.    

## <a name="list-the-blobs-in-a-blob-container"></a>Wyświetlanie obiektów blob w kontenerze obiektów blob

W tej części przedstawiono sposób wyświetlania obiektów blob w kontenerze obiektów blob. Odwołania do kodu przykładowej *testu blobcontainer* utworzony w sekcji [Tworzenie kontenera obiektów blob](#create-a-blob-container).

> [!NOTE]
> 
> Kod w tej sekcji założono, że zostały wykonane kroki opisane w sekcji [Konfigurowanie środowiska programowania](#set-up-the-development-environment). 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **ListBlobs** zwracającą **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. W ramach **ListBlobs** metody get **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi platformy Azure: (zmiany  *&lt;nazwy konta magazynu >* do nazwy konta magazynu Azure uzyskujesz dostęp do.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Pobierz **CloudBlobClient** obiekt reprezentuje klienta usługi blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pobierz **CloudBlobContainer** obiekt, który reprezentuje odwołanie do nazwy kontenera obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Aby wyświetlić listę obiektów blob w kontenerze obiektów blob, użyj **CloudBlobContainer.ListBlobs** metody. **CloudBlobContainer.ListBlobs** metoda zwraca **IListBlobItem** obiekt, który można rzutować na **CloudBlockBlob**, **CloudPageBlob**, lub **CloudBlobDirectory** obiektu. Poniższy fragment kodu wylicza wszystkie obiekty BLOB w kontenerze obiektów blob. Każdy obiekt blob jest rzutowane na odpowiedni obiekt na podstawie typu i jego nazwę (lub identyfikator URI w odniesieniu **CloudBlobDirectory**) zostanie dodany do listy.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Oprócz obiektów blob kontenerów obiektów blob może zawierać katalogów. Załóżmy, że masz kontenera obiektów blob o nazwie *blobcontainer testu* przy użyciu następujących hierarchii:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Użyty w poprzednim przykładzie kodu **obiekty BLOB** ciąg lista zawiera wartości podobnie do następującego:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Jak widać, lista zawiera tylko jednostki najwyższego poziomu; te zagnieżdżonych (*bar.png* i *baz.png*). Aby wyświetlić listę wszystkich jednostek w kontenerze obiektów blob, należy wywołać **CloudBlobContainer.ListBlobs** — metoda i przekazać **true** dla **useFlatBlobListing** parametru.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Ustawienie **useFlatBlobListing** parametr **true** zwraca płaska lista wszystkich jednostek w kontenerze obiektów blob i daje następujące wyniki:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folderu, kliknij prawym przyciskiem myszy **obiekty BLOB**, a z menu kontekstowego wybierz **Dodaj -> Widok**.

1. Na **Dodaj widok** okna dialogowego, wprowadź **ListBlobs** dla nazwy widoku i wybierz **Dodaj**.

1. Otwórz `ListBlobs.cshtml`i zmodyfikuj go, aby wygląda jak poniższy fragment kodu:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego **Html.ActionLink**, Dodaj następujący **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Uruchom aplikację i wybierz **wyświetlanie obiektów blob** aby zobaczyć wyniki, podobnie jak na poniższym zrzucie ekranu:
  
    ![Lista obiektów blob](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Pobieranie obiektów blob

W tej części przedstawiono sposób pobierania obiektu blob i albo zachować w lokalnej pamięci masowej lub odczytu zawartości na ciąg. Odwołania do kodu przykładowej *testu blobcontainer* utworzony w sekcji [Tworzenie kontenera obiektów blob](#create-a-blob-container).

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **DownloadBlob** zwracającą **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. W ramach **DownloadBlob** metody get **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi platformy Azure: (zmiany  *&lt;nazwy konta magazynu >* do nazwy konta magazynu Azure uzyskujesz dostęp do.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Pobierz **CloudBlobClient** obiekt reprezentuje klienta usługi blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pobierz **CloudBlobContainer** obiekt, który reprezentuje odwołanie do nazwy kontenera obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Pobierz przez wywołanie obiektu blob odwołania **CloudBlobContainer.GetBlockBlobReference** lub **CloudBlobContainer.GetPageBlobReference** metody. (Zmień  *&lt;nazwa obiektu blob >* do nazwy obiektu blob pobierasz.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Aby pobrać obiektu blob, użyj **CloudBlockBlob.DownloadToStream** lub **CloudPageBlob.DownloadToStream** metody, w zależności od typu obiektu blob. Poniższy kod używa fragment **CloudBlockBlob.DownloadToStream** metodę, aby przesłać zawartość obiektu blob do obiektu strumienia, który jest następnie utrwalony na plik lokalny: (zmiana  *&lt;nazwy pliku lokalnego >* pliku pełną nazwę reprezentującą miejscu obiektu blob pobranych.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego **Html.ActionLink**, Dodaj następujący **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uruchom aplikację i wybierz **pobieranie obiektu blob** można pobrać obiektu blob. Określony w obiekcie blob **CloudBlobContainer.GetBlockBlobReference** wywołanie metody pobiera do lokalizacji określonej w **File.OpenWrite** wywołania metody. 

## <a name="delete-blobs"></a>Usuwanie obiektów blob

Następujące kroki ilustrują sposób usuwania obiektu blob:

> [!NOTE]
> 
> Kod w tej sekcji założono, że zostały wykonane kroki opisane w sekcji [Konfigurowanie środowiska programowania](#set-up-the-development-environment). 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **DeleteBlob** zwracającą **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Pobierz **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu i informacji o koncie magazynu z konfiguracji usługi platformy Azure: (zmiany  *&lt;nazwy konta magazynu >* do nazwy konta magazynu Azure uzyskujesz dostęp do.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Pobierz **CloudBlobClient** obiekt reprezentuje klienta usługi blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pobierz **CloudBlobContainer** obiekt, który reprezentuje odwołanie do nazwy kontenera obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Pobierz przez wywołanie obiektu blob odwołania **CloudBlobContainer.GetBlockBlobReference** lub **CloudBlobContainer.GetPageBlobReference** metody. (Zmień  *&lt;nazwa obiektu blob >* do nazwy obiektu blob usuwasz.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego **Html.ActionLink**, Dodaj następujący **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uruchom aplikację i wybierz **usuwania obiektów blob** można usunąć obiektu blob, określona w **CloudBlobContainer.GetBlockBlobReference** wywołania metody. 

## <a name="next-steps"></a>Następne kroki
Wyświetl więcej poradników dotyczących funkcji, aby dowiedzieć się więcej o dodatkowych opcjach przechowywania danych na platformie Azure.

  * [Rozpoczynanie pracy z magazynu tabel platformy Azure i programu Visual Studio połączone usługi (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Rozpoczynanie pracy z magazynem kolejek Azure i programu Visual Studio połączone usługi (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
