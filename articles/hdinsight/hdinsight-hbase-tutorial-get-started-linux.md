---
title: "Rozpoczynanie pracy z bazą danych HBase w usłudze Azure HDInsight | Microsoft Docs"
description: "Postępuj zgodnie z tym samouczkiem bazy danych HBase, aby rozpocząć korzystanie z bazy danych Apache HBase na platformie Hadoop w usłudze HDInsight. Utwórz tabele z poziomu powłoki HBase i wykonuj zapytania przy użyciu aplikacji Hive."
keywords: apache hbase,hbase,hbase shell,hbase tutorial
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: bdc00541e55ea68f0b18456f8fa1a1ac51116d69
ms.lasthandoff: 03/11/2017


---
# <a name="hbase-tutorial-get-started-using-apache-hbase-in-hdinsight"></a>Samouczek HBase: rozpoczęcie korzystania z bazy danych Apache HBase w usłudze HDInsight

Dowiedz się, jak utworzyć klaster HBase w usłudze HDInsight i tabele bazy danych HBase oraz jak wykonywać zapytania dotyczące tabel za pomocą aplikacji Hive. Aby uzyskać ogólne informacje o bazie danych HBase, zobacz [Omówienie bazy danych HBase w usłudze HDInsight][hdinsight-hbase-overview].

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem korzystania z tego samouczka HBase należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Bezpieczna powłoka (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [Program curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Tworzenie klastra HBase
W poniższej procedurze użyto szablonu usługi Azure Resource Manager do utworzenia klastra HBase opartego na systemie Linux w wersji 3.4 i zależnego domyślnego konta usługi Azure Storage. Aby zapoznać się z parametrami używanymi w tej procedurze oraz innymi metodami tworzenia klastra, zobacz temat [Tworzenie opartych na systemie Linux klastrów Hadoop w usłudze HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal. Szablon znajduje się w publicznym kontenerze obiektów blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. W bloku **Wdrożenie niestandardowe** wprowadź wartości następujących opcji:
   
   * **Subskrypcja**: wybierz subskrypcję platformy Azure, które zostanie użyta do utworzenia klastra.
   * **Grupa zasobów**: utwórz nową grupę usługi Azure Resource Management lub użyj istniejącej.
   * **Lokalizacja**: określ lokalizację grupy zasobów. 
   * **ClusterName**: wprowadź nazwę tworzonego klastra HBase.
   * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to **admin**.
   * **Nazwa użytkownika i hasło SSH**: domyślna nazwa użytkownika to **sshuser**.  Tę nazwę można zmienić.
     
     Inne parametry są opcjonalne.  
     
     Każdy klaster zależy od konta usługi Azure Storage. Po usunięciu klastra dane pozostają zachowane na koncie magazynu. Domyślna nazwa konta magazynu klastra to nazwa klastra z dołączonym ciągiem „store”. Jest ona umieszczona w kodzie w sekcji zmiennych szablonu.
3. Zaznacz pozycję **Wyrażam zgodę na powyższe warunki i postanowienia**, a następnie kliknij przycisk **Kup**. Utworzenie klastra trwa około 20 minut.

> [!NOTE]
> Po usunięciu klastra HBase można utworzyć inny klaster HBase za pomocą tego samego domyślnego kontenera obiektów blob. Nowy klaster przejmie tabele bazy danych HBase utworzone w oryginalnym klastrze. Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase przed usunięciem klastra.
> 
> 

## <a name="create-tables-and-insert-data"></a>Tworzenie tabel i wstawianie danych
Protokół SSH umożliwia połączenie z klastrami HBase, a następnie korzystanie z powłoki HBase w celu tworzenia tabel bazy danych HBase, wstawiania danych i wykonywania zapytań. Aby uzyskać informacje na temat korzystania z protokołu SSH, zobacz tematy [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oraz [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

Dla większości użytkowników dane są wyświetlane w formacie tabelarycznym:

![Dane tabelaryczne usługi HDInsight HBase][img-hbase-sample-data-tabular]

W bazie danych HBase, która jest implementacją BigTable, te same dane wyglądają następująco:

![Dane BigTable usługi HDInsight HBase][img-hbase-sample-data-bigtable]

Stanie się to bardziej zrozumiałe po zakończeniu następnej procedury.  

**Aby użyć powłoki HBase**

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
   
    ![Powłoka HBase HDInsight Hadoop][img-hbase-shell]
4. Pobierz pojedynczy wiersz:
   
        get 'Contacts', '1000'
   
    Wyświetlone zostaną te same wyniki, co w przypadku polecenia scan, ponieważ istnieje tylko jeden wiersz.
   
    Aby uzyskać więcej informacji na temat schematu tabeli bazy danych HBase, zobacz [Introduction to HBase Schema Design][hbase-schema] (Wprowadzenie do projektowania schematu bazy danych HBase). Więcej poleceń bazy danych HBase można znaleźć w [Podręczniku bazy danych Apache HBase][hbase-quick-start].
5. Wyjdź z powłoki:
   
        exit

**Aby zbiorczo załadować dane do tabeli kontaktów HBase**

Baza danych HBase obsługuje kilka metod ładowania danych do tabel.  Aby uzyskać więcej informacji, zobacz temat [Ładowanie zbiorcze](http://hbase.apache.org/book.html#arch.bulk.load).

Przykładowy plik danych został przekazany do publicznego kontenera obiektów blob, *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Plik danych ma następującą zawartość:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

Możesz utworzyć plik tekstowy i przesłać go na swoje konto magazynu. Aby uzyskać instrukcje, zobacz [Przekazywanie danych dla zadań Hadoop w usłudze HDInsight][hdinsight-upload-data].

> [!NOTE]
> W tej procedurze jest używana tabela kontaktów HBase utworzona w poprzedniej procedurze.
> 
> 

1. Z poziomu bezpiecznej powłoki (SSH) uruchom następujące polecenie, aby przekształcić plik danych do postaci StoreFiles i zapisać go w ścieżce względnej określonej przez parametr Dimporttsv.bulk.output:.  Jeśli jest otwarta powłoka HBase, użyj polecenia exit, aby z niej wyjść.
   
        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
2. Uruchom następujące polecenie, aby przekazać dane z katalogu /example/data/storeDataFileOutput do tabeli HBase:
   
        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
3. Możesz otworzyć powłokę HBase i użyć polecenia scan w celu wyświetlenia zawartości tabeli.

## <a name="use-hive-to-query-hbase"></a>Uruchamianie zapytania bazy danych HBase przy użyciu programu Hive
Korzystając z programu Hive, można wykonywać zapytania dotyczące danych w tabelach HBase. W tej sekcji zostanie utworzona tabela programu Hive odwzorowująca dane w tabeli HBase, która będzie używana do wykonywania zapytań o dane w tabeli HBase.

> [!NOTE]
> Jeśli program Hive i baza danych HBase znajdują się w różnych klastrach w tej samej sieci wirtualnej, należy przekazać kworum dozorcy podczas wywoływania powłoki usługi Hive:
>
>       hive --hiveconf hbase.zookeeper.quorum=zk0-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net,zk1-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net,zk2-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net --hiveconf zookeeper.znode.parent=/hbase-unsecure  
>
>

1. Otwórz program **PuTTY** i połącz się z klastrem.  Zapoznaj się z instrukcjami w poprzedniej procedurze.
2. Otwórz powłokę programu Hive.
   
       hive
       
3. Uruchom poniższy skrypt HiveQL, aby utworzyć tabelę programu Hive, która mapuje dane na tabelę HBase. Upewnij się, że utworzono wspomnianą wcześniej w tym samouczku tabelę przykładową, używając powłoki HBase przed uruchomieniem tej instrukcji.
   
        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');
4. Uruchom poniższy skrypt HiveQL, aby wykonać zapytanie o dane w tabeli HBase:
   
         SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Korzystanie z interfejsów API REST HBase przy użyciu programu Curl
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
        -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
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

## <a name="check-cluster-status"></a>Sprawdzanie stanu klastra
Baza danych HBase w usłudze HDInsight jest dostarczana z interfejsem użytkownika sieci Web służącym do monitorowania klastrów. Za pośrednictwem interfejsu użytkownika sieci Web możesz przesyłać żądania dotyczące statystyk lub informacji o regionach.

**Aby uzyskać dostęp do głównego interfejsu użytkownika HBase**

1. Otwórz interfejs użytkownika sieci Web systemu Ambari pod adresem https://&lt;nazwa_klastra>.azurehdinsight.net.
2. W menu po lewej kliknij pozycję **HBase**.
3. Na górze strony kliknij pozycję **Szybkie linki**, wskaż aktywny link węzła dozorcy, a następnie kliknij pozycję **Główny interfejs użytkownika bazy danych HBase**.  Interfejs użytkownika zostanie otwarty w innej karcie przeglądarki:

  ![Główny interfejs użytkownika HDInsight HBase](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  Główny interfejs użytkownika HBase zawiera następujące sekcje:

  - serwery regionów
  - wzorce kopii zapasowej
  - tabele
  - zadania
  - atrybuty oprogramowania

## <a name="delete-the-cluster"></a>Usuwanie klastra
Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase przed usunięciem klastra.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Następne kroki
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

