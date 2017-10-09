---
title: "Samouczek dotyczący przygotowywania danych do klasyfikowania irysów w usłudze Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). Jest to część 1 dotycząca przygotowania danych."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/25/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e88b6bf74b7492353e5d5d004bde12fa4787e405
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-1-prepare-data"></a>Klasyfikowanie irysów, część 1: przygotowanie danych
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest pierwszą częścią trzyczęściowej serii. W tym samouczku zapoznamy się z podstawowymi zagadnieniami usługi Azure Machine Learning (wersja zapoznawcza). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Tworzenie projektu w aplikacji Azure Machine Learning Workbench
> * Tworzenie pakietu przygotowywania danych
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

W celu uproszczenia w tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/Iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym macOS jest niemal identyczne.

## <a name="launch-azure-machine-learning-workbench"></a>Uruchamianie aplikacji Azure Machine Learning Workbench
Postępuj zgodnie z instrukcjami przewodnika [Install and create Quickstart (Instalowanie i tworzenie — szybki start)](quickstart-installation.md), aby zainstalować aplikację Azure Machine Learning Workbench, która zawiera również interfejs wiersza polecenia. Uruchom aplikację usługi Azure Machine Learning Workbench i w razie potrzeby zaloguj się.

## <a name="create-a-new-project"></a>Tworzenie nowego projektu
1. W okienku **PROJEKTY** kliknij ikonę **+**, aby utworzyć **nowy projekt**.

   ![nowy obszar roboczy](media/tutorial-classifying-iris/new_ws.png)

2. Wypełnij szczegóły w obszarze **Tworzenie nowego projektu**. 

   ![Nowy projekt](media/tutorial-classifying-iris/new_project.png)

   - Wypełnij pole **Nazwa projektu**, wprowadzając nazwę projektu. Na przykład użyj wartości **myIris**.
   - Wybierz **katalog projektu**, w którym projekt zostanie utworzony. Na przykład użyj wartości **C:\Temp**. 
   - Wprowadź **Opis projektu**. 
   - Pole **Repozytorium Git** jest opcjonalne i może być puste. Istniejące puste repozytorium Git można udostępnić w usłudze VSTS (repozytorium bez gałęzi głównej). Dzięki temu można włączyć roaming i udostępniać scenariusze później. Aby uzyskać więcej informacji, zapoznaj się z artykułem [Using Git repo (Korzystanie z repozytorium Git)](using-git-ml-project.md). 
   - Wybierz **obszar roboczy** — w tym samouczku używany jest przykładowy obszar roboczy **IrisGarden**. 
   - Wybierz szablon **Classifying Iris** z listy szablonów projektu. 

3. Kliknij przycisk **Utwórz**, aby utworzyć projekt. Projekt zostanie utworzony i otwarty.

## <a name="create-a-data-preparation-package"></a>Tworzenie pakietu przygotowywania danych
1. Otwórz plik `iris.csv` z obszaru **Widok plików**. Ten plik jest prostą tabelą z 5 kolumnami i 150 wierszami. Zawiera cztery kolumny elementów liczbowych i docelową kolumnę ciągów. Nie zawiera nagłówków kolumn.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   >Uwaga: nie zaleca się dołączania plików danych do folderu projektu, szczególnie w przypadku, gdy rozmiar pliku jest duży. Dołączamy plik `iris.csv` do tego szablonu w celach demonstracyjnych, ponieważ ten plik jest niewielki. Aby uzyskać więcej informacji, zapoznaj się z artykułem [How to read and write large data files (Jak odczytywać i zapisywać duże pliki danych)](how-to-read-write-files.md).

2. W obszarze **Widok danych** kliknij ikonę **+**, aby dodać nowe źródło danych. Zostanie uruchomiony kreator **Dodawanie źródła danych**. 

   ![widok danych](media/tutorial-classifying-iris/data_view.png)

