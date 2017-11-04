---
title: "Uruchomienie zadania usługi Hadoop przy użyciu bazy danych rozwiązania Cosmos Azure i usługi HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak uruchomić proste zadanie Hive, Pig i MapReduce z bazy danych rozwiązania Cosmos Azure i usłudze Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c8789f08a37466862120dda88a0bce7da3e9a91
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="Azure Cosmos DB-HDInsight"></a>Uruchom zadanie Apache Hive, Pig lub Hadoop przy użyciu bazy danych rozwiązania Cosmos Azure i usługi HDInsight
W tym samouczku przedstawiono sposób uruchamiania [Apache Hive][apache-hive], [Apache Pig][apache-pig], i [Apache Hadoop] [ apache-hadoop] zadań MapReduce w usłudze Azure HDInsight z łącznikiem usługi Hadoop DB rozwiązania Cosmos. Łącznik usługi Hadoop DB rozwiązania cosmos umożliwia rozwiązania Cosmos bazy danych, które będą działać jako źródło i ujście zadań Hive, Pig i MapReduce. W tym samouczku będzie używać rozwiązania Cosmos bazy danych jako źródła danych i docelowy zadania usługi Hadoop.

Po ukończeniu tego samouczka, będziesz mieć możliwość odpowiedzieć na następujące pytania:

* Jak załadować dane z rozwiązania Cosmos bazy danych przy użyciu zadania Hive, Pig lub MapReduce?
* Sposób przechowywania danych w bazie danych rozwiązania Cosmos przy użyciu zadania Hive, Pig lub MapReduce?

Zalecamy rozpoczęcie pracy od obejrzenia poniższego klipu wideo, w którym przeprowadzana za pomocą zadania Hive za pomocą rozwiązania Cosmos bazę danych i usługi HDInsight.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Następnie wróć do tego artykułu, gdy otrzymasz pełne szczegóły na uruchamianie zadania usługi analiza danych DB rozwiązania Cosmos w.

> [!TIP]
> Ten samouczek zakłada, że masz wcześniejszego doświadczenia w używaniu platformy Apache Hadoop, Hive i Pig. Jeśli jesteś nowym użytkownikiem usługi Apache Hadoop, Hive i Pig, zaleca się znaleźć [dokumentację Apache Hadoop][apache-hadoop-doc]. W tym samouczku przyjęto założenie, czy masz doświadczenie z rozwiązania Cosmos bazy danych, a konto DB rozwiązania Cosmos. Jeśli jesteś nowym użytkownikiem rozwiązania Cosmos bazy danych lub nie masz konta rozwiązania Cosmos bazy danych, można znaleźć na naszych [wprowadzenie] [ getting-started] strony.
>
>

Nie masz czasu na ukończenie tego samouczka i po prostu chcesz uzyskać skrypty programu PowerShell pełny przykład Hive, Pig i MapReduce? Nie ma problemu, pobierz je [tutaj][hdinsight-samples]. Pobieranie zawiera również pliki hql, pig i java dla tych przykładów.

## <a name="NewestVersion"></a>Najnowsza wersja
<table border='1'>
    <tr><th>Wersja łącznika usługi Hadoop</th>
        <td>1.2.0</td></tr>
    <tr><th>Identyfikator Uri skryptu</th>
        <td>https://portalcontent.blob.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Data modyfikacji</th>
        <td>04/26/2016</td></tr>
    <tr><th>HDInsight obsługiwane wersje</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Dziennik zmian</th>
        <td>Zaktualizowano rozwiązania Cosmos Azure DB Java SDK 1.6.0</br>
            Dodano obsługę dla kolekcji partycjonowanych jako źródło i ujście</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Wymagania wstępne
Przed wykonaniem instrukcji zawartych w tym samouczku, upewnij się, że masz następujące czynności:

