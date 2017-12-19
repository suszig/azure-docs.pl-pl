---
title: "Wprowadzenie do magazynu obiektów blob platformy Azure i programu Visual Studio połączone usługi (platformy ASP.NET Core) | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę przy użyciu magazynu obiektów blob platformy Azure w projekcie platformy ASP.NET Core w programie Visual Studio po połączeniu z kontem magazynu przy użyciu programu Visual Studio usług połączonych"
services: storage
documentationcenter: 
author: camsoper
manager: wpickett
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: casoper
ms.openlocfilehash: 889afa471eeb556662cddf8eb383a905f08b1f01
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Wprowadzenie do magazynu obiektów blob platformy Azure i programu Visual Studio połączone usługi (platformy ASP.NET Core)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Magazyn obiektów blob platformy Azure to usługa, która przechowywania danych niestrukturalnych w chmurze w postaci obiektów blob. Magazyn obiektów blob umożliwia przechowywanie dowolnego typu danych tekstowych lub binarnych, takich jak dokumenty, pliki multimedialne lub instalatory aplikacji. Magazyn obiektów blob jest również nazywany magazynem obiektów.

Ten samouczek pokazuje, jak napisać kod platformy ASP.NET Core dla niektórych typowych scenariuszy przy użyciu magazynu obiektów blob platformy Azure. Scenariusze obejmują tworzenie kontenera obiektów blob i przekazywanie, wyświetlanie, pobieranie i usuwanie obiektów blob.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

## <a name="set-up-the-development-environment"></a>Konfigurowanie środowiska deweloperskiego

Ta sekcja przeprowadzi Cię przez Konfigurowanie środowiska deweloperskiego, w tym tworzenie aplikacji ASP.NET MVC, dodawanie połączenia usług połączonych dodawania kontrolera i określanie dyrektywy wymaganej przestrzeni nazw.

### <a name="create-an-aspnet-mvc-app-project"></a>Utwórz projekt aplikacji platformy ASP.NET MVC

1. Otwórz program Visual Studio.

1. Wybierz **Plik -> Nowy -> Projekt** z poziomu menu głównego

1. Na **nowy projekt** okna dialogowego i określ opcje jako wyróżnione na poniższej ilustracji:

    ![Tworzenie projektu platformy ASP.NET Core](./media/vs-storage-aspnet-core-getting-started-blobs/new-project.png)

1. Kliknij przycisk **OK**.

1. Na **nowy projekt ASP.NET** okna dialogowego i określ opcje jako wyróżnione na poniższej ilustracji:

    ![Określ MVC](./media/vs-storage-aspnet-core-getting-started-blobs/new-mvc.png)

1. Kliknij przycisk **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Połącz się z kontem magazynu platformy Azure za pomocą połączenia usług

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz z menu kontekstowego **podłączonej usługi -> Dodaj**.

1. Na **dodać podłączonej usługi** okno dialogowe, wybierz opcję **magazynu w chmurze z usługą Azure Storage**, a następnie wybierz **Konfiguruj**.

    ![Podłączonej usługi okna dialogowego](./media/vs-storage-aspnet-core-getting-started-blobs/connected-services.png)

1. Na **usługi Azure Storage** okno dialogowe, wybierz konto magazynu Azure do użycia na potrzeby tego samouczka.  Aby utworzyć nowe konto magazynu Azure, kliknij przycisk **Utwórz nowe konto magazynu** i wypełnij formularz.  Po wybraniu istniejącego konta magazynu lub tworzenia nowej, kliknij przycisk **Dodaj**.  Visual Studio zostanie zainstalowany pakiet NuGet dla usługi Azure Storage i parametry połączenia magazynu **appsettings.json**.

