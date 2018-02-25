---
title: "Dodawanie rozwiązania do zarządzania klastrem HDInsight Analiza dzienników Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć widoki niestandardowe dla klastrów usługi HDInsight za pomocą usługi Analiza dzienników Azure."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: f8a073fbf40feaef97053bb1165d63bbca3e3feb
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Dodawanie rozwiązania do zarządzania klastrem usługi HDInsight do analizy dzienników

Usługa HDInsight zapewnia rozwiązania do zarządzania specyficznych dla klastra, które można dodać do usługi Analiza dzienników Azure. [Rozwiązania do zarządzania](../log-analytics/log-analytics-add-solutions.md) dodawania nowych funkcji do [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), zapewniając dodatkowe dane i narzędzi analizy do analizy dzienników. Te rozwiązania zbierania miar wydajności ważne z klastrów HDInsight i zapewniają narzędzia do wyszukiwania metryki. Te rozwiązania udostępniają wizualizacje i pulpity nawigacyjne dla większości klastra obsługiwane w usłudze HDInsight. Przy użyciu metryk, które należy zebrać z rozwiązania, można utworzyć niestandardowe reguły monitorowania i alertów. 

W tym artykule Dowiedz się jak dodać rozwiązania do zarządzania specyficznych dla klastra z obszarem roboczym usługi Operations Management Suite.

## <a name="prerequisites"></a>Wymagania wstępne

* Należy skonfigurować klaster usługi HDInsight, aby użyć usługi Analiza dzienników Azure. Aby uzyskać instrukcje, zobacz [Użyj Azure Log Analytics z klastrami HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="add-cluster-specific-management-solutions"></a>Dodawanie rozwiązania do zarządzania specyficznych dla klastra

W tej sekcji możesz dodać to rozwiązanie do zarządzania klastra HBase z istniejącym obszarem roboczym usługi Operations Management Suite.

1. Otwórz klaster HDInsigt w portalu Azure kliknij **monitorowanie**, a następnie kliknij przycisk **Otwórz pulpit nawigacyjny OMS**.

    ![Otwórz pulpit nawigacyjny usługi Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "OMS Otwórz pulpit nawigacyjny")

1. Na pulpicie nawigacyjnym OMS kliknij **galerii rozwiązań** lub **Widok projektanta** ikonę w lewym okienku.

    ![Dodaj rozwiązanie do zarządzania w Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Dodaj rozwiązanie do zarządzania w Operations Management Suite")

2. W galerii rozwiązań kliknij jedną z następujących Kafelki:

    - HDInsight Hadoop monitorowania
    - HDInsight HBase monitorowania (wersja zapoznawcza)
    - HDInsight Kafka monitorowania
    - HDInsight Storm monitorowania
    - Monitorowanie Spark w usłudze HDInsight

3. Na następnym ekranie kliknij **Dodaj**.  Poniższy zrzut ekranu przedstawia przycisk Dodaj monitorowania bazy danych HBase.

     ![Dodaj rozwiązanie do zarządzania HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "HBase Dodaj rozwiązanie do zarządzania")

4. Widać kafelka na pulpicie nawigacyjnym OMS rozwiązania do zarządzania HBase. Klaster skojarzony z pakietem Operations Management Suite (jako część z warunkiem wstępnym w tym artykule) w przypadku klastra HBase, Kafelek zawiera nazwę klastra, a liczba węzłów w klastrze.

    ![Rozwiązanie do zarządzania HBase dodane](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "HBase dodać rozwiązanie do zarządzania")

## <a name="next-steps"></a>Kolejne kroki

* [Analiza dzienników Azure zapytania do monitorowania klastrów usługi HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Zobacz także

* [Praca z Operations Management Suite Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Tworzyć reguły alertów w analizy dzienników](../log-analytics/log-analytics-alerts-creating.md)
