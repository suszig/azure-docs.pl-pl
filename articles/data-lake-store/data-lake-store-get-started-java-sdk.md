<properties
   pageTitle="Korzystanie z zestawu Java SDK usługi Data Lake Store w celu projektowania aplikacji | Azure"
   description="Korzystanie z zestawu Java SDK usługi Azure Data Lake Store w celu projektowania aplikacji"
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
   ms.date="08/18/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu języka Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw SDK .NET](data-lake-store-get-started-net-sdk.md)
- [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
- [Interfejs API REST](data-lake-store-get-started-rest-api.md)
- [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak używać zestawu Java SDK usługi Azure Data Lake Store, aby utworzyć konto usługi Azure Data Lake i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake, zobacz temat [Usługa Azure Data Lake Store](data-lake-store-overview.md).

## Zestaw Java SDK usługi Azure Data Lake Store

Poniższe linki udostępniają lokalizację pobierania zestawu Java SDK usługi Data Lake Store oraz dokumentacji dotyczącej zestawu Java SDK. Do celów tego samouczka nie trzeba pobierać zestawu SDK ani wykonywać czynności opisanych w dokumencie referencyjnym. Te linki mają tylko charakter informacyjny.

* Kod źródłowy zestawu Java SDK dla usługi Data Lake Store jest dostępny w serwisie [GitHub](https://github.com/Azure/azure-sdk-for-java).
* Dokumentacja dotycząca zestawu Java SDK dla usługi Data Lake Store jest dostępna na stronie [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/).

## Wymagania wstępne

* Zestaw Java Development Kit (JDK) 8 (z użyciem języka Java w wersji 1.8).
* IntelliJ lub inne odpowiednie środowisko programistyczne Java. Jest to opcjonalne, ale zalecane. W poniższych instrukcjach jest używane środowisko IntelliJ.
* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Włączenie subskrypcji platformy Azure** w celu uzyskania publicznej wersji zapoznawczej usługi Data Lake Store. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).
* **Utworzenie aplikacji usługi Azure Active Directory**. Istnieją dwa sposoby uwierzytelniania z użyciem usługi Azure Active Directory — **interakcyjne** i **nieinterakcyjne**. W zależności od sposobu uwierzytelniania należy spełnić różne wymagania wstępne.
    * **Uwierzytelnianie interaktywne** — w usłudze Azure Active Directory należy utworzyć **aplikację klienta natywnego**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Uzyskaj **identyfikator klienta** i **identyfikator URI przekierowania** dla aplikacji.
        - Ustaw uprawnienia delegowane.

    * **Uwierzytelnianie nieinteraktywne** (używane w tym artykule) — w usłudze Azure Active Directory należy utworzyć **aplikację sieci Web**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Pobierz **identyfikator klienta**, **klucz tajny klienta** oraz **identyfikator URI przekierowania** dla aplikacji
        - Ustaw uprawnienia delegowane.
        - Przypisz aplikację usługi Azure Active Directory do roli. Rola może być na poziomie zakresu, na którym chcesz nadać uprawnienie do aplikacji usługi Azure Active Directory. Na przykład możesz przypisać aplikację na poziomie subskrypcji lub na poziomie grupy zasobów.

    Instrukcje dotyczące pobierania tych wartości, ustawiania uprawnień i przypisywania ról znajdują się w temacie [Tworzenie aplikacji i nazwy głównej usługi Active Directory przy użyciu portalu](../resource-group-create-service-principal-portal.md).

## Jak wykonać uwierzytelnianie przy użyciu usługi Azure Active Directory?

Poniższy fragment kodu zawiera kod uwierzytelniania **nieinteraktywnego**, w przypadku którego aplikacja udostępnia własne poświadczenia.

Przed wykonaniem kroków tego samouczka należy zezwolić aplikacji na tworzenie zasobów na platformie Azure. **Zdecydowanie zalecamy**, aby do celów związanych z tym samouczkiem nadać tej aplikacji uprawnienia współautora dotyczące nowej, nieużywanej i pustej grupy zasobów w subskrypcji platformy Azure.

## Tworzenie aplikacji Java

1. Otwórz środowisko IntelliJ i utwórz nowy projekt języka Java przy użyciu szablonu **Command Line App** (Aplikacja wiersza polecenia). Wykonaj kroki kreatora, aby utworzyć projekt.

2. Otwórz opcję **Plik** -> **Struktura projektu** -> **Moduły** (w obszarze Ustawienia projektu) -> **Zależności** -> **+** -> **Biblioteka** -> **Z programu Maven**.

3. Wyszukaj następujące pakiety Maven i dodaj je do projektu:

    * com.microsoft.azure:azure-mgmt-datalake-store:1.0.0-beta1.2
    * com.microsoft.azure:azure-mgmt-datalake-store-uploader:1.0.0-beta1.2
    * com.microsoft.azure:azure-client-authentication:1.0.0-beta2

4. W lewym okienku przejdź do pozycji **src**, **main**, **java**, **\<package name>**, a następnie otwórz plik **Main.java** i zastąp istniejący blok kodu poniższym kodem. Ponadto podaj wartości parametrów wywoływanych we fragmencie kodu, takich jak **localFolderPath**, **DATA-LAKE-STORE-NAME** i **RESOURCE-GROUP-NAME** oraz zastąp symbole zastępcze **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** i **SUBSCRIPTION-ID** informacjami na temat subskrypcji i jej katalogu Azure Active Directory. Aby uzyskać informacje o tym, jak znaleźć te informacje, zobacz [podręcznik tworzenia nazw głównych usług platformy Azure](../resource-group-authenticate-service-principal.md).

    Ten kod przechodzi przez procesy tworzenia konta usługi Data Lake Store, tworzenia plików w magazynie, łączenia plików, pobierania pliku i usunięcia konta.

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreAccountManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreFileSystemManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.store.uploader.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;

        public class Main {
            final static String ADLS_ACCOUNT_NAME = <DATA-LAKE-STORE-NAME>;
            final static String RESOURCE_GROUP_NAME = "<RESOURCE-GROUP-NAME>";
            final static String LOCATION = "East US 2";
            final static String TENANT_ID = "<TENANT-ID>";
            final static String SUBSCRIPTION_ID =  "<SUBSCRIPTION-ID>";
            final static String CLIENT_ID = "<CLIENT-ID>";
            final static String CLIENT_SECRET = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

            private static DataLakeStoreAccountManagementClientImpl _adlsClient;
            private static DataLakeStoreFileSystemManagementClientImpl _adlsFileSystemClient;

            public static void main(String[] args) throws Exception {
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(CLIENT_ID, TENANT_ID, CLIENT_SECRET, null);
                SetupClients(creds);

                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");

                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", SUBSCRIPTION_ID));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.accounts().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.name());
                }
                WaitForNewline("Account(s) displayed.", "Uploading file.");

                // Upload a file to Data Lake Store: file1.csv
                UploadFile(localFolderPath + "file1.csv", "/file1.csv");
                WaitForNewline("File uploaded.", "Appending newline.");

                // Append newline to file1.csv
                AppendToFile("/file1.csv", "\r\n");
                WaitForNewline("Newline appended.", "Creating file.");

                // Create a new file in Data Lake Store: file2.csv
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("File created.", "Concatenating files.");

                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");

                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlsClient.withSubscriptionId(SUBSCRIPTION_ID);
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }

            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.withLocation(LOCATION);

                _adlsClient.accounts().create(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME, adlsParameters);
            }

            // Create file
            public static void CreateFile(String path) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path);
            }

            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path, bytesContents, force);
            }

            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().append(ADLS_ACCOUNT_NAME, path, bytesContents);
            }

            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().concat(ADLS_ACCOUNT_NAME, destFilePath, srcFilePaths);
            }

            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().delete(ADLS_ACCOUNT_NAME, filePath);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().getFileStatus(ADLS_ACCOUNT_NAME, path).getBody().fileStatus();
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().listFileStatus(ADLS_ACCOUNT_NAME, directoryPath).getBody().fileStatuses().fileStatus();
            }

            // Upload file
            public static void UploadFile(String srcPath, String destPath) throws Exception {
                UploadParameters parameters = new UploadParameters(srcPath, destPath, ADLS_ACCOUNT_NAME);
                FrontEndAdapter frontend = new DataLakeStoreFrontEndAdapterImpl(ADLS_ACCOUNT_NAME, _adlsFileSystemClient);
                DataLakeStoreUploader uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.execute();
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, AdlsErrorException {
                InputStream stream = _adlsFileSystemClient.fileSystems().open(ADLS_ACCOUNT_NAME, srcPath).getBody();

                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();

                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }

                pWriter.println(fileContents);
                pWriter.close();
            }

            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.accounts().delete(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME);
            }
        }


6. Uruchom aplikację. Postępuj zgodnie z monitami, aby uruchomić aplikację i ukończyć jej działanie.

## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=sep16_HO1-->


