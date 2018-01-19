---
title: "Platforma Apache Spark strukturalnych przesyłania strumieniowego z Kafka rozwiązania Cosmos Azure DB — usługa Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać Apache Spark strukturalnych strumienia do odczytywania danych z Apache Kafka i zapisz go do bazy danych Azure rozwiązania Cosmos. W tym przykładzie można przesyłać strumieniowo dane za pomocą notesu Jupyter z platformy Spark w usłudze HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/16/2018
ms.author: larryfr
ms.openlocfilehash: 2709bab433bdc108b313c58e0844236dd4af38bf
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="use-spark-structured-streaming-with-kafka-and-azure-cosmos-db"></a>Użyj strukturę przesyłania strumieniowego Spark DB Kafka i Azure rozwiązania Cosmos

Dowiedz się, jak użyć strukturalnych przesyłania strumieniowego Spark można odczytać danych z Kafka Apache na Azure HDInsight, a następnie zapisać dane do bazy danych Azure rozwiązania Cosmos.

Azure DB rozwiązania Cosmos jest globalnie rozproszone i wiele modeli bazy danych. W tym przykładzie korzysta z interfejsu API SQL bazy danych modelu. Aby uzyskać więcej informacji, zobacz [Zapraszamy do bazy danych Azure rozwiązania Cosmos](../cosmos-db/introduction.md) dokumentu.

Struktura przesyłania strumieniowego Spark jest aparatem przetwarzania strumienia oparty na Spark SQL. Umożliwia express przesyłania strumieniowego obliczenia taka sama jak partii obliczeń na danych statycznych. Aby uzyskać więcej informacji o strukturze przesyłania strumieniowego, zobacz [strukturalnych przesyłania strumieniowego przewodnik programowania w języku [alfa]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) w serwisie Apache.org.

> [!IMPORTANT]
> W tym przykładzie użyto Spark 2.1 w usłudze HDInsight 3,6. Strukturalne Streaming jest uznawany za __alfa__ 2.1 Spark.
>
> Kroki opisane w tym dokumencie Tworzenie grupy zasobów platformy Azure, który zawiera zarówno Spark w usłudze HDInsight i Kafka w klastrze usługi HDInsight. Te klastry są oba znajdujących się w sieci wirtualnej platformy Azure, dzięki czemu klastra Spark do bezpośredniego komunikowania się z Kafka klastra.
>
> Po zakończeniu kroków w tym dokumencie, pamiętaj, aby usuwać te klastry, aby zapobiec zmianom nadmiarowe.

## <a name="create-the-clusters"></a>Tworzenie klastrów

Kafka Apache na HDInsight nie zapewnia dostępu do brokerzy Kafka za pośrednictwem publicznej sieci internet. Wszystkie elementy, które komunikuje się Kafka musi być w tej samej sieci wirtualnej platformy Azure jako węzły w klastrze Kafka. W tym przykładzie zarówno Kafka, jak i Spark klastrach znajdują się w sieci wirtualnej platformy Azure. Na poniższym diagramie przedstawiono sposób komunikacji przepływa między klastrami:

