---
title: "Ścieżka obiektu blob zmiany z domyślnej | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować funkcję Azure do zmiany nazwy ścieżki pliku obiektów blob (w podglądzie prywatnym)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 057d4d7370207859617eb63238bf425bfa6d3e16
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="change-a-blob-path-from-the-default-path-private-preview"></a>Zmienianie ścieżki obiektu blob ze ścieżki domyślnej (podglądzie prywatnym)

W tym artykule opisano, jak można skonfigurować funkcję Azure, aby zmienić domyślną ścieżkę pliku obiektu blob. 

## <a name="prerequisites"></a>Wymagania wstępne

Upewnij się, że masz definicji zadania, który został poprawnie skonfigurowany w zasobie danych hybrydowych w grupie zasobów.

## <a name="create-an-azure-function"></a>Tworzenie funkcji platformy Azure

Aby utworzyć funkcję platformy Azure, wykonaj następujące czynności:

1. Przejdź do witryny [Azure Portal](http://portal.azure.com/).

2. W górnej części okienka po lewej stronie, kliknij przycisk **nowy**. 

3. W **wyszukiwania** wpisz **aplikacji funkcji**, a następnie naciśnij klawisz Enter.

    ![W polu wyszukiwania wpisz "Funkcja aplikacji"](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. W **wyniki** kliknij **aplikacji funkcji**.

    ![Wybierz zasób aplikacji funkcji z listy wyników](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    **Aplikacji funkcji** zostanie otwarte okno.

5. Kliknij przycisk **Utwórz**.

    ![Przycisk "Utwórz" okna aplikacji — funkcja](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Na **aplikacji funkcji** blok konfiguracji, wykonaj następujące czynności:

    a. W **Nazwa aplikacji** wpisz nazwę aplikacji.
    
    b. W **subskrypcji** wpisz nazwę subskrypcji.

    c. W obszarze **grupy zasobów**, kliknij przycisk **Utwórz nowy**, a następnie wpisz nazwę grupy zasobów.

    d. W **Plan hostingu** wpisz **zaplanować użycie**.

    e. W **lokalizacji** wpisz lokalizację.

    f. W obszarze **konta magazynu**wybierz istniejące konto magazynu lub Utwórz nowe konto magazynu. Konto magazynu jest używana wewnętrznie do funkcji.

    ![Wprowadź nowe dane konfiguracji funkcji aplikacji](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Kliknij przycisk **Utwórz**.  
    Funkcja aplikacji zostanie utworzona.

8. W okienku po lewej stronie kliknij **więcej usług**, a następnie wykonaj następujące czynności:
    
    a. W **filtru** wpisz **usługi aplikacji**.
    
    b. Kliknij przycisk **usługi aplikacji**. 

    ![Łącze "Więcej usług" w okienku po lewej stronie](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Na liście usług aplikacji kliknij nazwę aplikacji funkcji.  
    Zostanie otwarta strona aplikacji funkcji.

10. W okienku po lewej stronie kliknij **nową funkcję**, a następnie wykonaj następujące czynności: 

    a. W **języka** listy, wybierz **C#**.
    
    b. W tablicy Kafelki szablonu, wybierz **QueueTrigger CSharp**.

    c. W **nazwy funkcji** wpisz nazwę funkcji.

    d. W **Nazwa kolejki** wpisz nazwę definicji zadania transformacji danych.

    e. W obszarze **połączenia konta magazynu**, kliknij przycisk **nowe**, a następnie wybierz konto, które odpowiada zadania transformacji danych.  
        Zanotuj nazwę połączenia. Wymagana jest nazwa później w funkcji platformy Azure.

       ![Utwórz nową funkcję C#](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Kliknij przycisk **Utwórz**.  
    **Funkcja** zostanie otwarte okno.

11. W **funkcja** okna, uruchom _csx_ pliku, a następnie wykonaj następujące czynności:

    a. Wklej następujący kod:

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip to copy the blob to new container, if source blob doesn't exist
            log.Info($"The specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy to new container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    b. Zastąp **STORAGE_CONNECTIONNAME** w wierszu 11 z połączeniem konta magazynu (zobacz punkt 8 c).

    c. U góry po lewej, kliknij przycisk **zapisać**.

    ![Zapisz — funkcja](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. Aby wykonać funkcję, Dodaj więcej plików w następujący sposób:

    a. Kliknij przycisk **wyświetlać pliki**.

       ![Łącze "Wyświetl pliki"](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Kliknij pozycję **Dodaj**.
    
    c. Typ **project.json**, a następnie naciśnij klawisz Enter.
    
    d. W **project.json** plików, wklej następujący kod:

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    e. Kliknij pozycję **Zapisz**.

Utworzono funkcji platformy Azure. Ta funkcja jest wywoływane zawsze, gdy nowy obiekt blob jest generowany przez zadanie transformacji danych.

## <a name="next-steps"></a>Następne kroki

[Użyj Menedżera danych StorSimple interfejsu użytkownika Służącego do przekształcenia danych](storsimple-data-manager-ui.md)
