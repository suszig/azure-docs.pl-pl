---
title: "Umożliwia monitorowanie klastrów usługi HDInsight Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi Analiza dzienników Azure do monitorowania zadania uruchomione w klastrze usługi HDInsight."
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
ms.openlocfilehash: c1c74c797449c2fa6d76438f9ec33eaf0fe51af4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Analiza dzienników Azure umożliwia monitorowanie klastrów usługi HDInsight (wersja zapoznawcza)

W tym artykule możesz dowiedzieć się, jak na potrzeby monitorowania operacji klastrów usługi HDInsight Hadoop Analiza dzienników Azure.

Analiza dzienników jest usługą [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) który monitoruje chmury i lokalnych środowiskach utrzymywać ich dostępności i wydajności. Zbiera ona dane generowane przez zasoby w środowiskach chmurowych i lokalnych oraz inne narzędzia do monitorowania, aby przeprowadzać analizę na podstawie wielu źródeł. 

## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**. Przed rozpoczęciem tego samouczka musisz dysponować subskrypcją platformy Azure. Zobacz [Utwórz bezpłatne konto platformy Azure już dzisiaj](https://azure.microsoft.com/free).

* **Klaster Azure HDInsight**. Obecnie można użyć Azure OMS z następujące typy klastrów usługi HDInsight:
    * Usługa Hadoop
    * platforma Spark
    * HBase
    * Storm
    * Kafka
    * Interakcyjne gałęzi

    Aby uzyskać instrukcje dotyczące sposobu tworzenia klastra usługi HDInsight, zobacz [Rozpoczynanie pracy z usługą Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).


* **Obszar roboczy analizy dzienników**. Ten obszar roboczy można traktować jako unikatowy środowiska analizy dzienników z własnych danych repozytorium, źródła danych i rozwiązania. Musi mieć jeden taki obszar roboczy już utworzone, które można skojarzyć z klastrami Azure HDInsight. Aby uzyskać instrukcje, zobacz [Tworzenie obszaru roboczego analizy dzienników](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-azure-log-analytics"></a>Konfigurowanie klastra usługi HDInsight do używania usługi Analiza dzienników Azure

W tej sekcji skonfigurujesz istniejącego klastra usługi HDInsight Hadoop do użycia z obszarem roboczym usługi Analiza dzienników Azure do monitorowania, zadań, dzienników debugowania itd.

1. W portalu Azure, w lewym okienku kliknij **klastrów usługi HDInsight**, a następnie kliknij nazwę klastra, aby skonfigurować usługi Analiza dzienników Azure.

2. W bloku klastra, w lewym okienku kliknij **monitorowanie**.

3. W okienku po prawej stronie kliknij **włączyć**, a następnie wybierz istniejący obszar roboczy analizy dzienników. Kliknij pozycję **Zapisz**.

    ![Włącz monitorowanie klastrów usługi HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Włącz monitorowanie klastrów usługi HDInsight")

4. Gdy klaster jest skonfigurowany do używania analizy dzienników dla monitorowania, zobacz **Otwórz pulpit nawigacyjny OMS** opcji u góry karty. Kliknij przycisk.

    ![Otwórz pulpit nawigacyjny OMS](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "OMS Otwórz pulpit nawigacyjny")

5. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia platformy Azure. Użytkownik jest kierowany do pulpitu nawigacyjnego OMS firmy Microsoft.

    ![Portal Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "portal usługi Operations Management Suite")

## <a name="next-steps"></a>Następne kroki
* [Dodawanie rozwiązania do zarządzania klastrem usługi HDInsight do analizy dzienników](hdinsight-hadoop-oms-log-analytics-management-solutions.md)