![Diagram klastry Spark i Kafka w sieci wirtualnej platformy Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Usługa Kafka jest ograniczone do komunikacji w sieci wirtualnej. Inne usługi w klastrze, takich jak SSH i Ambari, są dostępne za pośrednictwem Internetu. Aby uzyskać więcej informacji na portach publicznego dostępne z usługą HDInsight, zobacz [portów i identyfikatory URI używany przez HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Podczas tworzenia sieci wirtualnej platformy Azure, Kafka, i Spark klastrów ręcznie, łatwiej jest użyć szablonu usługi Azure Resource Manager. Wykonaj następujące kroki, aby wdrożyć sieci wirtualnej platformy Azure, Kafka i Spark klastrów do subskrypcji platformy Azure.

1. Poniższy przycisk umożliwia logowanie do platformy Azure i otwórz szablon w portalu Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/> </a>

    Szablon usługi Azure Resource Manager znajduje się w repozytorium GitHub dla tego projektu ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Ten szablon umożliwia tworzenie następujących zasobów:

    * Kafka w klastrze HDInsight 3,6.

    * Platforma Spark w klastrze HDInsight 3,6.

    * Sieci wirtualnej platformy Azure, który zawiera klastry usługi HDInsight.

    * Baza danych interfejsu API Azure rozwiązania Cosmos bazy danych SQL.

    > [!IMPORTANT]
    > Strukturalne notesu przesyłania strumieniowego w tym przykładzie wymaga platforma Spark w usłudze HDInsight 3,6. Jeśli używasz starszej wersji platformy Spark w usłudze HDInsight, wystąpią błędy, gdy za pomocą notesu.

2. Użyj poniższych informacji, aby wypełnić wpisy na **wdrożenie niestandardowe** sekcji:
   
    ![Niestandardowe wdrożenie usługi HDInsight](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Grupa zasobów**: Utwórz grupę lub wybierz istniejący. Ta grupa zawiera klastra usługi HDInsight.

    * **Lokalizacja**: Wybierz lokalizację lokalizacji geograficznej blisko.

    * **Podstawowa nazwa klastra**: Ta wartość jest używana jako nazwa podstawowa platformy Spark i Kafka klastrów. Na przykład wprowadzenie **hdi** tworzy Spark klastra o nazwie spark hdi__ i Kafka klastra o nazwie **kafka hdi**.

    * **Nazwa użytkownika logowania klastra**: nazwa użytkownika administratora klastrów platformy Spark i Kafka.

    * **Hasło logowania klastra**: hasła użytkownika administratora klastrów platformy Spark i Kafka.

    * **Nazwa użytkownika SSH**: użytkownika SSH do tworzenia klastrów platformy Spark i Kafka.

    * **Hasło SSH**: hasło dla użytkownika SSH dla klastrów Spark i Kafka.

3. Odczyt **warunków i postanowień**, a następnie wybierz **akceptuję warunki i postanowienia, o których wspomniano**.

4. Ponadto sprawdź **Przypnij do pulpitu nawigacyjnego** , a następnie wybierz **zakupu**. Tworzenie klastrów trwa około 20 minut.

Po utworzeniu zasobów strony podsumowania jest wyświetlany.

![Informacje o grupie zasobów dla sieci wirtualnej i klastrów](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Należy zauważyć, że nazwy klastrów usługi HDInsight są **nazwę BAZOWĄ spark** i **nazwę BAZOWĄ kafka**, gdzie nazwę BAZOWĄ jest podana nazwa do szablonu. Te nazwy można użyć w kolejnych krokach, podczas nawiązywania połączenia z klastrów.

## <a name="get-the-kafka-brokers"></a>Pobierz Kafka brokerzy

Kod w tym przykładzie nawiązanie Kafka brokera hosty w klastrze Kafka. Aby znaleźć adres dwa hosty brokera Kafka, skorzystaj z następującego przykładu programu PowerShell lub Bash:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Po wyświetleniu monitu wprowadź hasło do konta logowania (Administrator) klastra

> [!NOTE]
> W tym przykładzie oczekuje `$CLUSTERNAME` zawierać nazwy klastra Kafka.
>
> W tym przykładzie użyto [jq](https://stedolan.github.io/jq/) narzędzie do analizy danych z dokumentu JSON.

Dane wyjściowe będą podobne do następującego tekstu:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Zapisz te informacje, które jest używane w następujących sekcjach tego dokumentu.

## <a name="get-the-notebooks"></a>Pobierz notebooki

Kod, na przykład opisanych w niniejszym dokumencie jest dostępny na [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>Przekaż notebooki

Wykonaj następujące kroki, aby przekazać notesów z projektu do programu Spark w klastrze usługi HDInsight:

1. W przeglądarce sieci web nawiązać notesu Jupyter w klastrze Spark. Następujący adres URL, Zastąp `CLUSTERNAME` z nazwą użytkownika __Spark__ klastra:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Po wyświetleniu monitu wprowadź logowania do klastra (admin) i hasło użyte podczas tworzenia klastra.

2. Górny prawej części strony, użyj funkcji __przekazać__ przycisk, aby przekazać __strumienia taksówki danych do kafka.ipynb__ pliku do klastra. Wybierz __Otwórz__ uruchomić przekazywanie.

3. Znajdź __strumienia taksówki danych do kafka.ipynb__ wpisu na liście notesów, a następnie wybierz __przekazać__ przycisk obok siebie.

4. Powtórz kroki od 1 do 3, aby załadować __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ notesu.

## <a name="load-taxi-data-into-kafka"></a>Ładowanie danych taksówki do Kafka

Gdy pliki zostały przekazane, wybierz __strumienia taksówki danych do kafka.ipynb__ wpis, aby otworzyć notesu. Postępuj zgodnie z instrukcjami, aby załadować dane do Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Proces taksówki danych przy użyciu strukturalnych przesyłania strumieniowego Spark

Na stronie głównej notesu Jupyter wybierz __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ wpisu. Postępuj zgodnie z instrukcjami notesu do transmisji danych z Kafka i w bazie danych rozwiązania Cosmos Azure przy użyciu strukturalnych przesyłania strumieniowego Spark.

## <a name="next-steps"></a>Kolejne kroki

Teraz, kiedy znasz sposób użycia strukturalnych przesyłania strumieniowego Spark, należy znaleźć w następujących dokumentach, aby dowiedzieć się więcej na temat pracy z Spark, Kafka i bazy danych rozwiązania Cosmos Azure:

* [Sposób użycia platformy Spark przesyłania strumieniowego (DStream) z Kafka](hdinsight-apache-spark-with-kafka.md).
* [Rozpoczynać notesu Jupyter i Spark w usłudze HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Zapraszamy do platformy Azure rozwiązania Cosmos bazy danych](../cosmos-db/introduction.md)