3. Wybierz opcję **Pliki/Katalog** i wybierz plik lokalny `iris.csv`. Zaakceptuj ustawienia domyślne dla każdego ekranu, a na koniec kliknij pozycję **Zakończ**. 

   ![wybierz iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Na potrzeby tego ćwiczenia wybierz plik `iris.csv` z bieżącego katalogu projektu, ponieważ w przeciwnym wypadku dalsze kroki mogą zakończyć się niepowodzeniem. 

4. Zostanie utworzony nowy plik `iris-1.dsource`. Plik ma unikalną nazwę ze znakami kreski i 1, ponieważ przykładowy projekt zawiera już nienumerowany plik `iris.dsource`.  Plik zostanie otwarty i widoczne będą dane. Do zestawu danych zostaną automatycznie dodane nagłówki kolumn od `Column1` do `Column5`. Przewiń w dół i zwróć uwagę na to, że ostatni wiersz z zestawu danych jest pusty. Dzieje się tak z powodu dodatkowego podziału wiersza w pliku csv.

   ![widok danych irysów](media/tutorial-classifying-iris/iris_data_view.png)

5. Teraz kliknij przycisk **Metryki**. Zapoznaj się z histogramami i kompletnym zestawem statystyk dla poszczególnych kolumn. Możesz również kliknąć przycisk **Dane**, aby ponownie wyświetlić dane. 

   ![widok danych irysów](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Kliknij przycisk **Przygotuj** obok przycisku **Metryki** (albo przycisk **Dane**, jeśli korzystasz z widoku metryk). Pojawi się okno dialogowe **Przygotowywanie**. Projekt przykładowy zawiera już plik `iris.dprep`, dlatego domyślnie prosi o utworzenie nowego przepływu danych w istniejącym pakiecie przygotowania danych **iris.dprep**. Zmień wartość listy rozwijanej na **+Nowy pakiet przygotowania danych** i wprowadź nową wartość „iris-1”, a następnie kliknij przycisk **OK**.

   ![widok danych irysów](media/tutorial-classifying-iris/new_dprep.png)

Nowy pakiet przygotowania bazy o nazwie `iris-1.dprep` zostanie utworzony i otwarty w edytorze przygotowywania danych.

Teraz wykonajmy kilka prostych operacji przygotowania danych. Zmień nazwy kolumn, klikając nagłówki poszczególnych kolumn, dzięki czemu tekst nagłówka stanie się dostępny do edycji. W pięciu kolumnach wprowadź odpowiednio wartości `Sepal Length`, `Sepal Width`, `Petal Length`, `Petal Width` i `Species`.

![zmiana nazw kolumn](media/tutorial-classifying-iris/rename_column.png)

Wybierz kolumnę `Species`, a następnie kliknij ją prawym przyciskiem myszy. Wybierz opcję **Liczby wartości**. 

![liczba wartości](media/tutorial-classifying-iris/value_count.png)

Ta akcja spowoduje utworzenie histogramu z czterema słupkami. Zwróć uwagę na to, że kolumna docelowa zawiera trzy osobne wartości: `Iris_virginica`, `Iris_versicolor`, `Iris-setosa`. Istnieje również jeden wiersz z wartością `(null)`. Pozbądźmy się tego wiersza, wybierając słupek reprezentujący wartość null. Aby go usunąć, klikamy przycisk filtru **-**. 

![liczba wartości](media/tutorial-classifying-iris/filter_out.png)

Podczas zmiany nazw kolumn i usuwania wiersza o wartości null każde wykonywane działanie jest rejestrowane jako krok przygotowania danych w okienku **KROKI**. Te kroki można edytować (aby dostosować ich ustawienia), można zmieniać ich kolejność, a nawet je usuwać.

![kroki](media/tutorial-classifying-iris/steps.png)

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

Teraz zamknij edytor przygotowania danych. (Nie martw się — zawartość jest zapisywana automatycznie). Kliknij prawym przyciskiem myszy plik **iris-1.dprep**, aby wyświetlić menu kontekstowe, a następnie wybierz opcję **Generuj plik kodu dostępu do danych**. 

![generowanie kodu](media/tutorial-classifying-iris/generate_code.png)

Zostanie utworzony plik **iris-1.py** z następującymi wstępnie wypełnionymi dwoma wierszami kodu (razem z komentarzami):

```python
# This code snippet will load the referenced package and return a DataFrame.
# If the code is run in a PySpark environment, the code will return a
# Spark DataFrame. If not, the code will return a Pandas DataFrame.

from azureml.dataprep.package import run
df = run('iris.dprep', dataflow_idx=0)
```
Ten fragment kodu wywołuje logikę utworzoną jako pakiet przygotowywania danych. W zależności od kontekstu, w którym ten kod jest uruchamiany, `df` może być klasą [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) — gdy jest uruchamiany w środowisku uruchomieniowym Python, albo klasą [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html), gdy jest uruchamiany w kontekście platformy Spark. Więcej informacji na temat przygotowywania danych w aplikacji Azure Machine Learning Workbench zawiera przewodnik [Getting Started with Data Preparation (Wprowadzenie do przygotowywania danych)](data-prep-getting-started.md).

## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z pierwszą z trzech części samouczka wiesz już, jak używać aplikacji Azure Machine Learning Workbench, aby wykonywać następujące czynności:
> [!div class="checklist"]
> * Tworzenie nowego projektu 
> * Tworzenie pakietu przygotowywania danych
> * Generowanie kodu Python/PySpark w celu wywołania pakietu przygotowywania danych

Możesz teraz przejść do następnej części serii, w której będziesz kompilować model uczenia maszynowego.
> [!div class="nextstepaction"]
> [Kompilowanie modelu](tutorial-classifying-iris-part-2.md)

