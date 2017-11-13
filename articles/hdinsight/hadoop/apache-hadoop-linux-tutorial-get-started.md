---
title: "Wprowadzenie do usług Hadoop i Hive w usłudze Azure HDInsight | Microsoft Docs"
description: "Dowiedz się, jak tworzyć klastry usługi HDInsight i wykonywać zapytania na danych przy użyciu technologii Hive."
keywords: "wprowadzenie do usługi hadoop,hadoop linux,hadoop szybki start,wprowadzenie do usługi hive,hive szybki start"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: 983c7d1f7e3b562a8b89f6afaf52a3289ab33f17
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Samouczek Hadoop: rozpoczęcie korzystania z usługi Hadoop w usłudze HDInsight

Informacje o sposobie tworzenia klastrów [Hadoop](http://hadoop.apache.org/) w usłudze HDInsight i uruchamiania zadań Hive w usłudze HDInsight. [Apache Hive](https://hive.apache.org/) jest najbardziej popularnym składnikiem w ekosystemie usługi Hadoop. Obecnie usługa HDInsight obejmuje [siedem różnych typów klastrów](apache-hadoop-introduction.md#overview). Każdy typ klastra obsługuje inny zestaw składników. Wszystkie typy klastrów obsługują technologię Hive. Aby uzyskać listę obsługiwanych składników w usłudze HDInsight, zobacz artykuł [Nowości w wersjach klastra Hadoop dostarczanych z usługą HDInsight](../hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka potrzebna będzie:

* **Subskrypcja platformy Azure**: aby utworzyć bezpłatne konto próbne na jeden miesiąc, przejdź do [azure.microsoft.com/free](https://azure.microsoft.com/free).

## <a name="create-cluster"></a>Tworzenie klastra

Większość zadań usługi Hadoop to zadania wsadowe. Tworzysz klaster, uruchamiasz pewne zadania, a następnie usuwasz klaster. W tej sekcji tworzysz klaster Hadoop w usłudze HDInsight przy użyciu [szablonu usługi Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy.md). Znajomość szablonów usługi Resource Manager nie jest wymagana do korzystania z tego samouczka. Inne metody tworzenia klastrów i opis właściwości używanych w tym samouczku znajdziesz w artykule [Tworzenie klastrów usługi HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Użyj selektora u góry strony, aby wybrać opcje tworzenia klastra.

Użyty w tym samouczku szablon usługi Resource Manager znajduje się w serwisie [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). 

1. Kliknij poniższy obraz, aby zalogować się do platformy Azure i otworzyć szablon usługi Resource Manager w witrynie Azure Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Wprowadź lub wybierz poniższe wartości:
   
    ![Szablon Menedżera zasobów wprowadzenia do usługi HDInsight Linux w portalu](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Wdrażanie klastra Hadoop w usłudze HDInsight przy użyciu witryny Azure Portal i szablonu menedżera grupy zasobów").
   
    * **Subskrypcja**: wybierz subskrypcję platformy Azure.
    * **Grupa zasobów**: utwórz grupę zasobów lub wybierz istniejącą.  Grupa zasobów jest kontenerem składników platformy Azure.  W tym przypadku grupa zasobów zawiera klaster usługi HDInsight i zależne konto usługi Azure Storage. 
    * **Lokalizacja**: wybierz lokalizację platformy Azure, w której chcesz utworzyć klaster.  Wybierz lokalizację znajdującą się blisko, aby zapewnić lepszą wydajność. 
    * **Typ klastra**: na potrzeby tego samouczka wybierz opcję **hadoop**.
    * **Nazwa klastra**: wprowadź nazwę klastra Hadoop.
    * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to **admin**.
    * **Nazwa użytkownika i hasło SSH**: domyślna nazwa użytkownika to **sshuser**.  Tę nazwę można zmienić. 
     
    Niektóre właściwości zostały umieszczone w kodzie w szablonie.  Te wartości można skonfigurować z szablonu.

    * **Lokalizacja**: lokalizacja klastra i zależne konto magazynu używają tej samej lokalizacji co grupa zasobów.
    * **Wersja klastra**: 3.5
    * **Typ systemu operacyjnego**: Linux
    * **Liczba węzłów procesu roboczego**: 2

     Każdy klaster zależy od [konta usługi Azure Storage](../hdinsight-hadoop-use-blob-storage.md) lub od [konta usługi Azure Data Lake](../hdinsight-hadoop-use-data-lake-store.md). Jest ono określane jako domyślne konto magazynu. Klaster usługi HDInsight i jego domyślne konto magazynu muszą znajdować się wspólnie w tym samym regionie Azure. Usunięcie klastrów nie powoduje usunięcia konta magazynu. 
     
     Aby uzyskać więcej informacji o tych właściwościach, zobacz [Create Hadoop clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) (Tworzenie klastrów platformy Hadoop w usłudze HDInsight).

3. Wybierz pozycję **Wyrażam zgodę na powyższe warunki i postanowienia** i pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Kup**. Na pulpicie nawigacyjnym portalu powinien zostać wyświetlony nowy kafelek zatytułowany **Wdrażanie szablonu wdrożenia**. Utworzenie klastra trwa około 20 minut. Po utworzeniu klastra napis na kafelku zmieni się na podaną nazwę grupy zasobów. Portal automatycznie otworzy grupę zasobów w nowym bloku. Na liście jest wyświetlany klaster i domyślny magazyn.
   
    ![Grupa zasobów wprowadzenia do usługi HDInsight Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Grupa zasobów klastra usługi Azure HDInsight").

4. Kliknij nazwę klastra, aby otworzyć klaster w nowym bloku.

   ![Ustawienia klastra wprowadzenia do usługi HDInsight Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "Właściwości klastra usługi HDInsight")


## <a name="run-hive-queries"></a>Uruchamianie zapytań Hive
[Apache Hive](hdinsight-use-hive.md) jest najbardziej popularnym składnikiem używanym w usłudze HDInsight Istnieje wiele sposobów uruchamiania zadań Hive w usłudze HDInsight. W tym samouczku użyjesz widoku Hive narzędzia Ambari, korzystając z portalu. Aby poznać inne metody przesyłania zadań Hive, zobacz temat [Używanie Hive w usłudze HDInsight](hdinsight-use-hive.md).

1. Z poprzedniego zrzutu ekranu kliknij pozycję **Pulpit nawigacyjny klastra**, a następnie kliknij pozycję **Pulpit nawigacyjny klastra usługi HDInsight**.  Aby otworzyć narzędzie Ambari, możesz także przejść pod adres **https://&lt;ClusterName>.azurehdinsight.net**, gdzie &lt;ClusterName> to klaster utworzony w poprzedniej sekcji.
2. Wprowadź nazwę użytkownika Hadoop i hasło określone w poprzedniej części. Domyślna nazwa użytkownika to **admin**.
3. Otwórz widok **Hive View** pokazany na poniższym zrzucie ekranu:
   
    ![Wybieranie widoków Ambari](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "Menu przeglądarki HDInsight Hive").
4. W części strony **Query Editor** (Edytor zapytań) wklej poniższe instrukcje HiveQL do arkusza:
   
        SHOW TABLES;
   
   > [!NOTE]
   > Średnik jest wymagany przez Hive.       
   > 
   > 
5. Kliknij przycisk **Execute** (Wykonaj). Sekcja **Query Process Results** (Wyniki przetwarzania zapytania) powinna pojawić się poniżej edytora zapytań, prezentując informacje o zadaniu. 
   
    Po zakończeniu przetwarzania zapytania sekcja **Query Process Results** (Wyniki przetwarzania zapytania) będzie prezentować wyniki operacji. Powinna być widoczna jedna tabela o nazwie **hivesampletable**. Ta przykładowa tabela składnika Hive jest dostarczana z wszystkimi klastrami usługi HDInsight.
   
    ![Widoki usługi HDInsight Hive](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "Edytor zapytań widoku usługi HDInsight Hive").
6. Powtórz kroki 4 i 5, aby uruchomić następujące zapytanie:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > Zwróć uwagę na rozwijane menu **Save results** (Zapisz wyniki) w lewym górnym rogu sekcji **Query Process Results** (Wyniki przetwarzania zapytania). Służy ono do pobierania wyników lub zapisywania ich w magazynie usługi HDInsight w formacie pliku CSV.
   > 
   > 
7. Kliknij przycisk **History** (Historia) w celu uzyskania listy zadań.

Po zakończeniu zadania Hive można [wyeksportować wyniki do bazy danych Azure SQL lub bazy danych programu SQL Server](apache-hadoop-use-sqoop-mac-linux.md), można także [wizualizować wyniki za pomocą programu Excel](apache-hadoop-connect-excel-power-query.md). Aby uzyskać więcej informacji o korzystaniu z Hive w usłudze HDInsight, zobacz [Używanie Hive i HiveQL z usługą Hadoop w usłudze HDInsight do analizy przykładowego pliku Apache log4j](hdinsight-use-hive.md).

## <a name="clean-up-the-tutorial"></a>Czyszczenie na koniec samouczka
Po ukończeniu korzystania z samouczka warto usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane. 

> [!NOTE]
> Przy użyciu [fabryki danych Azure](../hdinsight-hadoop-create-linux-clusters-adf.md) można tworzyć klastry usługi HDInsight na żądanie i skonfigurować ustawienie TimeToLive, aby usuwać te klastry automatycznie. 
> 
> 

**Usuwanie klastra i/lub domyślnego konta magazynu**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na pulpicie nawigacyjnym portalu kliknij kafelek z nazwą grupy zasobów użytą podczas tworzenia klastra.
3. Kliknij przycisk **Usuń** w bloku zasobów, aby usunąć grupę zasobów zawierającą klaster i domyślne konto magazynu, lub kliknij nazwę klastra na kafelku **Zasoby**, a następnie kliknij przycisk **Usuń** w bloku klastra. Uwaga: usunięcie grupy zasobów powoduje usunięcie konta magazynu. Jeśli chcesz zachować konta magazynu, wybierz opcję usunięcia tylko klastra.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

W razie problemów podczas tworzenia klastrów usługi HDInsight zapoznaj się z [wymaganiami dotyczącymi kontroli dostępu](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Następne kroki
Po przeczytaniu tego samouczka umiesz utworzyć oparty na systemie Linux klaster usługi HDInsight przy użyciu szablonu usługi Resource Manager oraz wykonywać podstawowe zapytania Hive.

Aby dowiedzieć się więcej na temat analizowania danych za pomocą usługi HDInsight, zobacz następujące artykuły:

* Aby dowiedzieć się więcej o korzystaniu z programu Hive z usługą HDInsight, w tym poznać sposoby wykonywania zapytań Hive z programu Visual Studio, zobacz artykuł [Korzystanie z programu Hive z usługą HDInsight](hdinsight-use-hive.md).
* Aby dowiedzieć się więcej na temat języka Pig używanego do przekształcania danych, zobacz [Korzystanie z języka Pig z usługą HDInsight](hdinsight-use-pig.md).
* Aby dowiedzieć się więcej o MapReduce, czyli sposobie pisania programów przetwarzających dane w usłudze Hadoop, zobacz [Używanie MapReduce z usługą HDInsight](hdinsight-use-mapreduce.md).
* Aby dowiedzieć się więcej o używaniu narzędzi HDInsight Tools for Visual Studio do analizowania danych w usłudze HDInsight, zobacz [Wprowadzenie do używania narzędzi Visual Studio Hadoop dla usługi HDInsight](apache-hadoop-visual-studio-tools-get-started.md).

Jeśli chcesz już zacząć korzystać z własnych danych i dowiedzieć się więcej o sposobie przechowywania danych w usłudze HDInsight lub sposobie pobierania danych do usługi HDInsight, zobacz następujące tematy:

* Aby uzyskać informacje o sposobie używania usługi Azure Storage przez usługę HDInsight, zobacz [Używanie usługi Azure Storage z usługą HDInsight](../hdinsight-hadoop-use-blob-storage.md).
* Aby uzyskać informacje na temat przekazywania danych do usługi HDInsight, zobacz [Przekazywanie danych do usługi HDInsight](../hdinsight-upload-data.md).

Jeśli chcesz dowiedzieć się więcej o tworzeniu klastra usługi HDInsight i zarządzaniu nim, zobacz następujące tematy:

* Aby uzyskać więcej informacji na temat zarządzania opartym na systemie Linux klastrem usługi HDInsight, zobacz [Zarządzanie klastrami usługi HDInsight za pomocą narzędzia Ambari](../hdinsight-hadoop-manage-ambari.md).
* Aby dowiedzieć się więcej na temat opcji, które można wybrać podczas tworzenia klastra usługi HDInsight, zobacz [Tworzenie klastra usługi HDInsight w systemie Linux przy użyciu niestandardowych opcji](../hdinsight-hadoop-provision-linux-clusters.md).
* Jeśli znasz system Linux i usługę Hadoop, ale chcesz poznać szczegóły dotyczące usługi Hadoop w usłudze HDInsight, zobacz [Praca z usługą HDInsight w systemie Linux](../hdinsight-hadoop-linux-information.md). Artykuł zawiera następujące informacje:
  
  * Adresy URL dla usług uruchamianych w klastrze, takich jak Ambari i WebHCat
  * Lokalizacja plików usługi Hadoop oraz przykłady lokalnego systemu plików
  * Korzystanie z usługi Azure Storage (WASB) zamiast systemu plików HDFS jako domyślnego magazynu danych

[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


