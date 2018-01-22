---
title: "Przekształcanie danych przy użyciu platformy Spark w usłudze Azure Data Factory | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje krok po kroku przekształcania danych za pomocą platformy Spark w usłudze Azure Data Factory."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: 8bd9382ed5a855368533c6bf2305682861c109c0
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Przekształcanie danych w chmurze za pomocą działania platformy Spark w usłudze Azure Data Factory
W tym samouczku użyjesz witryny Azure Portal do utworzenia potoku usługi Data Factory, który przekształca dane przy użyciu działania platformy Spark i połączonej usługi HDInsight na żądanie. Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Tworzenie fabryki danych. 
> * Tworzenie potoku przy użyciu działania Spark.
> * Wyzwalanie uruchomienia potoku
> * Monitorowanie uruchomienia potoku.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="prerequisites"></a>Wymagania wstępne
* **Konto usługi Azure Storage**. Utworzysz skrypt w języku Python i plik wejściowy, a następnie przekażesz je do usługi Azure Storage. Dane wyjściowe programu platformy Spark są przechowywane na tym koncie magazynu. Klaster platformy Spark na żądanie używa tego samego konta magazynu, jako swojego podstawowego magazynu.  
* Zainstalowanie programu **Azure PowerShell**. Wykonaj instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Przekazywanie skryptu w języku Python do konta usługi Blob Storage
1. Utwórz plik w języku Python o nazwie **WordCount_Spark.py** i następującej zawartości: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Zastąp wartość **&lt;storageAccountName&gt;** nazwą swojego konta usługi Azure Storage. Następnie zapisz plik. 
3. W usłudze Azure Blob Storage utwórz kontener o nazwie **adftutorial**, jeśli nie istnieje. 
4. Utwórz folder o nazwie **spark**.
5. Utwórz podfolder o nazwie **script** w folderze **spark**. 
6. Przekaż plik **WordCount_Spark.py** do podfolderu **script**. 


