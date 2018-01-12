---
title: "Tworzenie aplikacji do uczenia maszynowego Apache Spark w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Instrukcje krok po kroku dotyczące tworzenia aplikacji platformy Apache Spark uczenia maszynowego Spark w usłudze HDInsight clusters za pomocą notesu Jupyter"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: 22a3d220966fef77e131fbeb3ea46a1f81a9ada5
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Tworzenie aplikacji do uczenia maszynowego Apache Spark w usłudze Azure HDInsight

Informacje o sposobie tworzenia komputera Apache Spark nauki aplikacji przy użyciu klastra Spark w usłudze HDInsight. W tym artykule pokazano, jak używać dostępnych notesu Jupyter z klastrem umożliwiający zbudowanie i przetestowanie aplikacji. Aplikacja używa przykładowych danych HVAC.csv, który jest dostępny na wszystkich klastrach domyślnie.

**Wymagania wstępne:**

Należy dysponować następującymi elementami:

* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [klastrów utworzyć Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Informacje w zestawie danych
Przed możemy rozpocząć tworzenie aplikacji, Daj nam poznać struktury danych, dla którego budujemy aplikacji oraz od rodzaju analizy, które spowoduje wykonanie na danych. 

W tym artykule używamy próbki **HVAC.csv** pliku danych, który jest dostępny na koncie magazynu Azure, który jest skojarzony z klastrem usługi HDInsight. W ramach konta magazynu, plik jest w **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Pobierz i Otwórz plik CSV można pobrać migawek danych.  

![Migawki danych używanych na przykład learning maszyny Spark](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "migawki dane używane na potrzeby Spark machine learning przykład")

Dane przedstawia temperatury docelowych i rzeczywistego temperatury w budynku, który ma zainstalowane systemy HVAC. Załóżmy, że **systemu** kolumna reprezentuje identyfikator systemu i **SystemAge** kolumna reprezentuje liczbę lat systemu HVAC miało miejsce w budynku.

Używamy tych danych do prognozowania, czy budynku będzie hotter lub colder podstawie na temperatury docelowej, identyfikator systemowy i wiek systemu.

## <a name="app"></a>Napisać aplikację learning maszyny Spark przy użyciu Spark MLlib
W tej aplikacji potoku Spark ML używany do przeprowadzania klasyfikacji dokumentu. W potoku firma Microsoft podzielone dokumentu słów, konwertować wyrazy wektor funkcji numerycznych i koniec kompilacji modelu prognozowania, używając funkcji wektorów i etykiety. Wykonaj poniższe kroki, aby utworzyć aplikację.

1. W [Portalu Azure](https://portal.azure.com/) na tablicy startowej kliknij kafelek klastra Spark (jeśli został przypięty do tablicy startowej). Możesz także przejść do klastra, wybierając polecenia **Przeglądaj wszystko** > **Klastry usługi HDInsight**.   
2. W bloku klastra Spark kliknij pozycję **Pulpit nawigacyjny klastra**, a następnie opcję **Jupyter Notebook**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.
   
   > [!NOTE]
   > Można również przejść do aplikacji Jupyter Notebook dla klastra, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Utwórz nowy notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.
   
    ![Tworzenie notesu Jupyter, na przykład learning maszyny Spark](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "tworzenie notesu Jupyter, na przykład learning maszyny Spark")
4. Zostanie utworzony i otwarty nowy notes o nazwie Untitled.pynb. Kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.
   
    ![Podaj nazwę notesu Spark machine learning przykład](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Podaj nazwę notesu Spark machine learning przykład")
5. Ponieważ notes został utworzony z użyciem jądra PySpark, nie ma konieczności jawnego tworzenia kontekstów. Konteksty Spark i Hive zostaną automatycznie utworzone po uruchomieniu pierwszej komórki kodu. Możesz zacząć od importowania typów, które są wymagane dla tego scenariusza. W pustej komórce Wklej poniższy fragment kodu, a następnie naciśnij klawisz **SHIFT + ENTER**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Musi teraz załadować danych (hvac.csv), przeanalizować go i użyć go do nauczenia modelu. W tym celu należy zdefiniować funkcję, która sprawdza, czy rzeczywiste temperatury w budynku jest większa niż temperatury docelowej. Jeśli rzeczywista temperatury jest większa, kompilacja jest gorących, wskazywane przez wartość **1.0**. W przypadku rzeczywistych temperatury mniejszym budynku jest zimnych, wskazywane przez wartość **0,0**. 
   
    Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfiguruje potok learning maszyny Spark, która składa się z trzech etapów: tokenizatora, hashingTF i lr. Aby uzyskać więcej informacji na temat nowości potoku i jak działa zobacz <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning potoku</a>.
   
    Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Dopasuj potoku do dokumentu szkolenia. Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.
   
        model = pipeline.fit(training)
3. Sprawdź dokument szkolenia, aby punkt kontrolny postęp z aplikacją. Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.
   
        training.show()
   
    To powinno zawierać dane wyjściowe podobne do następujących:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Przejdź wstecz i sprawdź dane wyjściowe na pierwotnych pliku CSV. Na przykład pierwszy wiersz pliku CSV zawiera te dane:

    ![Migawki Spark machine learning przykładowe dane wyjściowe](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "migawki danych wyjściowych na Spark machine learning przykład")

    Zwróć uwagę, jak rzeczywiste temperatury jest mniejsza niż sugerowanie temperatury docelowej kompilacja jest zimnych. Dlatego w danych wyjściowych, szkolenia wartość **etykiety** w pierwszym wierszu jest **0,0**, co oznacza, że kompilacja nie jest dynamicznej.

1. Przygotuj zestawu danych do uruchomienia przed trenowanego modelu. Aby to zrobić, możemy przejdzie na identyfikator systemu i wiek systemu (oznaczonego jako **SystemInfo** w danych wyjściowych szkolenia), i modelu czy prognozowania, czy kompilacja o tym identyfikatorze systemu i wiek systemu będzie hotter (wskazywane przez 1.0) lub chłodniej (wskazywane przez 0,0).
   
   Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Na koniec tworzenia prognoz na danych testowych. Wklej poniższy fragment kodu w pustej komórce i naciśnij klawisz **SHIFT + ENTER**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Powinny pojawić się dane wyjściowe podobne do następujących:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   W pierwszym wierszu Prognozowanie widać, że system HVAC o identyfikatorze 20 i wiek systemu 25 lat, kompilacja będzie gorących (**prognozowania = 1.0**). Wartość pierwszego DenseVector (0.49999) odnosi się do prognozowania 0,0 i drugiej wartości (0.5001) odpowiada prognozowania 1.0. W danych wyjściowych, nawet jeśli druga wartość jest tylko nieznacznie wyższe modelu zawiera **prognozowania = 1.0**.
4. Po zakończeniu działania aplikacji należy ją zamknąć, aby zwolnić zasoby. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj). Spowoduje to zakończenie pracy i zamknięcie notesu.

## <a name="anaconda"></a>Użyj Anaconda scikit — Dowiedz się biblioteki Spark uczenia maszynowego
Klastry platformy Apache Spark w usłudze HDInsight obejmują bibliotekami Anaconda. Obejmuje to także **scikit — Dowiedz się** biblioteki uczenia maszynowego. Biblioteka zawiera także różne zestawy danych, które służą do tworzenia przykładowej aplikacji bezpośrednio z notesu Jupyter. Przykłady dotyczące używania scikit — Dowiedz się, biblioteka, zobacz [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Zobacz też
* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](apache-spark-use-bi-tools.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](apache-spark-machine-learning-mllib-ipython.md)
* [Przesyłanie strumieniowe Spark: korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji do przesyłania strumieniowego w czasie rzeczywistym](apache-spark-eventhub-streaming.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin.md)
* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](apache-spark-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami
* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
