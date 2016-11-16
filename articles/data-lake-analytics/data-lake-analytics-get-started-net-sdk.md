---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu zestawu SDK programu .NET | Microsoft Docs"
description: "Dowiedz się, jak zestaw SDK programu .NET umożliwia tworzenie kont usługi Data Lake Store, tworzenie zadań usługi Data Lake Analytics i przesyłanie zadań napisanych w języku SQL U. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 1dfcbc3d-235d-4074-bc2a-e96def8298b6
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/26/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 60deb681b1090444f5c178fb0c9b0458ea83f73d


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-net-sdk"></a>Samouczek: rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu zestawu SDK programu .NET
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informacje na temat używania zestawu Azure SDK programu .NET w celu przesyłania zadań napisanych przy użyciu skryptu [U-SQL](data-lake-analytics-u-sql-get-started.md) do usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

W ramach tego samouczka utworzysz aplikację konsolową języka C# umożliwiającą przesyłanie zadania U-SQL, które będzie odczytywać zawartość pliku z wartościami rozdzielonymi tabulatorami (TSV) i konwertować ją do pliku z wartościami rozdzielanymi przecinkami (CSV). Aby wykonać kroki opisane w tym samouczku, korzystając z innych obsługiwanych narzędzi, kliknij odpowiednią kartę w górnej części tego artykułu.

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Zainstalowany program Visual Studio 2015, Visual Studio 2013 Update 4 lub Visual Studio 2012 z językiem Visual C++**.
* **Zestaw Microsoft Azure SDK dla programu .NET w wersji 2.5 lub nowszej**.  Można go zainstalować przy użyciu [Instalatora platformy sieci Web](http://www.microsoft.com/web/downloads/platform.aspx).
* **Konto usługi Azure Data Lake Analytics**. Więcej informacji można znaleźć w artykule [Manage Data Lake Analytics using Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) (Zarządzanie usługą Data Lake Analytics przy użyciu zestawu Azure SDK programu .NET).

## <a name="create-console-application"></a>Tworzenie aplikacji konsolowej
W ramach tego samouczka przetworzysz wybrane dzienniki wyszukiwania.  Dziennik wyszukiwania może być przechowywany w usłudze Data Lake Store lub w usłudze Azure Blob Storage. 

Przykładowy dziennik wyszukiwania znajduje się w publicznym kontenerze usługi Azure Blob. Działanie tej aplikacji obejmuje pobranie pliku na stację roboczą, a następnie przekazanie pliku do domyślnego konta usługi Data Lake Store w ramach swojego konta usługi Data Lake Analytics.

**Aby utworzyć skrypt U-SQL**

Zadania usługi Data Lake Analytics są napisane w języku U-SQL. Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

Utwórz plik **SampleUSQLScript.txt** za pomocą poniższego skryptu U-SQL i umieść go w ścieżce katalogu **C:\temp\**.  Ścieżka jest ustalona w aplikacji .NET, która zostanie utworzona w następnej procedurze.  

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();

Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()**, a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()**. 

W programie w języku C# musisz przygotować plik **/Samples/Data/SearchLog.tsv** i folder **/Output/**.    

Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake. Można także użyć ścieżek bezwzględnych.  Na przykład: 

    adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

Aby uzyskać dostęp do plików na połączonych kontach usługi Storage, należy użyć ścieżek bezwzględnych.  Składnia dla plików przechowywanych na połączonym koncie usługi Azure Storage jest następująca:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

> [!NOTE]
> Obecnie występuje znany problem z usługą Azure Data Lake.  Jeśli przykładowa aplikacja zostanie przerwana lub wystąpi błąd, może być konieczne ręczne usunięcie kont usług Data Lake Store i Data Lake Analytics utworzonych przez skrypt.  Jeśli nie znasz portalu Azure, zapoznaj się z przewodnikiem [Zarządzanie usługą Azure Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-manage-use-portal.md), który ułatwi Ci rozpoczęcie pracy.       
> 
> 

**Aby utworzyć aplikację**

1. Otwórz program Visual Studio.
2. Utwórz aplikację konsolową języka C#.
3. Otwórz konsolę zarządzania pakietami NuGet, a następnie uruchom następujące polecenia:
   
        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package WindowsAzure.Storage
4. Wewnątrz elementu Program.cs wklej następujący kod:
   
        using System;
        using System.IO;
        using System.Collections.Generic;
        using System.Threading;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.WindowsAzure.Storage.Blob;
   
        namespace SdkSample
        {
          class Program
          {
            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
   
            private static string _adlaAccountName = "<Enter an Existing Data Lake Analytics Account Name>";
            private static string _adlsAccountName = "<Enter the default Data Lake Store Account Name>";
   
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
   
            private static void Main(string[] args)
            {
                string localFolderPath = @"c:\temp\";
   
                // Connect to Azure
                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);
   
                SetupClients(creds, SUBSCRIPTIONID);
   
                // Transfer the source file from a public Azure Blob container to Data Lake Store.
                CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
                blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
                UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
                WaitForNewline("Source data file prepared.", "Submitting a job.");

                // Submit the job
                Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
                WaitForNewline("Job submitted.", "Waiting for job completion.");

                // Wait for job completion
                WaitForJob(jobId);
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output
                DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");

                  WaitForNewline("Job output downloaded. You can now exit.");
            }

            public static ServiceClientCredentials AuthenticateAzure(
                string domainName,
                string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static void SetupClients(ServiceClientCredentials tokenCreds, string subscriptionId)
            {
                _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
                _adlaClient.SubscriptionId = subscriptionId;

                _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                Console.WriteLine(reason + "\r\nPress ENTER to continue...");

                Console.ReadLine();

                if (!String.IsNullOrWhiteSpace(nextAction))
                    Console.WriteLine(nextAction);
            }


            // List all Data Lake Analytics accounts within the subscription
            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                Console.WriteLine("You have %i Data Lake Analytics account(s).", accounts.Count);
                for (int i = 0; i < accounts.Count; i++)
                {
                    Console.WriteLine(accounts[i].Name);
                }

                return accounts;
            }
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
                var script = File.ReadAllText(scriptPath);

                var jobId = Guid.NewGuid();
                var properties = new USqlJobProperties(script);
                var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
                var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);

                return jobId;
            }

            public static JobResult WaitForJob(Guid jobId)
            {
                var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                while (jobInfo.State != JobState.Ended)
                {
                    jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                }
                return jobInfo.Result.Value;
            }
          }
        }

1. Naciśnij klawisz **F5**, aby uruchomić aplikację. Dane wyjściowe wyglądają tak:
   
    ![Dane wyjściowe zadania U-SQL usługi Azure Data Lake Analytics utworzonego przy użyciu zestawu SDK programu .NET](./media/data-lake-analytics-get-started-net-sdk/data-lake-analytics-dotnet-job-output.png)
2. Sprawdź plik wyjściowy.  Domyślna ścieżka i nazwa pliku to c:\Temp\SearchLog-from-Data-Lake.csv.

## <a name="see-also"></a>Zobacz też
* Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
* Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Aby rozpocząć tworzenie aplikacji w języku U-SQL, zobacz artykuł [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
* Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
* Aby zapoznać się z omówieniem usługi Data Lake Analytics, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).




<!--HONumber=Nov16_HO2-->


