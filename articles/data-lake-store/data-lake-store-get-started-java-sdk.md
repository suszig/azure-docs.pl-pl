---
title: "Korzystanie z zestawu SDK Java do programowania aplikacji w usłudze Azure Data Lake Store| Microsoft Docs"
description: "Użyj zestawu SDK Java usługi Azure Data Lake Store, aby utworzyć konto usługi Data Lake Store i wykonywać podstawowe operacje w tej usłudze"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 3e4302e7c5b54a46994eccaa8cf9ecbb0a84446d
ms.lasthandoff: 03/03/2017


---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu języka Java
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Dowiedz się, jak używać zestawu SDK Java usługi Azure Data Lake Store, aby wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych itp. Aby uzyskać więcej informacji o usłudze Data Lake, zobacz temat [Usługa Azure Data Lake Store](data-lake-store-overview.md).

Dostęp do dokumentów interfejsu API zestawu SDK Java usługi Azure Data Lake Store można uzyskać w [dokumentacji interfejsu API języka Java usługi Azure Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Wymagania wstępne
* Zestaw Java Development Kit (JDK 7 lub nowszy, korzystający z języka Java w wersji 1.7 lub nowszej)
* Konto usługi Azure Data Lake Store. Postępuj zgodnie z instrukcjami w temacie [Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą witryny Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Ten samouczek używa programu Maven na potrzeby zależności między kompilacją i projektem. Chociaż istnieje możliwość kompilacji bez używania systemu kompilacji, takiego jak Maven lub Gradle, systemy te znacznie ułatwiają zarządzanie zależnościami.
* (Opcjonalnie) Wtyczka [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) lub [Eclipse](https://www.eclipse.org/downloads/) przypominająca środowisko IDE lub podobna.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>W jaki sposób uwierzytelniać za pomocą usługi Azure Active Directory?
W tym samouczku do pobrania tokenu usługi Azure Active Directory (uwierzytelnianie „service to service”) używamy klucza tajnego klienta aplikacji Azure AD. Ten token służy do utworzenia obiektu klienta usługi Data Lake Store w celu wykonywania operacji na plikach i operacji na katalogach. Aby uzyskać informacje na temat uwierzytelniania w usłudze Azure Data Lake Store za pomocą klucza tajnego klienta, wykonamy następujące czynności wysokiego poziomu:

1. Utworzenie aplikacji sieci Web Azure AD
2. Pobranie identyfikatora klienta, klucza tajnego klienta i punktu końcowego tokenu dla aplikacji sieci Web Azure AD.
3. Skonfigurowanie dostępu dla aplikacji sieci Web Azure AD do pliku/folderu usługi Data Lake Store, do którego chcesz uzyskać dostęp z tworzonej aplikacji Java.

Aby uzyskać instrukcje na temat sposobu wykonania tych czynności, zobacz [Tworzenie aplikacji usługi Active Directory](data-lake-store-authenticate-using-active-directory.md).

Usługa Azure Active Directory oferuje również inne opcje pobierania tokenu. Do wyboru jest wiele różnych mechanizmów uwierzytelniania odpowiednich dla danego scenariusza, na przykład aplikacja działająca w przeglądarce, aplikacja rozproszona jako aplikacja tradycyjna albo aplikacja serwera uruchomiona lokalnie lub na maszynie wirtualnej platformy Azure. Można również wybrać różne rodzaje poświadczeń, takie jak hasła, certyfikaty, uwierzytelnianie 2-etapowe itd. Ponadto usługa Azure Active Directory umożliwia synchronizowanie lokalnych użytkowników usługi Active Directory z chmurą. Aby uzyskać szczegółowe informacje, zobacz [Scenariusze uwierzytelniania dla usługi Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java
Przykładowy kod dostępny [w usłudze GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) przeprowadzi Cię przez proces tworzenia plików w magazynie, łączenia plików, pobierania pliku i usuwania niektórych plików z magazynu. Ta część artykułu przeprowadzi Cię przez najważniejsze fragmenty kodu.

1. Utwórz projekt Maven za pomocą polecenia [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) w wierszu polecenia lub w środowisku IDE. Aby uzyskać instrukcje dotyczące sposobu tworzenia projektu Java przy użyciu wtyczki IntelliJ, zobacz [tutaj](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Aby uzyskać instrukcje dotyczące sposobu tworzenia projektu Java przy użyciu wtyczki Eclipse, zobacz [tutaj](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Dodaj poniższe zależności do Twojego pliku **pom.xml** programu Maven. Dodaj poniższy fragment tekstu między tagiem **\</version>** i tagiem **\</project>**:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.4</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    Pierwszą zależnością jest użycie zestawu SDK usługi Data Lake Store (`azure-data-lake-store-sdk`) z repozytorium maven. Drugą zależnością (`slf4j-nop`) jest określenie, jaka struktura rejestrowania ma być używana dla tej aplikacji. Zestaw SDK usługi Data Lake Store używa fasady rejestrowania [slf4j](http://www.slf4j.org/), która umożliwia wybór spośród wielu popularnych struktur rejestrowania, takich jak log4j, rejestrowanie Java, logback itp. lub brak rejestrowania. W tym przykładzie wyłączymy rejestrowanie, dlatego używamy powiązania **slf4j-nop**. Aby używać innych opcji rejestrowania w aplikacji, zobacz [tutaj](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Dodawanie kodu aplikacji
Istnieją trzy główne fragmenty kodu.

1. Uzyskiwanie tokenu usługi Azure Active Directory
2. Token umożliwia utworzenie klienta usługi Data Lake Store.
3. Klient usługi Data Lake Store służy do wykonywania operacji.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Krok 1. Uzyskaj token usługi Azure Active Directory
Zestaw SDK usługi Data Lake Store udostępnia wygodne metody zarządzania tokenami zabezpieczającymi, które są potrzebne do komunikacji z kontem usługi Data Lake Store. Jednak zestaw SDK nie zmusza do używania tylko tych metod. Możesz również używać jakichkolwiek innych sposobów uzyskiwania tokenu, na przykład przy użyciu [zestawu SDK usługi Azure Active Directory](https://github.com/AzureAD/azure-activedirectory-library-for-java) lub własnego kodu niestandardowego.

Aby uzyskać token dla utworzonej wcześniej aplikacji sieci Web usługi Active Directory za pomocą zestawu SDK usługi Data Lake Store, użyj jednej z podklas klasy `AccessTokenProvider` (w poniższym przykładzie użyto podklasy `ClientCredsTokenProvider`). Dostawca tokenu zapisuje w pamięci poświadczenia użyte do uzyskania tokenu i automatycznie odnawia token, gdy zbliża się czas jego wygaśnięcia. Można utworzyć własne podklasy klasy `AccessTokenProvider`, tak aby tokeny były uzyskiwane przez kod klienta, ale na razie skorzystajmy po prostu z podklasy dostępnej w zestawie SDK.

Zastąp wartość **FILL-IN-HERE** (Wypełnij tutaj) rzeczywistymi wartościami dla aplikacji sieci Web usługi Azure Active Directory.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Krok 2. Utwórz obiekt klienta usługi Azure Data Lake Store (ADLStoreClient)
Utworzenie obiektu [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) wymaga określenia nazwy konta usługi Data Lake Store oraz dostawcy tokenu, który został wygenerowany w ostatnim kroku. Pamiętaj, że nazwa konta usługi Data Lake Store musi być w pełni kwalifikowaną nazwą domeny. Na przykład zastąp ciąg **FILL-IN-HERE** (Wypełnij tutaj) czymś w rodzaju **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Krok 3. Używaj obiektu ADLStoreClient do wykonywania operacji na plikach i katalogach
Poniższy kod zawiera przykładowe fragmenty niektórych typowych operacji. Aby zobaczyć inne operacje, możesz zapoznać się z pełną [dokumentacją interfejsu API zestawu SDK Java usługi Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/) obiektu **ADLStoreClient**.

Pamiętaj, że pliki są odczytywane i zapisywane za pomocą standardowych strumieni Java. Oznacza to, że możesz umieścić dowolne strumienie Java na wierzchu strumieni usługi Data Lake Store, aby korzystać ze standardowych funkcji języka Java (np. strumieni drukowania sformatowanych danych wyjściowych albo dowolnych strumieni kompresji lub szyfrowania na potrzeby dodatkowych funkcji itp.).

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is the same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Krok4. Skompiluj i uruchom aplikację
1. Aby przeprowadzić uruchomienie z poziomu środowiska IDE, znajdź i naciśnij przycisk **Run** (Uruchom). Aby przeprowadzić uruchomienie z poziomu programu Maven, użyj polecenia [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).
2. Aby wygenerować autonomiczny plik JAR, który można uruchomić z poziomu wiersza polecenia, skompiluj plik JAR zawierający wszystkie zależności, używając [wtyczki zestawu Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Przykładowy sposób wykonania tych czynności znajduje się w pliku pom.xml w [przykładowym kodzie źródłowym w serwisie github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml).

## <a name="next-steps"></a>Następne kroki
* [Zapoznawanie się z dokumentacją JavaDoc dotyczącą zestawu SDK Java](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


