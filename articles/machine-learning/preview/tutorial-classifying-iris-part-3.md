---
title: "Wdrażanie modelu na potrzeby usług Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). W części trzeciej omówiono wdrażanie modelu."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 11/2/2017
ms.openlocfilehash: b6cdd135d2d264c8b4ede1592c686cdeea3d0a59
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="classify-iris-part-3-deploy-a-model"></a>Klasyfikowanie irysów, część 3: wdrażanie modelu
Usługi Azure Machine Learning (wersja zapoznawcza) stanowią zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków. Pozwala ono analitykom przygotowywać dane, opracowywać eksperymenty i wdrażać modele na skalę chmury.

Niniejszy samouczek jest trzecią częścią trzyczęściowej serii. W tej części samouczka użyjesz usługi Azure Machine Learning (wersja zapoznawcza), aby:

> [!div class="checklist"]
> * Lokalizowanie pliku modelu.
> * Generowanie skryptu oceny i pliku schematu.
> * Przygotowywanie środowiska.
> * Tworzenie usługi internetowej czasu rzeczywistego.
> * Uruchamianie usługi internetowej czasu rzeczywistego.
> * Sprawdzanie danych wyjściowych obiektu blob. 

 W tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym Mac OS jest niemal identyczne.

## <a name="prerequisites"></a>Wymagania wstępne
Ukończ najpierw dwie pierwsze części z tej serii samouczków:

   * Zapoznaj się z [samouczkiem dotyczącym przygotowywania danych](tutorial-classifying-iris-part-1.md), aby utworzyć zasoby usługi Azure Machine Learning i zainstalować aplikację Azure Machine Learning Workbench.

   * Zapoznaj się z [samouczkiem dotyczącym budowania modelu](tutorial-classifying-iris-part-2.md), aby utworzyć model regresji logistycznej w usłudze Azure Machine Learning.

Aparat platformy Docker musisz zainstalować i uruchomić lokalnie. Alternatywnie możesz przeprowadzić wdrożenie w klastrze usługi Azure Container Service na platformie Azure.

