---
title: Samouczek budowania modelu na potrzeby usługi Azure Machine Learning (wersja zapoznawcza) | Microsoft Docs
description: W całej serii tego samouczka kompleksowo przedstawiono sposób korzystania z usługi Azure Machine Learning (wersja zapoznawcza). To jest część druga, w której omówiono eksperymenty.
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs, gcampanella
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/7/2018
ms.openlocfilehash: 3e7f1b25757dc627f0f42a34c1a42b2d421c06c9
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-classify-iris-part-2---build-a-model"></a>Samouczek: klasyfikowanie irysów, część 2 — budowanie modelu
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Niniejszy samouczek jest **drugą częścią trzyczęściowej serii**. W tej części samouczka użyjesz usługi Azure Machine Learning (wersja zapoznawcza), aby:

> [!div class="checklist"]
> * używać środowiska roboczego usługi Azure Machine Learning,
> * otwierać skrypty i przeglądać kod,
> * wykonywać skrypty w środowisku lokalnym,
> * przeglądać historię uruchamiania,
> * wykonywać skrypty w lokalnym środowisku platformy Docker,
> * wykonywać skrypty w oknie lokalnego interfejsu wiersza polecenia platformy Azure,
> * wykonywać skrypty w zdalnym środowisku platformy Docker,
> * wykonywać skrypty w środowisku usługi Azure HDInsight w chmurze.