> [!TIP]
> Aby dowiedzieć się, jak utworzyć konto magazynu z [portalu Azure](https://portal.azure.com), zobacz [Utwórz konto magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Konto magazynu platformy Azure można również utworzyć za pomocą [programu Azure PowerShell](../storage/common/storage-powershell-guide-full.md), [interfejsu wiersza polecenia Azure](../storage/common/storage-azure-cli.md), lub [powłoki chmury Azure](../cloud-shell/overview.md).


### <a name="create-an-mvc-controller"></a>Tworzenie kontrolera MVC 

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów**i z menu kontekstowego wybierz **kontrolera -> Dodaj**.

    ![Dodawanie kontrolera do aplikacji platformy ASP.NET Core MVC](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-menu.png)

1. Na **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC — pusty**i wybierz **Dodaj**.

    ![Określ typ kontrolera MVC](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller.png)

1. Na **Dodaj kontroler** okna dialogowego, nazwy kontrolera *BlobsController*i wybierz **Dodaj**.

    ![Nazwa kontrolera MVC](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-name.png)

1. Dodaj następujące *przy użyciu* dyrektywy `BlobsController.cs` pliku:

    ```csharp
    using System.IO;
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Łączenie z kontem magazynu i Pobierz odwołanie do kontenera

Kontener obiektów blob jest hierarchią zagnieżdżone obiekty BLOB i folderów.  Pozostałe kroki w tym dokumencie wymagają odwołanie do kontenera obiektów blob, dzięki czemu kod powinien być umieszczony własnej metody do ponownego użycia.

Poniższe kroki Tworzenie metody, aby nawiązać połączenie z kontem magazynu za pomocą parametrów połączenia w **appsettings.json** i utworzyć odwołanie do kontenera.  Ustawienie parametrów połączenia w **appsettings.json** będzie miała nazwę w formacie `<storageaccountname>_AzureStorageConnectionString`. 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie **GetCloudBlobContainer** zwracającą **CloudBlobContainer**.  Pamiętaj zastąpić `<storageaccountname>_AzureStorageConnectionString` rzeczywistą nazwą klucza w **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json");
        IConfigurationRoot Configuration = builder.Build();
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.Parse(Configuration["ConnectionStrings:aspnettutorial_AzureStorageConnectionString"]);
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Mimo że *testu blobcontainer* nie istnieje jeszcze, ten kod tworzy odwołanie do niej, więc kontenera mogą być tworzone za pomocą `CreateIfNotExists` metod przedstawionych w następnym kroku.

## <a name="create-a-blob-container"></a>Tworzenie kontenera obiektów blob

Poniższe kroki przedstawiają sposób tworzenia kontenera obiektów blob:

1. Dodaj metodę o nazwie `CreateBlobContainer` zwracającą `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Pobierz `CloudBlobContainer` obiekt, który reprezentuje odwołanie do nazwy kontenera żądanego obiektu blob. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Wywołanie `CloudBlobContainer.CreateIfNotExists` metody, aby utworzyć kontener, jeśli jeszcze nie istnieje. `CloudBlobContainer.CreateIfNotExists` Metoda zwraca **true** Jeśli kontener nie istnieje i został utworzony pomyślnie. W przeciwnym razie **false** jest zwracany.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExistsAsync().Result;
    ```

1. Aktualizacja `ViewBag` o nazwie kontenera obiektów blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Poniżej pokazano ukończonej `CreateBlobContainer` metody:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExistsAsync().Result;
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **widoków** folderu, a z menu kontekstowego wybierz **Dodaj -> Nowy Folder**. Nazwa nowego folderu *obiekty BLOB*. 

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

1. Wyszukaj nieuporządkowaną listę, która wygląda następująco: `<ul class="nav navbar-nav">`.  Po wykonaniu ostatniego `<li>` element na liście, Dodaj poniższy kod HTML, aby dodać inny element menu nawigacji:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="CreateBlobContainer">Create blob container</a></li>
    ```

1. Uruchom aplikację i wybierz **Tworzenie kontenera obiektów Blob** aby zobaczyć wyniki, podobnie jak na poniższym zrzucie ekranu:
  
    ![Tworzenie kontenera obiektów blob](./media/vs-storage-aspnet-core-getting-started-blobs/create-blob-container-results.png)

    Jak wspomniano wcześniej, `CloudBlobContainer.CreateIfNotExists` metoda zwraca **true** tylko, gdy kontener nie istnieje i zostanie utworzony. W związku z tym, jeśli aplikacja jest uruchamiana, gdy istnieje w kontenerze, metoda zwraca **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Przekazywanie obiektu blob do kontenera obiektów blob

Raz [kontener obiektów blob jest tworzony](#create-a-blob-container), przekazać pliki do tego kontenera. Ta sekcja przeprowadzi Cię przez przekazanie pliku lokalnego do kontenera obiektów blob. W krokach założono Brak kontenera obiektów blob o nazwie *blobcontainer testu*. 

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie `UploadBlob` która zwraca wartość typu ciąg.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. W ramach `UploadBlob` metody get `CloudBlobContainer` obiekt, który reprezentuje odwołanie do nazwy kontenera żądanego obiektu blob. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Jak opisano wcześniej, usługa Azure storage obsługuje typy inny obiektu blob. W tym samouczku korzysta z blokowych obiektów blob.  Aby pobrać odwołanie do blokowego obiektu blob, należy wywołać `CloudBlobContainer.GetBlockBlobReference` metody.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Nazwa obiektu blob jest częścią adres URL używany do pobierania obiektu blob i może być dowolnym ciągiem, łącznie z nazwą pliku.

1. Gdy istnieje odwołanie do obiektu blob, Przekaż dowolny strumień danych do niego przez wywołanie obiektu blob odwołania `UploadFromStream` metody. `UploadFromStream` Metoda tworzy obiekt blob, jeśli nie istnieje lub zastąpiony, jeśli istnieje. (Zmień  *&lt;przekazywania pliku >* do w pełni kwalifikowana ścieżka do pliku do przekazania.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStreamAsync(fileStream).Wait();
    }
    ```
    
    Poniżej pokazano ukończonej `UploadBlob` — metoda (z w pełni kwalifikowana ścieżka do pliku do przekazania):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego `<li>` element na liście, Dodaj poniższy kod HTML, aby dodać inny element menu nawigacji:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="UploadBlob">Upload blob</a></li>
    ```

1. Uruchom aplikację i wybierz **przekazywanie obiektu blob**.  Wyraz "Sukces" powinien być wyświetlany.
    
    ![Powodzenie weryfikacji](./media/vs-storage-aspnet-core-getting-started-blobs/upload-blob.png)
  
Sekcja - [listę obiektów blob w kontenerze obiektów blob](#list-the-blobs-in-a-blob-container) -ilustruje sposób wyświetlić listę obiektów blob w kontenerze obiektów blob.    

## <a name="list-the-blobs-in-a-blob-container"></a>Wyświetlanie obiektów blob w kontenerze obiektów blob

W tej części przedstawiono sposób wyświetlania obiektów blob w kontenerze obiektów blob. Odwołania do kodu przykładowej *testu blobcontainer* utworzony w sekcji [Tworzenie kontenera obiektów blob](#create-a-blob-container).

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie `ListBlobs` zwracającą `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. W ramach `ListBlobs` metody get `CloudBlobContainer` obiekt, który reprezentuje odwołanie do kontenera obiektów blob. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Aby wyświetlić listę obiektów blob w kontenerze obiektów blob, użyj `CloudBlobContainer.ListBlobsSegmentedAsync` metody. `CloudBlobContainer.ListBlobsSegmentedAsync` Metoda zwraca `BlobResultSegment` zawierający `IListBlobItem` obiektów, które mogą być rzutowane na `CloudBlockBlob`, `CloudPageBlob`, lub `CloudBlobDirectory` obiektów. Poniższy fragment kodu wylicza wszystkie obiekty BLOB w kontenerze obiektów blob. Każdy obiekt blob jest rzutowane na odpowiedni obiekt na podstawie typu i jego nazwę (lub identyfikator URI w odniesieniu `CloudBlobDirectory`) zostanie dodany do listy.

    ```csharp
    List<string> blobs = new List<string>();
    BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
    foreach (IListBlobItem item in resultSegment.Results)
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
    Poniżej pokazano ukończonej `ListBlobs` metody:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
        foreach (IListBlobItem item in resultSegment.Results)
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
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folderu, kliknij prawym przyciskiem myszy **obiekty BLOB**, a z menu kontekstowego wybierz **Dodaj -> Widok**.

1. Na **Dodaj widok** okna dialogowego, wprowadź `ListBlobs` dla nazwy widoku i wybierz **Dodaj**.

1. Otwórz `ListBlobs.cshtml`i Zastąp zawartość następującym kodem:

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

1. Po wykonaniu ostatniego `<li>` element na liście, Dodaj poniższy kod HTML, aby dodać inny element menu nawigacji:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="ListBlobs">List blobs</a></li>
    ```

1. Uruchom aplikację i wybierz **wyświetlanie obiektów blob** aby zobaczyć wyniki, podobnie jak na poniższym zrzucie ekranu:
  
    ![Lista obiektów blob](./media/vs-storage-aspnet-core-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Pobieranie obiektów blob

W tej części przedstawiono sposób pobierania obiektu blob i albo zachować w lokalnej pamięci masowej lub odczytu zawartości na ciąg. Odwołania do kodu przykładowej *testu blobcontainer* utworzony w sekcji [Tworzenie kontenera obiektów blob](#create-a-blob-container).

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie `DownloadBlob` która zwraca wartość typu ciąg.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. W ramach `DownloadBlob` metody get `CloudBlobContainer` obiekt, który reprezentuje odwołanie do kontenera obiektów blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Pobierz obiektu blob odwołania przez wywołanie metody `CloudBlobContainer.GetBlockBlobReference` metody. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Aby pobrać obiektu blob, użyj `CloudBlockBlob.DownloadToStream` metody. Poniższy kod przesyła zawartość obiektu blob do obiektu strumienia, który jest następnie utrwalone w lokalnym pliku (zmiana  *&lt;nazwy pliku lokalnego >* reprezentującą pliku w pełni kwalifikowaną nazwę, gdzie ma zostać pobrane obiektu blob.): 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStreamAsync(fileStream).Wait();
    }
    ```
    
    Poniżej pokazano ukończonej `ListBlobs` — metoda (z pełną ścieżkę do pliku lokalnego tworzona):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego `<li>` element na liście, Dodaj poniższy kod HTML, aby dodać inny element menu nawigacji:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DownloadBlob">Download blob</a></li>
    ```

1. Uruchom aplikację i wybierz **pobieranie obiektu blob** można pobrać obiektu blob. Określony w obiekcie blob `CloudBlobContainer.GetBlockBlobReference` wywołanie metody pobiera do lokalizacji określonej w `File.OpenWrite` wywołania metody.  Tekst "Sukces" powinien być wyświetlany w przeglądarce. 

## <a name="delete-blobs"></a>Usuwanie obiektów blob

Następujące kroki ilustrują sposób usuwania obiektu blob:

1. Otwórz plik `BlobsController.cs`.

1. Dodaj metodę o nazwie `DeleteBlob` która zwraca wartość typu ciąg.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. W ramach `DeleteBlob` metody get `CloudBlobContainer` obiekt, który reprezentuje odwołanie do kontenera obiektów blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Pobierz obiektu blob odwołania przez wywołanie metody `CloudBlobContainer.GetBlockBlobReference` metody. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Aby usunąć obiekt blob, użyj `Delete` metody.

    ```csharp
    blob.DeleteAsync().Wait();
    ```
    
    Wypełniony `DeleteBlob` metoda powinna wyglądać następująco:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.DeleteAsync().Wait();
        return "success!";
    }
    ```

1. W **Eksploratora rozwiązań**, rozwiń węzeł **Shared -> widoki** folder, a następnie otwórz `_Layout.cshtml`.

1. Po wykonaniu ostatniego `<li>` element na liście, Dodaj poniższy kod HTML, aby dodać inny element menu nawigacji:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DeleteBlob">Delete blob</a></li>
    ```

1. Uruchom aplikację i wybierz **usuwania obiektów blob** można usunąć obiektu blob, określona w `CloudBlobContainer.GetBlockBlobReference` wywołania metody.  Tekst "Sukces" powinna zostać wyświetlona w przeglądarce.  Kliknij przycisk przeglądarki **ponownie** przycisk, a następnie wybierz **wyświetlanie obiektów blob** można zweryfikować obiektu blob nie jest już w kontenerze.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób przechowywania listy i pobrać obiekty BLOB w usłudze Azure Storage za pomocą platformy ASP.NET Core.  Wyświetl więcej poradników dotyczących funkcji, aby dowiedzieć się więcej o dodatkowych opcjach przechowywania danych na platformie Azure.

  * [Rozpoczynanie pracy z magazynu tabel platformy Azure i programu Visual Studio połączone usługi (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Rozpoczynanie pracy z magazynem kolejek Azure i programu Visual Studio połączone usługi (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
