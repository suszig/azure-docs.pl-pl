---
title: "Włącz publiczny dostęp do odczytu do kontenerów i obiektów blob w magazynie obiektów Blob platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak udostępnić kontenerów i obiektów blob dla dostępu anonimowego i sposób programowy dostęp."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: tamram
ms.openlocfilehash: f52079c72be298daaa45074e516f911022780392
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Zarządzanie dostępem anonimowym w trybie odczytu do kontenerów i obiektów blob
Można włączyć anonimowego, publiczny dostęp do odczytu do kontenera i jego obiektów blob w magazynie obiektów Blob Azure. W ten sposób można przyznać dostęp tylko do odczytu do tych zasobów, bez udostępniania swój klucz konta usługi i bez konieczności sygnatury dostępu współdzielonego (SAS).

Najlepiej w scenariuszach, w którym ma niektórych obiektów blob zawsze być dostępna dla anonimowy dostęp do odczytu jest publiczny dostęp do odczytu. Aby uzyskać bardziej precyzyjną kontrolę można utworzyć sygnatury dostępu współdzielonego. Sygnatury dostępu współdzielonego umożliwiają stosowanie ograniczony dostęp przy użyciu różnych uprawnień w określonym przedziale czasu. Aby uzyskać więcej informacji o tworzeniu udostępnionych sygnatur dostępu, zobacz [używanie udostępnionych sygnatur dostępu (SAS) w usłudze Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Udzielanie uprawnień użytkownikom anonimowym do kontenerów i obiektów blob
Domyślnie kontener i wszystkie obiekty BLOB w nim mogą być używane tylko przez właściciela konta magazynu. Aby udostępnić użytkownikom anonimowym uprawnienia odczytu do kontenera i jego obiektów blob, można ustawić uprawnień kontenera, aby zezwolić na publiczny dostęp. Użytkownicy anonimowi mogą odczytywać obiekty BLOB w kontenerze publicznie bez uwierzytelniania żądania.

Kontener można skonfigurować następujące uprawnienia:

* **Dostęp do odczytu żadnego elementu publicznego public:** kontenera i jego obiektów blob są dostępne tylko przez właściciela konta magazynu. Jest to wartość domyślna dla wszystkich nowych kontenerów.
* **Dostęp do obiektów blob tylko odczytu publicznego:** obiekty BLOB w kontenerze może zostać odczytany przez żądania od użytkowników anonimowych, ale nie są dostępne dane kontenera. Anonimowe klientów nie można wyliczyć obiektów blob w kontenerze.
* **Pełne publiczny dostęp do odczytu:** wszystkie dane obiektów blob i kontener może zostać odczytany przez żądania od użytkowników anonimowych. Klientów można wyliczyć obiektów blob w kontenerze przez żądania od użytkowników anonimowych, ale nie można wyliczyć kontenery w ramach konta magazynu.

Następujące służy do ustawiania uprawnień kontenera:

* [Witryna Azure Portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Interfejs wiersza polecenia platformy Azure 2.0](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programowo za pomocą jednej z bibliotek klienckich magazynu lub interfejsu API REST

### <a name="set-container-permissions-in-the-azure-portal"></a>Ustaw uprawnienia kontenera w portalu Azure
Aby ustawić uprawnienia do kontenera w [portalu Azure](https://portal.azure.com), wykonaj następujące kroki:

1. Otwórz z **konta magazynu** bloku w portalu. Można znaleźć konta magazynu, wybierając **kont magazynu** w bloku portalu menu głównego.
1. W obszarze **usługa BLOB** w bloku menu wybierz **kontenery**.
1. Kliknij prawym przyciskiem myszy w wierszu kontenera lub wybierz przycisk wielokropka, aby otworzyć kontenera **menu kontekstowe**.
1. Wybierz **zasady dostępu** w menu kontekstowym.
1. Wybierz **dostęp typu** z menu rozwijanego.

    ![Okno dialogowe metadanych kontenera Edycja](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>Ustawianie uprawnień kontenera z platformą .NET
Aby ustawić uprawnienia do kontenera przy użyciu języka C# i biblioteki klienta usługi Storage dla platformy .NET, najpierw pobrać kontenera istniejące uprawnienia, wywołując **GetPermissions** metody. Następnie ustaw **PublicAccess** właściwość **BlobContainerPermissions** obiektu, który jest zwracany przez **GetPermissions** metody. Na koniec wywołania **ustawiania** metody zaktualizowano uprawnienia.

Poniższy przykład ustawia kontenera uprawnienia do pełnej publiczny dostęp do odczytu. Uprawnienia do publiczny dostęp do odczytu obiektów blob jedynie ustawia **PublicAccess** właściwości **BlobContainerPublicAccessType.Blob**. Aby usunąć wszystkie uprawnienia dla użytkowników anonimowych, ustaw dla właściwości **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Dostęp do kontenerów i obiektów blob anonimowo
Klient, który uzyskuje dostęp do kontenerów i obiektów blob anonimowo można użyć konstruktorów, które nie wymagają poświadczeń. W poniższych przykładach pokazano na kilka różnych sposobów odwołanie do obiektu Blob usługi zasobów anonimowo.

### <a name="create-an-anonymous-client-object"></a>Tworzenie obiektu anonimowego klienta
Można utworzyć nowy obiekt klienta usługi dla dostępu anonimowego, podając konto punkt końcowy usługi Blob. Jednak również musi znać nazwę kontenera na tym koncie, który jest dostępny dla dostępu anonimowego.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Odwołanie kontenera anonimowego
Jeśli adres URL do kontenera, który jest dostępny anonimowo, można go bezpośrednio odwoływać kontenera.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Odwołanie obiektu blob anonimowo
Jeśli adres URL obiektu blob, który jest dostępny dla dostępu anonimowego, może odwoływać się bezpośrednio przy użyciu tego adresu URL obiektu blob:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Funkcje dostępne dla użytkowników anonimowych
Poniższej tabeli operacje może zostać wywołana przez użytkowników anonimowych ustawianą ACL kontenera umożliwiających dostęp do publicznego.

| Operacji REST | Uprawnienie o pełnej publiczny dostęp do odczytu | Uprawnienie o publiczny dostęp do odczytu obiektów blob tylko |
| --- | --- | --- |
| Kontenery listy |Tylko właściciel |Tylko właściciel |
| Tworzenie kontenera |Tylko właściciel |Tylko właściciel |
| Pobierz właściwości kontenera |Wszystkie |Tylko właściciel |
| Pobranie metadanych kontenera |Wszystkie |Tylko właściciel |
| Ustaw metadanych kontenera |Tylko właściciel |Tylko właściciel |
| Pobierz ACL kontenera |Tylko właściciel |Tylko właściciel |
| Ustaw ACL kontenera |Tylko właściciel |Tylko właściciel |
| Usunąć kontenera |Tylko właściciel |Tylko właściciel |
| Lista obiektów blob |Wszystkie |Tylko właściciel |
| Umieszczanie obiektu Blob |Tylko właściciel |Tylko właściciel |
| Pobierz obiekt Blob |Wszystkie |Wszystkie |
| Pobierz właściwości obiektu Blob |Wszystkie |Wszystkie |
| Ustaw właściwości obiektów Blob |Tylko właściciel |Tylko właściciel |
| Pobierz metadane obiektu Blob |Wszystkie |Wszystkie |
| Ustaw metadane obiektu Blob |Tylko właściciel |Tylko właściciel |
| Umieść bloku |Tylko właściciel |Tylko właściciel |
| Pobierz listę zablokowanych (tylko zatwierdzone bloki) |Wszystkie |Wszystkie |
| Pobierz listę bloku (tylko niezatwierdzone bloków lub wszystkie bloki) |Tylko właściciel |Tylko właściciel |
| Umieść zablokowanych |Tylko właściciel |Tylko właściciel |
| Usuwanie obiektów Blob |Tylko właściciel |Tylko właściciel |
| Kopiowanie obiektu Blob |Tylko właściciel |Tylko właściciel |
| Migawki obiektu Blob |Tylko właściciel |Tylko właściciel |
| Obiekt Blob dzierżawy |Tylko właściciel |Tylko właściciel |
| Umieść stronę |Tylko właściciel |Tylko właściciel |
| Get zakresów stron |Wszystkie |Wszystkie |
| Dołącz obiektów Blob |Tylko właściciel |Tylko właściciel |

## <a name="next-steps"></a>Następne kroki

* [Uwierzytelnianie dla usług Azure Storage](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Przy użyciu sygnatury dostępu współdzielonego (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Delegowanie dostępu za pomocą sygnatury dostępu współdzielonego](https://msdn.microsoft.com/library/azure/ee395415.aspx)