W tym samouczku wykorzystywany jest ponadczasowy [zbiór danych na temat irysów](https://en.wikipedia.org/wiki/Iris_flower_data_set). Zrzuty ekranu dotyczą systemu Windows, ale działanie na komputerach z systemem operacyjnym Mac OS jest niemal identyczne.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne
Ukończ pierwszą część z tej serii samouczka. Przed rozpoczęciem niniejszego samouczka najpierw prześledź [samouczek dotyczący przygotowywania danych](tutorial-classifying-iris-part-1.md), aby utworzyć zasoby usługi Azure Machine Learning i zainstalować aplikację Azure Machine Learning Workbench.

Opcjonalnie możesz eksperymentować, uruchamiając skrypty dla lokalnego kontenera platformy Docker. Aby to zrobić, potrzebny będzie aparat platformy Docker (wystarczy wersja Community Edition) zainstalowany i uruchomiony lokalnie na maszynie z systemem Windows lub Mac OS. Aby uzyskać więcej informacji o instalowaniu platformy Docker, zobacz [Instrukcje dotyczące instalacji platformy Docker](https://docs.docker.com/engine/installation/).

Aby poeksperymentować z wysyłką skryptu uruchamianego w kontenerze platformy Docker na zdalnej maszynie wirtualnej platformy Azure lub w klastrze usługi Azure HDInsight Spark, możesz wykonać [instrukcje, aby utworzyć klaster usługi HDInsight lub maszynę wirtualną analizy danych platformy Azure w oparciu o system Ubuntu](how-to-create-dsvm-hdi.md).

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Przeglądanie pliku iris_sklearn.py i plików konfiguracji
1. Otwórz aplikację Azure Machine Learning Workbench i otwórz projekt **myIris**, który został utworzony w poprzedniej części tej serii samouczka.

2. Po otwarciu projektu wybierz przycisk **Pliki** (ikona folderu) w okienku po lewej stronie, aby otworzyć listę plików w folderze projektu.

3. Wybierz plik **iris_sklearn.py**. Kod Python zostanie otwarty w nowej karcie edytora tekstów wewnątrz aplikacji Workbench.

   ![Otwieranie pliku](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Kod, który zostanie wyświetlony, nie musi być dokładnie taki sam jak kod powyżej, ponieważ ten przykładowy projekt jest często aktualizowany.

4. Przejrzyj kod skryptu Python, aby zapoznać się ze stylem kodowania. Skrypt wykonuje następujące zadania:

   - Ładuje pakiet **iris.dprep** przeznaczony do przygotowania danych celem utworzenia struktury danych [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Użyj pakietu `iris.dprep` przeznaczonego do przygotowania danych, który jest dostarczany razem z projektem przykładowym i powinien być taki sam jak plik `iris-1.dprep` utworzony w części 1 tego samouczka.

   - Dodaje losowe funkcje utrudniające rozwiązanie problemu. Losowość jest konieczna, ponieważ Iris jest małym zestawem danych, który można łatwo sklasyfikować z niemal 100-procentową dokładnością.

   - Używa biblioteki [scikit-learn](http://scikit-learn.org/stable/index.html) uczenia maszynowego w celu zbudowania modelu regresji logistycznej. 

   - Serializuje model przez użycie biblioteki [pickle](https://docs.python.org/3/library/pickle.html) w pliku w folderze `outputs`. Skrypt następnie ładuje plik i deserializuje go z powrotem do pamięci.

   - Za pomocą modelu po deserializacji przewiduje nowy rekord. 

   - Rysuje dwa wykresy, macierz pomyłek oraz wieloklasową krzywą ROC (Receiver Operating Characteristic) przy użyciu biblioteki [matplotlib](https://matplotlib.org/), a następnie zapisuje te elementy w folderze `outputs`.

   - Obiekt `run_logger` jest używany przez cały czas w celu rejestrowania współczynnika uregulowania i modelowania dokładności w dziennikach. Te wartości są automatycznie oznaczane na wykresach w historii uruchamiania.


## <a name="execute-irissklearnpy-script-in-a-local-environment"></a>Wykonywanie skryptu iris_sklearn.py w środowisku lokalnym

Przygotujmy się do uruchomienia skryptu **iris_sklearn.py** po raz pierwszy. Ten skrypt wymaga pakietów **scikit-learn** i **matplotlib**. Pakiet **scikit-learn** jest już zainstalowany przez aplikację Azure Machine Learning Workbench. Nadal jednak należy zainstalować pakiet **matplotlib**. 

1. W aplikacji Azure Machine Learning Workbench wybierz menu **Plik**, a następnie wybierz polecenie **Otwórz wiersz polecenia**, aby otworzyć wiersz polecenia. To okno interfejsu wiersza polecenia nazywamy *oknem interfejsu wiersza polecenia aplikacji Azure Machine Learning Workbench* (*oknem CLI*).

2. W oknie interfejsu wiersza polecenia wprowadź następujące polecenie, aby zainstalować pakiet języka Python o nazwie **matplotlib**. Powinno to zająć mniej niż minutę.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Jeśli pominięto poprzednie polecenie `pip install`, kod w `iris_sklearn.py` zostanie uruchomiony pomyślnie. Jeśli uruchomisz wyłącznie polecenie `iris_sklearn.py`, kod nie wygeneruje wykresu wyjściowej macierzy pomyłek ani wykresu wieloklasowej krzywej ROC, które przedstawiono w wizualizacjach historycznych.

3. Wróć do okna aplikacji Workbench. 

4. Na pasku narzędzi w górnej części karty **iris_sklearn.py** wybierz pozycję **lokalne** jako środowisko wykonywania i `iris_sklearn.py` jako skrypt do uruchomienia.

5. Następnie przejdź na prawą stronę paska narzędzi i wprowadź `0.01` w polu **Argumenty**. Ta wartość odpowiada współczynnikowi uregulowania modelu regresji logistycznej.

   ![Kontrolka Uruchom](media/tutorial-classifying-iris/run_control.png)

6. Wybierz przycisk **Uruchom**. Nastąpi natychmiastowe zaplanowanie zadania. To zadanie jest widoczne w panelu **Zadania** po prawej stronie okna aplikacji Workbench. 

7. Po kilku chwilach stan zadania zostanie zmieniony z **Przesyłanie** na **Uruchomiono**, a na koniec na wartość **Zakończono**.

   ![Uruchamianie skryptu sklearn](media/tutorial-classifying-iris/run_sklearn.png)

8. Wybierz wyraz **Ukończono** w tekście stanu zadania w panelu **Zadania**. Zostanie otwarte okno podręczne, w którym widoczny będzie tekst wyjścia standardowego (stdout) dla uruchomienia. Aby zamknąć tekst stdout, wybierz przycisk **Zamknij** (**x**) w prawym górnym rogu okna podręcznego.

9. W tym samym stanie zadania w okienku **Zadania** wybierz niebieski tekst **iris_sklearn.py [n]** (_n_ to numer uruchomienia) tuż nad stanem **Zakończono** i czasem uruchomienia. Zostanie otwarte okno **Właściwości uruchamiania**, które zawiera następujące informacje dotyczące konkretnego uruchomienia:
   - Informacje w oknie **Właściwości uruchomienia**
   - **Dane wyjściowe**
   - **Metryki**
   - **Wizualizacje**, jeśli są dostępne
   - **Dzienniki** 

   Gdy uruchomienie zostanie zakończone, pojawi się okno podręczne z następującymi wynikami:

   >[!NOTE]
   >Do zestawu szkoleniowego wprowadziliśmy generowanie losowe, dlatego wyniki mogą się nieznacznie różnić od widocznych tutaj.

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
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```

10. Zamknij kartę **Właściwości uruchomienia**, a następnie wróć do karty **iris_sklearn.py**. 

11. Powtórz te czynności dla dodatkowych uruchomień. 

    W polu **Argumenty** wprowadź serię wartości z zakresu od `0.001` do `10`. Wybierz przycisk **Uruchom**, aby wykonać ten kod jeszcze kilka razy. Zmieniana za każdym razem wartość argumentu jest wprowadzana do modelu regresji logistycznej w kodzie, przez co wyszukane dane są za każdym razem różne.

## <a name="review-the-run-history-in-detail"></a>Przeglądanie szczegółów historii uruchamiania
W aplikacji Azure Machine Learning Workbench każde wykonanie skryptu jest przechwytywane jako rekord w historii uruchamiania. Jeśli otworzysz widok **Uruchomienia**, możesz wyświetlić historię uruchamiania konkretnego skryptu.

1. Aby otworzyć listę **Uruchomienia**, wybierz przycisk **Uruchomienia** (ikona zegara) na pasku narzędzi po lewej stronie. Następnie wybierz skrypt **iris_sklearn.py**, aby wyświetlić **Pulpit nawigacyjny uruchomień** skryptu `iris_sklearn.py`.

   ![Widok uruchomienia](media/tutorial-classifying-iris/run_view.png)

2. Zostanie otwarta karta **Pulpit nawigacyjny uruchomień**. Przejrzyj statystyki przechwycone z wielu uruchomień. Wykresy są renderowane w górnej części karty. Każde uruchomienie ma kolejny numer, a szczegóły dotyczące uruchomienia są wyświetlane na liście w tabeli u dołu ekranu.

   ![Pulpit nawigacyjny uruchomień](media/tutorial-classifying-iris/run_dashboard.png)

3. Filtruj tabelę i wybieraj dowolne wykresy, aby wyświetlić stan, czas trwania, dokładność i współczynnik uregulowania każdego uruchomienia. 

4. W tabeli **Uruchomienia** wybierz dwa lub trzy uruchomienia, a następnie wybierz przycisk **Porównaj**, aby otworzyć okienko szczegółowego porównania. Porównaj pozycje zestawione obok siebie. Wybierz przycisk Wstecz **listy Uruchomienia** w lewym górnym rogu okienka **Porównanie**, aby wrócić do **Pulpitu nawigacyjnego uruchomień**.

5. Wybierz pojedyncze uruchomienie, aby wyświetlić widok szczegółów uruchomienia. Zwróć uwagę na to, że statystyki dotyczące wybranego uruchomienia będą widoczne w sekcji **Właściwości uruchomienia**. Pliki zapisane w folderze danych wyjściowych będą widoczne na liście w sekcji **Dane wyjściowe** — można je stamtąd pobrać.

   ![Szczegóły uruchomienia](media/tutorial-classifying-iris/run_details.png)

   Dwa wykresy — macierz pomyłek i wieloklasowa krzywa ROC — zostaną zrenderowane w sekcji **Wizualizacje**. Wszystkie pliki dziennika można znaleźć w sekcji **Dzienniki**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Wykonywanie skryptów w lokalnym środowisku platformy Docker

Możesz łatwo konfigurować dodatkowe środowiska wykonawcze, takich jak platforma Docker, i uruchamiać skrypty w tych środowiskach. 

>[!IMPORTANT]
>Aby można było wykonać ten krok, aparat platformy Docker musi zostać lokalnie zainstalowany i uruchomiony. Aby uzyskać więcej informacji, zapoznaj się z [instrukcjami dotyczącymi instalacji platformy Docker](https://docs.docker.com/install/).

1. W okienku po lewej stronie wybierz ikonę **folderu**, aby otworzyć listę **Pliki** dla projektu. Rozwiń folder `aml_config`. 

2. Istnieje kilka wstępnie skonfigurowanych środowisk: **docker-python**, **docker-spark** oraz **lokalne**. 

   Każde środowisko ma dwa pliki, takie jak `docker.compute` (środowiska **docker python** i **docker spark**) i `docker-python.runconfig`. Otwórz każdy plik, aby zobaczyć, że niektóre opcje można skonfigurować w edytorze tekstów.  

   Aby wyczyścić, wybierz przycisk **Zamknij** (**x**) na kartach wszystkich otwartych edytorów tekstu.

3. Uruchom skrypt **iris_sklearn.py**, używając środowiska **docker-python**: 

   - Na pasku narzędzi po lewej stronie wybierz ikonę **zegara**, aby otworzyć okienko **Uruchomienia**. Wybierz pozycję **Wszystkie uruchomienia**. 
   - U góry karty **Wszystkie uruchomienia** wybierz **docker-python** jako środowisko docelowe zamiast domyślnego środowiska **lokalnego**. 
   - Następnie po prawej stronie wybierz **iris_sklearn.py** jako skrypt do uruchomienia. 
   - Pozostaw pole **Argumenty** puste, ponieważ skrypt określa wartość domyślną. 
   - Wybierz przycisk **Uruchom**.

4. Sprawdź, czy nowe zadanie uruchamia się. To zadanie jest widoczne w okienku **Zadania** po prawej stronie okna aplikacji Workbench.

   Jeśli uruchamiasz zadanie na platformie Docker po raz pierwszy, jego ukończenie potrwa kilka minut dłużej. 

   W tle aplikacja Azure Machine Learning Workbench tworzy nowy plik platformy Docker. 
   Nowy plik odwołuje się do obrazu podstawowego platformy Docker określonego w pliku `docker.compute` oraz pakietów zależności języka Python określonych w pliku `conda_dependencies.yml`. 
   
   Aparat platformy Docker wykonuje następujące zadania:

    - Pobiera obraz podstawowy z platformy Azure.
    - Instaluje pakiety języka Python określone w pliku `conda_dependencies.yml`.
    - Uruchamia kontener platformy Docker.
    - Kopiuje (albo — w zależności od konfiguracji uruchamiania — tworzy do niej odniesienie) lokalną kopię folderu projektu.      
    - Wykonuje skrypt `iris_sklearn.py`.

   Wyniki końcowe powinny być dokładnie takie same jak wtedy, gdy miejscem docelowym jest środowisko **lokalne**.

5. Teraz wypróbujmy platformę Spark. Obraz podstawowy platformy Docker zawiera wstępnie zainstalowane i skonfigurowane wystąpienie platformy Spark, którego można używać do wykonywania skryptu PySpark. Jest to prosty sposób tworzenia i testowania programu na platformę Spark bez konieczności samodzielnego instalowania i konfigurowania platformy Spark. 

   Otwórz plik `iris_spark.py`. Ten skrypt ładuje plik danych `iris.csv` i używa algorytmu regresji logistycznej z biblioteki Spark Machine Learning do klasyfikowania zestawu danych Iris. Teraz zmień środowisko uruchomieniowe na **docker-spark**, a skrypt na **iris_spark.py**, a następnie uruchom go ponownie. Proces trwa nieco dłużej, ponieważ sesja platformy Spark musi zostać utworzona i uruchomiona w kontenerze platformy Docker. Możesz także zobaczyć, że wyjście standardowe różni się od wyjścia standardowego dla pliku `iris_spark.py`.

6. Rozpocznij kilka dodatkowych uruchomień i poeksperymentuj z różnymi argumentami. 

7. Otwórz plik `iris_spark.py`, aby zobaczyć model regresji logistycznej utworzony za pomocą biblioteki Spark Machine Learning. 

8. Korzystaj z okienka **Zadania**, widoku listy historii uruchamiania i widoku szczegółów uruchomień w różnych środowiskach wykonawczych.

## <a name="execute-scripts-in-the-azure-machine-learning-cli-window"></a>Wykonywanie skryptów w oknie interfejsu wiersza polecenia usługi Azure Machine Learning

1. W aplikacji Azure Machine Learning Workbench otwórz okno wiersza polecenia, wybierz menu **Plik**, a następnie wybierz polecenie **Otwórz wiersz polecenia**. Wiersz polecenia uruchomi się w folderze projektu z monitem `C:\Temp\myIris\>`.

   >[!IMPORTANT]
   >Musisz użyć okna wiersza polecenia (otwartego z aplikacji Workbench), aby wykonać poniższe kroki.

2. Użyj wiersza polecenia, aby zalogować się na platformie Azure. 

   Podczas uwierzytelniania w zasobach platformy Azure aplikacja Workbench i interfejs wiersza polecenia używają osobnych pamięci podręcznych poświadczeń. Wystarczy wykonać tę czynność jeden raz i poświadczenia zostaną zapamiętane do czasu wygaśnięcia tokenu z pamięci podręcznej. Polecenie **az account list** zwraca listę subskrypcji dostępnych na potrzeby logowania. Jeśli istnieje więcej niż jedna subskrypcja, użyj wartości identyfikatora z odpowiedniej subskrypcji. Ustaw subskrypcję jako domyślnie używane konto przy użyciu polecenia **az account set -s**, a następnie podaj wartość identyfikatora subskrypcji. Potwierdź ustawienia za pomocą polecenia **account show**.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <SubscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Po uwierzytelnieniu i ustawieniu bieżącego kontekstu subskrypcji platformy Azure wprowadź poniższe polecenia w oknie interfejsu wiersza polecenia w celu zainstalowania pakietu **matplotlib** i prześlij skrypt w języku Python jako eksperyment do uruchomienia.

   ```azurecli
   REM you don't need to run this command if you have installed matplotlib locally from the previous steps
   pip install matplotlib
   
   REM kicks off an execution of the iris_sklearn.py file against the local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Przejrzyj dane wyjściowe. Dane wyjściowe i wyniki są takie same jak w przypadku wcześniejszego użycia aplikacji Workbench do uruchomienia skryptu. 

5. Uruchom ten sam skrypt ponownie, używając środowiska wykonawczego Docker (jeśli platforma Docker została zainstalowana na komputerze).

   ```azurecli
   REM executes iris_sklearn.py in the local Docker container Python environment
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM executes iris_spark.py in the local Docker container Spark environment
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```

6. W aplikacji Workbench wybierz ikonę **Folder** w lewym okienku, aby wyświetlić listę plików projektu, i otwórz skrypt w języku Python o nazwie **run.py**. 

   Ten skrypt jest przydatny do zapętlania różnych współczynników uregulowania. Uruchom eksperyment wiele razy z tymi współczynnikami. Ten skrypt uruchamia zadanie `iris_sklearn.py` ze współczynnikiem uregulowania `10.0` (bardzo duża liczba). Następnie skrypt zmniejsza współczynnik o połowę w kolejnym uruchomieniu. Robi tak, aż współczynnik nie będzie mniejszy niż `0.005`. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Aby wykonać skrypt **run.py** z wiersza polecenia, uruchom następujące polecenia:

   ```cmd
   REM submits iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Gdy działanie skryptu `run.py` zostanie zakończone, w widoku listy historii uruchamiania w aplikacji Workbench będzie można zobaczyć wykres różnych metryk.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Wykonywanie w kontenerze platformy Docker na komputerze zdalnym
W celu wykonania skryptu w kontenerze platformy Docker na komputerze zdalnym z systemem Linux wymagany jest dostęp za pośrednictwem powłoki SSH (nazwa użytkownika i hasło) do tego komputera zdalnego. Ponadto na maszynie należy zainstalować i uruchomić aparat platformy Docker. Najprostszym sposobem uzyskania takiego komputera z systemem Linux jest utworzenie maszyny wirtualnej Data Science Virtual Machine (DSVM) opartej na dystrybucji Ubuntu na platformie Azure. Dowiedz się, [jak utworzyć maszynę wirtualną DSVM opartą na dystrybucji Ubuntu do użycia w aplikacji Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>Maszyna wirtualna DSVM oparta na dystrybucji CentOS *nie* jest obsługiwana.

1. Gdy maszyna wirtualna zostanie utworzona, można dołączyć ją jako środowisko wykonawcze, jeśli wygenerowano parę plików `.runconfig` i `.compute`. Użyj następującego polecenia, aby wygenerować pliki. Nadajmy nowemu środowisku nazwę `myvm`.
 
   ```azurecli
   REM creates an myvm compute target
   az ml computetarget attach remotedocker --name myvm --address <IP address> --username <username> --password <password>
   ```
   
   >[!NOTE]
   >Adres IP może być również adresowaną publicznie w pełni kwalifikowaną nazwą domeny (FQDN), taką jak `vm-name.southcentralus.cloudapp.azure.com`. Dobrym rozwiązaniem jest dodanie nazwy FQDN do maszyny wirtualnej DSVM i użycie jej zamiast adresu IP. Takie rozwiązanie jest dobrym pomysłem, ponieważ w pewnym momencie maszynę wirtualną można będzie wyłączyć w związku z oszczędnościami. Ponadto przy następnym uruchomieniu maszyny wirtualnej adres IP może już być zmieniony.

   >[!NOTE]
   >Oprócz uwierzytelniania nazwy użytkownika i hasła możesz określić klucz prywatny i odpowiednie hasło (jeśli istnieją) przy użyciu opcji `--private-key-file` i (opcjonalnie) `--private-key-passphrase`.

   Następnie uruchom poniższe polecenie, aby skonstruować obraz platformy Docker na maszynie wirtualnej i przygotować go na uruchamianie skryptów:
   
   ```azurecli
   REM prepares the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >W pliku `myvm.runconfig` możesz również zmienić wartość `PrepareEnvironment` z domyślnej wartości `false` na `true`. Ta zmiana spowoduje automatyczne przygotowanie kontenera platformy Docker jako części pierwszego uruchomienia.

2. Edytuj wygenerowany plik `myvm.runconfig` w folderze `aml_config` i zmień ustawienie Framework z wartości domyślnej `PySpark` na wartość `Python`:

   ```yaml
   Framework: Python
   ```
   >[!NOTE]
   >Rozwiązanie PySpark powinno działać, ale użycie języka Python jest bardziej wydajne, jeśli do uruchomienia skryptu w języku Python faktycznie nie potrzeba sesji platformy Spark.

3. Wydaj to samo polecenie jak poprzednio w oknie interfejsu wiersza polecenia, używając tym razem środowiska docelowego _myvm_:
   ```azurecli
   REM executes iris_sklearn.py in a remote Docker container
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   Polecenie zostanie wykonane tak, jakby używane było środowisko `docker-python`, ale wykonanie ma miejsce na zdalnej maszynie wirtualnej z systemem Linux. W oknie interfejsu wiersza polecenia pojawią się takie same informacje wyjściowe.

4. Wypróbujmy użycie platformy Spark w kontenerze. Otwórz Eksploratora plików. Można to zrobić również z okna interfejsu wiersza polecenia, pod warunkiem że potrafisz się posługiwać prostymi poleceniami do manipulowania plikami. Utwórz kopię pliku `myvm.runconfig` i nadaj jej nazwę `myvm-spark.runconfig`. Edytuj nowy plik, aby zmienić wartość ustawienia `Framework` z `Python` na `PySpark`:
   ```yaml
   Framework: PySpark
   ```
   Nie wprowadzaj żadnych zmian w pliku `myvm.compute`. W celu przeprowadzenia wykonania na platformie Spark zostanie wykorzystany ten sam obraz platformy Docker na tej samej maszynie wirtualnej. W nowym pliku `myvm-spark.runconfig` pole `Target` wskazuje ten sam plik `myvm.compute` za pośrednictwem jego nazwy `myvm`.

5. Wpisz następujące polecenie, aby uruchomić skrypt **iris_spark.py** w wystąpieniu platformy Spark uruchomionej w zdalnym kontenerze Docker:
   ```azureli
   REM executes iris_spark.py in a Spark instance on a remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Wykonywanie skryptu w klastrze usługi HDInsight
Ten skrypt można również uruchomić w klastrze usługi HDInsight Spark. Dowiedz się, [jak utworzyć klaster usługi HDInsight Spark do użycia w aplikacji Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[!NOTE] 
>Klaster usługi HDInsight musi używać usługi Azure Blob Storage jako magazynu głównego. Korzystanie z magazynu usługi Azure Data Lake nie jest jeszcze obsługiwane.

1. Jeśli masz dostęp do platformy Spark dla klastra usługi Azure HDInsight, wygeneruj polecenie konfiguracji uruchomieniowej HDInsight podobne do pokazanego tutaj. Jako parametry podaj nazwę klastra usługi HDInsight, swoją nazwę użytkownika usługi HDInsight i hasło. Użyj następującego polecenia:

   ```azurecli
   REM creates a compute target that points to a HDInsight cluster
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <username> --password <password>

   REM prepares the HDInsight cluster
   az ml experiment prepare -c myhdi
   ```

   Nazwa FQDN węzła głównego klastra to zazwyczaj `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >`username` jest nazwą użytkownika powłoki SSH klastra. Wartość domyślna to `sshuser`, o ile nie została zmieniona podczas konfigurowania usługi HDInsight. Wartością nie jest `admin`, ponieważ jest to drugi użytkownik utworzony podczas konfiguracji w celu zapewnienia dostępu do administracyjnej witryny internetowej klastra. 

2. Uruchom następujące polecenie, aby uruchomić skrypt **iris_spark.py** w klastrze usługi HDInsight:

   ```azurecli
   REM executes iris_spark on the HDInsight cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >W przypadku wykonywania względem zdalnego klastra usługi HDInsight możesz również wyświetlić szczegóły wykonania zadania YARN (Yet Another Resource Negotiator) pod adresem `https://<cluster_name>.azurehdinsight.net/yarnui`, korzystając z konta użytkownika `admin`.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z drugą częścią trzyczęściowej serii samouczka wiesz już, jak używać usługi Azure Machine Learning, aby:
> [!div class="checklist"]
> * używać środowiska roboczego usługi Azure Machine Learning,
> * otwierać skrypty i przeglądać kod,
> * wykonywać skrypty w środowisku lokalnym,
> * przeglądać historię uruchamiania,
> * wykonywać skrypty w lokalnym środowisku platformy Docker,
> * wykonywać skrypty w oknie lokalnego interfejsu wiersza polecenia platformy Azure,
> * wykonywać skrypty w zdalnym środowisku platformy Docker,
> * wykonywać skrypty w środowisku usługi HDInsight w chmurze.

Możesz teraz przejść do trzeciej części tej serii. Utworzyliśmy model regresji logistycznej, a teraz wdrożymy go jako usługę internetową czasu rzeczywistego.

> [!div class="nextstepaction"]
> [Samouczek 3 — klasyfikowanie irysów: wdrażanie modeli](tutorial-classifying-iris-part-3.md)
