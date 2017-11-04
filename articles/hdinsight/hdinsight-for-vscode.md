---
title: "Narzędzia HDInsight Azure - Użyj Visual Studio Code Hive, LLAP lub pySpark | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać narzędzi HDInsight Azure dla programu Visual Studio Code do tworzenia, przesyłania zapytań i skryptów."
Keywords: "VScode, narzędzia Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interakcyjne Hive, interakcyjne zapytania"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: jgao
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 9c1d0e0520df30306c1647cf1f3ec86c8a4fd8f5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-hdinsight-tool-for-visual-studio-code"></a>Narzędzie Azure HDInsight dla kodu programu Visual Studio

Dowiedz się, jak używać narzędzi HDInsight Azure dla programu Visual Studio Code (VSCode) do tworzenia, przesyłania zadań Hive, interakcyjnych zapytań Hive i skryptów pySpark. Narzędzia HDInsight Azure można zainstalować na platformach obsługiwanych przez VSCode tym systemu Windows, Linux i MacOS. Wymagania wstępne dla różnych platform można znaleźć.


## <a name="prerequisites"></a>Wymagania wstępne

Następujące elementy są wymagane do wykonywania czynności opisanych w tym artykule:

- Klaster usługi HDInsight.  Aby utworzyć klaster, zobacz [Rozpoczynanie pracy z usługą HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono jest tylko wymagane dla systemów Linux i MacOS.

## <a name="install-the-hdinsight-tools"></a>Instalowanie narzędzi HDInsight Tools
   
Po zainstalowaniu wymagań wstępnych, można zainstalować narzędzi HDInsight Azure dla VSCode. 

**Aby zainstalować usługi Azure HDInsight tools**

1. Otwórz **kodu programu Visual Studio**.
2. Kliknij przycisk **rozszerzenia** w okienku po lewej stronie. Wprowadź **HDInsight** w polu wyszukiwania.
3. Kliknij przycisk **zainstalować** obok **narzędzia Azure HDInsight**. Po kilku sekundach **zainstalować** przycisk zostanie zmieniony na **Załaduj ponownie**.
4. Kliknij przycisk **Załaduj ponownie** aktywować **narzędzia Azure HDInsight** rozszerzenia.
5. Kliknij przycisk **ponownego załadowania okna** o potwierdzenie. Widać **narzędzia Azure HDInsight** w okienku rozszerzenia.

   ![HDInsight dla instalacji programu Visual Studio kodu Python](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Otwórz obszar roboczy usługi HDInsight

Przed podłączeniem do platformy Azure, należy utworzyć w VSCode obszaru roboczego.

**Aby otworzyć obszar roboczy**

1. Z **pliku** menu, kliknij przycisk **Otwórz Folder**, określ istniejącego folderu lub Utwórz nowy folder jako folder roboczy. Folder będzie wyświetlany w okienku po lewej stronie.
2. W lewym okienku kliknij pozycję **nowy plik** ikona obok folderu roboczego.

   ![Nowy plik](./media/hdinsight-for-vscode/new-file.png)
3. Nazwa nowego pliku .hql (zapytań programu Hive) lub rozszerzenie pliku .py (Spark skryptu). Powiadomienie **XXXX_hdi_settings.json** plik konfiguracji jest automatycznie dodawany do folderu roboczego.
4. Otwórz **XXXX_hdi_settings.json** z **EXPLORER**, lub kliknij prawym przyciskiem myszy na edytora skryptów, aby wybrać **ustawić konfiguracji**. Można skonfigurować zapisu logowania, klastra domyślnego i parametry przesyłania zadania, jak pokazano w przykładzie w pliku. Można także pozostawić pozostałe parametry puste.

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z usługą Azure

Przed skryptów można przesłać do klastrów usługi HDInsight z VSCode, należy nawiązać konta platformy Azure.

**Do nawiązania połączenia platformy Azure**

1. Utwórz nowy folder roboczy i nowy plik skryptu, jeśli nie masz.
2. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **HDInsight: logowania** z menu kontekstowego. Można również nacisnąć **CTRL + SHIFT + P** i wprowadzając **HDInsight: logowania**.

    ![Narzędzia HDInsight Tools for Visual Studio Code Zaloguj](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)
3. Postępuj zgodnie z instrukcjami logowania **dane wyjściowe** okienko się zalogować.

    **Azure:** ![narzędzia HDInsight Tools for Visual Studio Code informacje dotyczące logowania](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Po nawiązaniu połączenia, nazwa konta platformy Azure jest wyświetlany na pasku stanu, w lewym dolnym rogu okna VSCode. 

    > [!NOTE]
    > Otwórz przeglądarkę z trybu prywatnych lub incognito z powodu problemu znane Azure authentication. Konto platformy Azure ma dwa składniki włączone, zalecane jest uwierzytelnianie telefonu zamiast numeru Pin.
  

4. Kliknij prawym przyciskiem myszy Edytuj skryptu, aby otworzyć menu kontekstowego. Z menu kontekstowego można wykonywać następujące zadania:

    - Wyloguj
    - Lista klastrów
    - Ustaw domyślny klastra
    - Wysyłanie zapytań programu Hive interakcyjne
    - Prześlij skrypt wsadowy gałęzi
    - Przedstawia interakcyjnych zapytań PySpark
    - Prześlij skrypt wsadowy PySpark
    - Konfiguracja zestawu

## <a name="list-hdinsight-clusters"></a>Lista klastrów usługi HDInsight

Aby przetestować połączenie, można wyświetlić listę z klastrami HDInsight:

**Aby wyświetlić listę klastrów usługi HDInsight w ramach Twojej subskrypcji platformy Azure**
1. Otwórz obszar roboczy i połączenia z platformą Azure. Zobacz [HDInsight Otwórz obszar roboczy](#open-hdinsight-workspace) i [Connect Azure](#connect-to-azure).
2. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **HDInsight: klaster listą** z menu kontekstowego. 
3. Hive i Spark klastry są wyświetlane w **dane wyjściowe** okienka.

    ![Domyślna konfiguracja klastra zestawu](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Ustaw domyślny klastra
1. Otwórz obszar roboczy i połączenia z platformą Azure. Zobacz [HDInsight Otwórz obszar roboczy](#open-hdinsight-workspace) i [Connect Azure](#connect-to-azure).
2. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie kliknij przycisk **HDInsight: Ustaw domyślny klastra**. 
3. Wybierz klaster jako klastra domyślnego dla bieżącego pliku skryptu. Narzędzia automatycznie Aktualizuj plik konfiguracji, **XXXX_hdi_settings.json**. 

   ![Domyślna konfiguracja klastra zestawu](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-azure-environment"></a>Ustaw środowiska platformy Azure 
1. Otwórz palety polecenia, naciskając **CTRL + SHIFT + P**.
2. Wprowadź **HDInsight: Ustaw środowiska platformy Azure**.
3. Wybierz jedną z metod z platformy Azure i AzureChina jako domyślny wpis nazwy logowania.
4. W tym samym czasie naszego narzędzia już zapisane wybranej domyślny wpis logowania do **XXXX_hdi_settings.json**. Można również bezpośrednio zaktualizować go w tym pliku konfiguracji. 

   ![Domyślna konfiguracja wpis logowania zestawu](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Wysyłanie zapytań programu Hive interakcyjne

Narzędzia HDInsight Tools for VSCode umożliwiają przesyłanie interakcyjnych zapytań Hive w klastrach HDInsight interakcyjne zapytania.

1. Utwórz nowy folder roboczy i nowy plik skryptu Hive, jeśli nie masz.
2. Połącz się z kontem platformy Azure, a następnie skonfiguruj klastra domyślnego, jeśli nie zostało to jeszcze zrobione.
3. Skopiuj i wklej następujący kod do pliku programu Hive, a następnie zapisz go.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie kliknij przycisk **HDInsight: Hive interakcyjne** przesłać zapytanie. Narzędzia pozwalają również na przesyłanie bloku kodu zamiast pliku całego skryptu, za pomocą menu kontekstowego. Wkrótce po, wynik zapytania jest wyświetlany na nowej karcie:

   ![interakcyjne wynik gałęzi](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **WYNIKI** panel: Zapisz wynik całego jako CSV, JSON, programu EXCEL na ścieżkę lokalną lub wystarczy zaznaczyć wiele wierszy.
    - **KOMUNIKATY** panel: kliknięcie **wiersza** numer, jego przechodzi do pierwszego wiersza uruchamianie skryptu.

Porównywanie z [wykonywania zadania wsadowego Hive](#submit-hive-batch-scripts), interakcyjne Kwerenda trwa znacznie krócej.

## <a name="submit-hive-batch-scripts"></a>Przesyłanie skryptów usługi partia zadań Hive

1. Utwórz nowy folder roboczy i nowy plik skryptu Hive, jeśli nie masz.
2. Połącz się z kontem platformy Azure, a następnie skonfiguruj klastra domyślnego, jeśli nie zostało to jeszcze zrobione.
3. Skopiuj i wklej następujący kod do pliku programu Hive, a następnie zapisz go.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie kliknij przycisk **HDInsight: Hive partii** można przesłać zadania Hive. 
4. Wybierz klaster, w którym chcesz przesłać do.  

    Po przesłaniu zadania Hive, przesyłanie informacji sukcesu i identyfikator zadania jest wyświetlana w **dane wyjściowe** panelu. I otwiera **PRZEGLĄDARKI sieci WEB** które dzienniki w czasie rzeczywistym zadania i stan pokazano.

   ![przedstawia wynik zadania Hive](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

Porównywanie z [przesyłanie interakcyjnych zapytań Hive](#submit-interactive-hive-queries), znacznie dłużej zajmuje zadania wsadowego.

## <a name="submit-interactive-pyspark-queries"></a>Przedstawia interakcyjnych zapytań PySpark
Narzędzia HDInsight Tools for VSCode również włączyć przesyłanie interakcyjnych zapytań PySpark do klastry Spark.
1. Utwórz nowy folder roboczy i nowy plik skryptu z rozszerzeniem PY i, jeśli nie masz.
2. Połącz z kontem platformy Azure, jeśli nie zostało to jeszcze zrobione.
3. Skopiuj i wklej następujący kod do pliku skryptu:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Wyróżnij te skrypty i kliknij prawym przyciskiem myszy Edytor skryptów, a następnie kliknij przycisk **HDInsight: PySpark interakcyjne**.
5. Kliknij następujące **zainstalować** przycisku, jeśli nie zainstalowano **Python** rozszerzenia VSCode.
    ![HDInsight dla instalacji programu Visual Studio kodu Python](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Konfigurowanie środowiska python w systemie Jeśli nie zostanie zainstalowana. 
   - W systemie windows, Pobierz i zainstaluj [Python](https://www.python.org/downloads/). Następnie upewnij się, `Python` i `pip` w systemie ścieżki.
   - Instrukcja MacOS i Linux, zobacz [ustawić się PySpark interaktywnego środowiska dla programu Visual Studio Code](set-up-pyspark-interactive-environment.md).
7. Wybierz klaster, aby przesłać zapytanie PySpark. Wkrótce po, wynik zapytania jest wyświetlany w prawej nowej karty:

   ![przedstawia wynik zadania języka python](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Nasze narzędzie obsługuje również zapytania **klauzuli SQL**.

   ![przedstawia wynik zadania python](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) przesyłanie stan jest wyświetlany w lewym dolnym pasku stanu podczas uruchamiania zapytań. Nie można przesłać inne zapytania, gdy stan to **jądra PySpark (zajęty)**, w przeciwnym razie działanie jest zawieszenie.
9. Nasze klastrów można zachować sesję. Na przykład **= 100**już Zachowaj tę sesję w klastrze, teraz tylko Uruchom **drukowanie** do klastra.
 

## <a name="submit-pyspark-batch-job"></a>Prześlij zadanie wsadowe PySpark

1. Utwórz nowy folder roboczy i nowy plik skryptu z rozszerzeniem PY i, jeśli nie masz.
2. Połącz z kontem platformy Azure, jeśli nie zostało to jeszcze zrobione.
3. Skopiuj i wklej następujący kod do pliku skryptu:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie kliknij przycisk **HDInsight: partii PySpark**. 
5. Wybierz klaster, aby przesłać zadanie PySpark. 

   ![przedstawia wynik zadania języka python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Po przesłaniu zadania python, przesyłanie dzienników jest wyświetlany w obszarze **dane wyjściowe** okna w VSCode. **URL interfejsu użytkownika Spark** i **URL interfejsu użytkownika Yarn** są również wyświetlane. Adres URL można otworzyć w przeglądarce sieci web, aby śledzić stan zadania.


## <a name="additional-features"></a>Dodatkowe funkcje

HDInsight dla VSCode obsługuje następujące funkcje:

- **Funkcja automatycznego uzupełniania IntelliSense**. Sugestie są zdjęte ze stosu wokół — słowo kluczowe, metody, zmienne,... itd. Inne ikony reprezentują różnych typów obiektów:

    ![Narzędzia HDInsight Tools for Visual Studio kodu IntelliSense typy obiektów](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Znacznik błąd IntelliSense**. Usługa języka podkreśla błędy edycji skryptu Hive.     
- **Najważniejsze funkcje składni**. Usługa języka używa kolorów do odróżnienia zmienne, słowa kluczowe, typ danych, funkcji, itp. 

    ![Narzędzia HDInsight Tools for Visual Studio Code prezentuje składni](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Następne kroki

### <a name="demo"></a>Demonstracja
* HDInsight dla VScode: [wideo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia

* [Debugowanie aplikacji Spark zdalnie za pośrednictwem sieci VPN przy użyciu zestawu narzędzi Azure for IntelliJ](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Debugowanie aplikacji Spark zdalnie za pośrednictwem usługi SSH przy użyciu zestawu narzędzi Azure for IntelliJ](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Użyj narzędzia HDInsight Tools for IntelliJ z Hortonworks piaskownicy](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Użyj narzędzia HDInsight Tools w zestawie narzędzi Azure dla programu Eclipse tworzenie aplikacji Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Wizualizacja gałęzi przy użyciu usługi Microsoft Power BI w usłudze Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Konfigurowanie środowiska interakcyjne PySpark dla kodu programu Visual Studio](set-up-pyspark-interactive-environment.md)
* [Umożliwia uruchamianie zapytań Hive w usłudze Azure HDInsight Zeppelin](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](spark/apache-spark-use-bi-tools.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Przesyłanie strumieniowe Spark: Korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji przesyłania strumieniowego w czasie rzeczywistym](spark/apache-spark-eventhub-streaming.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](spark/apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="managing-resources"></a>Zarządzanie zasobami
* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](spark/apache-spark-job-debugging.md)



