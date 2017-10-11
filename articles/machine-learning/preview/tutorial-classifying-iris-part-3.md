---
title: "Wdrażanie modelu na potrzeby usługi Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). To jest część 3 dotycząca wdrażania modelu."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/27/2017
ms.translationtype: HT
ms.sourcegitcommit: 57278d02a40aa92f07d61684e3c4d74aa0ac1b5b
ms.openlocfilehash: 28d74eb9da4210f59758b90ae88ce5a5eb62621a
ms.contentlocale: pl-pl
ms.lasthandoff: 09/28/2017

---

# <a name="classifying-iris-part-3-deploy-a-model"></a>Klasyfikowanie irysów, część 3: wdrażanie modelu
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest trzecią częścią trzyczęściowej serii. W tej części samouczka użyjesz usługi Azure Machine Learning (wersja zapoznawcza), aby dowiedzieć się, jak:

> [!div class="checklist"]
> * Lokalizowanie pliku modelu
> * Generowanie skryptu oceny i pliku schematu
> * Przygotowywanie środowiska
> * Tworzenie usługi internetowej czasu rzeczywistego
> * Uruchamianie usługi internetowej czasu rzeczywistego
> * Sprawdzanie danych wyjściowych obiektu blob 

 W celu uproszczenia w tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym macOS jest niemal identyczne.

## <a name="prerequisites"></a>Wymagania wstępne
Ukończ najpierw dwie pierwsze części z tej serii samouczków:
- Najpierw zapoznaj się z [samouczkiem dotyczącym przygotowywania danych](tutorial-classifying-iris-part-1.md), aby utworzyć zasoby usługi Azure Machine Learning i zainstalować aplikację Azure Machine Learning Workbench.
- Następnie zapoznaj się z [samouczkiem dotyczącym budowania modelu](tutorial-classifying-iris-part-2.md), aby utworzyć model regresji logistycznej w usłudze Azure Machine Learning.

