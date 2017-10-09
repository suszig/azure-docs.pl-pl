---
title: "Budowanie modelu na potrzeby usługi Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs"
description: "W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). Jest to część 2 poświęcona eksperymentom."
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
ms.openlocfilehash: f1f270b95a60e847ce7d29c2d7d60c57891924cc
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-2-build-a-model"></a>Klasyfikowanie irysów, część 2: budowanie modelu
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest drugą częścią trzyczęściowej serii. W tej części samouczka użyjesz usługi Azure Machine Learning (wersja zapoznawcza), aby dowiedzieć się, jak:

> [!div class="checklist"]
> * Pracować w aplikacji Azure Machine Learning Workbench
> * Otwierać skrypty i przeglądać kod
> * Wykonywać skrypty w środowisku lokalnym
> * Przeglądać historię uruchamiania
> * Wykonywać skrypty w lokalnym środowisku platformy Docker
> * Wykonywać skrypty w oknie lokalnego interfejsu wiersza polecenia platformy Azure
> * Wykonywać skrypty w zdalnym środowisku platformy Docker
> * Wykonywać skrypty w środowisku usługi HDInsight w chmurze

W celu uproszczenia w tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/Iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym macOS jest niemal identyczne.

## <a name="prerequisites"></a>Wymagania wstępne
Ukończ najpierw pierwszą część z tej serii samouczka. Przed rozpoczęciem niniejszego samouczka najpierw prześledź [samouczek dotyczący przygotowywania danych](tutorial-classifying-iris-part-1.md), aby utworzyć zasoby usługi Azure Machine Learning i zainstalować aplikację Azure Machine Learning Workbench.

