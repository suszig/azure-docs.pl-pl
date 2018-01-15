---
title: "Wprowadzenie do platformy Apache Kafka — usługa Azure HDInsight | Microsoft Docs"
description: "Dowiedz się, jak utworzyć klaster Apache Kafka w usłudze Azure HDInsight. Dowiedz się, jak utworzyć tematy, subskrybentów i użytkowników."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 24133adc6e6b16c69a8b124f13e684fce26b115f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="start-with-apache-kafka-on-hdinsight"></a>Wprowadzenie do platformy Apache Kafka w usłudze HDInsight

Dowiedz się, jak utworzyć klaster [Apache Kafka](https://kafka.apache.org) w usłudze Azure HDInsight i używać go. Kafka to rozproszona platforma przesyłania strumieniowego typu „open source”, dostępna z usługą HDInsight. Jest ona często używana jako broker komunikatów, ponieważ oferuje funkcje podobne do kolejki komunikatów dotyczących publikowania i subskrybowania.

> [!NOTE]
> Obecnie są dostępne dwie wersje platformy Kafka w usłudze HDInsight: 0.9.0 (HDInsight 3.4) i 0.10.0 (HDInsight 3.5 i 3.6). W procedurach przedstawionych w tym artykule przyjęto założenie, że jest używana platforma Kafka w usłudze HDInsight 3.6.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Tworzenie klastra platformy Kafka

Aby utworzyć klaster platformy Kafka w usłudze HDInsight, wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz pozycję **+ NOWY** i pozycję **Zbieranie danych i analiza**, a następnie pozycję **HDInsight**.
   
    ![Tworzenie klastra usługi HDInsight](./media/apache-kafka-get-started/create-hdinsight.png)

2. W bloku **Podstawowe** wprowadź następujące informacje:

    * **Nazwa klastra**: nazwa klastra usługi HDInsight.
    * **Subskrypcja**: wybierz subskrypcję, której chcesz użyć.
    * **Nazwa użytkownika logowania klastra** i **Hasło logowania klastra**: dane logowania podczas uzyskiwania dostępu do klastra przy użyciu protokołu HTTPS. Te poświadczenia umożliwiają dostęp do takich usług jak interfejs użytkownika sieci Web Ambari lub interfejs API REST.
    * **Nazwa użytkownika SSH (Secure Shell)**: nazwa logowania używana podczas uzyskiwania dostępu do klastra za pośrednictwem protokołu SSH. Domyślnie hasło jest takie samo jak hasło logowania klastra.
    * **Grupa zasobów**: grupa zasobów, w której ma zostać utworzony klaster.
    * **Lokalizacja**: region platformy Azure, w którym ma zostać utworzony klaster.

        > [!IMPORTANT]
        > Aby zapewnić wysoką dostępność danych, zalecamy wybranie lokalizacji (regionu), który zawiera __trzy domeny błędów__. Aby uzyskać więcej informacji, zobacz sekcję [Wysoka dostępność danych](#data-high-availability).
   
 ![Wybieranie subskrypcji](./media/apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Wybierz pozycję **Typ klastra**, a następnie ustaw następujące wartości z bloku **Konfiguracja klastra**:
   
    * **Typ klastra**: Kafka

    * **Wersja**: Kafka 0.10.0 (HDI 3.6)

    * **Warstwa klastra**: Standardowa
     
 Zapisz ustawienia przy użyciu przycisku **Wybierz**.
     
 ![Wybieranie typu klastra](./media/apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Po wybraniu typu klastra ustaw typ klastra przy użyciu przycisku __Wybierz__. Następnie zakończ konfigurację podstawową za pomocą przycisku __Dalej__.

5. W bloku **Magazyn** wybierz lub utwórz konto magazynu. Na potrzeby procedury w tym dokumencie pozostaw wartości domyślne w pozostałych polach. Zapisz konfigurację magazynu za pomocą przycisku __Dalej__.

    ![Konfigurowanie ustawień konta magazynu dla usługi HDInsight](./media/apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Opcjonalnie w bloku __Aplikacje__ wybierz przycisk __Dalej__, aby kontynuować. W tym przykładzie nie są wymagane żadne aplikacje.

7. W bloku __Rozmiar klastra__ wybierz przycisk __Dalej__, aby kontynuować.

    > [!WARNING]
    > Aby zapewnić dostępność platformy Kafka w usłudze HDInsight, klaster musi zawierać co najmniej trzy węzły procesu roboczego. Aby uzyskać więcej informacji, zobacz sekcję [Wysoka dostępność danych](#data-high-availability).

    ![Ustawianie rozmiaru klastra Kafka](./media/apache-kafka-get-started/kafka-cluster-size.png)

    > [!IMPORTANT]
    > Parametr **liczby dysków na węzeł procesu roboczego** pozwala sterować skalowalnością platformy Kafka w usłudze HDInsight. Platforma Kafka w usłudze HDInsight używa dysku lokalnego maszyn wirtualnych w klastrze. Ze względu na duże obciążenie we/wy platformy Kafka używana jest usługa [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md), która zapewnia wysoką przepływność i więcej miejsca do magazynowania w każdym węźle. Można wybrać typ dysku zarządzanego __Standardowy__ (HDD) lub __Premium__ (SSD). Dyski w warstwie Premium są używane przez maszyny wirtualne serii DS i GS. Wszystkie pozostałe typy maszyn wirtualnych korzystają z dysków standardowych.

8. W bloku __Ustawienia zaawansowane__ wybierz przycisk __Dalej__, aby kontynuować.

9. W bloku **Podsumowanie** przejrzyj konfigurację klastra. Zmień niepoprawne ustawienia przy użyciu linków __Edytuj__. Utwórz klaster przy użyciu przycisku __Utwórz__.
   
    ![Podsumowanie konfiguracji klastra](./media/apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Tworzenie klastra może potrwać do 20 minut.

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

> [!IMPORTANT]
> Wykonując poniższe kroki, musisz użyć klienta SSH. Aby uzyskać więcej informacji, zobacz dokument [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Na kliencie nawiąż połączenie z klastrem przy użyciu protokołu SSH:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Zamień ciąg **SSHUSER** na nazwę użytkownika SSH podaną podczas tworzenia klastra. Zamień ciąg **CLUSTERNAME** na nazwę klastra.

Po wyświetleniu monitu wprowadź hasło, którego użyto dla konta SSH.

Aby uzyskać informacje, zobacz [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Uzyskiwanie informacji dotyczących hosta dozorcy i brokera

Podczas pracy z platformą Kafka musisz znać dwie wartości hostów: *Dozorca* i *Broker*. Te hosty są używane z interfejsem API platformy Kafka i wieloma narzędziami oferowanymi z platformą Kafka.

Poniżej przedstawiono procedurę tworzenia zmiennych środowiskowych z informacjami o hoście. Użyto tych zmiennych środowiskowych w procedurze opisanej w tym dokumencie.

1. Z poziomu połączenia SSH z klastrem zainstaluj narzędzie `jq` za pomocą poniższego polecenia. To narzędzie służy do analizowania dokumentów JSON i jest przydatne podczas pobierania informacji o hoście brokera:
   
    ```bash
    sudo apt -y install jq
    ```

2. Aby ustawić zmienne środowiskowe przy użyciu informacji pobranych z systemu Ambari, użyj następujących poleceń:

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Ustaw nazwę klastra Kafka jako wartość elementu `CLUSTERNAME=`. Ustaw hasło logowania (administratora), którego użyto podczas tworzenia klastra, jako wartość elementu `PASSWORD=`.

    Następujący tekst jest przykładem zawartości elementu `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    Następujący tekst jest przykładem zawartości elementu `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > Polecenie `cut` umożliwia obcięcie listy hostów do dwóch pozycji. Nie trzeba podawać pełnej listy hostów podczas tworzenia odbiorcy ani producenta platformy Kafka.
   
    > [!WARNING]
    > Informacje zwracane z tej sesji nie zawsze są dokładne. Skalowanie klastra powoduje dodanie lub usunięcie nowych brokerów. W przypadku niepowodzenia lub zamiany węzła nazwa hosta węzła może ulec zmianie.
    >
    > Pobierz informacje o hostach dozorcy i brokera krótko przed sprawdzeniem, czy masz prawidłowe informacje.

## <a name="create-a-topic"></a>Tworzenie tematu

Platforma Kafka przechowuje strumienie danych w kategoriach nazywanych *tematami*. Z poziomu połączenia SSH z węzłem głównym klastra utwórz temat za pomocą skryptu dostarczonego z platformą Kafka:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

To polecenie umożliwia nawiązanie połączenia z dozorcą przy użyciu informacji o hoście przechowywanych w elemencie `$KAFKAZKHOSTS`, a następnie utworzenie tematu platformy Kafka o nazwie **test**. Aby sprawdzić, czy temat został utworzony, użyj następującego skryptu w celu wyświetlenia listy tematów:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

To polecenie generuje listę tematów platformy Kafka, która zawiera temat **test**.

## <a name="produce-and-consume-records"></a>Tworzenie i używanie rekordów

Platforma Kafka przechowuje *rekordy* w tematach. Rekordy są tworzone przez *producentów* i używane przez *odbiorców*. Producenci tworzą rekordy dla *brokerów* platformy Kafka. Każdy węzeł procesu roboczego w klastrze usługi HDInsight jest brokerem platformy Kafka.

Poniżej przedstawiono procedurę zapisywania rekordów w utworzonym wcześniej temacie testowym i odczytywania ich za pomocą odbiorcy:

1. Z poziomu sesji SSH zapisz rekordy w temacie przy użyciu skryptu dostarczonego z platformą Kafka:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Po wykonaniu tego polecenia nie następuje powrót do wiersza polecenia. Zamiast tego wpisz kilka komunikatów tekstowych, a następnie użyj klawiszy **Ctrl+C**, aby zatrzymać wysyłanie do tematu. Każdy wiersz jest wysyłany jako oddzielny rekord.

2. Odczytaj rekordy z tematu przy użyciu skryptu dostarczonego z platformą Kafka:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --zookeeper $KAFKAZKHOSTS --topic test --from-beginning
    ```
   
    To polecenie umożliwia pobranie rekordów z tematu i ich wyświetlenie. Polecenie `--from-beginning` informuje odbiorcę, aby rozpocząć od początku strumienia w celu pobrania wszystkich rekordów.

3. Użyj klawiszy __Ctrl+C__, aby zatrzymać odbiorcę.

## <a name="producer-and-consumer-api"></a>Interfejs API producenta i odbiorcy

Możesz też programowo tworzyć rekordy i korzystać z nich przy użyciu [interfejsów API platformy Kafka](http://kafka.apache.org/documentation#api). Aby skompilować producenta i odbiorcę języka Java, wykonaj następujące kroki w środowisku deweloperskim.

> [!IMPORTANT]
> Następujące składniki muszą być zainstalowane w środowisku deweloperskim:
>
> * [Zestaw Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) lub równoważny, taki jak OpenJDK.
>
> * [Apache Maven](http://maven.apache.org/)
>
> * Klient SSH i polecenie `scp`. Aby uzyskać więcej informacji, zobacz dokument [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Pobierz przykłady ze strony [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). W przypadku przykładu producenta i odbiorcy użyj projektu w katalogu `Producer-Consumer`. Ten przykład zawiera następujące klasy:
   
    * **Run** — uruchamia odbiorcę lub producenta.

    * **Producer** — zapisuje 1 000 000 rekordów w temacie.

    * **Consumer** — odczytuje rekordy z tematu.

2. Aby utworzyć pakiet jar, przejdź do katalogu `Producer-Consumer` i użyj następującego polecenia:

    ```
    mvn clean package
    ```

    To polecenie tworzy katalog o nazwie `target`, który zawiera plik o nazwie `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Aby skopiować plik `kafka-producer-consumer-1.0-SNAPSHOT.jar` do klastra usługi HDInsight, użyj następujących poleceń:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Zamień ciąg **SSHUSER** na nazwę użytkownika SSH klastra i zamień ciąg **CLUSTERNAME** na nazwę klastra. Po wyświetleniu monitu wprowadź hasło użytkownika SSH.

4. Gdy polecenie `scp` zakończy kopiowanie pliku, połącz się z klastrem przy użyciu protokołu SSH. Zapisz rekordy w temacie testowym przy użyciu poniższego polecenia:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. Po zakończeniu procesu odczytaj rekordy z tematu za pomocą następującego polecenia:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Zostanie wyświetlona liczba odczytanych rekordów wraz z liczbą rekordów. Może zostać wyświetlonych więcej niż 1 000 000 zarejestrowanych rekordów, ponieważ wiele rekordów wysłano do tematu przy użyciu skryptu w poprzednim kroku.

6. Użyj klawiszy __Ctrl+C__, aby zakończyć działanie odbiorcy.

### <a name="multiple-consumers"></a>Wielu odbiorców

Odbiorcy platformy Kafka używają grupy odbiorców podczas odczytywania rekordów. Korzystanie z tej samej grupy przez wielu odbiorców umożliwia równoważenie obciążenia podczas przeprowadzania odczytu z tematu. Każdy odbiorca w grupie odbiera część rekordów. Aby zobaczyć, jak działa ten proces, wykonaj następujące czynności:

1. Otwórz nową sesję SSH w klastrze, tak aby istniały dwie sesje. W każdej sesji uruchom odbiorcę z tym samym identyfikatorem grupy odbiorców za pomocą następującego polecenia:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    To polecenie uruchamia odbiorcę za pomocą identyfikatora grupy `mygroup`.

    > [!NOTE]
    > Aby ustawić element `$KAFKABROKERS` w tej sesji SSH, użyj poleceń przedstawionych w sekcji [Uzyskiwanie informacji dotyczących hosta dozorcy i brokera](#getkafkainfo).

2. W każdej sesji są zliczane rekordy odebrane z tematu. Suma obu sesji powinna być taka sama jak wartość otrzymana wcześniej od odbiorcy.

Użycie przez klientów w tej samej grupie jest obsługiwane przez partycje tematu. Utworzony wcześniej temat `test` ma osiem partycji. Jeśli otworzysz osiem sesji SSH i uruchomisz odbiorcę we wszystkich sesjach, każdy odbiorca odczyta rekordy z jednej partycji tematu.

> [!IMPORTANT]
> Grupa odbiorców nie może zawierać więcej wystąpień odbiorców niż partycji. W tym przykładzie jedna grupa odbiorców może zawierać maksymalnie ośmiu odbiorców, ponieważ tyle partycji znajduje się w temacie. Może też istnieć wiele grup odbiorców — każda z nich może zawierać maksymalnie ośmiu odbiorców.

Rekordy na platformie Kafka są przechowywane w kolejności, w której zostały odebrane na partycji. Aby dostarczać rekordy *na partycji* w określonej kolejności, utwórz grupę odbiorców, w której liczba wystąpień odbiorców jest zgodna z liczbą partycji. Aby dostarczać rekordy *w temacie* w określonej kolejności, utwórz grupę odbiorców z jednym wystąpieniem odbiorcy.

## <a name="streaming-api"></a>Interfejs API przesyłania strumieniowego

Interfejs API przesyłania strumieniowego został dodany do platformy Kafka w wersji 0.10.0. Wcześniejsze wersje korzystają z platformy Apache Spark lub systemu Storm na potrzeby przetwarzania strumienia.

1. Jeśli jeszcze tego nie zrobiono, pobierz przykłady ze strony [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) do środowiska deweloperskiego. W przypadku przykładu przesyłania strumieniowego użyj projektu w katalogu `streaming`.
   
    Ten projekt zawiera tylko jedną klasę (`Stream`), która odczytuje rekordy z utworzonego wcześniej tematu `test`. Zlicza ona odczytane wyrazy oraz emituje każdy wyraz i każdą liczbę do tematu o nazwie `wordcounts`. Proces tworzenia tematu `wordcounts` przedstawiono w dalszej części tej sekcji.

2. W wierszu polecenia w środowisku deweloperskim zmień katalogi na lokalizację katalogu `Streaming` i utwórz pakiet jar przy użyciu następującego polecenia:

    ```bash
    mvn clean package
    ```

    To polecenie tworzy katalog o nazwie `target`, który zawiera plik o nazwie `kafka-streaming-1.0-SNAPSHOT.jar`.

3. Aby skopiować plik `kafka-streaming-1.0-SNAPSHOT.jar` do klastra usługi HDInsight, użyj następujących poleceń:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Zamień ciąg **SSHUSER** na nazwę użytkownika SSH klastra i zamień ciąg **CLUSTERNAME** na nazwę klastra. Po wyświetleniu monitu wprowadź hasło użytkownika SSH.

4. Gdy polecenie `scp` zakończy kopiowanie pliku, połącz się z klastrem przy użyciu protokołu SSH, a następnie utwórz temat `wordcounts` za pomocą następującego polecenia:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Następnie uruchom proces przesyłania strumieniowego za pomocą następującego polecenia:
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    To polecenie uruchamia proces przesyłania strumieniowego w tle.

6. Wyślij komunikaty do tematu `test` za pomocą poniższego polecenia. Te komunikaty są przetwarzane przez przykład przesyłania strumieniowego:
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Aby wyświetlić dane wyjściowe zapisane w temacie `wordcounts` przez proces przesyłania strumieniowego, użyj następującego polecenia:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > Aby wyświetlić dane, musisz poinstruować odbiorcę, aby wypisał klucz i deserializator do użycia dla klucza i wartości. Nazwa klucza ma postać wyrazu, a wartość klucza ma postać liczby.
   
    Dane wyjściowe będą podobne do następującego tekstu:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > Liczba rośnie przy każdym napotkaniu wyrazu.

7. Użyj klawiszy __Ctrl+C__, aby zakończyć działanie odbiorcy, a następnie przywróć zadanie przesyłania strumieniowego z tła na pierwszy plan za pomocą polecenia `fg`. Użyj klawiszy __Ctrl+C__, aby zakończyć działanie.

## <a name="data-high-availability"></a>Wysoka dostępność danych

Każdy region (lokalizacja) świadczenia usługi Azure udostępnia _domeny błędów_. Domena błędów to logiczna grupa bazowego sprzętu w centrum danych platformy Azure. Wszystkie domeny błędów korzystają ze wspólnego źródła zasilania i przełącznika sieciowego. Maszyny wirtualne i dyski zarządzane, które implementują węzły w klastrze usługi HDInsight są rozdzielone między te domeny błędów. Taka architektura ogranicza wpływ potencjalnych awarii sprzętu fizycznego.

Aby uzyskać informacje dotyczące liczby domen błędów w regionie, zobacz dokument [Availability of Linux virtual machines (Dostępność maszyn wirtualnych z systemem Linux)](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

> [!IMPORTANT]
> Zalecamy wybranie regionu świadczenia usługi Azure zawierającego trzy domeny błędów oraz użycie współczynnika replikacji o wartości 3.

Jeśli musisz wybrać region, który zawiera tylko dwie domeny błędów, użyj współczynnika replikacji o wartości 4, aby równomiernie rozłożyć repliki na dwie domeny błędów.

### <a name="kafka-and-fault-domains"></a>Platforma Kafka i domeny błędów

Platforma Kafka nie uwzględnia domen błędów. Utworzone repliki partycji tematów mogą nie zostać prawidłowo rozpowszechnione w celu zapewnienia wysokiej dostępności. Aby zapewnić wysoką dostępność, użyj [narzędzia do ponownego równoważenia partycji platformy Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Narzędzie to należy uruchomić w sesji połączenia SSH z węzłem głównym klastra Kafka.

Aby zapewnić najwyższą dostępność danych na platformie Kafka, należy stosować ponowne równoważenie replik partycji dla tematu w następujących sytuacjach:

* Po utworzeniu nowego tematu lub partycji

* Po przeskalowaniu klastra w górę

## <a name="delete-the-cluster"></a>Usuwanie klastra

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Rozwiązywanie problemów

W razie problemów podczas tworzenia klastrów usługi HDInsight zapoznaj się z [wymaganiami dotyczącymi kontroli dostępu](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Następne kroki

W tym dokumencie przedstawiono podstawowe informacje dotyczące pracy z platformą Apache Kafka w usłudze HDInsight. Dowiedz się więcej o pracy z platformą Kafka, korzystając z następujących zasobów:

* [Analizowanie dzienników Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicate data between Kafka clusters (Replikowanie danych między klastrami Kafka)](apache-kafka-mirroring.md)
* [Use Apache Spark streaming (DStream) with Kafka on HDInsight (Używanie strumieni (DStream) platformy Apache Spark z platformą Kafka w usłudze HDInsight)](../hdinsight-apache-spark-with-kafka.md)
* [Use Apache Spark Structured Streaming with Kafka on HDInsight (Używanie strumieni ze strukturą platformy Apache Spark z platformą Kafka w usłudze HDInsight)](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Używanie systemu Apache Storm z platformą Kafka w usłudze HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Nawiązywanie połączenia z platformą Kafka za pośrednictwem sieci wirtualnej platformy Azure](apache-kafka-connect-vpn-gateway.md)
