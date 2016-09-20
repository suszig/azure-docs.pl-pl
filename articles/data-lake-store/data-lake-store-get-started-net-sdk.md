<properties
   pageTitle="Korzystanie z zestawu SDK .NET usługi Data Lake Store w celu projektowania aplikacji | Azure"
   description="Korzystanie z zestawu SDK .NET usługi Azure Data Lake Store w celu projektowania aplikacji"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/18/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store z użyciem zestawu SDK .NET

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw SDK .NET](data-lake-store-get-started-net-sdk.md)
- [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
- [Interfejs API REST](data-lake-store-get-started-rest-api.md)
- [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak używać zestawu SDK .NET usługi Azure Data Lake Store, aby utworzyć konto usługi Azure Data Lake i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake, zobacz [Azure Data Lake Store](data-lake-store-overview.md).

## Wymagania wstępne

* Program Visual Studio w wersji 2013 lub 2015. Poniższe instrukcje opisują korzystanie z programu Visual Studio 2015.
* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Włączenie subskrypcji platformy Azure** w celu uzyskania publicznej wersji zapoznawczej usługi Data Lake Store. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).
* **Utworzenie aplikacji usługi Azure Active Directory**. Istnieją dwa sposoby uwierzytelniania z użyciem usługi Azure Active Directory — **interaktywny** i **nieinteraktywny**. W zależności od sposobu uwierzytelniania należy spełnić różne wymagania wstępne.
    * **Uwierzytelnianie interakcyjne** (używane w tym artykule) — w usłudze Azure Active Directory należy utworzyć **aplikację Native Client**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Uzyskaj **identyfikator klienta** i **identyfikator URI przekierowania** dla aplikacji.
        - Ustaw uprawnienia delegowane.

    * **Uwierzytelnianie nieinterakcyjne** — w usłudze Azure Active Directory należy utworzyć **aplikację sieci Web**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Pobierz **identyfikator klienta**, **klucz tajny klienta** oraz **identyfikator URI przekierowania** dla aplikacji.
        - Ustaw uprawnienia delegowane.
        - Przypisz aplikację usługi Azure Active Directory do roli. Rola może być na poziomie zakresu, na którym chcesz nadać uprawnienie do aplikacji usługi Azure Active Directory. Na przykład możesz przypisać aplikację na poziomie subskrypcji lub na poziomie grupy zasobów. 

    Instrukcje dotyczące pobierania tych wartości, ustawiania uprawnień i przypisywania ról znajdują się w temacie [Tworzenie aplikacji i nazwy głównej usługi Active Directory przy użyciu portalu](../resource-group-create-service-principal-portal.md).

## Tworzenie aplikacji .NET

1. Otwórz program Visual Studio i utwórz aplikację konsolową.

2. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.

3. W obszarze **Nowy projekt** wpisz lub wybierz następujące wartości:

  	| Właściwość | Wartość                       |
  	|----------|-----------------------------|
  	| Kategoria | Szablony/Visual C#/Windows |
  	| Szablon | Aplikacja konsolowa         |
  	| Nazwa     | CreateADLApplication        |

4. Kliknij przycisk **OK**, aby utworzyć projekt.

5. Dodaj pakiety Nuget do projektu.

    1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij polecenie **Zarządzaj pakietami NuGet**.
    2. W **Menedżerze pakietów Nuget** upewnij się, że **Źródło pakietów** jest ustawione na **nuget.org** i że zaznaczone jest pole wyboru **Uwzględnij wersję wstępną**.
    3. Wyszukaj i zainstaluj następujące pakiety usługi Data Lake Store:

        * `Microsoft.Azure.Management.DataLake.Store`
        * `Microsoft.Azure.Management.DataLake.StoreUploader`

        ![Dodawanie źródła pakietów Nuget](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. Zainstaluj również pakiet `Microsoft.IdentityModel.Clients.ActiveDirectory` na potrzeby uwierzytelniania usługi Azure Active Directory. Upewnij się, że *usunięte jest zaznaczenie* pola wyboru **Uwzględnij wersję wstępną**, aby zainstalować stabilną wersję tego pakietu.

        ![Dodawanie źródła pakietów Nuget](./media/data-lake-store-get-started-net-sdk/adl.install.azure.auth.png "Create a new Azure Data Lake account")


    5. Zamknij **Menedżera pakietów Nuget**.

7. Otwórz plik **Program.cs**, usuń istniejący kod, a następnie dołącz poniższe instrukcje, aby dodać odwołania do przestrzeni nazw.

        using System;
        using System.IO;
        using System.Security;
        using System.Text;
        using System.Collections.Generic;
        using System.Linq;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;

8. Zadeklaruj zmienne w sposób przedstawiony poniżej i podaj wartości dla nazwy usługi Data Lake Store oraz nazwy grupy zasobów. Usługa Data Lake Store o podanej nazwie zostanie utworzona przez aplikację. Grupa zasobów podana tutaj powinna już istnieć. Upewnij się również, że podana tutaj ścieżka lokalna i nazwa pliku istnieją na komputerze. Dodaj poniższy fragment kodu po deklaracjach przestrzeni nazw.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Z pozostałych akapitów tego artykułu możesz dowiedzieć się, jak używać dostępnych metod .NET w celu wykonywania operacji, takich jak uwierzytelnianie użytkowników, tworzenie konta usługi Data Lake Store, przekazywanie plików itp. Jeśli szukasz kompletnego przykładu pracy z usługą Data Lake Store, zobacz [Dodatek](#appendix-sample-code) w dolnej części tego artykułu.

## Uwierzytelnianie użytkownika

Dostępne są dwa sposoby uwierzytelniania za pomocą usługi Azure Active Directory:

* **Interakcyjne**, gdy użytkownik loguje się przy użyciu aplikacji. Ten sposób jest implementowany przez metodę `AuthenticateUser` we fragmencie kodu poniżej.

* **Nieinterakcyjne**, gdzie aplikacja udostępnia własne poświadczenia. Ten sposób jest implementowany przez metodę `AuthenticateAppliaction` we fragmencie kodu poniżej.

### Uwierzytelnianie interakcyjne

Poniższy fragment kodu przedstawia metodę `AuthenticateUser`, która służy do logowania interakcyjnego.

    // Authenticate the user with AAD through an interactive popup.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
    
        var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
            PromptBehavior.Auto, UserIdentifier.AnyUser);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }

### Uwierzytelnianie nieinterakcyjne

Poniższy fragment kodu przedstawia metodę `AuthenticateApplication`, która służy do logowania nieinterakcyjnego.

    // Authenticate the application with AAD through the application's secret key.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
        var credential = new ClientCredential(appClientId, clientSecret);
    
        var tokenAuthResult = authContext.AcquireToken(resource, credential);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }
    
