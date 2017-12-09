---
title: Analizowanie danych czujnika z systemu Apache Storm i bazy danych HBase | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak nawiązać połączenie z siecią wirtualną systemu Apache Storm. Za pomocą Storm HBase do przetwarzania danych czujnika z Centrum zdarzeń i wizualizację go z D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/19/2017
ms.author: larryfr
ms.openlocfilehash: 8c8cda26f2b9b564dee330e4883ec12f39feb652
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analizowanie danych czujnika z systemu Apache Storm, Centrum zdarzeń i bazy danych HBase w usłudze HDInsight (Hadoop)

Dowiedz się, jak używać systemu Apache Storm w usłudze HDInsight do przetwarzania danych czujnika z Centrum zdarzeń platformy Azure. Dane są następnie przechowywane do bazy danych Apache HBase w usłudze HDInsight i wizualizowane za pomocą D3.js.

Szablon usługi Azure Resource Manager używany w tym dokumencie przedstawia sposób tworzenia wielu zasobów platformy Azure w grupie zasobów. Szablon tworzy sieci wirtualnej platformy Azure, dwa klastry usługi HDInsight (Storm oraz bazy danych HBase) i aplikacji sieci Web platformy Azure. Node.js stosowania pulpit nawigacyjny sieci web w czasie rzeczywistym jest wdrażana automatycznie do aplikacji sieci web.

> [!NOTE]
> Informacje przedstawione w tym dokumencie i przykład, w tym dokumencie wymagają 3,6 wersji usługi HDInsight.
>
> Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

## <a name="architecture"></a>Architektura

![diagram architektury](./media/apache-storm-sensor-data-analysis/devicesarchitecture.png)

W tym przykładzie składa się z następujących składników:

* **Usługa Azure Event Hubs**: zawiera dane, które mają być zbierane z czujników.
* **STORM w usłudze HDInsight**: udostępnia w czasie rzeczywistym przetwarzania danych z Centrum zdarzeń.
* **Baza danych HBase w usłudze HDInsight**: udostępnia trwałym magazynem danych NoSQL dla danych po przetworzeniu przez Storm.
* **Usługi sieci wirtualnej Azure**: umożliwia bezpieczną komunikację między Storm w usłudze HDInsight i bazy danych HBase w usłudze hdinsight.
  
  > [!NOTE]
  > Sieć wirtualna jest wymagana podczas korzystania z bazy danych HBase w języku Java klienta interfejsu API. Nie jest widoczne za pośrednictwem publicznego bramy w przypadku klastrów HBase. Instalowanie klastrów HBase i Storm do tej samej sieci wirtualnej umożliwia klastra Storm (lub inne systemy w sieci wirtualnej) bezpośredni dostęp do bazy danych HBase przy użyciu interfejsu API klienta.