* Konto rozwiązania Cosmos bazy danych, bazy danych i kolekcji dokumentów wewnątrz. Aby uzyskać więcej informacji, zobacz [wprowadzenie do korzystania z bazy danych rozwiązania Cosmos][getting-started]. Importować przykładowe dane do Twojego konta DB rozwiązania Cosmos z [narzędzia importu DB rozwiązania Cosmos][import-data].
* Przepustowość. Odczyty i zapisy z HDInsight będą liczone kierunku jednostek żądania przeznaczonym dla kolekcji.
* Pojemność dodatkowe procedurę składowaną w ramach każdej wyjściowego kolekcji. Procedury składowane są używane do przesyłania wynikowy dokumentów.
* Wydajność wynikowy dokumentów z zadań Hive, Pig i MapReduce.
* [*Opcjonalnie*] pojemności dla dodatkowych kolekcji.

> [!WARNING]
> Aby uniknąć tworzenia nowej kolekcji podczas wszystkich zadań, możesz drukowanie wyników do stdout, Zapisz dane wyjściowe z kontenerem WASB lub określ już istniejącą kolekcję. W przypadku określania istniejącą kolekcję, zostanie utworzona nowych dokumentów w kolekcji i już istniejących dokumentów będzie dotyczył tylko, jeśli istnieje konflikt w *identyfikatorów*. **Łącznik automatycznie spowoduje zastąpienie istniejących dokumentów konflikt identyfikatorów**. Tę funkcję można wyłączyć przez ustawienie opcji upsert na wartość false. Jeśli występuje konflikt upsert ma wartość false, zakończy się niepowodzeniem zadania usługi Hadoop; raportowania błędów konflikt identyfikatora.
>
>

## <a name="ProvisionHDInsight"></a>Krok 1: Tworzenie nowego klastra usługi HDInsight
W tym samouczku używana akcji skryptu z portalu Azure do dostosowania z klastrem usługi HDInsight. W tym samouczku używamy portalu Azure do tworzenia klastra usługi HDInsight. Aby uzyskać instrukcje dotyczące sposobu używania poleceń cmdlet programu PowerShell lub zestawu .NET SDK usługi HDInsight, zapoznaj się z [HDInsight dostosować klastry za pomocą akcji skryptu] [ hdinsight-custom-provision] artykułu.

1. Zaloguj się do [portalu Azure][azure-portal].
2. Kliknij przycisk **+ nowy** górnej części nawigacji po lewej stronie, wyszukaj **HDInsight** w górnym pasku wyszukiwania w nowym bloku.
3. **HDInsight** opublikowanych przez **Microsoft** pojawi się w górnej części wyników. Kliknij go, a następnie kliknij przycisk **Utwórz**.
4. W nowym klastrze HDInsight utworzyć bloku, wprowadź użytkownika **nazwy klastra** i wybierz **subskrypcji** chcesz udostępnić ten zasób w obszarze.

    <table border='1'>
        <tr><td>Nazwa klastra</td><td>Nazwa klastra.<br/>
Nazwa DNS musi uruchomić i kończyć się znakiem alfanumeryczne i może zawierać kresek.<br/>
Pole musi być ciągiem od 3 do 63 znaków.</td></tr>
        <tr><td>Nazwa subskrypcji</td>
            <td>Jeśli masz więcej niż jedną subskrypcję platformy Azure, wybierz subskrypcję, która będzie obsługiwać z klastrem usługi HDInsight. </td></tr>
    </table>
5.Kliknij przycisk **wybierz typ klastra** i ustaw następujące właściwości do określonych wartości.

    <table border='1'>
        <tr><td>Typ klastra</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Warstwy klastrów</td><td><strong>Standardowa</strong></td></tr>
        <tr><td>System operacyjny</td><td><strong>Windows</strong></td></tr>
        <tr><td>Wersja</td><td>Najnowsza wersja</td></tr>
    </table>

    Teraz, kliknij przycisk **wybierz**.

    ![Podaj szczegóły klastra Hadoop HDInsight][image-customprovision-page1]
6. Polecenie **poświadczenia** można ustawić Twoje poświadczenia logowania i zdalnego dostępu. Wybierz użytkownika **nazwa użytkownika logowania klastra** i **hasło logowania klastra**.

    Zdalne w klastrze, wybierz opcję *tak* w dolnej części bloku i podaj nazwę użytkownika i hasło.
7. Polecenie **źródła danych** można ustawić lokalizacji głównej dla dostępu do danych. Wybierz **metodę wyboru** i określenia już istniejącego konta magazynu lub Utwórz nową.
8. W tym samym bloku określić **domyślny kontener** i **lokalizacji**. I kliknij przycisk **wybierz**.

   > [!NOTE]
   > Wybierz lokalizację bliski DB rozwiązania Cosmos regionu konta w celu poprawy wydajności
   >
   >
