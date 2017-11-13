---
title: "Samouczek dotyczący przygotowywania danych do klasyfikowania irysów w usługach Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). W części pierwszej omówiono przygotowywanie danych."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/28/2017
ms.openlocfilehash: cabba8ce04d62d35ca40b3ae35d9d40a6ec7b2b9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="classify-iris-part-1-prepare-the-data"></a>Klasyfikowanie irysów, część 1: przygotowanie danych
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest pierwszą częścią trzyczęściowej serii. W tym samouczku poznamy podstawowe zagadnienia z zakresu usług Azure Machine Learning (wersja zapoznawcza). Omawiane kwestie:
> [!div class="checklist"]
> * Tworzenie projektu w programie Azure Machine Learning Workbench.
> * Tworzenie pakietu przygotowywania danych.
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych.

W tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/Iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym Mac OS jest niemal identyczne.

## <a name="prerequisites"></a>Wymagania wstępne
- Utwórz konto usługi Eksperymentowanie w usłudze Azure Machine Learning.
- Zainstaluj program Azure Machine Learning Workbench.

Możesz postępować zgodnie z instrukcjami przewodnika Szybki start [Instalowanie i tworzenie](quickstart-installation.md), aby zainstalować aplikację Azure Machine Learning Workbench. Ta instalacja obejmuje również wieloplatformowe narzędzie interfejsu wiersza polecenia platformy Azure lub wiersz polecenia platformy Azure.

## <a name="create-a-new-project-in-azure-machine-learning-workbench"></a>Tworzenie nowego projektu w programie Azure Machine Learning Workbench
1. Uruchom aplikację Azure Machine Learning Workbench i w razie potrzeby zaloguj się. W okienku **PROJEKTY** wybierz znak plus (**+**), aby utworzyć **nowy projekt**.

   ![Nowy obszar roboczy](media/tutorial-classifying-iris/new_ws.png)

