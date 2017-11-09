---
title: "Wprowadzenie do platformy Apache Kafka w usłudze HDInsight Azure | Microsoft Docs"
description: "Uzyskaj informacje o platformie Apache Kafka w usłudze HDInsight: artykuł zawiera opis platformy, jej działania oraz informacje o tym, gdzie można znaleźć przykłady i informacje ułatwiające rozpoczęcie pracy."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/23/2017
ms.author: larryfr
ms.openlocfilehash: 5e05f07f2d9a15ab57f993e88b7cdeb03550f8ac
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Wprowadzenie do platformy Apache Kafka w usłudze HDInsight (wersja zapoznawcza)

[Apache Kafka](https://kafka.apache.org) to rozproszona platforma przesyłania strumieniowego typu „open source”, która umożliwia tworzenie aplikacji i potoków danych przesyłania strumieniowego w czasie rzeczywistym. Ponadto platforma Kafka oferuje funkcję brokera komunikatów, która działa podobnie do kolejki komunikatów. Pozwala ona publikować i subskrybować w nazwanych strumieniach danych. Platforma Kafka w usłudze HDInsight to zarządzana wysoce skalowalna usługa o wysokiej dostępności, która działa w chmurze platformy Microsoft Azure.

## <a name="why-use-kafka-on-hdinsight"></a>Dlaczego warto używać platformy Kafka w usłudze HDInsight?

Platforma Kafka oferuje następujące funkcje:

* Wzorzec przesyłania komunikatów dotyczących publikowania i subskrybowania: platforma Kafka udostępnia interfejs API producenta do publikowania rekordów w temacie platformy Kafka. Interfejs API klienta jest używany podczas subskrybowania tematu.

* Przetwarzanie strumienia: platforma Kafka jest często używana z systemem Apache Storm lub platformą Spark na potrzeby przetwarzania strumienia w czasie rzeczywistym. Na platformie Kafka 0.10.0.0 (HDInsight 3.5) wprowadzono interfejs API przesyłania strumieniowego, który umożliwia tworzenie rozwiązań do transmisji strumieniowej bez konieczności stosowania rozwiązań Storm i Spark.

* Skalowanie w poziomie: platforma Kafka dzieli strumienie na partycje w różnych węzłach klastra usługi HDInsight. Procesy klientów mogą być kojarzone z poszczególnymi partycjami, aby umożliwić równoważenie obciążenia podczas korzystania z rekordów.

* Dostarczanie w określonej kolejności: rekordy na każdej partycji są przechowywane w strumieniu w tej samej kolejności, w której zostały odebrane. Skojarzenie jednego procesu klienta z jedną partycją pozwala zagwarantować, że rekordy są przetwarzane we właściwej kolejności.

* Odporność na uszkodzenia: partycje mogą być replikowane między węzłami w celu zapewnienia odporności na uszkodzenia.

* Integracja z usługą Azure Managed Disks: usługa Managed Disks zapewnia wyższą skalowalność i przepływność dysków używanych przez maszyny wirtualne w klastrze usługi HDInsight.

    Usługa Managed Disks jest domyślnie włączona dla platformy Kafka w usłudze HDInsight. Liczbę dysków w węźle można skonfigurować podczas tworzenia usługi HDInsight. Aby uzyskać więcej informacji o usłudze Managed Disks, zobacz artykuł [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

    Aby uzyskać informacje dotyczące konfigurowania usługi Managed Disks na platformie Kafka w usłudze HDInsight, zobacz artykuł [Increase scalability of Kafka on HDInsight](apache-kafka-scalability.md) (Zwiększanie skalowalności platformy Kafka w usłudze HDInsight).

## <a name="use-cases"></a>Przypadki zastosowań

* **Komunikaty**: platforma Kafka obsługuje wzorzec przesyłania komunikatów dotyczących publikowania i subskrybowania, dlatego jest często używana jako broker komunikatów.

* **Śledzenie działań**: platforma Kafka rejestruje rekordy w określonej kolejności, dlatego może służyć do śledzenia i ponownego tworzenia działań. Mogą to być na przykład działania użytkownika w witrynie sieci Web lub aplikacji.

* **Agregacja**: przetwarzanie strumienia pozwala agregować informacje z różnych strumieni w celu łączenia i centralizowania informacji w formie danych operacyjnych.

* **Przekształcanie**: przetwarzanie strumienia umożliwia łączenie i urozmaicanie danych z wielu tematów wejściowych w formie tematów wyjściowych.

## <a name="architecture"></a>Architektura

![Konfiguracja klastra Kafka](./media/apache-kafka-introduction/kafka-cluster.png)

Ten diagram przedstawia typową konfigurację platformy Kafka korzystającą z grup konsumentów, partycjonowania i replikacji w celu zapewnienia równoległego odczytu zdarzeń przy zachowaniu odporności na uszkodzenia. Usługę Apache ZooKeeper, która zarządza stanem klastra platformy Kafka, zaprojektowano pod kątem obsługi jednoczesnych, odpornych transakcji o małych opóźnieniach. Platforma Kafka przechowuje rekordy w *tematach*. Rekordy są tworzone przez *producentów* i używane przez *odbiorców*. Producenci pobierają rekordy z *brokerów* platformy Kafka. Każdy węzeł procesu roboczego w klastrze usługi HDInsight jest brokerem platformy Kafka. Dla każdego użytkownika jest tworzona jedna partycja, co umożliwia równoległe przetwarzanie danych przesyłanych strumieniowo. Dzięki replikacji zapewniono dystrybucję partycji na węzłach, co gwarantuje ochronę przed awariami węzła (brokera). Partycja wiodąca jest oznaczona symbolem *(L)*. Ruch producenta jest kierowany do partycji wiodącej w każdym węźle przy użyciu stanu zarządzanego przez usługę ZooKeeper.

> [!IMPORTANT]
> Platforma Kafka nie uwzględnia sprzętu podstawowego (montowanego w stelażu) w centrum danych platformy Azure. Aby zapewnić odpowiednie równoważenie partycji w sprzęcie podstawowym, zobacz [Configure high availability of data (Kafka) (Konfigurowanie wysokiej dostępności danych [platforma Kafka])](apache-kafka-high-availability.md).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak korzystać z platformy Apache Kafka w usłudze HDInsight, użyj następujących linków:

* [Wprowadzenie do platformy Kafka w usłudze HDInsight](apache-kafka-get-started.md)

* [Tworzenie repliki platformy Kafka w usłudze HDInsight przy użyciu narzędzia MirrorMaker](apache-kafka-mirroring.md)

* [Używanie systemu Apache Storm z platformą Kafka w usłudze HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Używanie platformy Apache Spark z platformą Kafka w usłudze HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Nawiązywanie połączenia z platformą Kafka za pośrednictwem sieci wirtualnej platformy Azure](apache-kafka-connect-vpn-gateway.md)