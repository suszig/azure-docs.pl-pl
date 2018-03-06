---
title: "Samouczek dotyczący przygotowywania danych do klasyfikowania irysów w usługach Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). W części pierwszej omówiono przygotowywanie danych."
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/28/2018
ms.openlocfilehash: 0bef557ee1394e3c786fd2c54e821b5dea28fabf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-classify-iris-part-1---preparing-the-data"></a>Samouczek: klasyfikowanie irysów, część 1 — przygotowanie danych

Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest pierwszą częścią trzyczęściowej serii. W tym samouczku poznamy podstawowe zagadnienia z zakresu usług Azure Machine Learning (wersja zapoznawcza). Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie projektu w aplikacji Azure Machine Learning Workbench
> * Tworzenie pakietu przygotowywania danych
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

W tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/Iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym macOS jest niemal identyczne.

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Do ukończenia tego samouczka niezbędne są następujące elementy:
- Konto Eksperymentowanie w usłudze Azure Machine Learning
- Zainstalowana aplikacja Azure Machine Learning Workbench

Jeśli nie zostało to jeszcze zrobione, postępuj zgodnie z instrukcjami w artykule [Szybki start: instalacja i uruchamianie](quickstart-installation.md), aby skonfigurować to konto i zainstalować aplikację Azure Machine Learning Workbench. 

## <a name="create-a-new-project-in-workbench"></a>Tworzenie nowego projektu w aplikacji Workbench

Jeśli wykonywano kroki z artykułu [Szybki start: instalacja i uruchamianie](quickstart-installation.md), ten projekt powinien już istnieć i możesz przejść do następnej sekcji.

1. Uruchom aplikację Azure Machine Learning Workbench i w razie potrzeby zaloguj się. 
   
   + W systemie Windows aplikację uruchomisz, używając skrótu **Machine Learning Workbench** na pulpicie. 
   + W systemie macOS wybierz pozycję **Azure ML Workbench** w programie Launchpad.

1. W okienku **PROJEKTY** wybierz znak plus (+), a następnie wybierz pozycję **Nowy projekt**.  

   ![Nowy obszar roboczy](media/tutorial-classifying-iris/new_ws.png)