* **Witryny sieci Web pulpitu nawigacyjnego**: przykładowy pulpit nawigacyjny, który wykresy danych w czasie rzeczywistym.
  
  * Witryna sieci Web jest zaimplementowany w środowisku Node.js.
  * [Użyciu biblioteki Socket.IO](http://socket.io/) jest używany w czasie rzeczywistym komunikacji między topologii Storm i witryny sieci Web.
    
    > [!NOTE]
    > Przy użyciu biblioteki Socket.io komunikacji jest szczegóły implementacji. Możesz użyć dowolnej architektury komunikacji, takie jak Websocket raw lub SignalR.

  * [D3.js](http://d3js.org/) służy do danych, które są wysyłane do witryny sieci Web programu graph.

> [!IMPORTANT]
> Dwa klastry są wymagane, ponieważ nie ma żadnych obsługiwana metoda tworzenia jednego klastra usługi HDInsight Storm i bazy danych HBase.

Topologia odczytuje dane z Centrum zdarzeń za pomocą `org.apache.storm.eventhubs.spout.EventHubSpout` klasy i zapisuje dane do bazy danych HBase przy użyciu `org.apache.storm.hbase.bolt.HBaseBolt` klasy. Komunikacja z witryny sieci Web odbywa się przy użyciu [client.java użyciu biblioteki socket.io](https://github.com/nkzawa/socket.io-client.java).

Poniższy diagram wyjaśniono układ topologii:

![diagram topologii](./media/apache-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Ten diagram jest uproszczona prezentacja topologii. Dla każdej partycji w Centrum zdarzeń jest tworzone wystąpienie każdego składnika. Te wystąpienia są dystrybuowane między węzłami w klastrze, a dane są przesyłane między nimi w następujący sposób:
> 
> * Dane z spout Analizator jest równoważone.
> * Dane z analizatora do pulpitu nawigacyjnego i HBase są grupowane według Identyfikatora urządzenia tak, aby komunikaty z tego samego urządzenia zawsze przepływać do tego składnika.

### <a name="topology-components"></a>Składniki topologii

* **Elementów Spout Centrum zdarzeń**: spout jest podana jako część systemu Apache Storm w wersji 0.10.0 lub nowszej.
  
  > [!NOTE]
  > Spout Centrum zdarzeń, w tym przykładzie wymaga platformy Storm w klastrze usługi HDInsight w wersji 3.5 lub 3,6.

* **ParserBolt.java**: dane, które są emitowane przez elementy spout są nieprzetworzone dane JSON, a czasami więcej niż jedno zdarzenie jest emitowany naraz. Dany element bolt odczytuje dane emitowane przez elementy spout i analizuje komunikat JSON. Elementy bolt następnie emituje dane jako spójnej kolekcji, która zawiera wiele pól.
* **DashboardBolt.java**: ten składnik pokazano, jak wysyłać dane w czasie rzeczywistym do pulpitu nawigacyjnego sieci web przy użyciu biblioteki klienta użyciu biblioteki Socket.io dla języka Java.
* **nie hbase.yaml**: definicja topologii używany podczas pracy w trybie lokalnym. Nie używa składników bazy danych HBase.
* **z hbase.yaml**: definicja topologii używana podczas uruchamiania topologii w klastrze. Wykorzystuje ona składników bazy danych HBase.
* **dev.Properties**: informacje o konfiguracji do spout Centrum zdarzeń, HBase bolt i elementów pulpitu nawigacyjnego.

## <a name="prepare-your-environment"></a>Przygotowywanie środowiska

Przed skorzystaniem z tego przykładu, należy przygotować swoje Środowisko deweloperskie. Należy także utworzyć usługi Azure Event Hub, który jest używany w tym przykładzie.

Potrzebne są następujące elementy zainstalowane w środowisku programowania:

* [Node.js](http://nodejs.org/): używany do podglądu pulpitu nawigacyjnego sieci web lokalnie na środowiska deweloperskiego.
* [Java i JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): stworzono topologii Storm.
* [Maven](http://maven.apache.org/what-is-maven.html): używane do tworzenia i skompilować projekt.
* [Git](http://git-scm.com/): używany do pobierania projektu z usługi GitHub.
* **SSH** klienta: umożliwia łączenie się z klastrami HDInsight opartych na systemie Linux. Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Aby utworzyć Centrum zdarzeń, wykonaj czynności w [tworzenia Centrum zdarzeń](../../event-hubs/event-hubs-create.md) dokumentu.

> [!IMPORTANT]
> Zapisz nazwę Centrum zdarzeń, przestrzeń nazw i klucz dla RootManageSharedAccessKey. Te informacje jest używany do konfigurowania topologii Storm.

Nie trzeba klastra usługi HDInsight. Kroki opisane w tym dokumencie Podaj szablonu usługi Azure Resource Manager, która tworzy zasoby potrzebne w tym przykładzie. Szablon tworzy następujące zasoby:
 
* Sieć wirtualna platformy Azure
* Storm w klastrze usługi HDInsight (opartych na systemie Linux dwóch węzłów procesu roboczego)
* Baza danych HBase w klastrze usługi HDInsight (opartych na systemie Linux dwóch węzłów procesu roboczego)
* Aplikacji sieci Web Azure, który jest hostem sieci web pulpitu nawigacyjnego

## <a name="download-and-configure-the-project"></a>Pobierz i konfigurowanie projektu

Użyj następującego polecenia można pobrać projektu z usługi GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po zakończeniu wykonywania polecenia, należy mieć następującą strukturę katalogów:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> Ten dokument nie może pełne szczegóły zawarte w tym przykładzie kodu. Jednak kod jest całkowicie oznaczone jako.

Aby skonfigurować projekt, aby odczytać z Centrum zdarzeń, otwórz `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` pliku i Dodaj informacje Centrum zdarzeń do następujące wiersze:

```bash
eventhub.policy.key: the_key_for_RootManageSharedAccessKey
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Kompilowanie i testowanie lokalnie

> [!IMPORTANT]
> Lokalnie przy użyciu topologii wymaga działającego środowiska deweloperskiego Storm. Aby uzyskać więcej informacji, zobacz [Konfigurowanie środowiska projektowego Storm](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) w serwisie Apache.org.

Przed przeprowadzeniem jej testów, należy uruchomić Pulpit nawigacyjny, aby wyświetlić dane wyjściowe topologii i generować dane mają być przechowywane w Centrum zdarzeń.

> [!IMPORTANT]
> Składnik HBase Ta topologia nie jest aktywne, podczas testowania lokalnie. Interfejsu API języka Java klastra HBase nie jako dostępne spoza sieci wirtualnej Azure, która zawiera klastrów.

### <a name="start-the-web-application"></a>Uruchom aplikację sieci web

1. Otwórz wiersz polecenia i przejdź do `hdinsight-eventhub-example/dashboard`. Aby zainstalować zależności wymagane przez aplikację sieci web, użyj następującego polecenia:
   
    ```bash
    npm install
    ```

2. Aby uruchomić aplikację sieci web, użyj następującego polecenia:
   
    ```bash
    node server.js
    ```
   
    Zobaczysz komunikat podobny do następującego tekstu:
   
        Server listening at port 3000

3. Otwórz przeglądarkę sieci web, a następnie wprowadź `http://localhost:3000/` jako adres. Wyświetlane są stronę podobną do poniższej ilustracji:
   
    ![pulpit nawigacyjny sieci Web](./media/apache-storm-sensor-data-analysis/emptydashboard.png)
   
    Ten wiersz polecenia pozostaw otwarte. Po zakończeniu testowania, użyj klawiszy Ctrl-C, aby zatrzymać serwer sieci web.

### <a name="generate-data"></a>Generowanie danych

> [!NOTE]
> Kroki opisane w tej sekcji użycia środowiska Node.js, dzięki czemu mogą być używane na dowolnej platformie. Inne przykłady języka można znaleźć `SendEvents` katalogu.

1. Otwórz nowy wiersz, powłokę lub terminalu i przejdź do `hdinsight-eventhub-example/SendEvents/nodejs`. Aby zainstalować zależności wymagane przez aplikację, należy użyć następującego polecenia:

    ```bash
    npm install
    ```

2. Otwórz `app.js` plik w edytorze tekstów i Dodaj informacje o Centrum zdarzeń uzyskanymi wcześniej:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'Your-eventhub-namespace';
    // Event Hub Name
    var hubname ='Your-eventhub-name';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'RootManageSharedAccessKey';
    var my_key = 'Your-Key';
    ```
   
   > [!NOTE]
   > W tym przykładzie przyjęto założenie, że używasz `RootManageSharedAccessKey` na dostęp do Centrum zdarzeń.

3. Użyj następującego polecenia, aby wstawić nowe wpisy w Centrum zdarzeń:
   
    ```bash
    node app.js
    ```
   
    Pojawić kilka wierszy danych wyjściowych, które zawierają dane wysyłane do Centrum zdarzeń:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a>Skompilować i uruchomić topologii

1. Otwórz nowy wiersz polecenia i przejdź do `hdinsight-eventhub-example/TemperatureMonitor`. Aby skompilować i pakiet topologii, użyj następującego polecenia: 

    ```bash
    mvn clean package
    ```

2. Aby uruchomić topologii w trybie lokalnym, użyj następującego polecenia:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`Topologia jest uruchamiany w trybie lokalnym.
    * `--filter`używa `dev.properties` plik, aby wypełnić parametrów w definicji topologii.
    * `resources/no-hbase.yaml`używa `no-hbase.yaml` definicji topologii.
 
   Po rozpoczęciu topologii odczytuje wpisów z Centrum zdarzeń, a następnie wysyła je do pulpitu nawigacyjnego na komputerze lokalnym. Powinny pojawić się wiersze są wyświetlane na pulpicie nawigacyjnym w sieci web, podobnie do poniższej ilustracji:
   
    ![pulpit nawigacyjny z danymi](./media/apache-storm-sensor-data-analysis/datadashboard.png)

2. Pulpit nawigacyjny jest uruchomiona, użyj `node app.js` polecenie poprzednie kroki, aby wysłać nowe dane do usługi Event Hubs. Ponieważ wartości temperatury są losowo generowany, wykres należy zaktualizować pokazanie dużych zmian w temperatury.
   
   > [!NOTE]
   > Musi być w **hdinsight-eventhub — przykład/SendEvents/Nodejs** katalogu, korzystając z `node app.js` polecenia.

3. Po zweryfikowaniu, że aktualizacje pulpitu nawigacyjnego, Zatrzymaj topologię za pomocą klawiszy Ctrl + C. Ctrl + C umożliwia także zatrzymać serwer lokalnej sieci web.

## <a name="create-a-storm-and-hbase-cluster"></a>Tworzenie klastra Storm oraz bazy danych HBase

Kroki opisane w tej sekcji Użyj [szablonu usługi Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy.md) do utworzenia klastra sieci wirtualnej platformy Azure i Storm oraz bazy danych HBase w sieci wirtualnej. Szablon także tworzy aplikacji sieci Web platformy Azure i wdraża kopię pulpitu nawigacyjnego do niego.

> [!NOTE]
> Sieć wirtualna jest używana, co topologii uruchomionych na klaster Storm może komunikować się bezpośrednio z klastra HBase przy użyciu interfejsu API języka Java HBase.

Szablon usługi Resource Manager używany w tym dokumencie znajduje się w publicznym kontenerze obiektów blob w **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Kliknij poniższy przycisk, aby zalogować się do platformy Azure i otwórz szablon usługi Resource Manager w portalu Azure.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/apache-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z **wdrożenie niestandardowe** sekcji, wprowadź następujące wartości:
   
    ![Parametry HDInsight](./media/apache-storm-sensor-data-analysis/parameters.png)
   
   * **Podstawowa nazwa klastra**: Ta wartość jest używana jako nazwa podstawowa dla Storm oraz klastrów HBase. Na przykład wprowadzenie **abc** jest tworzony klaster Storm o nazwie **storm abc** i klaster HBase o nazwie **hbase abc**.
   * **Nazwa użytkownika logowania klastra**: nazwa użytkownika administratora w przypadku klastrów Storm oraz bazy danych HBase.
   * **Hasło logowania klastra**: hasła użytkownika administratora w przypadku klastrów Storm oraz bazy danych HBase.
   * **Nazwa użytkownika SSH**: użytkownika SSH do tworzenia klastrów Storm oraz bazy danych HBase.
   * **Hasło SSH**: hasło dla użytkownika SSH w przypadku klastrów Storm oraz bazy danych HBase.
   * **Lokalizacja**: utworzony klastrów w regionie.
     
     Kliknij przycisk **OK**, aby zapisać parametry.

3. Użyj **podstawy** sekcji, aby utworzyć grupę zasobów lub wybierz istniejący.
4. W **lokalizacja grupy zasobów** listy rozwijanej wybierz polecenie tej samej lokalizacji co zostanie wybrany do **lokalizacji** parametru w **ustawienia** sekcji.
5. Przeczytaj warunki i postanowienia, a następnie wybierz **akceptuję warunki i postanowienia, o których wspomniano**.
6. Ponadto sprawdź **Przypnij do pulpitu nawigacyjnego** , a następnie wybierz **zakupu**. Tworzenie klastrów trwa około 20 minut.

Po utworzeniu zasobów wyświetlane są informacje o grupie zasobów.

![Grupa zasobów dla sieci wirtualnej i klastrów](./media/apache-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Należy zauważyć, że nazwy klastrów usługi HDInsight są **nazwę BAZOWĄ storm** i **nazwę BAZOWĄ hbase**, gdzie nazwę BAZOWĄ jest podana nazwa do szablonu. Te nazwy można użyć w kolejnym kroku, podczas nawiązywania połączenia z klastrów. Należy również zauważyć, że nazwa witryny pulpitu nawigacyjnego **pulpitu nawigacyjnego nazwę bazową**. Ta wartość jest używana w dalszej części tego dokumentu.

## <a name="configure-the-dashboard-bolt"></a>Skonfiguruj bolt pulpitu nawigacyjnego

Aby wysyłać dane do pulpitu nawigacyjnego wdrożony jako aplikacja sieci web, należy zmodyfikować następujący wiersz w `dev.properties`pliku:

```yaml
dashboard.uri: http://localhost:3000
```

Zmień `http://localhost:3000` do `http://BASENAME-dashboard.azurewebsites.net` i Zapisz plik. Zastąp **nazwę BAZOWĄ** o nazwie podstawowej podany w poprzednim kroku. Umożliwia także grupy zasobów utworzone wcześniej, aby wybrać pulpitu nawigacyjnego i wyświetlić adres URL.

## <a name="create-the-hbase-table"></a>Tworzenie tabeli HBase

Aby przechowywać dane w bazie danych HBase, możemy utworzyć tabelę. Wstępne tworzenie zasobów, które wymaga Storm do zapisu, jako próby utworzenia zasobów z wewnątrz platformy Storm topologii może spowodować wiele wystąpień w trakcie tworzenia tego samego zasobu. Tworzenie zasobów spoza topologii i używać systemu Storm do odczytu/zapisu i analiz.

1. Używanie protokołu SSH do nawiązania połączenia klastra HBase przy użyciu protokołu SSH użytkownika i hasło do szablonu podczas tworzenia klastra. Na przykład, jeśli połączenie przy użyciu `ssh` polecenia, należy użyć następującej składni:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Zastąp `sshuser` przy użyciu nazwy użytkownika SSH podana podczas tworzenia klastra. Zastąp `clustername` z nazwą klastra HBase.

2. W sesji SSH należy uruchomić powłoki HBase.
   
    ```bash
    hbase shell
    ```
   
    Po załadowaniu powłoki, zobacz `hbase(main):001:0>` wiersza.

3. Z poziomu powłoki HBase wprowadź następujące polecenie, aby utworzyć tabelę do przechowywania danych czujników:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Sprawdź, czy tabela została utworzona za pomocą następującego polecenia:
   
    ```hbase
    scan 'SensorData'
    ```
   
    To polecenie zwróci informacje podobne do poniższego przykładu, wskazującą, czy 0 wierszy w tabeli.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Wprowadź `exit` do Wyjdź z powłoki HBase:

## <a name="configure-the-hbase-bolt"></a>Skonfiguruj HBase bolt

Można zapisać do bazy danych HBase z klastra Storm, należy podać szczegóły konfiguracji klastra HBase HBase bolt.

1. Aby pobrać dozorcy kworum dla klastra HBase, użyj jednej z poniższych przykładach:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Element `your_HDInsight_cluster_name` należy zastąpić nazwą klastra usługi HDInsight. Aby uzyskać więcej informacji na temat instalowania `jq` narzędzie, zobacz [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Po wyświetleniu monitu wprowadź hasło dla nazwy logowania administratora usługi HDInsight.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name'
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Zastąp "your_HDInsight_cluster_name o nazwie z klastrem usługi HDInsight. Po wyświetleniu monitu wprowadź hasło dla nazwy logowania administratora usługi HDInsight.
    >
    > W tym przykładzie wymaga programu Azure PowerShell. Aby uzyskać więcej informacji na temat używania programu Azure PowerShell, zobacz [wprowadzenie do programu Azure PowerShell](https://docs.microsoft.com/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    Informacje zwracane przez te przykłady jest podobny do następującego tekstu:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Te informacje są używane przez Storm do komunikowania się z klastra HBase.

2. Modyfikowanie `dev.properties` plik i dodać informacje kworum dozorcy do następującego:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Tworzenie pakietu i wdrożenia rozwiązania w usłudze HDInsight

W środowisku projektowania wykonaj następujące kroki do wdrożenia topologii Storm do klastra storm.

1. Z `TemperatureMonitor` katalogu, użyj następujące polecenie, aby wykonać nową kompilację i utworzyć JAR pakietu z projektu:
   
        mvn clean package
   
    To polecenie tworzy plik o nazwie `TemperatureMonitor-1.0-SNAPSHOT.jar in the `docelowego "katalogu projektu.

2. Użyj połączenia usługi, aby przekazać `TemperatureMonitor-1.0-SNAPSHOT.jar` i `dev.properties` pliki do klastra Storm. W poniższym przykładzie Zastąp `sshuser` użytkownika SSH podana podczas tworzenia klastra, i `clustername` o nazwie klastra Storm. Po wyświetleniu monitu wprowadź hasło dla użytkownika SSH.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Może upłynąć kilka minut, aby przekazać pliki.

    Aby uzyskać więcej informacji na temat używania `scp` i `ssh` poleceń z usługą HDInsight, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

3. Po przekazaniu pliku nawiązać połączenia z klastrem Storm przy użyciu protokołu SSH.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Zastąp `sshuser` przy użyciu nazwy użytkownika SSH. Zastąp `clustername` z nazwą klastra Storm.

4. Aby uruchomić topologii, użyj następującego polecenia w sesji SSH:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`przesyła topologii z usługą Nimbus, której przesłanie go do przełożonego węzłów w klastrze.
    * `--filter`używa `dev.properties` plik, aby wypełnić parametrów w definicji topologii.
    * `-R /with-hbase.yaml`używa `with-hbase.yaml` topologii uwzględniony w pakiecie.

5. Po uruchomieniu topologii, otwórz przeglądarkę Aby witryna sieci Web została opublikowana na platformie Azure, a następnie użyj `node app.js` polecenie, aby wysyłać dane do Centrum zdarzeń. Powinien zostać wyświetlony pulpit nawigacyjny sieci web, zaktualizować, aby wyświetlić informacje.
   
    ![pulpit nawigacyjny](./media/apache-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Wyświetlanie danych HBase

Aby podłączyć się do bazy danych HBase i sprawdź, czy dane zostały zapisane w tabeli, wykonaj następujące kroki:

1. Używanie protokołu SSH do nawiązania połączenia klastra HBase.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Zastąp `sshuser` przy użyciu nazwy użytkownika SSH. Zastąp `clustername` z nazwą klastra HBase.

2. W sesji SSH należy uruchomić powłoki HBase.
   
    ```bash
    hbase shell
    ```
   
    Po załadowaniu powłoki, zobacz `hbase(main):001:0>` wiersza.

3. Widok wierszy z tabeli:
   
    ```hbase
    scan 'SensorData'
    ```
   
    To polecenie zwraca informacje podobne do następującego, co oznacza, że istnieje danych w tabeli.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Ta operacja skanowania zwraca maksymalnie 10 wierszy z tabeli.

## <a name="delete-your-clusters"></a>Usuwanie klastrów

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Aby usunąć klastrów, magazynu i aplikacji sieci web w tym samym czasie, Usuń grupę zasobów, który je zawiera.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej przykładów dotyczących topologii Storm z usługą HDInsight, zobacz [przykładowe topologie dla systemu Storm w usłudze HDInsight](apache-storm-example-topology.md)

Aby uzyskać więcej informacji na temat systemu Apache Storm, zobacz [Apache Storm](https://storm.incubator.apache.org/) lokacji.

Aby uzyskać więcej informacji dotyczących bazy danych HBase w usłudze HDInsight, zobacz [HBase HDInsight Przegląd](../hbase/apache-hbase-overview.md).

Aby uzyskać więcej informacji o użyciu biblioteki Socket.io, zobacz [użyciu biblioteki socket.io](http://socket.io/) lokacji.

Aby uzyskać więcej informacji o D3.js, zobacz [D3.js — danych na dokumentach](http://d3js.org/).

Aby uzyskać informacje o tworzeniu topologii w języku Java, zobacz [Java opracowywania topologii Apache STORM w usłudze HDInsight](apache-storm-develop-java-topology.md).

Aby uzyskać informacji o tworzeniu topologii w środowisku .NET, zobacz [topologii opracowywania C# dla Apache Storm w usłudze HDInsight przy użyciu programu Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