9. Polecenie **cennik** wybierz liczbę i rodzaj węzłach. Możesz zachować konfigurację domyślną i skalować liczbę węzłów procesu roboczego na dalszym etapie.
10. Kliknij przycisk **konfiguracji opcjonalnej**, następnie **skryptu akcje** w bloku konfiguracji opcjonalnej.

     W akcji skryptu wprowadź następujące informacje, aby dostosować z klastrem usługi HDInsight.

     <table border='1'>
         <tr><th>Właściwość</th><th>Wartość</th></tr>
         <tr><td>Nazwa</td>
             <td>Określ nazwę akcji skryptu.</td></tr>
         <tr><td>Identyfikator URI skryptu</td>
             <td>Określ identyfikator URI do skryptu, które jest wywoływane, aby dostosować klastra.</br></br>
Wprowadź: </br> <strong>https://portalcontent.blob.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</td></tr>
         <tr><td>Główny</td>
             <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na węźle Head.</br></br>
             <strong>Zaznacz to pole wyboru</strong>.</td></tr>
         <tr><td>Węzeł roboczy</td>
             <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na węźle procesu roboczego.</br></br>
             <strong>Zaznacz to pole wyboru</strong>.</td></tr>
         <tr><td>Dozorca</td>
             <td>Kliknij pole wyboru, aby uruchomić skrypt programu PowerShell na dozorcy.</br></br>
             <strong>Zbędny</strong>.
             </td></tr>
         <tr><td>Parametry</td>
             <td>Określ parametry, jeśli jest to wymagane przez skrypt.</br></br>
             <strong>Brak parametrów potrzebne</strong>.</td></tr>
     </table>
11.Tworzyć nowe **grupy zasobów** lub użyj istniejącej grupy zasobów w ramach Twojej subskrypcji platformy Azure.
12. Teraz, sprawdź **Przypnij do pulpitu nawigacyjnego** śledzenia jej wdrożenia, a następnie kliknij przycisk **Utwórz**!

## <a name="InstallCmdlets"></a>Krok 2: Instalowanie i konfigurowanie programu Azure PowerShell
1. Zainstaluj program Azure PowerShell. Instrukcje można znaleźć [tutaj][powershell-install-configure].

   > [!NOTE]
   > Można również tylko dla zapytań programu Hive, można użyć edytora online Hive w usłudze HDInsight. Aby to zrobić, zaloguj się do [Azure Portal][azure-portal], kliknij przycisk **HDInsight** w okienku po lewej stronie, aby wyświetlić listę z klastrami HDInsight. Kliknij klaster ma być uruchamianie zapytań Hive, a następnie kliknij przycisk **konsoli zapytania**.
   >
   >
2. Otwórz zintegrowane środowisko obsługi skryptów programu Azure PowerShell:

   * Na komputerze z systemem Windows 8 lub Windows Server 2012 lub nowszym można użyć wbudowanych wyszukiwania. Na ekranie startowym wpisz **programu powershell ise** i kliknij przycisk **Enter**.
   * Na komputerze z systemem w wersji starszej niż Windows 8 lub Windows Server 2012 Użyj Start menu. W Start menu wpisz **wiersza polecenia** w polu wyszukiwania, a następnie na liście wyników, kliknij przycisk **wiersza polecenia**. W wierszu polecenia wpisz **powershell_ise** i kliknij przycisk **Enter**.
3. Dodaj konto platformy Azure.

   1. W okienku konsoli wpisz **Add-AzureAccount** i kliknij przycisk **Enter**.
   2. Wpisz adres e-mail skojarzony z subskrypcją platformy Azure i kliknij przycisk **Kontynuuj**.
   3. Wpisz hasło dla subskrypcji platformy Azure.
   4. Kliknij przycisk **Zaloguj**.
