---
title: "Wywoływanie programów Spark z fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wywołać Spark programy z fabryki danych Azure za pomocą działania MapReduce."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 0eff48ec65a01a2fc3fa9f7652dd8e1a0fc8dd2a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Wywoływanie programów Spark z potoków fabryki danych Azure

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Działanie gałęzi](data-factory-hive-activity.md)
> * [Działanie pig](data-factory-pig-activity.md)
> * [Działania MapReduce](data-factory-map-reduce.md)
> * [Działaniu przesyłania strumieniowego usługi Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Działanie Spark](data-factory-spark.md)
> * [Działanie wykonywania wsadowego w usłudze Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Działania aktualizowania zasobów w usłudze Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Działania procedur składowanych](data-factory-stored-proc-activity.md)
> * [Działania języka U-SQL usługi Data Lake Analytics](data-factory-usql-activity.md)
> * [Działania niestandardowe .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi fabryka danych Azure, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi fabryka danych, która jest w wersji zapoznawczej, zobacz [Przekształcanie danych za pomocą działania spark w fabryce danych w wersji 2](../transform-data-using-spark.md).

## <a name="introduction"></a>Wprowadzenie
Działanie Spark jest jednym z [działań przekształcania danych](data-factory-data-transformation-activities.md) obsługiwane przez usługi fabryka danych Azure. To działanie uruchamia określony program Spark w klastrze Apache Spark w usłudze Azure HDInsight.    

> [!IMPORTANT]
> - Działanie Spark nie obsługuje klastrów HDInsight Spark, które używają usługi Azure Data Lake Store jako podstawowy magazyn.
> - Działanie Spark obsługuje tylko istniejące (własne) klastry HDInsight Spark. Nie obsługuje usługi HDInsight połączony na żądanie.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Wskazówki: tworzenie potoku z działaniem Spark
Poniżej przedstawiono typowe etapy, aby utworzyć potok fabryki danych z działaniem Spark.  

1. Tworzenie fabryki danych.
2. Utwórz połączoną usługą magazynu Azure, aby połączyć magazyn Azure skojarzony z klastrem Spark w usłudze HDInsight z fabryką danych.     
2. Tworzenie usługi Azure HDInsight połączony do połączenia klastra Apache Spark w usłudze Azure HDInsight z fabryką danych.
3. Tworzenie zestawu danych, który odwołuje się do połączoną usługą magazynu Azure. Obecnie należy określić wyjściowy zestaw danych działania nawet jeśli dostępny jest nie wytworzonych.  
4. Utworzyć potok z działaniem Spark, która odwołuje się do usługi HDInsight połączone utworzone w #2. Działanie jest skonfigurowany z zestawu danych, utworzony w poprzednim kroku jako wyjściowego zestawu danych. Wyjściowy zestaw danych jest podstawę harmonogram (co godzinę, codziennie, itp.). W związku z tym należy określić wyjściowy zestaw danych, nawet jeśli działanie nie tworzy naprawdę danych wyjściowych.

### <a name="prerequisites"></a>Wymagania wstępne
1. Utwórz **ogólnego przeznaczenia konta magazynu Azure** przez następujące instrukcje w tym przewodnikiem: [Utwórz konto magazynu](../../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Utwórz **klastra Apache Spark w usłudze Azure HDInsight** przez następujące instrukcje w samouczku: [klastra Utwórz Apache Spark w usłudze Azure HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Skojarz konto magazynu Azure, który został utworzony w kroku #1 z tym klastrem.  
3. Pobierz i przejrzyj plik skryptu języka python **test.py** w lokalizacji: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Przekaż **test.py** do **pyFiles** folderu w **adfspark** kontenera w magazynie obiektów Blob platformy Azure. Tworzenie kontenera i folderu, jeśli nie istnieją.

### <a name="create-data-factory"></a>Tworzenie fabryki danych
Zacznijmy tworzenie fabryki danych w tym kroku.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Kliknij przycisk **NOWY** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij przycisk **Data Factory**.
3. W **nowa fabryka danych** bloku, wprowadź **SparkDF** dla nazwy.

   > [!IMPORTANT]
   > Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli zostanie wyświetlony błąd: **nazwa fabryki danych "SparkDF" nie jest dostępny**. Zmiana nazwy fabryki danych (na przykład yournameSparkDFdate i spróbuj ponownie utworzyć. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.   
4. Wybierz **subskrypcję Azure**, w ramach której chcesz utworzyć fabrykę danych.
5. Wybierz istniejący **grupy zasobów** lub Utwórz grupę zasobów platformy Azure.
6. Wybierz **Przypnij do pulpitu nawigacyjnego** opcji.  
6. Kliknij przycisk **Utwórz** w bloku **Nowa fabryka danych**.

   > [!IMPORTANT]
   > Aby utworzyć wystąpienia usługi Data Factory, użytkownik musi być członkiem roli [współautora usługi Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na poziomie subskrypcji/grupy zasobów.
7. Zobacz tworzony w fabryce danych **pulpitu nawigacyjnego** portalu Azure w następujący sposób:   
8. Po pomyślnym utworzeniu fabryki danych zostanie wyświetlona strona fabryki danych z zawartością fabryki danych. Jeśli strona fabryki danych nie jest widoczny, kliknij Kafelek fabrykę danych na pulpicie nawigacyjnym.

    ![Blok Fabryka danych](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Tworzenie połączonych usług
W tym kroku utworzysz dwa połączone usługi, co do połączenia z klastrem Spark fabrykę danych i innych, aby połączyć magazyn Azure z fabryką danych.  

#### <a name="create-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage
W tym kroku opisano łączenie konta usługi Azure Storage z fabryką danych. Zestaw danych utworzone w kroku później w tym przewodniku odnosi się do tej połączonej usługi. Usługa HDInsight połączone definiowana w następnym kroku odwołuje się do tej połączonej usługi zbyt.  

1. Kliknij przycisk **tworzenie i wdrażanie** na **fabryki danych** bloku fabryką danych. Powinien zostać uruchomiony Edytor fabryki danych.
2. Kliknij przycisk **Nowy magazyn danych** i wybierz opcję **Azure Storage**.

   ![Nowy magazyn danych — Azure Storage — menu](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Powinny pojawić się **skryptu JSON** do tworzenia usługi Azure Storage połączonej usługi w edytorze.

   ![Połączona usługa Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Zastąp **nazwa konta** i **klucz konta** z nazwy i klucza dostępu konta magazynu Azure. Aby dowiedzieć się, jak uzyskać klucz dostępu do magazynu, zapoznaj się z informacjami na temat sposobów wyświetlania, kopiowania i ponownego generowania kluczy dostępu do magazynu w sekcji [Zarządzanie kontem magazynu](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Aby wdrożyć połączonej usługi, kliknij przycisk **Wdróż** na pasku poleceń. Po pomyślnym wdrożeniu połączonej usługi okno **Wersja robocza-1** powinno zniknąć i w widoku drzewa po lewej stronie zostanie wyświetlona pozycja **AzureStorageLinkedService**.

#### <a name="create-hdinsight-linked-service"></a>Tworzenie usługi HDInsight połączone
W tym kroku utworzysz usługi Azure HDInsight połączony do połączenia z klastrem Spark w usłudze HDInsight z fabryką danych. Klaster usługi HDInsight służy do uruchomienia programu Spark określony w działaniu Spark potoku, w tym przykładzie.  

1. Kliknij przycisk **... Więcej** na pasku narzędzi kliknij **nowych obliczeń**, a następnie kliknij przycisk **klastra usługi HDInsight**.

    ![Tworzenie usługi HDInsight połączone](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Skopiuj i wklej poniższy fragment kodu do okna **Wersja robocza-1**. W edytorze JSON wykonaj następujące czynności:
    1. Określ **URI** dla klastra Spark w usłudze HDInsight. Na przykład: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Określ nazwę **użytkownika** kto ma dostęp do klastra Spark.
    3. Określ **hasło** dla użytkownika.
    4. Określ **połączonej usługi magazynu Azure** skojarzonego z klastrem Spark w usłudze HDInsight. W tym przykładzie jest: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Działanie Spark nie obsługuje klastrów HDInsight Spark, które używają usługi Azure Data Lake Store jako podstawowy magazyn.
    > - Działanie Spark obsługuje tylko istniejące (własne) klastra Spark w usłudze HDInsight. Nie obsługuje usługi HDInsight połączony na żądanie.

    Zobacz [połączoną usługą usługi HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) szczegółowe informacje na temat usługi HDInsight połączonej usługi.
3.  Aby wdrożyć połączonej usługi, kliknij przycisk **Wdróż** na pasku poleceń.  

### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
Wyjściowy zestaw danych jest podstawę harmonogram (co godzinę, codziennie, itp.). W związku z tym należy określić wyjściowy zestaw danych działania spark w potoku, nawet jeśli działanie nie tworzy naprawdę żadnych danych wyjściowych. Określenie zestawem danych wejściowych dla działania jest opcjonalne.

1. W **Edytorze fabryki danych** kliknij opcję **... więcej** na pasku poleceń, kliknij opcję **Nowy zestaw danych** i wybierz opcję **Azure Blob Storage**.  
2. Skopiuj i wklej poniższy fragment kodu do okna Wersja robocza-1. Fragment kodu JSON definiuje zestaw danych o nazwie **OutputDataset**. Ponadto określ że wyniki są przechowywane w kontenerze obiektów blob o nazwie **adfspark** i folder o nazwie **pyFiles/wyjścia**. Jak wspomniano wcześniej, ten zestaw danych jest fikcyjny zestawu danych. Program Spark, w tym przykładzie nie generuje żadnego wyniku. **Dostępności** sekcja określa, że wyjściowy zestaw danych jest tworzony codziennie.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Aby wdrożyć zestawu danych, kliknij przycisk **Wdróż** na pasku poleceń.


### <a name="create-pipeline"></a>Tworzenie potoku
W tym kroku możesz utworzyć potok wraz z **HDInsightSpark** działania. W tym przypadku wyjściowy zestaw danych jest elementem wpływającym na ustawienia harmonogramu, więc musisz utworzyć wyjściowy zestaw danych nawet wtedy, gdy działanie nie generuje żadnych danych wyjściowych. Jeśli w działaniu nie są używane żadne dane wejściowe, możesz pominąć tworzenie zestawu danych wejściowych. W związku z tym nie wejściowy zestaw danych został określony w tym przykładzie.

1. W **Edytor fabryki danych**, kliknij przycisk **... Więcej** na pasku poleceń, a następnie kliknij **nowy potok**.
2. Zastąp skryptu w oknie Projekt 1 następujący skrypt:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Pamiętaj o następujących kwestiach:
    - **Typu** właściwość jest ustawiona na **HDInsightSpark**.
    - **Właściwość rootPath** ustawiono **adfspark\\pyFiles** gdzie adfspark jest kontenera obiektów Blob platformy Azure i pyFiles jest poprawnie folder, w tym kontenerze. W tym przykładzie magazynu obiektów Blob Azure jest skojarzony z klastrem Spark. Można przekazać pliku do innego magazynu Azure. Jeśli tak zrobisz, Utwórz połączoną usługą magazynu Azure, aby połączyć konto magazynu z fabryką danych. Następnie określ jako wartość dla nazwy połączonej usługi **sparkJobLinkedService** właściwości. Zobacz [właściwości działania Spark](#spark-activity-properties) szczegóły dotyczące tej właściwości oraz inne właściwości obsługiwane przez działanie Spark.  
    - **EntryFilePath** ustawiono **test.py**, czyli plik python.
    - **GetDebugInfo** właściwość jest ustawiona na **zawsze**, co oznacza, że pliki dziennika są zawsze generowany (powodzenie lub niepowodzenie).

        > [!IMPORTANT]
        > Zaleca się, że nie należy ustawiać tej właściwości `Always` w środowisku produkcyjnym, chyba że są Rozwiązywanie problemów.
    - **Generuje** sekcja ma jeden wyjściowy zestaw danych. Należy określić wyjściowy zestaw danych, nawet jeśli spark program nie generuje żadnego wyniku. Wyjściowy zestaw danych dysków harmonogramu dla potoku (co godzinę, codziennie, itp.).  

        Aby uzyskać szczegółowe informacje dotyczące właściwości obsługiwanych przez działanie Spark, zobacz [Spark właściwości działania](#spark-activity-properties) sekcji.
3. Aby wdrożyć potok, kliknij przycisk **Wdróż** na pasku poleceń.

### <a name="monitor-pipeline"></a>Monitorowanie potoku
1. Kliknij przycisk **X** aby zamknąć Edytor fabryki danych bloków i przejdź do strony głównej fabryki danych. Kliknij przycisk **monitorowanie i zarządzanie** można uruchomić monitorowania aplikacji w innej karty.

    ![Monitorowanie i zarządzanie nimi kafelka](media/data-factory-spark/monitor-and-manage-tile.png)
2. Zmień **godzina rozpoczęcia** filtru na górze do **2/1/2017**i kliknij przycisk **Zastosuj**.
3. Tylko jedno okno działania powinny być widoczne, ponieważ istnieje tylko jeden dzień między (2017-02-01) czasu rozpoczęcia i zakończenia (2017-02-02) potoku. Upewnij się, że wycinek danych jest w **gotowe** stanu.

    ![Monitorowanie potoku](media/data-factory-spark/monitor-and-manage-app.png)    
4. Wybierz **okno działania** aby zobaczyć szczegóły dotyczące uruchamiania działania. Jeśli występuje błąd, zobaczysz szczegółowe informacje o nim w okienku po prawej stronie.

### <a name="verify-the-results"></a>Sprawdź wyniki

1. Uruchom **notesu Jupyter** przechodząc do klastra Spark w usłudze HDInsight: https://CLUSTERNAME.azurehdinsight.net/jupyter. Można również uruchomić Pulpit nawigacyjny klastra dla klastra Spark w usłudze HDInsight, a następnie uruchom **notesu Jupyter**.
2. Kliknij przycisk **nowy** -> **PySpark** uruchomić nowy notes.

    ![Nowego notesu Jupyter](media/data-factory-spark/jupyter-new-book.png)
3. Uruchom następujące polecenie kopiowania/wklejania tekst i naciskając klawisz **SHIFT + ENTER** na końcu drugiej instrukcji.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Upewnij się, że zostaną wyświetlone dane z tabeli hvac:  

    ![Wyniki zapytania Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Zobacz [uruchamiania zapytań Spark SQL](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md) sekcji, aby uzyskać szczegółowe instrukcje. 

### <a name="troubleshooting"></a>Rozwiązywanie problemów
Ponieważ ustawisz **getDebugInfo** do **zawsze**, zostanie wyświetlony **dziennika** podfolder **pyFiles** folderu w kontenerze obiektu Blob Azure. Plik dziennika w folderze dziennika zawiera dodatkowe szczegóły. Ten plik dziennika jest szczególnie przydatna w przypadku, gdy występuje błąd. W środowisku produkcyjnym można ustawić ją na **błąd**.

Aby uzyskać dodatkowe informacje o rozwiązywaniu, wykonaj następujące czynności:


1. Przejdź do `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Aplikacja YARN interfejsu użytkownika](media/data-factory-spark/yarnui-application.png)  
2. Kliknij przycisk **dzienniki** jednego uruchomienia prób.

    ![Strona aplikacji](media/data-factory-spark/yarn-applications.png)
3. Dodatkowe informacje o błędzie w dzienniku strony powinna zostać wyświetlona.

    ![Błąd dziennika](media/data-factory-spark/yarnui-application-error.png)

Poniższe sekcje zawierają informacje na temat jednostek fabryki danych do używania klastra Apache Spark i działania Spark w fabryce danych.

## <a name="spark-activity-properties"></a>Właściwości działania Spark
Oto przykład definicji JSON potoku z działaniem Spark:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

W poniższej tabeli opisano właściwości JSON używane w definicji JSON:

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| name | Nazwa działania w potoku. | Tak |
| description | Tekst opisujący działanie robi. | Nie |
| type | Ta właściwość musi mieć ustawioną HDInsightSpark. | Tak |
| linkedServiceName | Nazwa usługi HDInsight połączone, na którym jest uruchomiony Spark program. | Tak |
| Właściwość rootPath | Kontener obiektów Blob platformy Azure i folder zawierający plik Spark. Nazwa pliku jest rozróżniana wielkość liter. | Tak |
| entryFilePath | Ścieżka względna do folderu głównego Spark kodu/pakietu. | Tak |
| className | Klasy głównym aplikacji Java/Spark | Nie |
| Argumenty | Lista argumentów wiersza polecenia do programu Spark. | Nie |
| proxyUser | Konto użytkownika do personifikacji do wykonania programu Spark | Nie |
| sparkConfig | Określ wartości dla właściwości konfiguracji Spark wymienione w temacie: [konfiguracji Spark — właściwości aplikacji](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nie |
| getDebugInfo | Określa, kiedy Spark pliki dziennika są kopiowane do magazynu Azure używanego przez klaster usługi HDInsight (lub) został określony przez sparkJobLinkedService. Dozwolone wartości: None, zawsze lub niepowodzenie. Wartość domyślna: Brak. | Nie |
| sparkJobLinkedService | Magazyn Azure połączone usługi, która ma Spark plik zadania, zależności i dzienniki.  Jeśli nie określisz wartości dla tej właściwości, Magazyn skojarzony z klastrem usługi HDInsight jest używany. | Nie |

## <a name="folder-structure"></a>Struktura folderów
Działanie Spark nie obsługuje skryptu w wierszu jako Pig i do gałęzi działań. Zadań Spark jest również extensible więcej niż zadań Pig/Hive. W przypadku zadań Spark, możesz podać wiele zależności takich jak jar pakietów (umieszczona w języku java ścieżki klasy), pliki języka python (dotyczącymi PYTHONPATH) i innych plików.

Utwórz następującą strukturę folderów w magazynie obiektów Blob platformy Azure, odwołuje się usługa HDInsight połączone. Natomiast przekazywanie plików zależnych do odpowiednich podfolderów w folderze głównym reprezentowany przez **entryFilePath**. Na przykład Przekaż python pliki są kopiowane do podfolderu pyFiles i pliki jar do podfolderu słoików folderu głównego. W czasie wykonywania usługi fabryka danych oczekuje następującej struktury folderu w magazynie obiektów Blob platformy Azure:     

| Ścieżka | Opis | Wymagane | Typ |
| ---- | ----------- | -------- | ---- |
| . | Ścieżka katalogu głównego zadania Spark w połączonej usługi magazynu  | Tak | Folder |
| &lt;zdefiniowane przez użytkownika&gt; | Ścieżka do pliku wpisu zadania Spark | Tak | Plik |
| . / jars | Wszystkie pliki w tym folderze są przekazywane i dotyczącymi klasy java klastra | Nie | Folder |
| . / pyFiles | Wszystkie pliki w tym folderze są przekazywane i dotyczącymi PYTHONPATH klastra | Nie | Folder |
| . / pliki | Wszystkie pliki w tym folderze są przekazywane i dotyczącymi Moduł wykonujący katalog roboczy | Nie | Folder |
| . / archiwa | Wszystkie pliki w tym folderze są nieskompresowanych | Nie | Folder |
| . / dzienników | Folder, w którym są przechowywane dzienniki w klastrze Spark.| Nie | Folder |

Oto przykład do magazynu zawierającego dwa pliki zadania Spark w magazynie obiektów Blob Azure przywoływany przez usługę HDInsight połączone.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