## <a name="review-irissklearnpy-and-configuration-files"></a>Przeglądanie pliku iris_sklearn.py i plików konfiguracji
1. Uruchom aplikację **Azure Machine Learning Workbench** i otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu kliknij przycisk **Pliki** (ikona folderu) na lewym pasku narzędzi aplikacji Azure Machine Learning Workbench, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **iris_sklearn.py**, a kod Python zostanie otwarty w nowej karcie edytora tekstów wewnątrz aplikacji Workbench.

   ![otwieranie pliku](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Kod, który zostanie wyświetlony, nie musi być dokładnie taki sam jak kod powyżej, ponieważ ten przykładowy projekt jest często aktualizowany.

4. Przejrzyj kod skryptu Python, aby zapoznać się ze stylem kodowania. Zwróć uwagę na to, że skrypt wykonuje następujące zadania:

   - Ładuje pakiet **iris.dprep** przeznaczony do przygotowania danych celem utworzenia struktury danych [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Używamy pakietu `iris.dprep` przeznaczonego do przygotowania danych, który jest dostarczany razem z projektem przykładowym i powinien być taki sam jak plik `iris-1.dprep` utworzony w części 1 tego samouczka.

   - Dodaje losowe funkcje utrudniające rozwiązanie problemu. (Losowość jest konieczna, ponieważ Iris jest małym zestawem danych, który można łatwo sklasyfikować z niemal 100-procentową dokładnością).

   - Używa biblioteki [scikit-learn](http://scikit-learn.org/stable/index.html) uczenia maszynowego w celu zbudowania prostego modelu regresji logistycznej. 

   - Przy użyciu biblioteki [pickle](https://docs.python.org/2/library/pickle.html) serializuje model do pliku w folderze `outputs`, a następnie ładuje go i deserializuje z powrotem do pamięci.

   - Za pomocą modelu po deserializacji przewiduje nowy rekord. 

   - Kreśli dwa wykresy — macierz pomyłek oraz wieloklasową krzywą ROC — używając biblioteki [matplotlib](https://matplotlib.org/), a następnie zapisuje je w folderze `outputs`.

   - Przez cały czas działania kodu używany jest obiekt `run_logger` w celu rejestracji współczynnika uregulowania i dokładności modelu w dziennikach, które są następnie automatycznie wykreślane w historii uruchamiania.


## <a name="execute-irissklearnpy-script-in-local-environment"></a>Wykonywanie skryptu iris_sklearn.py w środowisku lokalnym

Przygotujmy się do uruchomienia skryptu **iris_sklearn.py** po raz pierwszy. Ten skrypt wymaga pakietów scikit-learn i matplotlib. Pakiet **scikit-learn** jest już zainstalowany przez aplikację Azure ML Workbench. Jednak należy zainstalować pakiet **matplotlib**. 

1. W aplikacji Azure Machine Learning Workbench kliknij menu **Plik** i wybierz polecenie **Otwórz wiersz polecenia**, aby uruchomić wiersz polecenia. To okno interfejsu wiersza polecenia nazywamy oknem interfejsu wiersza polecenia aplikacji Azure Machine Learning Workbench (oknem CLI).

2. W oknie interfejsu wiersza polecenia wpisz następujące polecenie, aby zainstalować pakiet języka Python o nazwie **matplotlib**. Powinno to zająć mniej niż minutę.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Jeśli pominiesz powyższe polecenie `pip install`, kod `iris_sklearn.py` zostanie uruchomiony, ale nie wygeneruje wykresu wyjściowej macierzy pomyłek ani wykresu wieloklasowej krzywej ROC, które przedstawiono w wizualizacjach historycznych.

3. Wróć do okna aplikacji Workbench. 

4. W lewym górnym rogu karty **iris_sklearn.py** obok ikony zapisywania kliknij listę rozwijaną, aby wybrać opcję **Uruchom konfigurację**.  Wybierz **lokalne** jako środowisko wykonawcze, a `iris_sklearn.py` jako skrypt do uruchomienia.

5. Następnie — po prawej stronie na tej samej karcie — wypełnij pole **Argumenty** wartością `0.01`. 

   ![img](media/tutorial-classifying-iris/run_control.png)

6. Kliknij przycisk **Uruchom**. Nastąpi natychmiastowe zaplanowanie zadania. To zadanie jest widoczne w panelu **Zadania** po prawej stronie okna aplikacji Workbench. 

7. Po kilku chwilach stan zadania zostanie zmieniony z **Przesyłanie** na **Uruchomiono** i na koniec będzie miał wartość **Zakończono**.

   ![uruchamianie skryptu sklearn](media/tutorial-classifying-iris/run_sklearn.png)

8. Kliknij wyraz **Ukończono** w tekście stanu zadania w panelu Zadania. Zostanie otwarte okno podręczne, w którym widoczny będzie tekst wyjścia standardowego (stdout) działającego skryptu. Aby zamknąć tekst stdout, kliknij przycisk **X** w prawym górnym rogu okna podręcznego.

9. W tym samym stanie zadania w panelu Zadania kliknij niebieski tekst **iris_sklearn.py [1]** tuż nad stanem **Zakończono** i czasem uruchomienia. Zostanie otwarta strona **Właściwości uruchomienia**, w której widoczne będą właściwości uruchomienia, pliki **Dane wyjściowe**, **Wizualizacje** oraz **Dzienniki** z danego uruchomienia. 

   Gdy uruchomienie zostanie zakończone, pojawi się okno podręczne z następującymi wynikami:

   >[!NOTE]
   >Do zestawu szkoleniowego wprowadziliśmy generowanie losowe, dlatego uzyskane wyniki mogą się nieznacznie różnić od widocznych tutaj.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details page.
   ```

10. Zamknij kartę **Właściwości uruchomienia**, a następnie wróć do karty **iris_sklearn.py**. 

11. Powtórz dodatkowe uruchomienia. 

    Do pola **Argumenty** wprowadź szereg różnych wartości liczbowych z zakresu od `0.001` do `10`. Kliknij przycisk **Uruchom**, aby wykonać ten kod jeszcze kilka razy. Zmieniana za każdym razem wartość argumentu jest wprowadzana do algorytmu regresji logistycznej w kodzie, przez co wyniki są za każdym razem różne.

## <a name="review-run-history-in-detail"></a>Przeglądanie szczegółów historii uruchamiania
W aplikacji Azure Machine Learning Workbench każde wykonanie skryptu jest przechwytywane jako rekord w historii uruchamiania. Historię uruchamiania konkretnego skryptu można wyświetlić, otwierając widok **Uruchomienia**.

1. Kliknij przycisk **Uruchomienia** (ikona zegara) na pasku narzędzi po lewej stronie, aby otworzyć listę **Uruchomienia**. Następnie kliknij skrypt **iris_sklearn.py**, aby wyświetlić **Pulpit nawigacyjny uruchomień** skryptu `iris_sklearn.py`.

   ![img](media/tutorial-classifying-iris/run_view.png)

2. Zostanie otwarta karta **Pulpit nawigacyjny uruchomień**. Przejrzyj statystyki przechwycone z wielu uruchomień. Wykresy są renderowane u góry karty, a u dołu strony znajduje się tabela z poszczególnymi uruchomieniami, które są ponumerowane i wyświetlane ze szczegółami.

   ![img](media/tutorial-classifying-iris/run_dashboard.png)

3. Filtruj tabelę i klikaj wykresy interakcyjnie, aby wyświetlić stan, czas trwania, dokładność i współczynnik uregulowania każdego uruchomienia. 

4. W tabeli **Uruchomienia** wybierz dwa lub trzy uruchomienia, a następnie kliknij przycisk **Porównaj**, aby otworzyć stronę szczegółowego porównania. Porównaj pozycje zestawione obok siebie. Kliknij przycisk Wstecz **listy Uruchomienia** w lewym górnym rogu strony porównania, aby wrócić do **Pulpitu nawigacyjnego uruchomień**.

5. Kliknij pojedyncze uruchomienie, aby wyświetlić widok szczegółów uruchomienia. Zwróć uwagę na to, że statystyki dotyczące wybranego uruchomienia będą widoczne w sekcji _Właściwości uruchomienia_. Pliki zapisane w folderze danych wyjściowych będą widoczne na liście w sekcji **Dane wyjściowe** i można je pobrać.

   ![img](media/tutorial-classifying-iris/run_details.png)

   Dwa wykresy — macierz pomyłek i wieloklasowa krzywa ROC — zostaną zrenderowane w sekcji **Wizualizacje**. Wszystkie pliki dziennika można znaleźć w sekcji **Dzienniki**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Wykonywanie skryptów w lokalnym środowisku platformy Docker

Usługa Azure ML umożliwia łatwe konfigurowanie dodatkowych środowisk wykonawczych, takich jak platforma Docker, i uruchamianie skryptów w tych środowiskach. 

>[!IMPORTANT]
>Aby można było wykonać ten krok, aparat platformy Docker musi być lokalnie zainstalowany i uruchomiony. Aby uzyskać więcej informacji, zajrzyj do przewodnika po instalacji.

1. Na pasku narzędzi po lewej stronie wybierz ikonę folderu, aby otworzyć listę **Pliki** dla projektu. Rozwiń folder `aml_config`. 

2. Zauważ, że istnieje kilka wstępnie skonfigurowanych środowisk, takich jak **docker-python**, **docker-spark** oraz **lokalne**. 

   Każde środowisko ma dwa pliki, takie jak `docker-python.compute` i `docker-python.runconfig`. Otwórz plik każdego rodzaju, aby zobaczyć, że niektóre opcje można skonfigurować w edytorze tekstów.  

   Zamknij (X) karty, aby wyczyścić każdy otwarty edytor tekstów.

3. Uruchom skrypt **iris_sklearn.py**, używając środowiska **docker-python**. 

   - Na pasku po lewej stronie kliknij ikonę zegara, aby otworzyć panel **Uruchomienia**. Kliknij opcję **Wszystkie uruchomienia**. 
   - U góry karty **Wszystkie uruchomienia** wybierz **docker-python** jako środowisko docelowe zamiast domyślnego środowiska **lokalnego**. 
   - Następnie po prawej stronie wybierz **iris_sklearn.py** jako skrypt do uruchomienia. 
   - Pozostaw pole **Argumenty** puste, ponieważ skrypt określa wartość domyślną. 
   - Kliknij przycisk **Uruchom**.

4. Zostanie uruchomione nowe zadanie, co będzie pokazane w panelu **Zadania** po prawej stronie okna aplikacji Workbench.

   Jeśli uruchamiasz skrypt na platformie Docker po raz pierwszy, uruchomienie trwa kilka minut dłużej. 

   W tle aplikacja Azure Machine Learning Workbench tworzy nowy plik docker odwołujący się do podstawowego obrazu platformy Docker, który jest określony w pliku `docker.compute`, a także tworzy pakiety zależności Python określone w pliku `conda_dependencies.yml`. Następnie aparat platformy Docker pobiera obraz podstawowy z platformy Azure, instaluje pakiety Python określone w pliku `conda_dependencies.yml` i uruchamia kontener platformy Docker. Później kopiuje (albo — w zależności od konfiguracji uruchamiania — tworzy do niej odniesienie) lokalną kopię folderu projektu i wykonuje skrypt `iris_sklearn.py`. Wynik końcowy powinien być dokładnie taki sam jak wtedy, gdy miejscem docelowym jest środowisko **lokalne**.

5. Teraz wypróbujmy platformę Spark. Obraz podstawowy platformy Docker zawiera preinstalowane i skonfigurowane wystąpienie platformy Spark. W związku z tym można za jego pomocą wykonać skrypt PySpark. Jest to prosty sposób tworzenia i testowania programu na platformę Spark bez konieczności samodzielnego instalowania i konfigurowania platformy Spark. 

   Otwórz plik `iris_pyspark.py`. Ten skrypt ładuje plik danych `iris.csv` i używa algorytmu regresji logistycznej z biblioteki Spark ML do klasyfikowania zestawu danych Iris. Teraz zmień środowisko uruchomieniowe na **docker spark**, a skrypt na **iris_pyspark.py** i uruchom ponownie. Trwa to nieco dłużej, ponieważ sesja platformy Spark musi zostać utworzona i uruchomiona w kontenerze platformy Docker. Możesz także zobaczyć, że wyjście standardowe różni się od wyjścia standardowego dla pliku `iris_pyspark.py`.

6. Wykonaj kilka dodatkowych uruchomień i poeksperymentuj z różnymi argumentami. 

7. Otwórz plik `iris_pyspark.py`, aby zobaczyć prosty model regresji logistycznej utworzony za pomocą biblioteki Spark ML. 

8. Korzystaj z panelu **Zadania**, widoku listy historii uruchamiania i widoku szczegółów uruchomień w różnych środowiskach wykonawczych.

## <a name="execute-scripts-in-the-azure-ml-cli-window"></a>Wykonywanie skryptów w oknie interfejsu wiersza polecenia usługi Azure ML

1. Za pomocą aplikacji Azure Machine Learning Workbench uruchom okno wiersza polecenia, klikając menu **Plik**, a następnie opcję **Otwórz wiersz polecenia**. Wiersz polecenia uruchomi się w folderze projektu z monitem `C:\Temp\myIris\>`.

   >[!Important]
   >Musisz użyć okna wiersza polecenia (uruchomionego z aplikacji Workbench), aby wykonać następujące czynności:

2. Użyj wiersza polecenia, aby zalogować się na platformie Azure. 

   Podczas uwierzytelniania w zasobach platformy Azure aplikacja Workbench i interfejs wiersza polecenia używają osobnych pamięci podręcznych poświadczeń. Wystarczy wykonać tę czynność jeden raz i poświadczenia zostaną zapamiętane do czasu wygaśnięcia tokenu z pamięci podręcznej. Polecenie **az account list** zwraca listę subskrypcji dostępnych na potrzeby logowania. Jeśli liczba subskrypcji jest większa niż jeden, użyj wartości identyfikatora odpowiedniej subskrypcji i ustaw ją jako domyślne konto za pomocą polecenia **az set account -s**, podając wartość tego identyfikatora. Potwierdź ustawienia za pomocą polecenia account show.

   ```azurecli
   REM login using aka.ms/devicelogin site.
   az login
   
   REM list all Azure subscriptions you have access to. 
   az account list -o table
   
   REM set the current Azure subscription to the one you want to use.
   az set account -s <subscriptionId>
   
   REM verify your current subscription is set correctly
   az account show
   ```

3. Po uwierzytelnieniu i ustawieniu bieżącego kontekstu subskrypcji platformy Azure wpisz poniższe polecenia w oknie interfejsu wiersza polecenia w celu zainstalowania pakietu matplotlib i prześlij skrypt w języku Python jako eksperyment do uruchomienia.

   ```azurecli
   REM You don't need to do this if you have installed matplotlib locally from the previous steps.
   pip install matplotlib
   
   REM Kick off an execution of the iris_sklearn.py file against local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Przejrzyj dane wyjściowe. Zauważ, że dane wyjściowe są takie same jak wynik poprzedniego uruchomienia w ramach niniejszego samouczka, gdy skrypt był uruchamiany przy użyciu aplikacji Workbench. 

5. Uruchom ten sam skrypt, używając środowiska wykonawczego Docker (jeśli platforma Docker jest zainstalowana na komputerze).

   ```azurecli
   REM Execute iris_sklearn.py in local Docker container Python environment.
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM Execute iris_pyspark.py in local Docker container Spark environment.
   az ml experiment submit -c docker-spark .\iris_pyspark.py 0.1
   ```
6. W aplikacji Azure Machine Learning Workbench kliknij ikonę Folder na lewym pasku narzędzi, aby wyświetlić listę plików projektu, i otwórz skrypt w języku Python o nazwie **run.py**. 

   Ten skrypt jest użyteczny do sprawdzania w pętli różnych współczynników uregulowania i wielokrotnego uruchamiania eksperymentu z tymi współczynnikami. Ten skrypt uruchamia zadanie `iris_sklearn.py` ze współczynnikiem uregulowania `10.0` (ta liczba jest przesadnie duża), a następnie zmniejsza ten współczynnik o połowę w kolejnym uruchomieniu i tak dalej, aż współczynnik zostanie zmniejszony do wartości nie mniejszej niż `0.005`. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Aby uruchomić skrypt **run.py** z wiersza polecenia, uruchom następujące polecenia:

   ```cmd
   REM Submit iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Gdy działanie skryptu `run.py` zostanie zakończone, w widoku listy historii uruchamiania w aplikacji Azure Machine Learning Workbench pojawi się wykres.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Wykonywanie w kontenerze platformy Docker na komputerze zdalnym
W celu wykonania skryptu w kontenerze platformy Docker na komputerze zdalnym z systemem Linux wymagany jest dostęp za pośrednictwem powłoki SSH (nazwa użytkownika i hasło) do tego komputera zdalnego. Ponadto na komputerze zdalnym musi być zainstalowany i uruchomiony aparat platformy Docker. Najprostszym sposobem uzyskania takiego komputera z systemem Linux jest utworzenie [maszyny wirtualnej Data Science Virtual Machine (DSVM) opartej na dystrybucji Ubuntu](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) na platformie Azure. (Uwaga: maszyna wirtualna DSVM oparta na dystrybucji CentOS NIE jest obsługiwana). 

1. Gdy maszyna wirtualna zostanie utworzona, można dołączyć ją jako środowisko wykonawcze, generując parę plików `.runconfig` i `.compute` przy użyciu poniższego polecenia. Nadajmy nowemu środowisku nazwę `myvm`.
 
   ```azurecli
   REM create myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >Wartość w obszarze adresu IP może być również adresowaną publicznie w pełni kwalifikowaną nazwą domeny, taką jak `vm-name.southcentralus.cloudapp.azure.com`. Dodanie w pełni kwalifikowanej nazwy domeny do maszyny wirtualnej DSVM i użycie jej w tym miejscu zamiast adresu IP jest dobrym rozwiązaniem, ponieważ w pewnym momencie można wyłączyć maszynę wirtualną, aby zaoszczędzić na kosztach. Ponadto przy następnym uruchomieniu maszyny wirtualnej adres IP może już być zmieniony.

   Następnie uruchom poniższe polecenie, aby skonstruować obraz platformy Docker na maszynie wirtualnej i przygotować go na uruchamianie skryptów.
   
   ```azurecli
   REM prepare the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >W pliku `myvm.runconfig` możesz również zmienić wartość `PrepareEnvironment` z domyślnej `false` na `true`. To spowoduje automatyczne przygotowanie kontenera platformy Docker przy pierwszym uruchomieniu.

2. Edytuj wygenerowany plik `myvm.runconfig` w folderze `aml_config` i zmień domyślne ustawienie Framework z wartości `PySpark` na wartość `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Pozostawienie wartości PySpark dla ustawienia Framework również powinno zapewnić poprawne działanie. Ale takie rozwiązanie jest nieco mniej wydajne, jeśli do uruchomienia skryptu w języku Python nie jest potrzebna sesja platformy Spark.

3. Wydaj to samo polecenie jak poprzednio w oknie interfejsu wiersza polecenia, jednak tym razem środowiskiem docelowym będzie środowisko _myvm_:
   ```azurecli
   REM execute iris_sklearn.py in remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Polecenie zostanie wykonane tak, jakby używane było środowisko `docker-python`, ale wykonanie ma miejsce na zdalnej maszynie wirtualnej z systemem Linux. W oknie interfejsu wiersza polecenia pojawią się takie same informacje wyjściowe.

4. Wypróbujmy platformę Spark w kontenerze. Otwórz Eksploratora plików (można to zrobić również z okna interfejsu wiersza polecenia, pod warunkiem że potrafisz się posługiwać prostymi poleceniami do manipulowania plikami). Utwórz kopię pliku `myvm.runconfig` i nadaj jej nazwę `myvm-spark.runconfig`. Edytuj nowy plik, aby zmienić wartość ustawienia `Framework` z `Python` na `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Nie wprowadzaj żadnych zmian w pliku `myvm.compute`. W celu przeprowadzenia wykonania na platformie Spark zostanie wykorzystany ten sam obraz platformy Docker na tej samej maszynie wirtualnej. W nowym pliku `myvy-spark.runconfig` pole `target` wskazuje ten sam plik `myvm.compute` za pośrednictwem jego nazwy `myvm`.

5. Wpisz poniższe polecenie, aby uruchomić je w wystąpieniu platformy Spark w zdalnym kontenerze platformy Docker:
   ```azureli
   REM execute iris_pyspark.py in Spark instance on remote Docker container
   az ml experiment submit -c myvm-spark .\iris_pyspark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Wykonywanie skryptu w klastrze usługi HDInsight
Ten skrypt można również uruchomić w rzeczywistym klastrze platformy Spark. 

1. Jeśli masz dostęp do platformy Spark dla klastra usługi Azure HDInsight, wygeneruj polecenie konfiguracji uruchomieniowej HDI podobne do pokazanego. Jako parametry podaj nazwę klastra usługi HDInsight, swoją nazwę użytkownika usługi HDInsight i hasło. Użyj następującego polecenia:

   ```azurecli
   REM create a compute target that points to a HDI cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepare the HDI cluster
   az ml experiment prepare -c myhdi
   ```

   W pełni kwalifikowana nazwa domeny węzła głównego klastra to zwykle `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >`username` jest nazwą użytkownika powłoki SSH klastra. Wartość domyślna to `sshuser`, o ile nie została zmieniona podczas aprowizowania usługi HDI. Nazwą nie jest `admin`, ponieważ jest to drugi użytkownik utworzony podczas aprowizowania w celu zapewnienia dostępu do administracyjnej witryny internetowej klastra. 

2. Uruchom następujące polecenie, a skrypt zostanie uruchomiony w klastrze usługi HDInsight:

   ```azurecli
   REM execute iris_pyspark on the HDI cluster
   az ml experiment submit -c myhdi .\iris_pyspark.py
   ```

   >[!NOTE]
   >W przypadku wykonywania względem zdalnego klastra usługi HDI możesz również wyświetlić szczegóły wykonania zadania YARN pod adresem `https://<cluster_name>.azurehdinsight.net/yarnui`, korzystając z konta użytkownika `admin`.


## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z drugą częścią trzyczęściowej serii samouczka wiesz już, jak używać usługi Azure Machine Learning, aby:
> [!div class="checklist"]
> * Pracować w aplikacji Azure Machine Learning Workbench
> * Otwierać skrypty i przeglądać kod
> * Wykonywać skrypty w środowisku lokalnym
> * Przeglądać historię uruchamiania
> * Wykonywać skrypty w lokalnym środowisku platformy Docker
> * Wykonywać skrypty w oknie lokalnego interfejsu wiersza polecenia platformy Azure
> * Wykonywać skrypty w zdalnym środowisku platformy Docker
> * Wykonywać skrypty w środowisku usługi HDInsight w chmurze

Możesz teraz przejść do trzeciej części tej serii. Utworzyliśmy model regresji logistycznej, a teraz wdrożymy go jako usługę internetową czasu rzeczywistego.

> [!div class="nextstepaction"]
> [Wdrażanie modelu](tutorial-classifying-iris-part-3.md)