## Tworzenie konta usługi Data Lake Store

Poniższy fragment kodu przedstawia metodę `CreateAccount`, która służy do tworzenia konta usługi Data Lake Store.

    // Create Data Lake Store account
    public static void CreateAccount()
    {
        var adlsParameters = new DataLakeStoreAccount(location: _location);
        _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
    } 

## Wyświetlanie listy wszystkich kont usługi Data Lake Store w ramach subskrypcji

Poniższy fragment kodu przedstawia metodę `ListAdlStoreAccounts`, która służy do tworzenia listy wszystkich kont usługi Data Lake Store należących do danej subskrypcji platformy Azure.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccount>(response);
    
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
    
        return accounts;
    }

## Tworzenie katalogu

Poniższy fragment kodu przedstawia metodę `CreateDirectory`, która służy do tworzenia katalogu w ramach konta usługi Data Lake Store.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## Przekazywanie pliku do usługi Data Lake Store

Poniższy fragment kodu przedstawia metodę `UploadFile`, która służy do przekazywania plików do konta usługi Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

## Uzyskiwanie informacji o pliku lub katalogu

Poniższy fragment kodu przedstawia metodę `GetItemInfo`, której można użyć, aby uzyskać informacje o pliku lub katalogu dostępnym w usłudze Data Lake Store. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## Wyświetlanie listy plików lub katalogów

Poniższy fragment kodu przedstawia metodę `ListItem`, która służy do wyświetlania listy plików i katalogów w ramach konta usługi Data Lake Store.
    
    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## Łączenie plików

Poniższy fragment kodu przedstawia metodę `ConcatenateFiles`, która umożliwia łączenie plików. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## Dołączanie do pliku

Poniższy fragment kodu przedstawia metodę `AppendToFile`, której można użyć, aby dołączyć dane do pliku zapisanego już w ramach konta usługi Data Lake Store.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## Pobieranie pliku

Poniższy fragment kodu przedstawia metodę `DownloadFile`, której można użyć, aby pobrać plik z konta usługi Data Lake Store.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);

        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## Usuwanie konta usługi Data Lake Store

Poniższy fragment kodu przedstawia metodę `DeleteAccount`, która służy do usuwania konta usługi Data Lake Store.

    // Delete account
    public static void DeleteAccount()
    {
        _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
    }

## Dodatek: kod przykładowy

Poniższy fragment kodu jest kompletnym przykładem kodu, który można skopiować i wkleić do aplikacji, aby zobaczyć całą operację w usłudze Data Lake Store. Przed uruchomieniem fragmentu kodu upewnij się, że podane zostały wymagane wartości, takie jak nazwa usługi Data Lake Store, nazwa grupy zasobów itp. Należy również podać wartości wymagane do uwierzytelniania usługi Azure Active Direcctory, na przykład **\<APPLICATION-CLIENT-ID>**, **\<APPLICATION-REPLY-URI>** oraz **\<SUBSCRIPTION-ID>**.