2. Wypełnij szczegóły w obszarze **Tworzenie nowego projektu**: 

   ![Nowy projekt](media/tutorial-classifying-iris/new_project.png)

   - Wypełnij pole **Nazwa projektu**, wprowadzając nazwę projektu. Na przykład użyj wartości **myIris**.
   - Wybierz **katalog projektu**, w którym projekt zostanie utworzony. Na przykład użyj wartości `C:\Temp\`. 
   - Pole **Opis projektu** jest opcjonalne. 
   - Pole **Repozytorium Git** jest również opcjonalne i może być puste. Istniejące puste repozytorium Git można udostępnić w usłudze Visual Studio Team Services (repozytorium bez gałęzi głównej). Jeśli korzystasz z istniejącego repozytorium Git, scenariusze mobilności i udostępniania możesz włączyć później. Więcej informacji znajduje się w temacie [Use Git repo (Używanie repozytorium Git)](using-git-ml-project.md). 
   - Wybierz **obszar roboczy** — w tym samouczku używany jest przykładowy obszar roboczy **IrisGarden**. 
   - Wybierz szablon **Classifying Iris** z listy szablonów projektu. 

3. Wybierz przycisk **Utwórz**. Projekt zostanie utworzony i otwarty.

## <a name="create-a-data-preparation-package"></a>Tworzenie pakietu przygotowywania danych
1. Otwórz plik **iris.csv** z obszaru **Widok plików**. Ten plik jest tabelą z 5 kolumnami i 150 wierszami. Zawiera cztery kolumny elementów liczbowych i docelową kolumnę ciągów. Nie zawiera nagłówków kolumn.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Nie dołączaj plików danych do folderu projektu, szczególnie w przypadku, gdy rozmiar pliku jest duży. Dołączamy plik **iris.csv** do tego szablonu w celach demonstracyjnych, ponieważ ten plik jest niewielki. Aby uzyskać więcej informacji, zobacz [How to read and write large data files (Jak odczytywać i zapisywać duże pliki danych)](how-to-read-write-files.md).

2. W obszarze **Widok danych** wybierz znak plus (**+**), aby dodać nowe źródło danych. Zostanie otwarta strona **Dodawanie źródła danych**. 

   ![Widok danych](media/tutorial-classifying-iris/data_view.png)

3. Pozostaw wartości domyślne, a następnie wybierz przycisk **Dalej**.  
 
   ![Wybierz iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Na potrzeby tego ćwiczenia wybierz plik **iris.csv** z bieżącego katalogu projektu. W przeciwnym razie dalsze kroki mogą zakończyć się niepowodzeniem. 

4. Zostanie utworzony nowy plik o nazwie **iris-1.dsource**. Plik ma unikalną nazwę z łącznikiem i cyfrą 1, ponieważ przykładowy projekt zawiera już nienumerowany plik **iris.dsource**.  

   Plik zostanie otwarty i widoczne będą dane. Do zestawu danych zostaną automatycznie dodane nagłówki kolumn od **Column1** do **Column5**. Przewiń w dół i zwróć uwagę, że ostatni wiersz z zestawu danych jest pusty. Wiersz jest pusty, ponieważ plik CSV zawiera dodatkowy podział wiersza.

   ![Widok danych irysów](media/tutorial-classifying-iris/iris_data_view.png)

5. Wybierz przycisk **Metryki**. Przyjrzyj się histogramom. Dla każdej kolumny został obliczony kompletny zestaw statystyk. Możesz również wybrać przycisk **Dane**, aby ponownie wyświetlić dane. 

   ![Widok danych irysów](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Wybierz przycisk **Przygotuj**. Zostanie otwarte okno dialogowe **Przygotowywanie**. 

   Przykładowy projekt zawiera plik **iris.dprep**. Domyślnie prosi o utworzenie nowego przepływu danych w istniejącym pakiecie przygotowywania danych **iris.dprep**. 

   Z menu rozwijanego wybierz polecenie **+ New Data Preparation Package** (+ Nowy pakiet przygotowywania danych), wprowadź nową wartość dla nazwy pakietu (**iris-1**), a następnie wybierz polecenie **OK**.

   ![Widok danych irysów](media/tutorial-classifying-iris/new_dprep.png)

   Nowy pakiet przygotowania danych o nazwie **iris-1.dprep** zostanie utworzony i otwarty w edytorze przygotowywania danych.

7. Teraz wykonajmy kilka podstawowych operacji przygotowania danych. Zmień nazwy kolumn. Zaznacz poszczególne nagłówki kolumn, aby przystąpić do ich edycji. 

   Wprowadź wartości **Długość słupka**, **Szerokość słupka**, **Długość płatka**, **Szerokość płatka** i **Gatunek** odpowiednio w pięciu kolumnach.

   ![Zmienianie nazw kolumn](media/tutorial-classifying-iris/rename_column.png)

8. Aby obliczyć różne wartości, wybierz kolumnę **Gatunek** i zaznacz ją, klikając prawym przyciskiem myszy. Z menu rozwijanego wybierz pozycję **Liczba wartości**. 

   ![Wybieranie pozycji Liczba wartości](media/tutorial-classifying-iris/value_count.png)

   Ta akcja powoduje otwarcie okienka **Inspektorzy**, które zawiera histogram z czterema paskami. Kolumna docelowa ma trzy różne wartości: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** i wartość **(null)**.

9. Aby odfiltrować wartości null, wybierz na wykresie pasek reprezentujący wartość null. Istnieje również jeden wiersz z wartością **(null)**. Aby usunąć ten wiersz, wybierz znak minus (**-**).

   ![Histogram liczby wartości](media/tutorial-classifying-iris/filter_out.png)

10. Poszczególne kroki zostały szczegółowo przedstawione w okienku **KROKI**. Każda akcja dotycząca zmiany nazw kolumn i filtrowania wierszy z wartością null została zarejestrowana jako krok przygotowywania danych. Poszczególne kroki można edytować, aby dostosować ustawienia, zmienić kolejność kroków i usunąć kroki.

   ![Kroki](media/tutorial-classifying-iris/steps.png)

11. Zamknij Kreatora przygotowywania danych. Wybierz polecenie **Zamknij** (x) na karcie **iris-1** oznaczonej ikoną wykresu. Twoja praca zostanie automatycznie zapisana w pliku **iris-1.dprep** wyświetlanym w obszarze oznaczonym nagłówkiem **Przygotowywanie danych**.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

1. Kliknij prawym przyciskiem myszy plik **iris-1.dprep**, aby wyświetlić menu kontekstowe, a następnie wybierz opcję **Generuj plik kodu dostępu do danych**. 

   ![Generowanie kodu](media/tutorial-classifying-iris/generate_code.png)

2. Zostanie otwarty nowy plik o nazwie **iris-1.py** z następującymi wierszami kodu:

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

   Ten fragment kodu wywołuje logikę utworzoną jako pakiet przygotowywania danych. W zależności od kontekstu uruchamiania kodu element `df` reprezentuje różne rodzaje ramek danych. Element [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) w przypadku wykonywania w środowisku uruchomieniowym języka Python, a element [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) — w przypadku wykonywane w kontekście usługi Spark. 

   Więcej informacji na temat przygotowywania danych w aplikacji Azure Machine Learning Workbench zawiera przewodnik [Get started with data preparation (Wprowadzenie do przygotowywania danych)](data-prep-getting-started.md).

## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z pierwszą z trzech części samouczka wiesz już, jak używać aplikacji Azure Machine Learning Workbench, aby wykonywać następujące czynności:
> [!div class="checklist"]
> * Tworzenie nowego projektu. 
> * Tworzenie pakietu przygotowywania danych.
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych.

Możesz teraz przejść do następnej części serii, w której będziesz kompilować model usługi Azure Machine Learning:
> [!div class="nextstepaction"]
> [Kompilowanie modelu](tutorial-classifying-iris-part-2.md)
