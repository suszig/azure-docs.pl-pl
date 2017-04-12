---
title: "Korzystanie z zestawu SDK .NET do programowania aplikacji w usłudze Azure Data Lake Store| Microsoft Docs"
description: "Użyj zestawu SDK .NET usługi Azure Data Lake Store, aby utworzyć konto usługi Data Lake Store i wykonywać podstawowe operacje w tej usłudze"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/07/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 0dbf6a121c07d7d1340898f51a38c3572e57b3a2
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store z użyciem zestawu SDK .NET
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Dowiedz się, jak używać [zestawu SDK .NET usługi Azure Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx), aby wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych itp. Aby uzyskać więcej informacji o usłudze Data Lake, zobacz temat [Usługa Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne
* **Program Visual Studio w wersji 2013, 2015 lub 2017**. Poniższe instrukcje opisują korzystanie z programu Visual Studio 2015 Update 2.

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Konto usługi Azure Data Lake Store**. Aby uzyskać instrukcje dotyczące tworzenia konta, zobacz artykuł [Rozpoczynanie pracy z usługą Azure Data Lake Store](data-lake-store-get-started-portal.md)

* **Utworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji usługi Azure AD można uwierzytelnić aplikację usługi Data Lake Store w usłudze Azure AD. Istnieją różne metody uwierzytelniania w usłudze Azure AD: **uwierzytelnianie użytkowników końcowych** i **uwierzytelnianie między usługami**. Instrukcje i dodatkowe informacje na temat uwierzytelniania można znaleźć w artykule [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) (Uwierzytelnianie w usłudze Data Lake Store przy użyciu usługi Azure Active Directory).

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
5. Dodaj pakiety Nuget do projektu.

   1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij polecenie **Zarządzaj pakietami NuGet**.
   2. W **Menedżerze pakietów Nuget** upewnij się, że **Źródło pakietów** jest ustawione na **nuget.org** i że zaznaczone jest pole wyboru **Uwzględnij wersję wstępną**.
   3. Wyszukaj i zainstaluj następujące pakiety NuGet:

      * `Microsoft.Azure.Management.DataLake.Store` — w tym samouczku jest używana wersja v1.0.4.
      * `Microsoft.Azure.Management.DataLake.StoreUploader` — w tym samouczku jest używana wersja v1.0.1.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` — w tym samouczku jest używana wersja v2.2.11.

        ![Dodawanie źródła pakietów Nuget](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Tworzenie nowego konta usługi Azure Data Lake")
   4. Zamknij **Menedżera pakietów Nuget**.
6. Otwórz plik **Program.cs**, usuń istniejący kod, a następnie dołącz poniższe instrukcje, aby dodać odwołania do przestrzeni nazw.

        using System;
        using System.IO;
    using System.Security.Cryptography.X509Certificates; // Wymagane tylko, jeśli używasz aplikacji usługi Azure AD utworzonej za pomocą certyfikatów korzystających z przestrzeni nazw System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;  using Microsoft.Azure.Management.DataLake.StoreUploader;  using Microsoft.IdentityModel.Clients.ActiveDirectory;  using Microsoft.Rest.Azure.Authentication;

7. Zadeklaruj zmienne w sposób przedstawiony poniżej i podaj wartości dla nazwy usługi Data Lake Store oraz nazwy już istniejącej grupy zasobów. Upewnij się również, że podana tutaj ścieżka lokalna i nazwa pliku istnieją na komputerze. Dodaj poniższy fragment kodu po deklaracjach przestrzeni nazw.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Z pozostałych akapitów tego artykułu możesz dowiedzieć się, w jaki sposób używać dostępnych metod .NET w celu wykonywania operacji, takich jak uwierzytelnianie, przekazywanie plików itp.

## <a name="authentication"></a>Authentication

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Jeśli używasz uwierzytelniania użytkowników końcowych (zalecane w przypadku tego samouczka)

Użyj tej funkcji z istniejącą natywną aplikacją usługi Azure AD w celu **interakcyjnego** uwierzytelniania aplikacji. Oznacza to, że zostanie wyświetlony monit o wprowadzenie poświadczeń platformy Azure.

W celu ułatwienia pracy poniższy fragment kodu używa wartości domyślnych identyfikatora klienta oraz identyfikatora URI przekierowania, które będą działać bez subskrypcji platformy Azure. Zalecamy użycie tej metody, aby szybciej zakończyć korzystanie z tego samouczka. W poniższym fragmencie kodu wystarczy podać wartość identyfikatora dzierżawy. Można go pobrać, wykonując instrukcje podane w części [Create an Active Directory Application](data-lake-store-end-user-authenticate-using-active-directory.md) (Tworzenie aplikacji usługi Active Directory).

    // User login via interactive popup
    // Use the client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Należy wiedzieć o kilku kwestiach związanych z powyższym fragmentem.

* Aby ułatwić szybsze zakończenie korzystania z tego samouczka, w tym fragmencie jest używana domena usługi Azure AD oraz identyfikator klienta, który jest domyślnie dostępny dla wszystkich subskrypcji platformy Azure. Dzięki temu można **użyć tego fragmentu w aplikacji w niezmienionej formie**.
* Jeśli jednak chcesz użyć własnej domeny usługi Azure AD i własnego identyfikatora klienta aplikacji, musisz utworzyć natywną aplikację usługi Azure AD, a następnie użyć identyfikatora dzierżawy usługi Azure AD, identyfikatora klienta i identyfikatora URI przekierowania utworzonej aplikacji. Instrukcje można znaleźć w temacie [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) (Tworzenie aplikacji usługi Active Directory na potrzeby uwierzytelniania użytkownika końcowego przy użyciu usługi Data Lake Store).

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Jeśli używasz uwierzytelniania między usługami z kluczem tajnym klienta
Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji w sposób **nieinterakcyjny** za pomocą wpisu tajnego klienta/klucza dla aplikacji/nazwy głównej usługi. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD. Instrukcje dotyczące tworzenia aplikacji sieci Web usługi Azure AD oraz pobierania identyfikatora klienta i wpisu tajnego klienta wymaganych w poniższym fragmencie kodu można znaleźć w temacie [Create an Active Directory Application for service-to-service authentication with Data Lake Store](data-lake-store-authenticate-using-active-directory.md) (Tworzenie aplikacji usługi Active Directory na potrzeby uwierzytelniania między usługami przy użyciu usługi Data Lake Store).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Jeśli używasz uwierzytelniania między usługami z certyfikatem

Trzecią możliwością jest użycie poniższego fragmentu kodu do uwierzytelniania aplikacji w sposób **nieinterakcyjny** przy użyciu certyfikatu dla aplikacji/nazwy głównej usługi w usłudze Azure Active Directory. Użyj tej metody wraz z istniejącą [aplikacją usługi Azure AD z certyfikatami](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>Tworzenie obiektów klienta
Poniższy fragment kodu tworzy konto usługi Data Lake Store i obiekty systemu plików klienta, które są używane do wysyłania żądań do usługi.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Wyświetlanie listy wszystkich kont usługi Data Lake Store w ramach subskrypcji
Poniższy fragment kodu umożliwia wyświetlenie listy wszystkich kont usługi Data Lake Store należących do danej subskrypcji platformy Azure.

    // List all ADLS accounts within the subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Tworzenie katalogu
Poniższy fragment kodu przedstawia metodę `CreateDirectory`, która służy do tworzenia katalogu w ramach konta usługi Data Lake Store.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Przekazywanie pliku
Poniższy fragment kodu przedstawia metodę `UploadFile`, która służy do przekazywania plików do konta usługi Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

Program `DataLakeStoreUploader` obsługuje cykliczne przekazywanie i pobieranie między lokalną ścieżką pliku i ścieżką pliku usługi Data Lake Store.    

## <a name="get-file-or-directory-info"></a>Uzyskiwanie informacji o pliku lub katalogu
Poniższy fragment kodu przedstawia metodę `GetItemInfo`, której można użyć, aby uzyskać informacje o pliku lub katalogu dostępnym w usłudze Data Lake Store.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Wyświetlanie listy plików lub katalogów
Poniższy fragment kodu przedstawia metodę `ListItem`, która służy do wyświetlania listy plików i katalogów w ramach konta usługi Data Lake Store.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Łączenie plików
Poniższy fragment kodu przedstawia metodę `ConcatenateFiles`, która umożliwia łączenie plików.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Dołączanie do pliku
Poniższy fragment kodu przedstawia metodę `AppendToFile`, której można użyć, aby dołączyć dane do pliku zapisanego już w ramach konta usługi Data Lake Store.

    // Append to file
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Pobieranie pliku
Poniższy fragment kodu przedstawia metodę `DownloadFile`, której można użyć, aby pobrać plik z konta usługi Data Lake Store.

    // Download file
    public static async Task DownloadFile(string srcPath, string destPath)
    {
        using (var stream = await _adlsFileSystemClient.FileSystem.OpenAsync(_adlsAccountName, srcPath))
        using (var fileStream = new FileStream(destPath, FileMode.Create))
        {
            await stream.CopyToAsync(fileStream);
        }
    }

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Dokumentacja zestawu SDK .NET usługi Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
* [Dokumentacja interfejsu REST usługi Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)