### <a name="upload-the-input-file"></a>Przekazywanie pliku wejściowego
1. Utwórz plik o nazwie **minecraftstory.txt** zawierający tekst. Program platformy Spark zlicza liczbę słów w tym tekście. 
2. Utwórz podfolder o nazwie `inputfiles` w folderze `spark`. 
3. Przekaż `minecraftstory.txt` do podfolderu `inputfiles`. 

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory). Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Nazwa nie jest dostępna — błąd](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
      W niektórych krokach w tym samouczku szybkiego startu zakłada się, że nazwa grupy zasobów to **ADFTutorialResourceGroup**. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Obecnie usługa Data Factory w wersji 2 umożliwia tworzenie fabryk danych tylko w regionach Wschodnie stany USA, Wschodnie stany USA 2 i Europa Zachodnia. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
    ![Strona główna fabryki danych](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić aplikację interfejsu użytkownika usługi Data Factory.

## <a name="create-linked-services"></a>Tworzenie połączonych usług
W tej sekcji zredagujesz dwie połączone usługi: 
    
- **Połączoną usługę Azure Storage**, która łączy konto usługi Azure Storage z fabryką danych. Ten magazyn jest używany przez klaster usługi HDInsight na żądanie. Zawiera on także skrypt platformy Spark do wykonania. 
- **Połączona usługa HDInsight na żądanie**. Usługa Azure Data Factory automatycznie tworzy klaster usługi HDInsight, uruchamia program platformy Spark i usuwa klaster usługi HDInsight, gdy jest on bezczynny przez wstępnie skonfigurowany czas. 

### <a name="create-an-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage

1. Na stronie **Wprowadzenie** przejdź do karty **Edycja** w lewym panelu, jak pokazano na poniższej ilustracji: 

    ![Kafelek Utwórz potok](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Kliknij pozycję **Połączenia** w dolnej części okna, a następnie kliknij pozycję **+ Nowy**. 

    ![Przycisk Nowe połączenie](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. W oknie **Nowa połączona usługa** wybierz pozycję **Azure Blob Storage**, a następnie kliknij pozycję **Kontynuuj**. 

    ![Wybieranie pozycji Azure Blob Storage](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. Wybierz **nazwę konta usługi Azure Storage** i kliknij pozycję **Zapisz**. 

    ![Określanie konta usługi Blob Storage](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>Tworzenie połączonej usługi HDInsight na żądanie

1. Kliknij ponownie przycisk **+ Nowy**, aby utworzyć kolejną połączoną usługę. 
2. W oknie **Nowa połączona usługa** wybierz pozycję **Azure HDInsight**, a następnie kliknij pozycję **Kontynuuj**. 

    ![Wybieranie usługi Azure HDInsight](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. W oknie **Nowa połączona usługa** wykonaj następujące czynności: 

    1. Wprowadź ciąg **AzureHDInsightLinkedService** w polu **Nazwa**.
    2. Upewnij się, że w polu **Typ** wybrano wartość **HDInsight na żądanie**.
    3. Wybierz wartość **AzureStorage1** w polu **Połączona usługa Azure Storage**. Ta połączona usługa została utworzona wcześniej. Jeśli użyto innej nazwy, podaj prawidłową nazwę tego pola. 
    4. Wybierz wartość **spark** jako **Typ klastra**.
    5. Wprowadź **identyfikator jednostki usługi**, który ma uprawnienia do tworzenia klastra usługi HDInsight. Jednostka usługi musi być członkiem roli współautora subskrypcji lub grupy zasobów, gdzie został utworzony klaster. Aby uzyskać szczegółowe informacje, zobacz [Create Azure Active Directory application and service principal (Tworzenie jednostki usługi i aplikacji usługi Azure Active Directory)](../azure-resource-manager/resource-group-create-service-principal-portal.md).
    6. Wprowadź **klucz jednostki usługi**. 
    7. Wybierz tę samą grupę zasobów, która została użyty podczas tworzenia fabryki danych dla **grupy zasobów**. W tej grupie zasobów jest tworzony klaster Spark. 
    8. Rozwiń węzeł **Typ systemu operacyjnego**.
    9. Wprowadź **nazwę** **użytkownika** klastra. 
    10. Wprowadź **hasło** dla użytkownika. 
    11. Kliknij pozycję **Zapisz**. 

        ![Ustawienia połączonej usługi HDInsight](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> Usługa Azure HDInsight ma ograniczenia całkowitej liczby rdzeni, których możesz użyć w każdym obsługiwanym przez nią regionie platformy Azure. Dla połączonej usługi HDInsight na żądanie zostanie utworzony klaster usługi HDInsight w tej samej lokalizacji usługi Azure Storage użytej jako jej podstawowy magazyn. Upewnij się, że masz wystarczająco duże limity przydziału dla klastra, aby można go było pomyślnie utworzyć. Aby uzyskać więcej informacji, zobacz [Set up clusters in HDInsight with Hadoop, Spark, Kafka and more (Konfigurowanie klastrów w usłudze HDInsight za pomocą platform Hadoop, Spark, Kafka i innych)](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="create-a-pipeline"></a>Tworzenie potoku

2. Kliknij przycisk + (plus), a następnie kliknij pozycję **Potok** w menu.

    ![Menu Nowy potok](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. W przyborniku **Działania** rozwiń pozycję **HDInsight**, a następnie przeciągnij i upuść działanie **Spark** z przybornika **Działania** do powierzchni projektanta potoku. 

    ![Przeciąganie i upuszczanie działania Spark](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. We właściwościach okna działania **Spark** u dołu wykonaj następujące czynności: 

    1. Przejdź do karty **Klaster usługi HDI**.
    2. Wybierz usługę **AzureHDInsightLinkedService** utworzoną w poprzednim kroku. 
        
    ![Określanie połączonej usługi HDInsight](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. Przejdź do karty **Skrypt/Jar** i wykonaj następujące czynności: 

    1. Wybierz wartość **AzureStorage1** w polu **Połączona usługa zadania**.
    2. Kliknij pozycję **Przeglądaj magazyn**. 
    3. Przejdź do **folderu adftutorial/spark/script**, wybierz plik **WordCount_Spark.py** i kliknij pozycję **Zakończ**.      

    ![Określanie skryptu platformy Spark](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. Aby zweryfikować potok, kliknij przycisk **Weryfikuj** na pasku narzędzi. Kliknij przycisk ze **strzałką w prawo** (>>), aby zamknąć okno weryfikacji. 
    
    ![Przycisk Weryfikuj](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. Kliknij przycisk **Opublikuj**. Interfejs użytkownika usługi Data Factory publikuje jednostki (połączone usług i potok) do usługi Azure Data Factory. 

## <a name="trigger-a-pipeline-run"></a>Wyzwalanie uruchomienia potoku
Kliknij pozycję **Wyzwól** na pasku narzędzi, a następnie kliknij pozycję **Wyzwól teraz**. 

![Wyzwól teraz](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>Monitorowanie działania potoku

1. Przejdź do karty **Monitorowanie**. Sprawdź, czy widzisz uruchomienie potoku. Utworzenie klastra Spark trwa około 20 minut. 

    ![Monitorowanie uruchomień potoku](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Okresowo klikaj pozycję **Odśwież**, aby sprawdzić stan uruchomienia potoku. 

    ![Stan uruchomienia potoku](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. Aby wyświetlić uruchomienia działania skojarzone z uruchomieniem potoku, kliknij akcję **Wyświetl uruchomienia działania** w kolumnie Akcje. Aby wrócić do widoku uruchomień potoków, możesz kliknąć pozycję **Potoki** u góry.

    ![Widok Uruchomienia działania](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>Sprawdzanie danych wyjściowych
Sprawdź, czy plik wyjściowy został utworzony w folderze spark/otuputfiles/wordcount kontenera adftutorial. 

![Weryfikacja danych wyjściowych](./media/tutorial-transform-data-spark-portal/verity-output.png)

Ten plik powinien zawierać każdy wyraz z wejściowego pliku tekstowego oraz liczbę powtórzeń wyrazu w pliku. Na przykład: 

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie przekształca dane przy użyciu działania Spark i połączonej usługi HDInsight dostępnej na żądanie. W tym samouczku omówiono: 

> [!div class="checklist"]
> * Tworzenie fabryki danych. 
> * Tworzenie potoku przy użyciu działania Spark.
> * Wyzwalanie uruchomienia potoku
> * Monitorowanie uruchomienia potoku.

Przejdź do następnego samouczka, aby dowiedzieć się, jak przekształcać dane, uruchamiając skrypt programu Hive w klastrze usługi Azure HDInsight, który znajduje się w sieci wirtualnej. 

> [!div class="nextstepaction"]
> [Samouczek: Przekształcanie danych za pomocą programu Hive w usłudze Azure Virtual Network](tutorial-transform-data-hive-virtual-network-portal.md).





