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
   ms.date="05/12/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu języka Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Zestaw Java SDK](data-lake-store-get-started-java-sdk.md)
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
* **Utworzenie aplikacji usługi Azure Active Directory**. Istnieją dwa sposoby uwierzytelniania z użyciem usługi Azure Active Directory — **interaktywny** i **nieinteraktywny**. W zależności od sposobu uwierzytelniania należy spełnić różne wymagania wstępne.
    * **Uwierzytelnianie interaktywne** — w usłudze Azure Active Directory należy utworzyć **aplikację klienta natywnego**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Uzyskaj **identyfikator klienta** i **identyfikator URI przekierowania** dla aplikacji.
        - Ustaw uprawnienia delegowane.

    * **Uwierzytelnianie nieinteraktywne** (używane w tym artykule) — w usłudze Azure Active Directory należy utworzyć **aplikację sieci Web**. Po utworzeniu aplikacji pobierz następujące wartości związane z tą aplikacją.
        - Pobierz **identyfikator klienta**, **klucz tajny klienta** oraz **identyfikator URI przekierowania** dla aplikacji.
        - Ustaw uprawnienia delegowane.
        - Przypisz aplikację usługi Azure Active Directory do roli. Rola może być na poziomie zakresu, na którym chcesz nadać uprawnienie do aplikacji usługi Azure Active Directory. Na przykład możesz przypisać aplikację na poziomie subskrypcji lub na poziomie grupy zasobów. 

    Instrukcje dotyczące pobierania tych wartości, ustawiania uprawnień i przypisywania ról znajdują się w temacie [Tworzenie aplikacji i nazwy głównej usługi Active Directory przy użyciu portalu](../resource-group-create-service-principal-portal.md).

## Jak wykonać uwierzytelnianie przy użyciu usługi Azure Active Directory?

Poniższy fragment kodu zawiera kod uwierzytelniania **nieinteraktywnego**, w przypadku którego aplikacja udostępnia własne poświadczenia.

Przed wykonaniem kroków tego samouczka należy zezwolić aplikacji na tworzenie zasobów na platformie Azure. **Zdecydowanie zalecamy**, aby do celów związanych z tym samouczkiem nadać tej aplikacji uprawnienia współautora dotyczące nowej, nieużywanej i pustej grupy zasobów w subskrypcji platformy Azure.

## Tworzenie aplikacji Java

1. Otwórz środowisko IntelliJ i utwórz nowy projekt języka Java przy użyciu szablonu **Command Line App** (Aplikacja wiersza polecenia). Wykonaj kroki kreatora, aby utworzyć projekt.

2. Kliknij prawym przyciskiem myszy projekt po lewej stronie ekranu, a następnie kliknij pozycję **Add Framework Support** (Dodaj obsługę struktury). Wybierz pozycję **Maven** i kliknij przycisk **OK**.

3. Otwórz nowo utworzony plik **„pom.xml”** i dodaj poniższy fragment tekstu między tagami **\</version >** i **\</project >**:

    >[AZURE.NOTE] Ten krok jest tymczasowy do momentu, aż zestaw SDK usługi Azure Data Lake Store zostanie udostępniony w programie Maven. Ten artykuł zostanie zaktualizowany po udostępnieniu zestawu SDK w programie Maven. Wszystkie przyszłe aktualizacje tego zestawu SDK będą dostępne za pośrednictwem programu Maven.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. Wybierz kolejno pozycje **File** (Plik), **Settings** (Ustawienia) i **Build, Execution, and Deployment** (Kompilacja, wykonywanie i wdrażanie). Rozwiń węzeł **Build Tools** (Narzędzia kompilacji), rozwiń węzeł **Maven**, a następnie rozwiń pozycję **Importing** (Importowanie). Zaznacz pole wyboru **Import Maven projects automatically** (Automatycznie importuj projekty Maven). Kliknij przycisk **Apply** (Zastosuj), a następnie kliknij przycisk **OK**.

5. W lewym okienku przejdź do pozycji **src**, **main**, **java**, **\<package name>**, a następnie otwórz plik **Main.java** i zastąp istniejący blok kodu poniższym kodem. Ponadto podaj wartości parametrów wywoływanych we fragmencie kodu, takie jak **localFolderPath**, **_adlsAccountName** i **_resourceGroupName** oraz zastąp symbole zastępcze **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** i **SUBSCRIPTION-ID**.

    Ten kod przechodzi przez procesy tworzenia konta usługi Data Lake Store, tworzenia plików w magazynie, łączenia plików, pobierania pliku i usunięcia konta.

        package com.company;
        
        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");
        
                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Account(s) displayed.", "Creating files.");
        
                // Create two files in Data Lake Store: file1.csv and file2.csv
                CreateFile("/file1.csv", "123,abc", true);
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("Files created.", "Concatenating files.");
        
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
        
                _adlsClient.setSubscriptionId(_subId);
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
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }
        
            // Create file
            public static void CreateFile(String path) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path);
            }
        
            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().append(_adlsAccountName, path, bytesContents);
            }
        
            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().concat(_adlsAccountName, destFilePath, srcFilePaths);
            }
        
            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(_adlsAccountName, filePath);
            }
        
            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().getFileStatus(_adlsAccountName, path).getBody().getFileStatus();
            }
        
            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().listFileStatus(_adlsAccountName, directoryPath).getBody().getFileStatuses().getFileStatus();
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(_adlsAccountName, srcPath).getBody();
        
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
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
            }
        }


6. Uruchom aplikację. Postępuj zgodnie z monitami, aby uruchomić aplikację i ukończyć jej działanie.

## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Jun16_HO2-->


