---
title: "Korzystanie z zestawu SDK .NET dla programu Microsoft Azure StorSimple Data zadań | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak użyć zestawu .NET SDK w celu uruchomienia zadania Menedżer StorSimple danych (w podglądzie prywatnym)"
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
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a>Użyj zestawu SDK programu .net zainicjować transformacji danych (w podglądzie prywatnym)

## <a name="overview"></a>Omówienie

W tym artykule opisano sposób korzystania z funkcji przekształcania danych w usłudze Menedżer StorSimple danych do przekształcania danych urządzenia StorSimple. Przekształcone dane są następnie używane przez innych usług platformy Azure w chmurze. Artykuł ma również wskazówki ułatwiające tworzenie przykładowej aplikacji konsoli .NET zainicjować zadania przekształcania danych, a następnie ją Śledź ukończenia.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz:
*   System z programu Visual Studio 2012 2013, 2015 lub zainstalowany 2017 r.
*   Zainstalowany program Azure Powershell. [Pobieranie programu Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Ustawienia konfiguracji w celu zainicjowania zadania przekształcania danych (instrukcjami, aby uzyskać te ustawienia są uwzględnione w tym miejscu).
*   Definicji zadania, który został poprawnie skonfigurowany w zasobie danych hybrydowych w grupie zasobów.
*   Wszystkie wymagane biblioteki dll. Pobierz te biblioteki dll z [repozytorium GitHub](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[skryptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) z repozytorium github.

## <a name="step-by-step"></a>Krok po kroku

Wykonaj poniższe kroki, aby użyć .NET w celu uruchomienia zadania przekształcania danych.

1. Aby pobrać parametry konfiguracji, wykonaj następujące czynności:
    1. Pobierz `Get-ConfigurationParams.ps1` ze skryptu repozytorium github w `C:\DataTransformation` lokalizacji.
    1. Uruchom `Get-ConfigurationParams.ps1` skryptu z repozytorium github. Wpisz następujące polecenie:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        Można przekazać wartości ActiveDirectoryKey i AppName.


2. Ten skrypt generuje następujące wartości:
    * Identyfikator klienta
    * Identyfikator dzierżawy
    * Kluczy Active Directory (tak samo, jak to objęte powyżej)
    * Identyfikator subskrypcji

3. Za pomocą programu Visual Studio 2012, 2013 lub 2015, Utwórz aplikację konsolową C# .NET.

    1. Uruchom **programu Visual Studio 2012/2013/2015**.
    1. Kliknij pozycję **Plik**, wskaż polecenie **Nowy** i kliknij pozycję **Projekt**.
    2. Rozwiń węzeł **Szablony** i wybierz opcję **Visual C#**.
    3. Wybierz opcję **Aplikacja konsolowa** z listy typów projektów po prawej stronie.
    4. Wprowadź **DataTransformationApp** dla **nazwa**.
    5. Wybierz **C:\DataTransformation** dla **lokalizacji**.
    6. Kliknij przycisk **OK**, aby utworzyć projekt.

4.  Teraz Dodaj wszystkie biblioteki dll w [biblioteki dll](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) folder jako **odwołania** w projekcie, który został utworzony. Aby pobrać pliki dll, wykonaj następujące czynności:

    1. W programie Visual Studio, przejdź do **Widok > Eksploratora rozwiązań**.
    1. Kliknij strzałkę w lewo projektu aplikacji przekształcania danych. Kliknij przycisk **odwołania** , a następnie kliknij prawym przyciskiem myszy, aby **Dodaj odwołanie**.
    2. Przejdź do lokalizacji folderu pakietów, wybierz wszystkie biblioteki dll i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **OK**.

5. Dodaj następujące instrukcje **using** do pliku źródłowego (Program.cs) w projekcie.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. Poniższy kod inicjuje wystąpienie zadania przekształcania danych. Dodaj ją w **metody Main**. Zastąp wartości parametrów konfiguracji uzyskany wcześniej. Podłącz wartości **Nazwa grupy zasobów** i **Nazwa zasobu danych hybrydowego**. **Nazwa grupy zasobów** jest ten, który obsługuje hybrydowego zasobów danych, na którym skonfigurowano definicji zadania.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. Określ parametry, z których trzeba uruchomić definicji zadania

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (LUB)

    Jeśli chcesz zmienić parametrów definicji zadania w czasie wykonywania, Dodaj następujący kod:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. Po zainicjowaniu Dodaj następujący kod do wyzwalania zadania przekształcania danych w definicji zadania. Podłącz odpowiednie **Nazwa definicji zadania**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. To zadanie będzie przekazywać pasujących plików istnieje w katalogu głównym woluminu StorSimple do określonego kontenera. Po przekazaniu pliku wiadomości zostało porzucone z taką samą nazwę jak definicji zadania w kolejce (w tym samym koncie magazynu jako kontener). Ten komunikat może służyć jako wyzwalacz zainicjować dalszego przetwarzania pliku.

10. Po wyzwoleniu zadania Dodaj następujący kod, aby śledzić zadania do wykonania.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a>Następne kroki

[Użyj Menedżera danych StorSimple interfejsu użytkownika Służącego do przekształcenia danych](storsimple-data-manager-ui.md).