## <a name="download-the-model-pickle-file"></a>Pobieranie pliku modelu utworzonego w pakiecie pickle
W poprzedniej części samouczka skrypt **iris_sklearn.py** został uruchomiony lokalnie w aplikacji Azure Machine Learning Workbench. Ta akcja spowodowała zserializowanie modelu regresji logistycznej przy użyciu popularnego pakietu **[pickle](https://docs.python.org/2/library/pickle.html)** do serializacji obiektów w języku Python. 

1. Uruchom aplikację **Azure Machine Learning Workbench** i otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu kliknij przycisk **Pliki** (ikona folderu) na lewym pasku narzędzi aplikacji Azure Machine Learning Workbench, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **iris_sklearn.py**, a kod Python zostanie otwarty w nowej karcie edytora tekstów wewnątrz aplikacji Workbench.

4. Przejrzyj plik **iris_sklearn.py**, aby dowiedzieć się, gdzie został wygenerowany plik z pakietu pickle. Użyj kombinacji klawiszy Control+F, aby otworzyć okno dialogowe znajdowania, a następnie znajdź słowo **pickle** w kodzie języka Python.

   Następujący fragment kodu przedstawia, jak został wygenerowany plik wyjściowy z pakietu pickle. Zwróć uwagę na to, że plik wyjściowy z pakietu pickle ma na dysku nazwę **model.pkl**. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Odszukaj plik modelu z pakietu pickle w plikach wyjściowych z poprzedniego przebiegu.
   
   Po uruchomieniu skryptu **iris_sklearn.py** plik modelu został zapisany w folderze **outputs** z nazwą **model.pkl**. Ten folder znajduje się w środowisku wykonywania wybieranym w celu uruchomienia skryptu, a nie w lokalnym folderze projektu. 
   
   - W celu znalezienia pliku użyj aplikacji Azure Machine Learning Workbench, a następnie kliknij przycisk **Przebiegi** (ikona zegara) na lewym pasku narzędzi, aby otworzyć listę **Wszystkie przebiegi**.  
   - Zostanie otwarta karta **Wszystkie przebiegi**. W tabeli przebiegów wybierz jeden z ostatnich przebiegów, w którym miejsce docelowe miało wartość **local**, a skrypt miał nazwę **iris_sklearn.py**. 
   - Zostanie otwarta strona **Właściwości przebiegu**. W prawym górnym narożniku strony zwróć uwagę na sekcję **Dane wyjściowe**. 
   - Pobierz plik z pakietu pickle, zaznaczając pole wyboru obok pliku **model.pkl**, a następnie kliknij przycisk **Pobierz**. Zapisz go w katalogu głównym folderu projektu. Będzie potrzebny w kolejnych krokach.

   ![Pobieranie pliku z pakietu pickle](media/tutorial-classifying-iris/download_model.png)

   Przeczytaj więcej na temat folderu **outputs** w artykule [How to read and write large data files (Jak odczytywać i zapisywać duże pliki danych)](how-to-read-write-files.md).

## <a name="get-scoring-and-schema-files"></a>Pobieranie plików oceniania i plików schematu
Aby wdrożyć usługę internetową wraz z plikiem modelu, potrzebny jest także skrypt oceniania i opcjonalnie schemat dla danych wejściowych usługi internetowej. Skrypt oceniania ładuje plik **model.pkl** z bieżącego folderu i używa go w celu wygenerowania nowo przewidywanej klasy Iris.  

1. Uruchom aplikację **Azure Machine Learning Workbench** i otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu kliknij przycisk **Pliki** (ikona folderu) na lewym pasku narzędzi aplikacji Azure Machine Learning Workbench, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **iris_score.py**. Zostanie otwarty skrypt w języku Python. Ten plik jest używany jako plik oceniania.

   ![Plik oceniania](media/tutorial-classifying-iris/model_data_collection.png)

4. Aby uzyskać plik schematu, uruchom skrypt. Na pasku poleceń wybierz środowisko **local** i skrypt **iris_score.py**, a następnie kliknij przycisk **Przebieg**. 

5. Ten skrypt utworzy w folderze **outputs** plik JSON, który będzie przechwytywał wejściowy schemat danych wymagany przez model.

6. Zwróć uwagę na okienko zadań po prawej stronie okna aplikacji Machine Learning Workbench. Poczekaj, aż stan najnowszego zadania **iris\_score.py** zmieni się na zielony wskaźnik **Ukończono**. Następnie kliknij hiperlink **iris\_score.py [1]** dla najnowszego przebiegu zadania, aby wyświetlić szczegóły przebiegu z przebiegu **iris_score.py**. 

7. Na stronie właściwości przebiegu w sekcji **Dane wyjściowe** wybierz nowo utworzony plik **service_schema.json**. **Sprawdź** plik i kliknij pozycję **Pobierz**. Zapisz plik w folderze głównym projektu.

8. Wróć do poprzedniej karty, na której został otwarty skrypt **iris_score.py**. 

   Zwróć uwagę na użycie zbierania danych, które umożliwia przechwycenie danych wejściowych i prognozowania modelu z usługi internetowej. Poniższe punkty mają szczególne znaczenie w odniesieniu do zbierania danych:

9. Przejrzyj kod u góry klasy ModelDataCollector przeznaczonej do importowania plików, która zawiera funkcję zbierania danych modelu:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Przejrzyj poniższe wiersze kodu, w których funkcja **init()** tworzy wystąpienia klasy ModelDataCollector:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Przejrzyj następujące wiersze kodu, w których funkcja **run(input_df)** zbiera dane wejściowe i dane prognozowania:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

Teraz można przystąpić do przygotowania środowiska do obsługi operacji modelu.

## <a name="prepare-to-operationalize-locally"></a>Przygotowywanie do lokalnej obsługi operacji
Użyj wdrożenia w _trybie lokalnym_, które będzie działać w kontenerach Docker na komputerze lokalnym.

_Trybu lokalnego_ można użyć do tworzenia i testowania. Aparat platformy Docker musi być uruchomiony lokalnie, aby możliwe było wykonanie poniższych kroków w celu rozpoczęcia obsługi operacji modelu. Aby korzystać z pomocy do poleceń, na końcu poleceń można umieszczać flagę `-h`.

>[!NOTE]
>Jeśli nie masz lokalnego aparatu platformy Docker, możesz kontynuować, tworząc dla wdrożenia klaster na platformie Azure. Pamiętaj tylko o usunięciu klastra po ukończeniu korzystania z samouczka, aby nie spowodować stałego naliczania opłat.

1. Otwórz interfejs wiersza polecenia w aplikacji Azure Machine Learning Workbench — w tym celu w menu Plik kliknij polecenie **Otwórz wiersz polecenia**.

   Wiersz polecenia otworzy się w bieżącej lokalizacji folderu projektu, **c:\temp\myIris>**.

2. Upewnij się, że dostawca zasobów platformy Azure **Microsoft.ContainerRegistry** został zarejestrowany w ramach Twojej subskrypcji. Zarejestruj tego dostawcę zasobów, aby można było utworzyć środowisko w kroku 3. Można sprawdzić, czy został on już zarejestrowany za pomocą następującego polecenia:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Powinny zostać wyświetlone dane wyjściowe podobne do poniższych: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   Jeśli dostawca **Microsoft.ContainerRegistry** nie został zarejestrowany, można zarejestrować go za pomocą następującego polecenia:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   Rejestracja może potrwać kilka minut, a jej stan można sprawdzić przy użyciu powyższego polecenia **az provider list** lub następującego polecenia:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   W trzecim wierszu danych wyjściowych znajduje się następująca informacja: **„registrationState”: „Rejestrowanie”**. Poczekaj chwilę i powtarzaj polecenie show do momentu, gdy dane wejściowe będą zawierać następującą informację: **„registrationState”: „Zarejestrowano”**.

3. Utwórz środowisko. Ten krok należy uruchomić jeden raz w każdym środowisku (np. raz w środowisku deweloperskim i raz w środowisku produkcyjnym). Dla pierwszego środowiska użyj _trybu lokalnego_. (Aby później skonfigurować środowisko w _trybie klastra_, możesz w poniższym poleceniu wypróbować przełącznik `-c` lub `--cluster`).

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Postępuj zgodnie z wyświetlanymi instrukcjami, aby aprowizować konto magazynu do przechowywania obrazów platformy Docker, rejestr ACR (Azure Container Registry) do wyświetlania list obrazów platformy Docker oraz konto usługi AppInsight do zbierania danych telemetrycznych. Jeśli został użyty przełącznik `-c`, on utworzy również klaster usługi ACS (Azure Container Service).
   
   Nazwa klastra służy do identyfikowania środowiska. Lokalizacja powinna być taka sama, jak lokalizacja konta zarządzania modelem utworzonego w witrynie Azure Portal.

4. Utwórz konto zarządzania modelami (to jest konfiguracja jednorazowa).  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Ustaw konto zarządzania modelami.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Ustaw środowisko.
Po zakończeniu konfiguracji ustaw zmienne środowiskowe wymagane do obsługi operacji — w tym celu użyj następującego polecenia. Użyj nazwy środowiska zastosowanej wcześniej w kroku 4. Użyj nazwy grupy zasobów, która została wyświetlona w danych wyjściowych w oknie wiersza polecenia po ukończeniu procesu instalacji.
   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Aby sprawdzić, czy środowisko obsługi operacji zostało poprawnie skonfigurowane na potrzeby lokalnego wdrożenia usługi internetowej, wprowadź poniższe polecenie:

   ```azurecli
   az ml env show
   ```

Teraz można przystąpić do tworzenia usługi internetowej czasu rzeczywistego.

## <a name="create-a-real-time-web-service-in-one-command"></a>Tworzenie usługi internetowej czasu rzeczywistego przy użyciu jednego polecenia
1. Użyj poniższego polecenia, aby utworzyć usługę internetową czasu rzeczywistego:

   ```azurecli
   az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Spowoduje to wygenerowanie identyfikatora usługi internetowej, którego będzie można użyć później.

   Następujące przełączniki są używane razem z poleceniem **az ml service create realtime**:
   * -n: nazwa aplikacji, musi zawierać tylko małe litery
   * -f: nazwa pliku skryptu oceniania
   * --model-file: plik modelu — w tym przypadku jest to plik model.pkl przetworzony przez pakiet pickle
   * -r: typ modelu — w tym przypadku jest to model języka Python
   * --collect-model-data true: umożliwia zbieranie danych

   >[!IMPORTANT]
   >Nazwa usługi (która jest również nową nazwą obrazu platformy Docker) musi zawierać tylko małe litery, ponieważ w przeciwnym razie wystąpi błąd. 

2. Po uruchomieniu polecenia model i plik oceniania są ładowane do konta magazynu utworzonego w ramach konfiguracji środowiska. Proces wdrażania tworzy obraz platformy Docker zawierający model, schemat oraz plik oceniania, a następnie wypycha go do rejestru rekordów ACR: **\<nazwa_rekordu_ACR\>.azureacr.io/\<nazwa_obrazu\>:\<wersja\>**. 

   Następnie ściąga ten obraz na komputer lokalny i uruchamia kontener platformy Docker oparty na tym obrazie. Jeśli środowisko jest konfigurowane w trybie klastra, kontener platformy Docker zostaje wdrożony do klastra ACS Kubernetes.

   W ramach wdrożenia na komputerze lokalnym zostaje utworzony punkt końcowy HTTP REST na potrzeby usługi internetowej. Po kilku minutach polecenie powinno zostać zakończone z komunikatem o powodzeniu i usługa internetowa powinna być gotowa do działania.

3. Polecenie **docker ps** umożliwia sprawdzenie działającego kontenera platformy Docker:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-using-separate-commands"></a>Tworzenie usługi internetowej czasu rzeczywistego przy użyciu oddzielnych poleceń
Zamiast używać przedstawionego powyżej polecenia **az ml service create realtime**, można również wykonać kroki oddzielnie. Najpierw należy zarejestrować model, a następnie wygenerować manifest, utworzyć obraz platformy Docker i utworzyć usługę internetową. Takie szczegółowe podejście zapewnia większą elastyczność na każdym kroku. Ponadto jednostki generowane w poszczególnych krokach mogą być używane ponownie i możliwe jest ponowne kompilowanie jednostek tylko w razie potrzeby.

1. Zarejestruj model, podając nazwę pliku z pakietu pickle.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   To polecenie spowoduje wygenerowanie identyfikatora modelu.

2. Tworzenie manifestu

   Aby utworzyć manifest, użyj tego polecenia i podaj wyjściowy identyfikator modelu z poprzedniego kroku:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f iris_score.py -r python -i <model ID> -s service_schema.json
   ```
   To polecenie spowoduje wygenerowanie identyfikatora manifestu.

3. Tworzenie obrazu platformy Docker

   Aby utworzyć obraz platformy Docker, użyj tego polecenia i podaj wyjściowy identyfikator manifestu z poprzedniego kroku:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   To polecenie spowoduje wygenerowanie identyfikatora obrazu platformy Docker.
   
4. Tworzenie usługi

   Aby utworzyć usługę, użyj podanego polecenia i podaj wyjściowy identyfikator obrazu z poprzedniego kroku:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   To polecenie spowoduje wygenerowanie identyfikatora usługi internetowej.

Teraz można przystąpić do uruchomienia usługi internetowej.

## <a name="run-the-real-time-web-service"></a>Uruchamianie usługi internetowej czasu rzeczywistego

Przetestuj uruchomienie usługi internetowej **irisapp**, wprowadzając do niej rekord w kodzie JSON zawierający tablicę czterech losowych liczb.

1. Podczas tworzenia usługi internetowej korzystaliśmy z przykładowych danych. W przypadku pracy w trybie lokalnym można wywołać polecenie **az ml service show realtime**. To wywołanie spowoduje pobranie przykładowego polecenia uruchomienia, które będzie pomocne w przypadku testowania tej usługi. Zostanie również pobrany adres URL oceniania, którego można użyć, aby wprowadzić usługę do własnej aplikacji niestandardowej:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Aby przetestować usługę, uruchom zwrócone polecenie uruchamiania usługi.

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Wynikiem jest **„2”** i jest to przewidywana klasa. (Twoje wyniki mogą się różnić). 

3. Jeśli chcesz uruchomić usługę spoza interfejsu wiersza polecenia, musisz pobrać klucze służące do uwierzytelniania:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Wyświetlanie zebranych danych w magazynie obiektów blob platformy Azure

1. Zaloguj się do [Azure Portal](https://portal.azure.com).

2. Odszukaj konta magazynu. W tym celu kliknij przycisk **Więcej usług**.

3. W polu wyszukiwania wpisz **Konta magazynu** i naciśnij klawisz **Enter**.

4. Ze strony wyszukiwania **Konta magazynu** wybierz zasób **Konto magazynu** pasujący do posiadanego środowiska. 

   > [!TIP]
   > Aby ustalić, które konto magazynu jest używane: otwórz aplikację Azure Machine Learning Workbench, wybierz projekt, nad którym pracujesz, a następnie otwórz wiersz polecenia z menu **Plik**. W wierszu polecenia wpisz `az ml env show -v` i sprawdź wartość *storage_account*. Jest to nazwa używanego konta magazynu.

5. Gdy zostanie otwarta strona **Konto magazynu**, kliknij pozycję **Kontenery** na liście po lewej stronie. Odszukaj kontener o nazwie **modeldata**. 
 
   Jeśli nie są widoczne żadne dane, być może musisz poczekać, aż upłynie 10 minut od pierwszego żądania usługi internetowej — po upływie tego czasu powinno zacząć się propagowanie danych do konta magazynu.

   Dane płyną do obiektów blob przy użyciu następującej ścieżki kontenera:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Z tych danych będzie można korzystać z poziomu obiektów blob platformy Azure. Istnieją różne narzędzia, które korzystają z oprogramowania firmy Microsoft i narzędzi typu open source, takie jak:

   - Azure ML Workbench: otwórz plik csv w aplikacji Azure ML Workbench, dodając plik csv jako źródło danych. 
   - Excel: otwieraj codzienne pliki csv jako arkusz kalkulacyjny.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): twórz wykresy zawierające dane pobierane z danych csv w obiektach blob.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): załaduj dane csv do tabeli programu hive i wykonaj zapytania SQL bezpośrednio w odniesieniu do obiektu blob.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): utwórz ramkę danych zawierającą dużą część danych csv.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z trzecią częścią trzyczęściowej serii samouczków wiesz już, jak używać usługi Azure Machine Learning w celu wykonania następujących czynności:
> [!div class="checklist"]
> * Lokalizowanie pliku modelu
> * Generowanie skryptu oceny i pliku schematu
> * Przygotowywanie środowiska
> * Tworzenie usługi internetowej czasu rzeczywistego
> * Uruchamianie usługi internetowej czasu rzeczywistego
> * Sprawdzanie danych wyjściowych obiektu blob 

Udało Ci się pomyślnie uruchomić skrypt szkolenia w różnych środowiskach obliczeniowych, utworzyć model, serializować model i rozpocząć obsługę operacji modelu za pośrednictwem usługi internetowej opartej na platformie Docker. 

Możesz teraz zająć się zaawansowanym przygotowywaniem danych:
> [!div class="nextstepaction"]
> [Zaawansowane przygotowywanie danych](tutorial-bikeshare-dataprep.md)


