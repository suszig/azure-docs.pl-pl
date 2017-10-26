---
title: "Zestaw SDK platformy .NET: operacje systemu plików w usłudze Azure Data Lake Store | Microsoft Docs"
description: "Użyj zestawu SDK platformy .NET usługi Azure Data Lake Store w celu wykonania takich operacji systemu plików w usłudze Data Lake Store, jak tworzenie folderów itp."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 7f6319dcf1ae66a686dd1c2ea2810b3041183098
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Operacje systemu plików w usłudze Azure Data Lake Store przy użyciu zestawu SDK platformy .NET
> [!div class="op_single_selector"]
> * [Zestaw SDK platformy .NET](data-lake-store-data-operations-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Z tego artykułu dowiesz się, jak wykonywać operacje systemu plików w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET. Operacje systemu plików obejmują tworzenie folderów w ramach konta usługi Data Lake Store, przekazywanie plików, pobieranie plików itp.

Aby uzyskać instrukcje na temat sposobu wykonywania operacji zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET, zobacz [Account management operations on Data Lake Store using .NET SDK (Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Wymagania wstępne
* **Program Visual Studio w wersji 2013, 2015 lub 2017**. Poniższe instrukcje korzystają z programu Visual Studio 2017.

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Konto usługi Azure Data Lake Store**. Aby uzyskać instrukcje dotyczące tworzenia konta, zobacz artykuł [Rozpoczynanie pracy z usługą Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Tworzenie aplikacji .NET
1. Otwórz program Visual Studio i utwórz aplikację konsolową.
2. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
3. W obszarze **Nowy projekt** wpisz lub wybierz następujące wartości:

   | Właściwość | Wartość |
   | --- | --- |
   | Kategoria |Szablony/Visual C#/Windows |
   | Szablon |Aplikacja konsolowa |
   | Nazwa |CreateADLApplication |
4. Kliknij przycisk **OK**, aby utworzyć projekt.
5. Dodaj pakiety NuGet do swojego projektu.

   1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij polecenie **Zarządzaj pakietami NuGet**.
   2. Na karcie **Menedżer pakietów NuGet** upewnij się, że **Źródło pakietów** jest ustawione na wartość **nuget.org** i że zaznaczone jest pole wyboru **Uwzględnij wersję wstępną**.
   3. Wyszukaj i zainstaluj następujące pakiety NuGet:

      * `Microsoft.Azure.Management.DataLake.Store` — w tym samouczku jest używana wersja v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` — w tym samouczku jest używana wersja v2.2.12.

        ![Dodawanie źródła pakietów NuGet](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Tworzenie nowego konta usługi Azure Data Lake")
   4. Zamknij **Menedżera pakietów NuGet**.
6. Otwórz plik **Program.cs**, usuń istniejący kod, a następnie dołącz poniższe instrukcje, aby dodać odwołania do przestrzeni nazw.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Zadeklaruj zmienne w pokazany poniżej sposób i podaj wartości dla symboli zastępczych. Upewnij się również, że podana tutaj ścieżka lokalna i nazwa pliku istnieją na komputerze.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static async void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Z pozostałych akapitów tego artykułu możesz dowiedzieć się, w jaki sposób używać dostępnych metod .NET w celu wykonywania operacji, takich jak uwierzytelnianie, przekazywanie plików itp.

## <a name="authentication"></a>Authentication

* Aby dowiedzieć się więcej o uwierzytelnianiu użytkowników końcowych w aplikacji, zobacz [End-user authentication with Data Lake Store using .NET SDK (Uwierzytelnianie użytkowników końcowych w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-end-user-authenticate-net-sdk.md).
* Aby dowiedzieć się więcej o uwierzytelnianiu „usługa-usługa” w aplikacji, zobacz [Service-to-service authentication with Data Lake Store using .NET SDK (Uwierzytelnianie „usługa-usługa” w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-objects"></a>Tworzenie obiektów klienta
Poniższy fragment kodu tworzy konto usługi Data Lake Store i obiekty systemu plików klienta, które są używane do wysyłania żądań do usługi.

    // Create client objects
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

## <a name="create-a-directory"></a>Tworzenie katalogu
Dodaj następującą metodę do swojej klasy. Ten fragment kodu przedstawia metodę `CreateDirectory()`, której możesz użyć do tworzenia katalogu w ramach konta usługi Data Lake Store.

    // Create a directory
    public static void CreateDirectory(string path)
    {
            _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `CreateDirectory()`.

    CreateDirectory(remoteFolderPath);
    Console.WriteLine("Created a directory in the Data Lake Store account. Press any key to continue ...");
    Console.ReadLine();

## <a name="upload-a-file"></a>Przekazywanie pliku
Dodaj następującą metodę do swojej klasy. Ten fragment kodu przedstawia metodę `UploadFile()`, której możesz użyć do przekazywania plików do konta usługi Data Lake Store. Zestaw SDK obsługuje cykliczne przekazywanie i pobieranie między lokalną ścieżką pliku i ścieżką pliku usługi Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `UploadFile()`.

    UploadFile(localFilePath, remoteFilePath, true);
    Console.WriteLine("Uploaded file in the directory. Press any key to continue...");
    Console.ReadLine();
    

## <a name="get-file-or-directory-info"></a>Uzyskiwanie informacji o pliku lub katalogu
Poniższy fragment kodu przedstawia metodę `GetItemInfo()`, której można użyć, aby uzyskać informacje o pliku lub katalogu dostępnym w usłudze Data Lake Store.

    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `GetItemInfo()`.

    
    var fileProperties = GetItemInfo(remoteFilePath);
    Console.WriteLine("The owner of the file at the path is:", fileProperties.Owner);
    Console.WriteLine("Press any key to continue...");
    Console.ReadLine();

## <a name="list-file-or-directories"></a>Wyświetlanie listy plików lub katalogów
Poniższy fragment kodu przedstawia metodę `ListItems()`, która służy do wyświetlania listy plików i katalogów w ramach konta usługi Data Lake Store.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `ListItems()`.

    var itemList = ListItems(remoteFolderPath);
    var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
    Console.WriteLine(String.Join("\r\n", fileMenuItems));
    Console.WriteLine("Files and directories listed. Press any key to continue ...");
    Console.ReadLine();

## <a name="concatenate-files"></a>Łączenie plików
Poniższy fragment kodu przedstawia metodę `ConcatenateFiles()`, która umożliwia łączenie plików.

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `ConcatenateFiles()`. Ten fragment kodu zakłada, że do konta usługi Data Lake Store został przekazany inny plik i że ścieżka pliku jest podana w ciągu *anotherRemoteFilePath*.

    string[] stringOfFiles = new String[] {remoteFilePath, anotherRemoteFilePath};
    string destFilePath = Path.Combine(remoteFolderPath, "Concatfile.txt");
    ConcatenateFiles(stringOfFiles, destFilePath);
    Console.WriteLine("Files concatinated. Press any key to continue ...");
    Console.ReadLine();

## <a name="append-to-a-file"></a>Dołączanie do pliku
Poniższy fragment kodu przedstawia metodę `AppendToFile()`, której można użyć, aby dołączyć dane do pliku zapisanego już w ramach konta usługi Data Lake Store.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `AppendToFile()`.

    AppendToFile(remoteFilePath, "123");
    Console.WriteLine("Content appended. Press any key to continue ...");
    Console.ReadLine();

## <a name="download-a-file"></a>Pobieranie pliku
Poniższy fragment kodu przedstawia metodę `DownloadFile()`, której można użyć, aby pobrać plik z konta usługi Data Lake Store.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:true);
    }

Dodaj poniższy fragment kodu do Twojej metody `Main()` w celu wywołania metody `DownloadFile()`.

    DownloadFile(destFilePath, localFilePath);
    Console.WriteLine("File downloaded. Press any key to continue ...");
    Console.ReadLine();

## <a name="see-also"></a>Zobacz też
* [Account management operations on Data Lake Store using .NET SDK (Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-get-started-net-sdk.md)
* [Dokumentacja zestawu SDK .NET usługi Data Lake Store](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
