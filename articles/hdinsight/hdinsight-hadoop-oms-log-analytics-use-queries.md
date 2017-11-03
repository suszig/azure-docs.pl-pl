---
title: "Zapytanie usługi Analiza dzienników Azure do monitorowania klastrów usługi HDInsight Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak uruchamiać zapytania dotyczące usługi Analiza dzienników Azure, aby monitorować zadania uruchomione w klastrze usługi HDInsight."
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
ms.date: 09/11/2017
ms.author: nitinme
ms.openlocfilehash: 8fe91bed69a1c06367346041d8caba4aaee4c82a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Analiza dzienników Azure zapytania do monitorowania klastrów usługi HDInsight (wersja zapoznawcza)

W tym artykule przyjrzymy się niektóre scenariusze dotyczące sposobu używania usługi Analiza dzienników Azure z klastrami Azure HDInsight. Są trzy najbardziej typowych scenariuszy:

* Analizowanie metrykę klastra usługi HDInsight w OMS
* Wyszukiwanie komunikatów dziennika określone dla klastrów usługi HDInsight
* Tworzenie na podstawie zdarzeń występujących w klastry alertów

## <a name="prerequisites"></a>Wymagania wstępne

* Należy skonfigurować klaster usługi HDInsight, aby użyć usługi Analiza dzienników Azure. Aby uzyskać instrukcje, zobacz [Użyj Azure Log Analytics z klastrami HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* Należy dodać rozwiązania do zarządzania specyficznych dla klastra usługi HDInsight z obszarem roboczym pakietu OMS zgodnie z opisem w [rozwiązań do zarządzania klastra Dodaj HDInsight do analizy dzienników](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics-in-oms"></a>Analizowanie metrykę klastra usługi HDInsight w OMS

W tej sekcji możemy wykonywania kroków do wyszukiwania określonych metryk dla klastra usługi HDInsight.

1. Otwórz pulpit nawigacyjny OMS. W portalu Azure, otwórz blok klastra usługi HDInsight, który skojarzone z usługi Analiza dzienników Azure, kliknij kartę monitorowanie, a następnie kliknij pozycję **Otwórz pulpit nawigacyjny OMS**.

    ![Otwórz pulpit nawigacyjny OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS Otwórz pulpit nawigacyjny")

2. Na pulpicie nawigacyjnym OMS z ekranu głównego, kliknij przycisk **wyszukiwania dziennika**.

    ![Otwórz dziennik wyszukiwania](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Otwórz dziennik wyszukiwania")

3. W oknie Dziennik wyszukiwania w **tutaj wyszukiwania Begin** polu tekstowym `*` aby wyszukać wszystkie metryki wszystkie dostępne metryki dla wszystkich klastrów HDInsight skonfigurowana do używania usługi Analiza dzienników Azure. Naciśnij klawisz ENTER.

    ![Wyszukaj wszystkie metryki](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Wyszukaj wszystkie metryki")

4. Powinny pojawić się dane wyjściowe podobne do następujących.

    ![Wyszukaj wszystkie wyniki metryki](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Wyszukaj wszystkie metryki danych wyjściowych.")

5. W lewym okienku w obszarze **typu** kategorii, wyszukiwania, a metryki, które chcesz odnajdywać dogłębną analizę. Ten samouczek umożliwia pobranie `metrics_resourcemanager_queue_root_default_CL`. Zaznacz pole wyboru odpowiadające metrykę, a następnie kliknij przycisk **Zastosuj**.

    > [!NOTE]
    > Może zajść potrzeba kliknięcia przycisku **[+] więcej** przycisk, aby znaleźć metryki, którego szukasz. Ponadto **Zastosuj** przycisku jest w dolnej części listy, więc musi przewiń go wyświetlać.
    > 
    >    
    Zwróć uwagę, że zapytanie w polu tekstowym teraz zmienia się do przedstawionego w polu wyróżnione na poniższym zrzucie ekranu:

    ![Wyszukaj określonych metryk](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "wyszukiwanie określonych metryk")

6. Użytkownik może teraz głębiej do szczegółów tej określonej metryki. Na przykład można dostosować teraz istniejące dane wyjściowe na podstawie średniej zasoby używane w 10-minutowych interwałach według nazwy klastra. Wpisz poniższe zapytanie w polu tekstowym zapytania.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure avg(UsedAMResourceMB_d) by ClusterName_s interval 10minute

    ![Wyszukaj określonych metryk](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics.png "wyszukiwanie określonych metryk")

7. Zamiast uściślenie na podstawie średniej zasoby używane, możesz użyć następującego zapytania uściślić wyniki na podstawie, gdy użyte zasoby maksymalną (a także percentyl 90 i 95) w oknie 10 minut.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure max(UsedAMResourceMB_d) , pct95(UsedAMResourceMB_d), pct90(UsedAMResourceMB_d)  by ClusterName_s interval 10minute

    ![Wyszukaj określonych metryk](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics-1.png "wyszukiwanie określonych metryk")

## <a name="search-for-specific-log-messages-in-hdinsight-clusters"></a>Wyszukiwanie komunikatów dziennika określonych w klastrach usługi HDInsight

W tej sekcji możemy przeprowadzenie kroki, aby wyglądały określonego okna czasowego komunikaty o błędach. Kroki opisane w tym miejscu są tylko jeden przykład, w jaki sposób można przyjeździe komunikat o błędzie można są zainteresowani. Można użyć dowolnej właściwości, która jest dostępna wyszukać błędy, które próbujesz odnaleźć.

1. Otwórz pulpit nawigacyjny OMS. W portalu Azure, otwórz blok klastra usługi HDInsight, który skojarzone z usługi Analiza dzienników Azure, kliknij kartę monitorowanie, a następnie kliknij pozycję **Otwórz pulpit nawigacyjny OMS**.

    ![Otwórz pulpit nawigacyjny OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS Otwórz pulpit nawigacyjny")

2. Na pulpicie nawigacyjnym OMS z ekranu głównego, kliknij przycisk **wyszukiwania dziennika**.

    ![Otwórz dziennik wyszukiwania](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Otwórz dziennik wyszukiwania")

3. W oknie Dziennik wyszukiwania w **tutaj wyszukiwania Begin** polu tekstowym `"Error"` (ze znakami cudzysłowu) do wyszukania wszystkich komunikatów o błędach dla wszystkich klastrów HDInsight skonfigurowana do używania usługi Analiza dzienników Azure. Naciśnij klawisz ENTER.

    ![Wyszukaj wszystkie błędy](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors.png "wyszukiwanie wszystkich błędów")

4. Powinny pojawić się dane wyjściowe podobne do następujących.

    ![Wyszukaj wszystkie dane wyjściowe błędów](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Wyszukaj wszystkie błędy danych wyjściowych")

5. W lewym okienku w obszarze **typu** kategorii, wyszukiwania typu błąd, który chcesz wyświetlić dogłębną analizę. Ten samouczek umożliwia pobranie `log_sparkappsexecutors_CL`. Zaznacz pole wyboru odpowiadające metrykę, a następnie kliknij przycisk **Zastosuj**.

    ![Wyszukaj określonych błędów](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error.png "wyszukiwanie określonego błędu")

        
6. Należy zauważyć, że teraz zmiany pokazano w poniższym polu wyróżnione oraz wyniki zapytania w polu tekstowym są precyzyjnych tylko w celu wyświetlenia błędu wybranego typu.

    ![Wyszukaj określone błędy w danych wyjściowych](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-output.png "wyszukiwania dla określonych błędów w danych wyjściowych")

7. Użytkownik może teraz szczegółowej analizy w ramach tej listy błędu za pomocą opcji dostępnych w okienku po lewej stronie. Na przykład można dostosować kwerendę tylko przyjrzeć się komunikaty o błędach z węzłem procesu roboczego określone.

    ![Wyszukaj określone błędy w danych wyjściowych](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "wyszukiwania dla określonych błędów w danych wyjściowych")

8. Można dodatkowo stref w na czas uważasz, że wystąpił błąd, wybierając odpowiedni czas w lewym okienku.

    ![Wyszukaj określone błędy w danych wyjściowych](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "wyszukiwania dla określonych błędów w danych wyjściowych")

9. Jesteś teraz do określonego błędu, którego szukasz. Możesz kliknąć **[+] Pokaż więcej** aby przyjrzeć się komunikat błędu.

    ![Wyszukaj określone błędy w danych wyjściowych](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "wyszukiwania dla określonych błędów w danych wyjściowych")

## <a name="create-alerts-to-track-events"></a>Tworzenie alertów do śledzenia zdarzeń

Pierwszy krok w celu utworzenia alertu jest na zapytania oparte na zostanie wyzwolony alert. Dla uproszczenia umożliwia Użyj następującego zapytania, który zawiera listę wszystkich nieudanych aplikacji uruchamianych w klastrach HDInsight.

    * (Typ = metrics_resourcemanager_queue_root_default_CL) AppsFailed_d > 0 

Można użyć dowolnego zapytania, który ma być tworzony alert.

1. Otwórz pulpit nawigacyjny OMS. W portalu Azure, otwórz blok klastra usługi HDInsight, który skojarzone z usługi Analiza dzienników Azure, kliknij kartę monitorowanie, a następnie kliknij pozycję **Otwórz pulpit nawigacyjny OMS**.

    ![Otwórz pulpit nawigacyjny OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS Otwórz pulpit nawigacyjny")

2. Na pulpicie nawigacyjnym OMS z ekranu głównego, kliknij przycisk **wyszukiwania dziennika**.

    ![Otwórz dziennik wyszukiwania](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Otwórz dziennik wyszukiwania")

3. W oknie wyszukiwania dziennika w **tutaj wyszukiwania Begin** tekst Wklej zapytanie, na którym chcesz utworzyć alert, naciśnij klawisz ENTER, a następnie kliknij pozycję **Alert** przycisku.

    ![Wprowadź zapytanie w celu utworzenia alertu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "wprowadź zapytanie w celu utworzenia alertu")

4. W **Dodaj regułę alertu** okna, wprowadź zapytanie i inne szczegóły, aby utworzyć alert, a następnie kliknij przycisk **zapisać**.

    ![Wprowadź zapytanie w celu utworzenia alertu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "wprowadź zapytanie w celu utworzenia alertu")

    W tym zrzucie ekranu pokazano tylko wyślemy wiadomość e-mail z powiadomieniem, jeśli kwerenda alertu pobiera wyjściowych.

5. Możesz także edytować lub usunąć istniejącego alertu. Aby to zrobić, z dowolnej strony w portalu OMS kliknij **ustawienia** ikony.

    ![Wprowadź zapytanie w celu utworzenia alertu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "wprowadź zapytanie w celu utworzenia alertu")

6. Z **ustawienia** kliknij przycisk **alerty** do wyświetlenia alertów został utworzony. Można również włączyć lub Wyłącz alert, go edytować lub usunąć go. Aby uzyskać więcej informacji, zobacz [Praca z reguły alertów w analizy dzienników](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Zobacz też

* [Praca z OMS analizy dzienników](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Tworzyć reguły alertów w analizy dzienników](../log-analytics/log-analytics-alerts-creating.md)