## <a name="download-the-model-pickle-file"></a>Pobieranie pliku modelu utworzonego w pakiecie pickle
W poprzedniej części samouczka skrypt **iris_sklearn.py** został uruchomiony lokalnie w programie Azure Machine Learning Workbench. Ta akcja spowodowała zserializowanie modelu regresji logistycznej przy użyciu popularnego pakietu [pickle](https://docs.python.org/2/library/pickle.html) do serializacji obiektów w języku Python. 

1. Otwórz aplikację Azure Machine Learning Workbench i otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu wybierz przycisk **Pliki** (ikona folderu) w okienku po lewej stronie, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **iris_sklearn.py**. Kod Python zostanie otwarty w nowej karcie edytora tekstów wewnątrz aplikacji Workbench.

4. Przejrzyj plik **iris_sklearn.py**, aby dowiedzieć się, gdzie został wygenerowany plik z pakietu pickle. Użyj kombinacji klawiszy Ctrl+F, aby otworzyć okno dialogowe **znajdowania**, a następnie znajdź słowo **pickle** w kodzie języka Python.

   Następujący fragment kodu przedstawia, jak został wygenerowany plik wyjściowy z pakietu pickle. Plik wyjściowy z pakietu pickle ma na dysku nazwę **model.pkl**. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Odszukaj plik modelu z pakietu pickle w plikach wyjściowych z poprzedniego przebiegu.
   
   Po uruchomieniu skryptu **iris_sklearn.py** plik modelu został zapisany w folderze **outputs** z nazwą **model.pkl**. Ten folder znajduje się w środowisku wykonywania wybieranym w celu uruchomienia skryptu, a nie w lokalnym folderze projektu. 
   
   - Aby zlokalizować plik, otwórz listę **Wszystkie uruchomienia**, wybierając przycisk **Uruchomienia** (ikona zegara) w lewym okienku.  
   - Zostanie otwarta karta **Wszystkie przebiegi**. W tabeli przebiegów wybierz jeden z ostatnich przebiegów, w którym miejsce docelowe miało wartość **local**, a skrypt miał nazwę **iris_sklearn.py**. 
   - Zostanie otwarte okienko **Właściwości przebiegu**. W prawej górnej sekcji okienka zwróć uwagę na sekcję **Dane wyjściowe**. 
   - Aby pobrać plik z pakietu pickle, zaznacz pole wyboru obok pliku **model.pkl**, a następnie wybierz przycisk **Pobierz**. Zapisz go w katalogu głównym folderu projektu. Plik jest potrzebny w kolejnych krokach.

   ![Pobieranie pliku z pakietu pickle](media/tutorial-classifying-iris/download_model.png)

   Przeczytaj więcej na temat folderu `outputs` w artykule [How to read and write large data files (Jak odczytywać i zapisywać duże pliki danych)](how-to-read-write-files.md).

## <a name="get-the-scoring-script-and-schema-files"></a>Generowanie skryptu oceny i plików schematu
Aby wdrożyć usługę internetową wraz z plikiem modelu, potrzebny jest także skrypt oceniania i opcjonalnie schemat dla danych wejściowych usługi internetowej. Skrypt oceniania ładuje plik **model.pkl** z bieżącego folderu i używa go w celu wygenerowania nowo przewidywanej klasy Iris.  

1. Otwórz aplikację Azure Machine Learning Workbench, a następnie otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu wybierz przycisk **Pliki** (ikona folderu) w okienku po lewej stronie, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **score_iris.py**. Zostanie otwarty skrypt w języku Python. Ten plik jest używany jako plik oceniania.

   ![Plik oceniania](media/tutorial-classifying-iris/model_data_collection.png)

4. Aby uzyskać plik schematu, uruchom skrypt. Na pasku poleceń wybierz środowisko **local** i skrypt **score_iris.py**, a następnie wybierz przycisk **Przebieg**. 

5. Ten skrypt utworzy w sekcji **Dane wyjściowe** plik JSON, który będzie przechwytywał wejściowy schemat danych wymagany przez model.

6. Zwróć uwagę na okienko **Zadania** po prawej stronie okienka **Pulpit nawigacyjny projektu**. Poczekaj, aż stan najnowszego zadania **score_iris.py** zmieni się na zielony wskaźnik **Ukończono**. Następnie wybierz hiperlink **score_iris.py [1]** dla najnowszego przebiegu zadania, aby wyświetlić szczegóły dotyczące przebiegu **score_iris.py**. 

7. W okienku **Właściwości przebiegu** w sekcji **Dane wyjściowe** wybierz nowo utworzony plik **service_schema.json**.  Zaznacz pole wyboru obok nazwy pliku, a następnie wybierz polecenie **Pobierz**. Zapisz plik w folderze głównym projektu.

8. Wróć do poprzedniej karty, na której został otwarty skrypt **score_iris.py**. Użycie zbierania danych umożliwia przechwycenie danych wejściowych i prognozowanie modelu z usługi internetowej. Poniższe kroki mają szczególne znaczenie w odniesieniu do zbierania danych.

9. Przejrzyj kod u góry klasy **ModelDataCollector** przeznaczonej do importowania plików, ponieważ zawiera funkcję zbierania danych modelu:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Przejrzyj poniższe wiersze kodu, w których funkcja **init()** tworzy wystąpienia klasy **ModelDataCollector**:

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

>[!NOTE]
>Aby móc wdrażać modele, musisz mieć dostęp właściciela do subskrypcji platformy Azure.

## <a name="prepare-to-operationalize-locally"></a>Przygotowywanie do lokalnej obsługi operacji
Użyj wdrożenia w _trybie lokalnym_, które będzie działać w kontenerach Docker na komputerze lokalnym.

_Trybu lokalnego_ można użyć do tworzenia i testowania. Aparat platformy Docker musi być uruchomiony lokalnie, aby możliwe było wykonanie poniższych kroków w celu rozpoczęcia obsługi operacji modelu. Aby korzystać z pomocy do poleceń, na końcu poleceń można umieszczać flagę `-h`.

>[!NOTE]
>Jeśli nie masz lokalnego aparatu platformy Docker, możesz kontynuować, tworząc dla wdrożenia klaster na platformie Azure. Pamiętaj tylko o usunięciu klastra po ukończeniu korzystania z samouczka, aby nie spowodować stałego naliczania opłat.

1. Otwórz interfejs wiersza polecenia.
   W aplikacji Azure Machine Learning Workbench w menu **Plik** wybierz polecenie **Otwórz wiersz polecenia**.

   Wiersz polecenia otworzy się w bieżącej lokalizacji folderu projektu, **c:\temp\myIris>**.

2. Upewnij się, że dostawca zasobów platformy Azure **Microsoft.ContainerRegistry** został zarejestrowany w ramach Twojej subskrypcji. Należy zarejestrować tego dostawcę zasobów, aby można było utworzyć środowisko w kroku 3. Można sprawdzić, czy został on już zarejestrowany, używając następującego polecenia:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Powinny pojawić się następujące dane wyjściowe: 
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
   Rejestracja może potrwać kilka minut. Stan można sprawdzić za pomocą poprzedniego polecenia **az provider list** lub następującego polecenia:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   W trzecim wierszu danych wyjściowych znajduje się następująca informacja: **„registrationState”: „Rejestrowanie”**. Poczekaj chwilę i powtarzaj polecenie **show** do momentu, gdy dane wejściowe będą zawierać następującą informację: **„registrationState”: „Registered”**.

3. Utwórz środowisko. Ten krok należy uruchomić raz dla każdego środowiska. Możesz na przykład uruchomić go jeden raz dla środowiska programistycznego i jeden raz dla środowiska produkcyjnego. Dla pierwszego środowiska użyj _trybu lokalnego_. Aby później skonfigurować środowisko w _trybie klastra_, możesz w poniższym poleceniu wypróbować przełącznik `-c` lub `--cluster`:

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Postępuj zgodnie z wyświetlanymi instrukcjami, aby aprowizować konto magazynu do przechowywania obrazów platformy Docker, rejestr Azure Container Registry, który wyświetla listy obrazów platformy Docker oraz konto usługi AppInsight, które zbiera dane telemetryczne. Jeśli został użyty przełącznik `-c`, utworzy on również klaster usługi Azure Container Service.
   
   Nazwa klastra służy do identyfikowania środowiska. Lokalizacja powinna być taka sama jak lokalizacja konta Zarządzanie modelami utworzonego w witrynie Azure Portal.

4. Utwórz konto zarządzania modelami. (Jest to jednorazowa konfiguracja).  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Ustaw konto zarządzania modelami.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Ustaw środowisko.

   Po zakończeniu konfiguracji użyj następującego polecenia, aby ustawić zmienne środowiskowe wymagane do obsługi operacji środowiska. Użyj nazwy środowiska zastosowanej wcześniej w kroku 4. Użyj tej samej nazwy grupy zasobów, która została wyświetlona w danych wyjściowych w oknie wiersza polecenia po ukończeniu procesu instalacji.

   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Aby sprawdzić, czy środowisko obsługi operacji zostało poprawnie skonfigurowane na potrzeby lokalnego wdrożenia usługi internetowej, wprowadź poniższe polecenie:

   ```azurecli
   az ml env show
   ```

Teraz można przystąpić do tworzenia usługi internetowej czasu rzeczywistego.

>[!NOTE]
>Można ponownie użyć konta i środowiska usługi Zarządzanie modelami w przypadku kolejnych wdrożeń usług internetowych. Nie trzeba tworzyć ich dla każdej usługi internetowej. Z kontem lub środowiskiem można skojarzyć wiele usług internetowych.

## <a name="create-a-real-time-web-service-in-one-command"></a>Tworzenie usługi internetowej czasu rzeczywistego przy użyciu jednego polecenia
1. Aby utworzyć usługę internetową czasu rzeczywistego, użyj poniższego polecenia:

   ```azurecli
   az ml service create realtime -f score_iris.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Spowoduje to wygenerowanie identyfikatora usługi internetowej, którego będzie można użyć później.

   Następujące przełączniki są używane razem z poleceniem **az ml service create realtime**:
   * `-n`: nazwa aplikacji, która musi zawierać tylko małe litery.
   * `-f`: nazwa pliku skryptu oceniania.
   * `--model-file`: plik modelu. W tym przypadku jest to plik z pakietu pickle model.pkl.
   * `-r`: typ modelu. W tym przypadku jest to model języka Python.
   * `--collect-model-data true`: umożliwia zbieranie danych.

   >[!IMPORTANT]
   >Nazwa usługi, która jest również nową nazwą obrazu platformy Docker, musi zawierać tylko małe litery. W przeciwnym razie wystąpi błąd. 

2. Po uruchomieniu polecenia model i plik oceniania są ładowane do konta magazynu utworzonego w ramach konfiguracji środowiska. Proces wdrażania tworzy obraz platformy Docker zawierający model, schemat oraz plik oceniania, a następnie wypycha go do rejestru rekordów Azure Container Registry: **\<nazwa_rekordu_ACR\>.azureacr.io/\<nazwa_obrazu\>:\<wersja\>**. 

   Polecenie ściąga obraz na komputer lokalny i uruchamia kontener Docker oparty na tym obrazie. Jeśli środowisko jest konfigurowane w trybie klastra, kontener platformy Docker zostaje wdrożony do klastra Azure Cloud Services Kubernetes.

   W ramach wdrożenia na komputerze lokalnym zostaje utworzony punkt końcowy HTTP REST na potrzeby usługi internetowej. Po kilku minutach polecenie powinno zostać zakończone z komunikatem o powodzeniu i usługa internetowa powinna być gotowa do działania.

3. Aby sprawdzić działanie kontenera Docker, użyj polecenia **docker ps**:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Tworzenie usługi internetowej czasu rzeczywistego przy użyciu oddzielnych poleceń
Zamiast używać przedstawionego wcześniej polecenia **az ml service create realtime**, można również wykonać kroki oddzielnie. 

Najpierw należy zarejestrować model. Następnie należy wygenerować manifest, utworzyć obraz platformy Docker i utworzyć usługę internetową. Takie szczegółowe podejście zapewnia większą elastyczność na każdym kroku. Ponadto jednostki generowane w poszczególnych krokach mogą być używane ponownie i możliwe jest ponowne kompilowanie jednostek tylko w razie potrzeby.

1. Zarejestruj model, podając nazwę pliku z pakietu pickle.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   To polecenie spowoduje wygenerowanie identyfikatora modelu.

2. Utwórz manifest.

   Aby utworzyć manifest, użyj następującego polecenia i podaj wyjściowy identyfikator modelu z poprzedniego kroku:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
   ```
   To polecenie spowoduje wygenerowanie identyfikatora manifestu.

3. Utwórz obraz platformy Docker.

   Aby utworzyć obraz platformy Docker, użyj następującego polecenia i podaj wyjściowy identyfikator manifestu z poprzedniego kroku:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   To polecenie spowoduje wygenerowanie identyfikatora obrazu platformy Docker.
   
4. Utwórz usługę.

   Aby utworzyć usługę, użyj następującego polecenia i podaj wyjściowy identyfikator obrazu z poprzedniego kroku:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   To polecenie spowoduje wygenerowanie identyfikatora usługi internetowej.

Teraz można przystąpić do uruchomienia usługi internetowej.

## <a name="run-the-real-time-web-service"></a>Uruchamianie usługi internetowej czasu rzeczywistego

W celu przetestowania uruchomienia usługi internetowej **irisapp** użyj rekordu w kodzie JSON zawierającego tablicę czterech losowych liczb:

1. Usługa internetowa zawiera przykładowe dane. W przypadku pracy w trybie lokalnym można wywołać polecenie **az ml service show realtime**. To wywołanie spowoduje pobranie przykładowego polecenia uruchomienia, które będzie pomocne w przypadku testowania tej usługi. Zostanie również pobrany adres URL oceniania, którego można użyć, aby wprowadzić usługę do własnej aplikacji niestandardowej:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Aby przetestować usługę, uruchom zwrócone polecenie uruchamiania usługi:

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Wynikiem jest **„2”** i jest to przewidywana klasa. (Twoje wyniki mogą się różnić). 

3. Aby uruchomić usługę spoza interfejsu wiersza polecenia, musisz pobrać klucze służące do uwierzytelniania:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Wyświetlanie zebranych danych w magazynie obiektów blob platformy Azure

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Odszukaj konta magazynu. W tym celu wybierz przycisk **Więcej usług**.

3. W polu wyszukiwania wprowadź frazę **konta magazynu**, a następnie naciśnij klawisz **Enter**.

4. Z pola wyszukiwania **Konta magazynu** wybierz zasób **Konto magazynu** pasujący do posiadanego środowiska. 

   > [!TIP]
   > Aby określić, które konto magazynu jest używane:
   > 1. Otwórz program Azure Machine Learning Workbench.
   > 2. Wybierz projekt, nad którym pracujesz.
   > 3. Z menu **Plik** otwórz wiersz polecenia.
   > 4. W wierszu polecenia wprowadź wartość `az ml env show -v` i sprawdź wartość *storage_account*. Jest to nazwa używanego konta magazynu.

5. Gdy okienko **Konto magazynu** zostanie otwarte, z listy po lewej stronie wybierz pozycję **Kontenery**. Odszukaj kontener o nazwie **modeldata**. 
 
   Jeśli nie są widoczne żadne dane, być może musisz poczekać, aż upłynie 10 minut od pierwszego żądania usługi internetowej — po upływie tego czasu powinno zacząć się propagowanie danych do konta magazynu.

   Dane płyną do obiektów blob przy użyciu następującej ścieżki kontenera:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Z tych danych będzie można korzystać z poziomu obiektów blob platformy Azure. Istnieją różne narzędzia, które korzystają z oprogramowania firmy Microsoft i narzędzi typu open source, takie jak:

   - Azure Machine Learning: otwórz plik CSV, dodając plik CSV jako źródło danych. 
   - Excel: otwórz dzienne pliki CSV jako arkusz kalkulacyjny.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): twórz wykresy zawierające dane pobierane z danych CSV w obiektach blob.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): załaduj dane CSV do tabeli programu hive i wykonaj zapytania SQL bezpośrednio w odniesieniu do obiektów blob.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): utwórz ramkę danych zawierającą dużą część danych CSV.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z trzecią częścią trzyczęściowej serii samouczków wiesz już, jak używać usług Azure Machine Learning w celu wykonania następujących czynności:
> [!div class="checklist"]
> * Lokalizowanie pliku modelu.
> * Generowanie skryptu oceny i pliku schematu.
> * Przygotowywanie środowiska.
> * Tworzenie usługi internetowej czasu rzeczywistego.
> * Uruchamianie usługi internetowej czasu rzeczywistego.
> * Sprawdzanie danych wyjściowych obiektu blob. 

Udało Ci się pomyślnie uruchomić skrypt szkolenia w różnych środowiskach obliczeniowych, utworzyć model, serializować model i rozpocząć obsługę operacji modelu za pośrednictwem usługi internetowej opartej na platformie Docker. 

Możesz teraz zająć się zaawansowanym przygotowywaniem danych:
> [!div class="nextstepaction"]
> [Zaawansowane przygotowywanie danych](tutorial-bikeshare-dataprep.md)
