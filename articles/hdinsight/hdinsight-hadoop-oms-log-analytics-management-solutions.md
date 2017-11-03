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
ms.date: 09/12/2017
ms.author: nitinme
ms.openlocfilehash: 21b474e37ef0a6037e05ee1fe8e5088cb3e3601d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics-preview"></a>Dodawanie rozwiązania do zarządzania klastrem usługi HDInsight do analizy dzienników (wersja zapoznawcza)

Usługa HDInsight zapewnia rozwiązania do zarządzania specyficznych dla klastra, które można dodać do usługi Analiza dzienników Azure. [Rozwiązania do zarządzania](../log-analytics/log-analytics-add-solutions.md) dodają funkcjonalność pakietu OMS i udostępniają usłudze Log Analytics dodatkowe narzędzia do obsługi danych i analizy. Te rozwiązania zbierania miar wydajności ważne z klastrów HDInsight i zapewniają narzędzia do wyszukiwania metryki. Te rozwiązania udostępniają wizualizacje i pulpity nawigacyjne dla większości klastra obsługiwane w usłudze HDInsight. Przy użyciu metryk, które należy zebrać z rozwiązania, można utworzyć niestandardowe reguły monitorowania i alertów. 

W tym artykule Dowiedz się jak dodać rozwiązania do zarządzania specyficznych dla klastra z obszarem roboczym pakietu OMS.

## <a name="prerequisites"></a>Wymagania wstępne

* Należy skonfigurować klaster usługi HDInsight, aby użyć usługi Analiza dzienników Azure. Aby uzyskać instrukcje, zobacz [Użyj Azure Log Analytics z klastrami HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="add-cluster-specific-management-solutions"></a>Dodawanie rozwiązania do zarządzania specyficznych dla klastra

W tej sekcji możesz dodać to rozwiązanie do zarządzania klastra HBase z istniejącym obszarem roboczym pakietu OMS. Podobnymi rozwiązaniami dla innych typów klastra usługi HDInsight będzie wkrótce dostępna.

1. Otwórz pulpit nawigacyjny OMS. W portalu Azure, otwórz blok klastra usługi HDInsight, który skojarzone z usługi Analiza dzienników Azure, kliknij kartę monitorowanie, a następnie kliknij pozycję **Otwórz pulpit nawigacyjny OMS**.

    ![Otwórz pulpit nawigacyjny OMS](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "OMS Otwórz pulpit nawigacyjny")

1. Na pulpicie nawigacyjnym OMS kliknij **galerii rozwiązań** lub ikonę Projektant widoków, w lewym okienku.

    ![Dodaj rozwiązanie do zarządzania w OMS](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Dodaj rozwiązanie do zarządzania w OMS")

2. Odnaleziono w galerii rozwiązań **monitorowania HBase HDInsight**, a następnie kliknij.

    ![Znajdź rozwiązania do zarządzania HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/find-hbase-management-solution.png "znaleźć rozwiązania do zarządzania HBase")

3. Na następnym ekranie kliknij **Dodaj**.

     ![Dodaj rozwiązanie do zarządzania HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "HBase Dodaj rozwiązanie do zarządzania")

4. Powinna zostać wyświetlona kafelka na pulpicie nawigacyjnym OMS rozwiązania do zarządzania HBase. W przypadku klastra, który jest skojarzony z usługą OMS (jako część z warunkiem wstępnym w tym artykule) klastra HBase Kafelek zawiera nazwę klastra, a liczba węzłów w klastrze.

    ![Rozwiązanie do zarządzania HBase dodane](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "HBase dodać rozwiązanie do zarządzania")

## <a name="next-steps"></a>Następne kroki

* [Analiza dzienników Azure zapytania do monitorowania klastrów usługi HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Zobacz też

* [Praca z OMS analizy dzienników](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Tworzyć reguły alertów w analizy dzienników](../log-analytics/log-analytics-alerts-creating.md)
