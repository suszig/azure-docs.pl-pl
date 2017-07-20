---
title: "Zwiększenie skalowalności klastra Apache Kafka — Azure HDInsight | Microsoft Docs"
description: "Dowiedz się, jak skonfigurować zarządzane dyski klastra Apache Kafka w usłudze Azure HDInsight w celu zwiększenia skalowalności."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/14/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 1b3e0d06c8b25158e421f02b587b4ae4836d80ad
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---

# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Konfigurowanie magazynu i skalowalności klastra Apache Kafka w usłudze HDInsight

Dowiedz się, jak skonfigurować liczbę zarządzanych dysków używanych przez klaster Apache Kafka w usłudze HDInsight.

Platforma Kafka w usłudze HDInsight używa dysku lokalnego maszyn wirtualnych w klastrze usługi HDInsight. Ze względu na duże obciążenie we/wy platformy Kafka usługa [Azure Managed Disks](../storage/storage-managed-disks-overview.md) jest używana do zapewnienia wysokiej przepływności i zwiększenia miejsca do magazynowania w każdym węźle. Jeśli platforma Kafka korzysta z tradycyjnych wirtualnych dysków twardych (VHD), rozmiar każdego węzła nie przekracza 1 TB. W przypadku dysków zarządzanych można użyć wielu dysków, aby osiągnąć 16 TB pamięci dla każdego węzła w klastrze.

Poniższy diagram przedstawia porównanie platformy Kafka w usłudze HDInsight przed użyciem dysków zarządzanych i platformy Kafka w usłudze HDInsight z dyskami zarządzanymi:

![Diagram przedstawiający porównanie platformy Kafka w usłudze HDInsight z użyciem jednego wirtualnego dysku twardego na maszynę wirtualną oraz z użyciem wielu dysków zarządzanych na maszynę wirtualną](./media/hdinsight-apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Konfigurowanie dysków zarządzanych: witryna Azure Portal

1. Aby zapoznać się z typowymi czynnościami tworzenia klastra przy użyciu witryny, wykonaj kroki opisane w temacie [Tworzenie klastra usługi HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md). Nie wykonuj procesu tworzenia w witrynie.

2. W bloku __Rozmiar klastra__ określ liczbę dysków w polu __Liczba dysków na węzeł procesu roboczego__.

    > [!NOTE]
    > Można wybrać typ dysku zarządzanego __Standardowy__ (HDD) lub __Premium__ (SSD). Dyski w warstwie Premium są używane przez maszyny wirtualne serii DS i GS. Wszystkie pozostałe typy maszyn wirtualnych korzystają z dysków standardowych.

    ![Obraz bloku rozmiaru klastra z wyróżnionymi dyskami dla każdego węzła procesu roboczego](./media/hdinsight-apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Konfigurowanie dysków zarządzanych: szablon usługi Resource Manager

Aby kontrolować liczbę dysków używanych przez węzły procesu roboczego w klastrze Kafka, użyj następującej sekcji szablonu:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Kompletny szablon przedstawiający sposób konfigurowania dysków zarządzanych można znaleźć pod adresem [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Następne kroki

Więcej informacji na temat pracy z klastrem Kafka w usłudze HDInsight można znaleźć w następujących dokumentach:

* [Tworzenie repliki platformy Kafka w usłudze HDInsight przy użyciu narzędzia MirrorMaker](hdinsight-apache-kafka-mirroring.md)
* [Używanie systemu Apache Storm z platformą Kafka w usłudze HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Używanie platformy Apache Spark z platformą Kafka w usłudze HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Nawiązywanie połączenia z platformą Kafka za pośrednictwem sieci wirtualnej platformy Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)

* [Blog usługi HDInsight zawierający informacje na temat dysków zarządzanych na platformie Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
