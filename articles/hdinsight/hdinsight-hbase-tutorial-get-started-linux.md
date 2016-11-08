---
title: 'Samouczek HBase: wprowadzenie do klastrów HBase opartych na systemie Linux na platformie Hadoop | Microsoft Docs'
description: Postępuj zgodnie z tym samouczkiem bazy danych HBase, aby rozpocząć korzystanie z bazy danych Apache HBase na platformie Hadoop w usłudze HDInsight. Utwórz tabele z poziomu powłoki HBase i wykonuj zapytania przy użyciu aplikacji Hive.
keywords: apache hbase,hbase,hbase shell,hbase tutorial
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun

ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/25/2016
ms.author: jgao

---
# Samouczek HBase: rozpoczęcie korzystania z bazy danych Apache HBase na platformie Hadoop opartej na systemie Linux w usłudze HDInsight
[!INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Dowiedz się, jak utworzyć klaster HBase w usłudze HDInsight i tabele bazy danych HBase oraz jak wykonywać zapytania dotyczące tabel za pomocą aplikacji Hive. Aby uzyskać ogólne informacje o bazie danych HBase, zobacz [Przegląd bazy danych HBase w usłudze HDInsight][hdinsight-hbase-overview].

Informacje przedstawione w tym dokumencie dotyczą klastrów usługi HDInsight opartych na systemie Linux. Aby uzyskać informacje o klastrach opartych na systemie Windows, przełącz kartę za pomocą wyboru kart w górnej części strony.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Wymagania wstępne
Przed rozpoczęciem korzystania z tego samouczka HBase należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Bezpieczna powłoka (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [Program curl](http://curl.haxx.se/download.html).

### Wymagania dotyczące kontroli dostępu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## Tworzenie klastra HBase
W poniższej procedurze do utworzenia klastra HBase jest używany szablon usługi Azure Resource Manager. Aby zapoznać się z parametrami używanymi w tej procedurze oraz innymi metodami tworzenia klastra, zobacz temat [Tworzenie opartych na systemie Linux klastrów Hadoop w usłudze HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w witrynie Azure Portal. Szablon znajduje się w publicznym kontenerze obiektów blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. W bloku **Parametry** wprowadź następujące informacje:
   
   * **ClusterName**: wprowadź nazwę tworzonego klastra HBase.
   * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to **admin**.
   * **Nazwa użytkownika i hasło SSH**: domyślna nazwa użytkownika to **sshuser**.  Tę nazwę można zmienić.
     
     Inne parametry są opcjonalne.  
     
     Każdy klaster zależy od konta Magazynu obiektów Blob platformy Azure. Po usunięciu klastra dane pozostają zachowane na koncie magazynu. Domyślna nazwa konta magazynu klastra to nazwa klastra z dołączonym ciągiem „store”. Jest ona umieszczona w kodzie w sekcji zmiennych szablonu.
3. Kliknij przycisk **OK**, aby zapisać parametry.
4. W bloku **Wdrożenie niestandardowe** kliknij pole listy rozwijanej **Grupa zasobów**, a następnie kliknij pozycję **Nowa**, aby utworzyć nową grupę zasobów.  Grupa zasobów jest kontenerem, który grupuje klaster, zależne konto magazynu oraz inne powiązane zasoby.
5. Kliknij opcję **Postanowienia prawne**, a następnie kliknij przycisk **Utwórz**.
6. Kliknij przycisk **Utwórz**. Utworzenie klastra trwa około 20 minut.

> [!NOTE]
> Po usunięciu klastra HBase można utworzyć inny klaster HBase za pomocą tego samego domyślnego kontenera obiektów blob. Nowy klaster przejmie tabele bazy danych HBase utworzone w oryginalnym klastrze. Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase przed usunięciem klastra.
> 
> 

## Tworzenie tabel i wstawianie danych
Protokół SSH umożliwia łączenie się z klastrami HBase i korzystanie z powłoki HBase w celu tworzenia tabel bazy danych HBase, wstawiania danych i wykonywania zapytań. Aby uzyskać informacje na temat korzystania z protokołu SSH w systemach Linux, Unix, OS X i Windows, zobacz tematy [Korzystanie z SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight w systemach Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oraz [Korzystanie z SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight w systemie Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

Dla większości użytkowników dane są wyświetlane w formacie tabelarycznym:

![hbase hdinsight dane tabelaryczne][img-hbase-sample-data-tabular]

W bazie danych HBase, która jest implementacją BigTable, te same dane wyglądają następująco:

![hdinsight hbase dane bigtable][img-hbase-sample-data-bigtable]

Stanie się to bardziej zrozumiałe po zakończeniu następnej procedury.  

**Korzystanie z powłoki HBase**

1. Z poziomu bezpiecznej powłoki (SSH) uruchom następujące polecenie:
   
        hbase shell
2. Utwórz bazę danych HBase z dwiema rodzinami kolumn:
   
        create 'Contacts', 'Personal', 'Office'
        list
3. Wstaw dowolne dane:
   
        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'
   
    ![powłoka hbase hdinsight hadoop][img-hbase-shell]
4. Pobierz pojedynczy wiersz:
   
        get 'Contacts', '1000'
   
    Wyświetlone zostaną te same wyniki, co w przypadku polecenia scan, ponieważ istnieje tylko jeden wiersz.
   
    Aby uzyskać więcej informacji na temat schematu tabeli HBase, zobacz [Wprowadzenie do projektu schematu HBase][hbase-schema]. Więcej poleceń HBase można znaleźć w [Podręczniku bazy danych Apache HBase][hbase-quick-start].
5. Wyjdź z powłoki:
   
        exit

**Ładowanie danych zbiorczo do tabeli kontaktów HBase**

Baza danych HBase obsługuje kilka metod ładowania danych do tabel.  Aby uzyskać więcej informacji, zobacz temat [Ładowanie zbiorcze](http://hbase.apache.org/book.html#arch.bulk.load).

Przykładowy plik danych został przekazany do publicznego kontenera obiektów blob *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Plik danych ma następującą zawartość:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Możesz utworzyć plik tekstowy i przesłać go na swoje konto magazynu. Aby uzyskać instrukcje, zobacz temat [Przekazywanie danych dotyczących zadań Hadoop w usłudze HDInsight][hdinsight-upload-data].

> [!NOTE]
> W tej procedurze jest używana tabela kontaktów HBase utworzona w poprzedniej procedurze.
> 
> 

1. Z poziomu bezpiecznej powłoki (SSH) uruchom następujące polecenie, aby przekształcić plik danych do postaci StoreFiles i zapisać go w ścieżce względnej określonej przez parametr Dimporttsv.bulk.output:.  Jeśli jest otwarta powłoka HBase, użyj polecenia exit, aby z niej wyjść.
   
        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
2. Uruchom następujące polecenie, aby przekazać dane z katalogu /example/data/storeDataFileOutput do tabeli HBase:
   
        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
3. Możesz otworzyć powłokę HBase i użyć polecenia scan w celu wyświetlenia zawartości tabeli.

## Uruchamianie zapytania bazy danych HBase przy użyciu programu Hive
Korzystając z programu Hive, można wykonywać zapytania dotyczące danych w tabelach HBase. W tej sekcji zostanie utworzona tabela programu Hive odwzorowująca dane w tabeli HBase, która będzie używana do wykonywania zapytań o dane w tabeli HBase.

1. Otwórz program **PuTTY** i połącz się z klastrem.  Zapoznaj się z instrukcjami w poprzedniej procedurze.
2. Otwórz powłokę programu Hive.
   
       hive
3. Uruchom poniższy skrypt HiveQL, aby utworzyć tabelę programu Hive, która mapuje dane do tabeli HBase. Upewnij się, że utworzono wspomnianą wcześniej w tym samouczku tabelę przykładową, używając powłoki HBase przed uruchomieniem tej instrukcji.
   
        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');
4. Uruchom poniższy skrypt HiveQL. Zapytanie programu Hive zostaje wykonane względem danych w tabeli HBase:
   
        SELECT count(*) FROM hbasecontacts;

## Korzystanie z interfejsów API REST HBase przy użyciu programu Curl
> [!NOTE]
> Używając programu Curl lub innego połączenia REST z usługą WebHCat, należy uwierzytelnić żądania, podając nazwę użytkownika i hasło administratora klastra usługi HDInsight. Należy również użyć nazwy klastra jako części identyfikatora URI stosowanego przy wysyłaniu żądań do serwera.
> 
> W przypadku poleceń w tej sekcji należy zastąpić ciąg **USERNAME** nazwą użytkownika w celu dokonania uwierzytelnienia w klastrze oraz zastąpić ciąg **PASSWORD** hasłem do konta użytkownika. Zastąp ciąg **CLUSTERNAME** nazwą klastra.
> 
> Interfejs API REST jest zabezpieczony za pomocą [uwierzytelniania podstawowego](http://en.wikipedia.org/wiki/Basic_access_authentication). Należy zawsze tworzyć żądania przy użyciu protokołu HTTPS (HTTP Secure), aby mieć pewność, że poświadczenia są bezpiecznie wysyłane do serwera.
> 
> 

1. W wierszu polecenia wpisz następujące polecenie, aby sprawdzić możliwość nawiązania połączenia z klastrem usługi HDInsight:
   
        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
   
    Użytkownik powinien otrzymywać odpowiedź podobną do następującej:
   
        {"status":"ok","version":"v1"}
   
    W tym poleceniu są używane następujące parametry:
   
   * **-u** — nazwa użytkownika i hasło używane do uwierzytelnienia żądania.
   * **-G** — parametr wskazujący, że jest to żądanie GET.
2. Użyj następującego polecenia, aby wyświetlić listę istniejących tabel HBase:
   
        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/
3. Użyj następującego polecenia, aby utworzyć nową tabelę HBase z dwiema rodzinami kolumn:
   
        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v
   
    Schemat jest podany w formacie JSon.
4. Użyj następującego polecenia, aby wstawić dane:
   
        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v
   
    Należy zakodować wartości określone w przełączniku -d w formacie base64.  W przykładzie:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Dole
     
     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) umożliwia wstawianie wielu wartości (wartości wsadowych).
5. Użyj następującego polecenia, aby pobrać wiersz:
   
        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Aby uzyskać więcej informacji o interfejsie Rest HBase, zobacz [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Podręcznik referencyjny Apache HBase).

## Sprawdzanie stanu klastra
Baza danych HBase w usłudze HDInsight jest dostarczana z interfejsem użytkownika sieci Web służącym do monitorowania klastrów. Za pośrednictwem interfejsu użytkownika sieci Web możesz przesyłać żądania dotyczące statystyk lub informacji o regionach.

Protokół SSH może również służyć do tunelowania żądań lokalnych, takich jak żądania sieci Web, do klastra usługi HDInsight. Żądania będą następnie kierowane do wymaganego zasobu, tak jakby pochodziły z węzła głównego klastra usługi HDInsight. Aby uzyskać więcej informacji, zobacz temat [Używanie SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Aby ustanowić sesję tunelowania SSH**

1. Otwórz program **PuTTY**.  
2. Jeśli podczas tworzenia konta użytkownika w trakcie procesu tworzenia podano klucz SSH, należy wykonać następujący krok, aby wybrać klucz prywatny używany podczas uwierzytelniania w klastrze:
   
    W obszarze **Category** (Kategoria) rozwiń listę **Connection** (Połączenie), rozwiń listę **SSH** i wybierz pozycję **Auth** (Uwierzytelnianie). Na koniec kliknij przycisk **Browse** (Przeglądaj) i wybierz plik ppk, który zawiera klucz prywatny.
3. W obszarze **Category** (Kategoria) kliknij opcję **Session** (Sesja).
4. Na ekranie sesji PuTTY w opcjach podstawowych wprowadź następujące wartości:
   
   * **Host Name** (Nazwa hosta): adres SSH serwera HDInsight w polu nazwy hosta (lub adresu IP). Adres SSH jest nazwą klastra, po którym następuje ciąg **-ssh.azurehdinsight.net**. Na przykład *mójklaster-ssh.azurehdinsight.net*.
   * **Port**: 22. Port SSH na podstawowym węźle głównym to port 22.  
5. W sekcji **Category** (Kategoria) z lewej strony okna dialogowego rozwiń listę **Connection** (Połączenie), rozwiń listę **SSH**, a następnie kliknij pozycję **Tunnels** (Tunele).
6. Podaj następujące informacje w formularzu opcji kontroli przekierowania portu SSH:
   
   * **Source port** (Port źródłowy) — port na komputerze klienckim, który ma być przekierowany. Na przykład 9876.
   * **Dynamic** (Dynamiczny) — umożliwia dynamiczny routing serwera proxy SOCKS.
7. Kliknij przycisk **Add** (Dodaj), aby dodać ustawienia.
8. Kliknij przycisk **Open** (Otwórz) w dolnej części okna dialogowego, aby otworzyć połączenie SSH.
9. Po wyświetleniu monitu zaloguj się do serwera przy użyciu konta SSH. Spowoduje to ustanowienie sesji SSH i włączenie tunelu.

**Aby znaleźć nazwę FQDN dozorcy przy użyciu narzędzia Ambari**

1. Przejdź do witryny https://<ClusterName>.azurehdinsight.net/.
2. Wprowadź dwa razy poświadczenia konta użytkownika klastra.
3. W lewym menu kliknij pozycję **dozorca**.
4. Kliknij jeden z trzech linków **serwera dozorcy** na liście podsumowania.
5. Skopiuj wartość **Nazwa hosta**. Na przykład zk0-NAZWA_KLASTRA.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Aby skonfigurować program kliencki (Firefox) i sprawdzić stan klastra**

1. Otwórz przeglądarkę Firefox.
2. Kliknij przycisk **Otwórz menu**.
3. Kliknij pozycję **Opcje**.
4. Kliknij opcję **Zaawansowane**, kliknij kartę **Sieć**, a następnie kliknij przycisk **Ustawienia**.
5. Wybierz opcję **Ręczna konfiguracja serwerów proxy**.
6. Wprowadź następujące wartości:
   
   * **Host SOCKS**: localhost
   * **Port**: użyj tego samego portu, który został skonfigurowany na potrzeby tunelowania SSH w programie PuTTY.  Na przykład 9876.
   * **SOCKS v5**: (wybrane)
   * **Zdalny DNS**: (wybrane)
7. Kliknij przycisk **OK**, aby zapisać zmiany.
8. Przejdź na adres http://&lt;FQDN ZooKeeper>:60010/master-status.

W klastrze wysokiej dostępności można znaleźć link do bieżącego aktywnego węzła głównego HBase obsługującego interfejs użytkownika sieci Web.

## Usuwanie klastra
Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase przed usunięciem klastra.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Następne kroki
W tym samouczku HBase dotyczącym usługi HDInsight pokazano, jak utworzyć klaster HBase i tabele oraz jak wyświetlać dane w tych tabelach z poziomu powłoki HBase. Przedstawiono również sposób wykonywania zapytań programu Hive względem danych w tabelach HBase oraz korzystania z interfejsów API REST HBase w języku C# w celu tworzenia tabel HBase i pobierania danych z tabeli.

Aby dowiedzieć się więcej, zobacz:

* [Przegląd bazy danych HBase w usłudze HDInsight][hdinsight-hbase-overview]: HBase jest bazą danych Apache NoSQL typu open source opartą na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości danych z częściową strukturą i bez struktury.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png



<!--HONumber=Sep16_HO5-->