4. Poniższy diagram identyfikuje ważne części środowiska skryptów programu PowerShell Azure.

    ![Diagram do programu Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Krok 3: Uruchomienie zadania Hive za pomocą rozwiązania Cosmos bazę danych i usługi HDInsight
> [!IMPORTANT]
> Wszystkie zmienne określają < > musi być wypełnione przy użyciu ustawień konfiguracji.
>
>

1. Ustaw następujące zmienne w okienku skrypt programu PowerShell.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Zacznijmy konstruowanie ciągu zapytania. Firma Microsoft będzie Napisz zapytanie Hive, które przyjmuje sygnatury czasowe wygenerowany przez system (_ts) i unikatowe identyfikatory (_rid) z kolekcji bazy danych Azure rozwiązania Cosmos wszystkie dokumenty, zlicza wszystkie dokumenty za minutę, a następnie zapisuje wyniki do nowej kolekcji bazy danych Azure rozwiązania Cosmos.</p>

    <p>Najpierw utwórz tabeli programu Hive z naszych kolekcji bazy danych Azure rozwiązania Cosmos. W okienku skrypt programu PowerShell Dodaj poniższy fragment kodu <strong>po</strong> fragment kodu z #1. Upewnij się, że obejmują przycinania t parametr opcjonalny DocumentDB.query naszych dokumentów, aby tylko _ts i _rid.</p>

   > [!NOTE]
   > **Nazewnictwo DocumentDB.inputCollections nie błędu.** Tak, firma Microsoft Zezwalaj na dodawanie wielu kolekcji jako dane wejściowe: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. Następnie utwórz tabeli programu Hive dla kolekcji danych wyjściowych. Właściwości dokumentu danych wyjściowych będzie miesiąc, dzień, godzinę, minutę i łączna liczba wystąpień.

   > [!NOTE]
   > **Jeszcze ponownie nazewnictwa DocumentDB.outputCollections nie błędu.** Tak, firma Microsoft Zezwalaj na dodawanie wielu kolekcji jako dane wyjściowe: </br>
   > "*DocumentDB.outputCollections*"="*\<Nazwa kolekcji usługi DocumentDB w danych wyjściowych 1\>*,*\<Nazwa kolekcji usługi DocumentDB w danych wyjściowych 2\>*" </br> Nazwy kolekcji są oddzielone bez spacji, za pomocą tylko jednego przecinka. </br></br>
   > Dokumenty będą okrężnego rozproszonych w wielu kolekcjach. Wsadowe dokumentów będą przechowywane w jednej kolekcji, a następnie drugi partii dokumenty będą przechowywane w następnej kolekcji i tak dalej.
   >
   >

       # Create a Hive table for the output data to DocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Na koniec załóżmy zgodności dokumentów przez miesiąc, dzień, godzinę i minutę i Wstaw wyniki do danych wyjściowych tabelę programu Hive.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Dodaj poniższy fragment kodu skryptu do tworzenia definicji zadania Hive z poprzedniego zapytania.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Można również użyć przełącznika - plik, aby określić plik skryptu HiveQL w systemie plików HDFS.
4. Dodaj poniższy fragment kodu, aby oszczędzić czas rozpoczęcia i przesłać zadania technologii Hive.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Dodaj następujące polecenie, aby czekać na ukończenie zadania Hive.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Dodaj następujący kod do wyjścia standardowego i godziny rozpoczęcia i zakończenia wydruku.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Uruchom** nowy skrypt! **Kliknij przycisk** przycisk zielony execute.
8. Sprawdzenie wyników. Zaloguj się do [portalu Azure][azure-portal].

   1. Kliknij przycisk <strong>Przeglądaj</strong> w okienku po lewej stronie. </br>
   2. Kliknij przycisk <strong>wszystko</strong> w prawym górnym rogu panelu przeglądania. </br>
   3. Znajdź i kliknij <strong>kont DB rozwiązania Cosmos Azure</strong>. </br>
   4. Następnie znaleźć użytkownika <strong>konto bazy danych Azure rozwiązania Cosmos</strong>, następnie <strong>bazy danych DB rozwiązania Cosmos Azure</strong> i <strong>Azure rozwiązania Cosmos DB kolekcji</strong> skojarzone z tą kolekcją dane wyjściowe określone zapytanie Hive.</br>
   5. Na koniec kliknij <strong>Eksploratora dokumentów</strong> pod <strong>Developer Tools</strong>.</br></p>

   Zobaczysz wyniki zapytania Hive.

   ![Wyniki zapytania hive][image-hive-query-results]

## <a name="RunPig"></a>Krok 4: Uruchom zadanie Pig przy użyciu rozwiązania Cosmos bazę danych i usługi HDInsight
> [!IMPORTANT]
> Wszystkie zmienne określają < > musi być wypełnione przy użyciu ustawień konfiguracji.
>
>

1. Ustaw następujące zmienne w okienku skrypt programu PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Zacznijmy konstruowanie ciągu zapytania. Firma Microsoft będzie Napisz zapytanie Pig, które przyjmuje sygnatury czasowe wygenerowany przez system (_ts) i unikatowe identyfikatory (_rid) z kolekcji bazy danych Azure rozwiązania Cosmos wszystkie dokumenty, zlicza wszystkie dokumenty za minutę, a następnie zapisuje wyniki do nowej kolekcji bazy danych Azure rozwiązania Cosmos.</p>
    <p>Po pierwsze ładowanie dokumentów z rozwiązania Cosmos bazy danych do usługi HDInsight. W okienku skrypt programu PowerShell Dodaj poniższy fragment kodu <strong>po</strong> fragment kodu z #1. Upewnij się dodać zapytanie bazy danych DocumentDB do opcjonalny parametr zapytania usługi DocumentDB przyciąć naszych dokumentów, aby tylko _ts i _rid.</p>

   > [!NOTE]
   > Tak, firma Microsoft Zezwalaj na dodawanie wielu kolekcji jako dane wejściowe: </br>
   > "*\<Nazwa kolekcji usługi DocumentDB w danych wejściowych 1\>*,*\<Nazwa kolekcji usługi DocumentDB w danych wejściowych 2\>*"</br> Nazwy kolekcji są oddzielone bez spacji, za pomocą tylko jednego przecinka. </b>
   >
   >

    Dokumenty będą okrężnego rozproszonych w wielu kolekcjach. Wsadowe dokumentów będą przechowywane w jednej kolekcji, a następnie drugi partii dokumenty będą przechowywane w następnej kolekcji i tak dalej.

        # Load data from Cosmos DB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Następnie umożliwia zgodności dokumentów przez miesiąc, dzień, godzinę, minutę i łączna liczba wystąpień.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Ponadto umożliwia przechowywanie wyników w naszej nowej kolekcji danych wyjściowych.

   > [!NOTE]
   > Tak, firma Microsoft Zezwalaj na dodawanie wielu kolekcji jako dane wyjściowe: </br>
   > "\<Nazwa kolekcji usługi DocumentDB w danych wyjściowych 1\>,\<Nazwa kolekcji usługi DocumentDB w danych wyjściowych 2\>"</br> Nazwy kolekcji są oddzielone bez spacji, za pomocą tylko jednego przecinka.</br>
   > Dokumenty będą okrężnego rozproszonych w wielu kolekcjach. Wsadowe dokumentów będą przechowywane w jednej kolekcji, a następnie drugi partii dokumenty będą przechowywane w następnej kolekcji i tak dalej.
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Dodaj poniższy fragment kodu skryptu do tworzenia definicji zadania Pig z poprzedniego zapytania.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Można również użyć przełącznika - plik, aby określić plik skryptu Pig na system plików HDFS.
6. Dodaj poniższy fragment kodu, aby zaoszczędzić czas rozpoczęcia i przesłać zadania programu Pig.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Dodaj następujące polecenie, aby czekać na ukończenie zadania Pig.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Dodaj następujący kod do wyjścia standardowego i godziny rozpoczęcia i zakończenia wydruku.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Uruchom** nowy skrypt! **Kliknij przycisk** przycisk zielony execute.
10. Sprawdzenie wyników. Zaloguj się do [portalu Azure][azure-portal].

    1. Kliknij przycisk <strong>Przeglądaj</strong> w okienku po lewej stronie. </br>
    2. Kliknij przycisk <strong>wszystko</strong> w prawym górnym rogu panelu przeglądania. </br>
    3. Znajdź i kliknij <strong>kont DB rozwiązania Cosmos Azure</strong>. </br>
    4. Następnie znaleźć użytkownika <strong>konto bazy danych Azure rozwiązania Cosmos</strong>, następnie <strong>bazy danych DB rozwiązania Cosmos Azure</strong> i <strong>Azure rozwiązania Cosmos DB kolekcji</strong> skojarzone z tą kolekcją dane wyjściowe określone w zapytaniu Pig.</br>
    5. Na koniec kliknij <strong>Eksploratora dokumentów</strong> pod <strong>Developer Tools</strong>.</br></p>

    Wyniki zapytania Pig będą widoczne.

    ![Wyniki zapytania pig][image-pig-query-results]

## <a name="RunMapReduce"></a>Krok 5: Uruchomienie zadania MapReduce przy użyciu bazy danych rozwiązania Cosmos Azure i usługi HDInsight
1. Ustaw następujące zmienne w okienku skrypt programu PowerShell.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Firma Microsoft będzie wykonywać zadania MapReduce, które zlicza liczbę wystąpień dla każdej właściwości dokumentu z kolekcji bazy danych Azure rozwiązania Cosmos. Dodaj następujący fragment kodu skryptu **po** fragment powyżej.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar jest dostarczany z niestandardowej instalacji łącznika usługi Hadoop DB rozwiązania Cosmos.
   >
   >
3. Dodaj następujące polecenie, aby przesłać zadania MapReduce.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Oprócz definicji zadania MapReduce musisz także podać nazwę klastra usługi HDInsight, której chcesz uruchomić zadanie MapReduce, a poświadczenia. AzureHDInsightJob rozpoczęcia jest wywołania asynchronicznego. Aby sprawdzić ukończenia zadania, użyj *AzureHDInsightJob oczekiwania* polecenia cmdlet.
4. Dodaj następujące polecenie, aby sprawdzić błędy dotyczące uruchomienie zadania MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Uruchom** nowy skrypt! **Kliknij przycisk** przycisk zielony execute.
6. Sprawdzenie wyników. Zaloguj się do [portalu Azure][azure-portal].

   1. Kliknij przycisk <strong>Przeglądaj</strong> w okienku po lewej stronie.
   2. Kliknij przycisk <strong>wszystko</strong> w prawym górnym rogu panelu przeglądania.
   3. Znajdź i kliknij <strong>kont DB rozwiązania Cosmos Azure</strong>.
   4. Następnie znaleźć użytkownika <strong>konto bazy danych Azure rozwiązania Cosmos</strong>, następnie <strong>bazy danych DB rozwiązania Cosmos Azure</strong> i <strong>Azure rozwiązania Cosmos DB kolekcji</strong> skojarzone z tą kolekcją dane wyjściowe określone w zadania MapReduce.
   5. Na koniec kliknij <strong>Eksploratora dokumentów</strong> pod <strong>Developer Tools</strong>.

      Zobaczysz wyników zadania MapReduce.

      ![MapReduce wyników zapytania][image-mapreduce-query-results]

## <a name="NextSteps"></a>Następne kroki
Gratulacje! Właśnie uruchomiono pierwszy zadań Hive, Pig i MapReduce przy użyciu bazy danych rozwiązania Cosmos Azure i usługi HDInsight.

Mamy Otwórz powierzając jej ich konserwację naszych łącznika usługi Hadoop. Jeśli chcesz, można współtworzyć na [GitHub][github].

Aby dowiedzieć się więcej, zobacz następujące artykuły:

* [Tworzenie aplikacji Java z usługi Documentdb][documentdb-java-application]
* [Tworzenie programów Java MapReduce dla platformy Hadoop w usłudze HDInsight][hdinsight-develop-deploy-java-mapreduce]
* [Rozpocząć korzystanie z usługi Hadoop przy użyciu Hive w usłudze HDInsight do analizy użycia przenośnych słuchawki][hdinsight-get-started]
* [Korzystać z usługi MapReduce z usługą HDInsight][hdinsight-use-mapreduce]
* [Korzystanie z programu Hive z usługą HDInsight][hdinsight-use-hive]
* [Korzystanie z języka Pig z usługą HDInsight][hdinsight-use-pig]
* [Dostosowywanie klastrów usługi HDInsight przy użyciu akcji skryptu][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]:../hdinsight/hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:../hdinsight/hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight/hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-pig]:../hdinsight/hadoop/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