1. Wypełnij pola formularza i wybierz przycisk **Utwórz**, aby utworzyć nowy projekt w aplikacji Workbench.

   Pole|Sugerowane wartości dla samouczka|Opis
   ---|---|---
   Project name (Nazwa projektu) | myIris |Wprowadź unikatową nazwę, która identyfikuje Twoje konto. Możesz użyć własnej nazwy albo nazwy działu lub projektu, która najlepiej identyfikuje eksperyment. Nazwa powinna mieć od 2 do 32 znaków. Powinna ona zawierać tylko znaki alfanumeryczne i znak łącznika (-). 
   Katalog projektu | c:\Temp\ | Określ katalog, w którym zostanie utworzony projekt.
   Opis projektu | _Pozostaw puste_ | Pole opcjonalne przydatne do opisywania projektów.
   Visualstudio.com |_Pozostaw puste_ | Pole opcjonalne. Projekt może być opcjonalnie skojarzony z repozytorium Git w usłudze Visual Studio Team Services na potrzeby kontroli źródła i współpracy. [Dowiedz się, jak to skonfigurować](https://docs.microsoft.com/en-us/azure/machine-learning/preview/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Obszar roboczy | IrisGarden (jeśli istnieje) | Wybierz obszar roboczy utworzony dla konta Eksperymentowania w witrynie Azure Portal. <br/>Jeśli samouczek Szybki start był wykonywany, powinien już istnieć obszar roboczy o nazwie IrisGarden. W przeciwnym razie wybierz obszar roboczy utworzony podczas tworzenia konta Eksperymentowania lub dowolny inny, którego chcesz użyć.
   Szablon projektu | Classifying Iris | Szablony zawierają skrypty i dane, których można użyć, aby zapoznać się z produktem. Ten szablon zawiera skrypty i dane potrzebne w tym samouczku Szybki start oraz innych samouczkach w tej witrynie dokumentacji. 

   ![Nowy projekt](media/tutorial-classifying-iris/new_project.png)
 
 Utworzony zostanie nowy projekt i otworzy się pulpit nawigacyjny projektu z tym projektem. Teraz możesz eksplorować stronę główną projektu, źródła danych, notesy i pliki kodu źródłowego. 

## <a name="create-a-data-preparation-package"></a>Tworzenie pakietu przygotowywania danych

W tej części samouczka będzie miało miejsce eksplorowanie danych i rozpoczęcie procesu przygotowywania danych. Podczas przygotowywania danych w aplikacji Azure Machine Learning Workbench reprezentacja JSON wykonywanych transformacji jest zapisywana w lokalnym pakiecie przygotowywania danych (plik dprep). Ten pakiet przygotowywania danych jest podstawowym kontenerem na potrzeby operacji związanych z przygotowywaniem danych w aplikacji Workbench.

Można go przekazać w celu wykonania do środowiska uruchomieniowego, takiego jak lokalne środowisko C#/CoreCLR, Scala/Spark lub Scala/HDI. gdzie kod jest generowany dla odpowiedniego środowiska uruchomieniowego w celu wykonania. 

1. Wybierz ikonę folderu, aby otworzyć widok plików, a następnie wybierz plik **iris.csv** w celu jego otwarcia.  

   Ten plik jest tabelą z 5 kolumnami i 150 wierszami. Zawiera cztery kolumny elementów liczbowych i docelową kolumnę ciągów. Nie zawiera nagłówków kolumn.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Nie dołączaj plików danych do folderu projektu, szczególnie w przypadku, gdy rozmiar pliku jest duży. Dołączamy plik **iris.csv** do tego szablonu w celach demonstracyjnych, ponieważ ten plik jest niewielki. Aby uzyskać więcej informacji, zobacz [How to read and write large data files (Jak odczytywać i zapisywać duże pliki danych)](how-to-read-write-files.md).

2. W obszarze **Widok danych** wybierz znak plus (**+**), aby dodać nowe źródło danych. Zostanie otwarta strona **Dodawanie źródła danych**. 

   ![Widok danych](media/tutorial-classifying-iris/data_view.png)

3. Wybierz pozycję **Pliki tekstowe(*.csv, .json, .txt, ...)** i kliknij przycisk **Dalej**.
   ![Źródło danych](media/tutorial-classifying-iris/data-source.png)
   

4. Wskaż plik **iris.csv** i kliknij przycisk **Dalej**.  
 
   ![Wybierz iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Na potrzeby tego ćwiczenia wybierz plik **iris.csv** z bieżącego katalogu projektu. W przeciwnym razie dalsze kroki mogą zakończyć się niepowodzeniem.
   
5. Pozostaw wartości domyślne i kliknij przycisk **Zakończ**.

6. Zostanie utworzony nowy plik o nazwie **iris-1.dsource**. Plik ma unikatową nazwę z sufiksem „-1”, ponieważ przykładowy projekt zawiera już nienumerowany plik **iris.dsource**.  

   Plik zostanie otwarty i widoczne będą dane. Do zestawu danych zostaną automatycznie dodane nagłówki kolumn od **Column1** do **Column5**. Przewiń w dół i zwróć uwagę, że ostatni wiersz z zestawu danych jest pusty. Wiersz jest pusty, ponieważ plik CSV zawiera dodatkowy podział wiersza.

   ![Widok danych irysów](media/tutorial-classifying-iris/iris_data_view.png)

7. Wybierz przycisk **Metryki**. Przyjrzyj się histogramom. Dla każdej kolumny został obliczony kompletny zestaw statystyk. Możesz również wybrać przycisk **Dane**, aby ponownie wyświetlić dane. 

   ![Widok danych irysów](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Wybierz przycisk **Przygotuj**. Zostanie otwarte okno dialogowe **Przygotowywanie**. 

   Przykładowy projekt zawiera plik **iris.dprep**. Domyślnie prosi o utworzenie nowego przepływu danych w istniejącym pakiecie przygotowywania danych **iris.dprep**. 

   Z menu rozwijanego wybierz polecenie **+ New Data Preparation Package** (+ Nowy pakiet przygotowywania danych), wprowadź nową wartość dla nazwy pakietu (**iris-1**), a następnie wybierz polecenie **OK**.

   ![Widok danych irysów](media/tutorial-classifying-iris/new_dprep.png)

   Nowy pakiet przygotowania danych o nazwie **iris-1.dprep** zostanie utworzony i otwarty w edytorze przygotowywania danych.

9. Teraz wykonajmy kilka podstawowych operacji przygotowania danych. Zaznacz poszczególne nagłówki kolumn, aby przystąpić do ich edycji, a następnie zmień nazwę każdej kolumny w następujący sposób: 

   W podanej kolejności wprowadź wartości **Długość słupka**, **Szerokość słupka**, **Długość płatka**, **Szerokość płatka** i **Gatunek** odpowiednio w pięciu kolumnach.

   ![Zmienianie nazw kolumn](media/tutorial-classifying-iris/rename_column.png)

10. Aby obliczyć różne wartości, wybierz kolumnę **Gatunek** i zaznacz ją, klikając prawym przyciskiem myszy. Z menu rozwijanego wybierz pozycję **Liczba wartości**. 

   Ta akcja powoduje otwarcie okienka **Inspektorzy** poniżej danych. Zostanie wyświetlony histogram z czterema słupkami. Kolumna docelowa ma trzy różne wartości: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** i wartość **(null)**.

   ![Wybieranie pozycji Liczba wartości](media/tutorial-classifying-iris/value_count.png)

11. Aby odfiltrować wartości null, wybierz etykietę „Null”, a następnie wybierz znak minus (**-**). Po chwili wiersz o wartości null zmieni kolor na szary w celu wskazania, że został on odfiltrowany. 

   ![Histogram liczby wartości](media/tutorial-classifying-iris/filter_out.png)

12. Poszczególne kroki zostały szczegółowo przedstawione w okienku **KROKI**. Każda akcja dotycząca zmiany nazw kolumn i filtrowania wierszy z wartością null została zarejestrowana jako krok przygotowywania danych. Poszczególne kroki można edytować, aby dostosować ustawienia, zmienić kolejność kroków i usunąć kroki.

   ![Kroki](media/tutorial-classifying-iris/steps.png)

13. Zamknij Kreatora przygotowywania danych. Wybierz polecenie **Zamknij** (x) na karcie **iris-1** oznaczonej ikoną wykresu. Twoja praca zostanie automatycznie zapisana w pliku **iris-1.dprep** wyświetlanym w obszarze oznaczonym nagłówkiem **Przygotowywanie danych**.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

<!-- The output/results of a Package can be explored in Python or via a Jupyter Notebook. A Package can be executed across multiple runtimes including local Python, Spark (including in Docker), and HDInsight. A Package contains one or more Dataflows that are the steps and transforms applied to the data. A Package may use another Package as a Data Source (referred to as a Reference Data Flow). -->

1. Znajdź plik **iris-1.dprep** na karcie Przygotowywanie danych.

1. Kliknij prawym przyciskiem myszy plik **iris-1.dprep**, a następnie z menu kontekstowego wybierz opcję **Generuj plik kodu dostępu do danych**. 

   ![Generowanie kodu](media/tutorial-classifying-iris/generate_code.png)

   Zostanie otwarty nowy plik o nazwie **iris-1.py** z następującymi wierszami kodu służącymi do wywołania logiki utworzonej jako pakiet przygotowywania danych:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   W zależności od kontekstu uruchamiania kodu element `df` reprezentuje różne rodzaje ramek danych. Element [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) w przypadku wykonywania w środowisku uruchomieniowym języka Python, a element [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) — w przypadku wykonywane w kontekście usługi Spark. 
   
   Aby dowiedzieć się, jak przygotowywać dane w aplikacji Azure Machine Learning Workbench, zapoznaj się z przewodnikiem [Wprowadzenie do przygotowywania danych](data-prep-getting-started.md).

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym samouczku aplikacja Azure Machine Learning Workbench została użyta w celu wykonania następujących czynności:
> [!div class="checklist"]
> * Tworzenie nowego projektu
> * Tworzenie pakietu przygotowywania danych
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

Możesz teraz przejść do następnej części serii samouczków, w której dowiesz się, w jaki sposób utworzyć model usługi Azure Machine Learning:
> [!div class="nextstepaction"]
> [Samouczek 2: tworzenie modeli](tutorial-classifying-iris-part-2.md)