Poniższy fragment kodu udostępnia metody, które mogą być stosowane w podejściu interakcyjnym i nieinterakcyjnym, przy czym blok kodu dla podejścia nieinterakcyjnego jest oznaczony jako komentarz. W przypadku metody interakcyjnej wymagane jest podanie identyfikatora klienta aplikacji AAD i identyfikatora URI przekierowania. Link w sekcji wymagań wstępnych prowadzi do instrukcji dotyczących uzyskiwania tych identyfikatorów.

>[AZURE.NOTE] Jeśli chcesz zmodyfikować fragment kodu i użyć metody nieinterakcyjnej (`AuthenticateApplication`), oprócz identyfikatora klienta i identyfikatora URI odpowiedzi klienta w danych wejściowych metody podaj także klucz uwierzytelniania klienta. Artykuł [Create Active Directory application and service principal using portal](../resource-group-create-service-principal-portal.md) (Tworzenie aplikacji i głównej nazwy usługi Active Directory przy użyciu portalu) zawiera również informacje dotyczące sposobu generowania i pobierania klucza uwierzytelniania klienta.
    
Na koniec upewnij się, że podana tutaj ścieżka lokalna i nazwa pliku istnieją na komputerze. Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).



    using System;
    using System.IO;
    using System.Security;
    using System.Text;
    using System.Collections.Generic;
    using System.Linq;

    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.Azure.Management.DataLake.StoreUploader;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;

    namespace SdkSample
    {
        class Program
        {
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

            private static string _adlsAccountName;
            private static string _resourceGroupName;
            private static string _location;

            private static void Main(string[] args)
            {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                _location = "East US 2";

                string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                string remoteFolderPath = "/data_lake_path/";
                string remoteFilePath = remoteFolderPath + "file.txt";

                // Authenticate the user
                var tokenCreds = AuthenticateUser("common", "https://management.core.windows.net/",
                    "<APPLICATION-CLIENT-ID>", new Uri("<APPLICATION-REPLY-URI>")); // TODO: Replace bracketed values.

                SetupClients(tokenCreds, "<SUBSCRIPTION-ID>"); // TODO: Replace bracketed value.

                // Run sample scenarios
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Creating a directory.");

                // Create a directory in the Data Lake Store
                CreateDirectory(remoteFolderPath);
                WaitForNewline("Directory created.", "Showing directory info.");

                // Get info about the directory in the Data Lake Store
                var itemInfo = GetItemInfo(remoteFolderPath);
                Console.WriteLine("Type: " + itemInfo.Type);
                Console.WriteLine("Last modified (UTC): " +
                                  new DateTime(1970, 1, 1, 0, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(
                                      itemInfo.ModificationTime.Value));
                WaitForNewline("Directory info shown.", "Uploading a file.");

                // Upload a file to the Data Lake Store
                UploadFile(localFilePath, remoteFilePath);
                WaitForNewline("File uploaded.", "Listing files and directories.");

                // List the files in the Data Lake Store
                var itemList = ListItems(remoteFolderPath);
                var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
                Console.WriteLine(String.Join("\r\n", fileMenuItems));
                WaitForNewline("Files and directories listed.", "Appending content to a file.");

                // Append to a file in the Data Lake Store
                AppendToFile(remoteFilePath, "123");
                WaitForNewline("Content appended.", "Downloading a file.");

                // Download a file from the Data Lake Store
                DownloadFile(remoteFilePath, localFilePath);
                WaitForNewline("File downloaded.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted. You can now exit.");
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                if (!String.IsNullOrWhiteSpace(nextAction))
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                    Console.WriteLine(nextAction);
                }
                else
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                }
            }

            // Authenticate the user with AAD through an interactive popup.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);

                var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
                    PromptBehavior.Auto, UserIdentifier.AnyUser);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            
            /*
            // Authenticate the application with AAD through the application's secret key.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
                var credential = new ClientCredential(appClientId, clientSecret);

                var tokenAuthResult = authContext.AcquireToken(resource, credential);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            */

            //Set up clients
            public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
            {
                _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
                _adlsClient.SubscriptionId = subscriptionId;

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            // Create account
            public static void CreateAccount()
            {
                // Create ADLS account
                var adlsParameters = new DataLakeStoreAccount(location: _location);
                _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }

            // Delete account
            public static void DeleteAccount()
            {
                _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
            }

            // List all ADLS accounts within the subscription
            public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
            {
                var response = _adlsClient.Account.List(_adlsAccountName);
                var accounts = new List<DataLakeStoreAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlsClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }

            // Upload a file
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            // Concatenate files
            public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
            {
                _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(string path)
            {
                return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(string directoryPath)
            {
                return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
            }

            // Download file
            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Append to file
            public static void AppendToFile(string path, string content)
            {
                var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

                _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
            }

            // Create a directory
            public static void CreateDirectory(string path)
            {
                _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
            }
        }
    }


## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=sep16_HO1-->


