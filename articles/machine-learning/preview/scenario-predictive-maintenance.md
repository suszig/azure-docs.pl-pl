---
title: Scenariuszu konserwacji predykcyjnej rzeczywistych | Dokumentacja firmy Microsoft
description: "Predykcyjnej obsługi rzeczywistych World scenariusz przy użyciu PySpark"
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 21cf8201236224244e6ed34f91f9c5c601ab9a79
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="predictive-maintenance-real-world-scenario"></a>Konserwacji predykcyjnej rzeczywistych scenariuszy.

Wpływ urządzeń nieplanowanych przestojów mogą być szkodliwe dla dowolnego rodzaju działalności. Jest szczególnie ważne, aby zachować uruchomiona w celu zmaksymalizowania wykorzystania i wydajności sprzętu pola, a minimalizując kosztowne, niezaplanowanego przestoju. Wczesne identyfikację problemów mogą pomóc przydzielić zasoby obsługi ograniczony w sposób ekonomiczny i zwiększyć jakość i procesy łańcucha dostaw. 

W tym scenariuszu Eksploruje stosunkowo [na dużą skalę symulowane zestawu danych](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) przechodzenia przez projekt nauki konserwacji predykcyjnej danych z wprowadzanie danych, funkcji zespołu inżynieryjnego, konstruowania modelu i operationalization modelu i wdrożenia. Kod dla całego procesu są zapisywane w notesach Jupyter przy użyciu PySpark środowiska roboczego uczenia Maszynowego Azure. Ostatecznego modelu wdrażania przy użyciu Azure Machine Learning Model zarządzania celu prognozowania awarii sprzętu w czasie rzeczywistym.   

## <a name="link-to-the-gallery-github-repository"></a>Połącz z repozytorium GitHub galerii

Poniżej znajduje się łącze do publicznego repozytorium GitHub raporty problem i udziały: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Omówienie przypadków użycia

Poważny problem, napotykają firmy w branży ciężki zasobów jest znacznych kosztów, skojarzonych z opóźnienia mechaniczne problemów. Większość firm są zainteresowani przewidywania, gdy te problemy, aby aktywnie zapobiec, przed ich wystąpieniem. Celem jest zmniejszenie kosztów, zmniejszając czas przestoju i możliwego zwiększania bezpieczeństwa. 

W tym scenariuszu przyjmuje pomysłów [podręcznikowym konserwacji predykcyjnej](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) aby zademonstrować konstruowania modelu predykcyjnego symulowane zestawu danych. Przykładowe dane pochodzi z typowymi elementami w wielu przypadkach użycia konserwacji predykcyjnej.

Problem biznesowy symulowane danych jest do prognozowania problemy spowodowane przez błędy składnika. W związku z tym jest pytania biznesowe "*jakie jest prawdopodobieństwo, że maszyna przestanie działać z powodu błędu składnika*?" Ten problem jest sformatowany jako problem wieloklasowej klasyfikacji (wiele składników dla poszczególnych komputerów) i algorytmu uczenia maszynowego służy do tworzenia modeli predykcyjnych. Model jest uczony w danych historycznych zbierane z maszyn. W tym scenariuszu użytkownik przechodzi przez różne kroki wdrażania modelu w środowisku Azure Machine Learning Workbench.

## <a name="prerequisites"></a>Wymagania wstępne

* [Konta Azure](https://azure.microsoft.com/en-us/free/) (bezpłatnych wersji próbnych są dostępne).
* Zainstalowana kopia programu [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) następujące [Przewodnik Szybki Start instalacji](./quickstart-installation.md) Aby zainstalować ten program i utworzyć obszaru roboczego.
* Azure Machine Learning Operationalization wymaga środowiska lokalnego wdrożenia i [modelu konto zarządzania](model-management-overview.md)

W tym przykładzie można uruchomić na dowolnym kontekście obliczeń AML Workbench. Jednak zaleca się uruchomić go z co najmniej 16 GB pamięci. W tym scenariuszu została zbudowana i przetestowany na maszynie z systemem Windows 10 z systemem zdalnym standard DS4_V2 [danych maszyny wirtualnej nauki dla systemu Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Wykonano operationalization modelu przy użyciu 0.1.0a22 wersji interfejsu CLI uczenia Maszynowego Azure.

## <a name="create-a-new-workbench-project"></a>Utwórz nowy projekt Workbench

Utwórz nowy projekt za pomocą tego przykładu jako szablon:
1.  Otwórz Azure Machine Learning Workbench
2.  Na **projekty** kliknij przycisk  **+**  podpisywania i wybierz **nowy projekt**
3.  W **Utwórz nowy projekt** okienka, wypełnij informacje dla nowego projektu
4.  W **szablony projektów wyszukiwania** polu wyszukiwania wpisz "Konserwacji predykcyjnej", wybierz **konserwacji predykcyjnej** szablonu
5.  Kliknij przycisk **Utwórz** przycisku

## <a name="prepare-the-notebook-server-computation-target"></a>Przygotowanie docelowy obliczeniowym serwera notesu

Uruchamianie na komputerze lokalnym, z AML Workbench `File` menu, wybierz opcję `Open Command Prompt` lub `Open PowerShell CLI`. Interfejs interfejsu wiersza polecenia umożliwia dostęp do usług platformy Azure przy użyciu `az` poleceń. Najpierw logowania do konta platformy Azure przy użyciu polecenia:

```
az login
``` 

To polecenie dostarcza klucz uwierzytelniania do użycia z `https:\\aka.ms\devicelogin` adresu URL. Interfejsu wiersza polecenia oczekuje, aż operację logowania za pomocą urządzenia zwraca i udostępnia pewne informacje o połączeniu. Dalej, jeśli masz lokalnym [docker](https://www.docker.com/get-docker) instalacji, należy przygotować środowisko lokalnym za pomocą następujących poleceń:

```
az ml experiment prepare --target docker --run-configuration docker
```

Zaleca się uruchamiania na [danych maszyny wirtualnej nauki dla systemu Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) wymagania dotyczące pamięci i dysku. Po skonfigurowaniu DSVM przygotować środowisko docker zdalnego za pomocą następujących dwóch poleceń:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Po nawiązaniu połączenia w kontenerze docker zdalne przygotowanie DSVM przy użyciu środowiska obliczeniowego docker: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Z rozwiązaniem docker z obliczeniowe przygotować środowisko, otwórz serwera notesu Jupyter w karcie notesów AML Workbench lub uruchomić z serwera opartego na przeglądarce: 
```
az ml notebook start
```

Przykład notesów są przechowywane w `Code` katalogu. Notebooki są skonfigurowane do uruchamiają się po kolei, uruchomienie na pierwszy (`Code\1_data_ingestion.ipynb`) notesu. Po otwarciu każdego notesu monit wybierz jądro obliczeniowe. Wybierz `[Project_Name]_Template [Connection_Name]` jądra do wykonania na DSVM wcześniej skonfigurowany.

## <a name="data-description"></a>Opis elementu danych

[Symulowane danych](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) składa się z pięciu plików z wartościami rozdzielanymi przecinkami (CSV). Skorzystaj z linków, aby uzyskać więcej bardziej szczegółowy opis zestawów danych.

* [Maszyny](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): funkcje rozróżnianie poszczególnych maszyn. Na przykład wieku i modelu.
* [Błąd](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): dziennik błędów zawiera błędy nierozdzielające zgłoszony, gdy komputer jest nadal działa. Błędy te nie są traktowane jako błędy, chociaż może być predykcyjnej zdarzenia błędu w przyszłości. Błąd daty i godziny są zaokrąglane do najbliższej godziny, ponieważ dane telemetryczne są zbierane z szybkością co godzinę.
* [Obsługa](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): konserwacji dziennika zawiera rekordy zarówno zaplanowanych, jak i nieplanowanych konserwacji. Zaplanowanej konserwacji odpowiada regularnych kontroli składników, niezaplanowaną konserwację mogą wynikać z awarii mechaniczne lub innych spadek wydajności. Data / Godzina konserwacji są zaokrąglane do najbliższej godziny, ponieważ zbieranych danych telemetrycznych z szybkością co godzinę.
* [Dane telemetryczne](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): telemetrii danych składa się z szeregu czasowego środki z wielu czujników w każdej maszynie. Dane są rejestrowane przez uśrednianie czujnik każdego interwału jedną godzinę.
* [Błędy](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): błędy odpowiadają zamiany składnika w dzienniku konserwacji. Każdy rekord zawiera identyfikator komputera, typu składnika i zastąpienie datę i godzinę. Te rekordy są używane do tworzenia etykiety, które podejmuje próbę prognozowania modelu uczenia maszynowego.

Zobacz [wprowadzanie danych](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) scenariusza notesu Jupyter w sekcji kodu, aby pobrać zestawów danych pierwotnych z repozytorium GitHub i utworzyć zestawy danych PySpark tej analizy.

## <a name="scenario-structure"></a>Scenariusz — struktura
Zawartość dla tego scenariusza znajduje się w temacie [repozytorium GitHub](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

[Readme](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) pliku przedstawiono przepływ pracy z Przygotowanie danych, budowanie modelu, a następnie wdrażania rozwiązania do produkcji. Każdy krok przepływu pracy są umieszczane w notesu Jupyter w [kod](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) folder w repozytorium.   

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Ten notes pobiera pięć plików wejściowych CSV, niektóre Oczyszczanie wstępne danych i wizualizacji. Notesu konwertuje zestawów danych do formatu PySpark i zapisuje go do kontenera obiektów blob platformy Azure do użycia w notesie engineering funkcji.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Za pomocą zestawu danych pierwotnych z obiektów blob platformy Azure, modelu, które funkcje są konstruowane przy użyciu podejścia serii (czas standardowy), telemetrii, błędy i obsługi danych. Zamienianie składnika niepowodzenie związane są używane do skonstruowania etykiety modelu opisujące, których nie powiodła się. Dane funkcji etykietą jest zapisywany w obiektów blob platformy Azure dla modelu tworzenie notesu.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Funkcja etykietą zestawu danych, za pomocą notesu modelowania dzieli dane na pociągu i deweloperów zestawów danych wzdłuż sygnaturę daty i godziny. Notesu jest Instalator zestawu eksperymentów z `pyspark.ml.classification` modeli. Dane szkoleniowe jest Przekształcono, a użytkownik może wypróbować albo `DecisionTreeClassifier` lub `RandomForestClassifier`, manipulowanie hyperparameters można znaleźć najbardziej modelu. Wydajność zależy od obliczenia miary statystyk dla zestawu danych deweloperów. Te statystyki są rejestrowane w Workbench AML Uruchom ekranu czasu w celu śledzenia. Przy każdym uruchomieniu notesu zapisuje modelu wynikowego na dysku lokalnym systemem jądra notesu Jupyter. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Za pomocą modelu ostatni zapisany na dysku lokalnym (jądro notesu Jupyter), ten notes kompilacje składniki wdrażania modelu do usługi sieci web platformy Azure. Pełnej eksploatacji zasoby są kompaktowanie do `o16n.zip` plików przechowywanych w innym kontenerze obiektów blob platformy Azure. Plik zip zawiera:

* `service_schema.json` Plik definicji schematu dla wdrożenia. 
* `pdmscore.py` Init() i run() funkcje wymagane przez usługę sieci web platformy Azure
* `pdmrfull.model` Katalog definicji modelu.
    
 Notesu testy funkcji z definicji modelu przed spakowaniem zasoby operationalization dla wdrożenia. Instrukcje dotyczące wdrażania znajdują się na końcu notesu.

## <a name="conclusion"></a>Podsumowanie

W tym scenariuszu zestawienie czytnik jak zbudować rozwiązanie do konserwacji predykcyjnej kompleksowe przy użyciu PySpark w środowisku notesu Jupyter w Workbench uczenia Maszynowego Azure. Ten przykładowy scenariusz zawiera również szczegóły modelu wdrożenia za pośrednictwem środowiska Azure Machine Learning Model zarządzania w celu prognozowania czasu rzeczywistego urządzenia awarii.

## <a name="references"></a>Dokumentacja

Dostępne są inne przykłady przypadków użycia konserwacji predykcyjnej na różnych platformach:

* [Szablon rozwiązania konserwacji predykcyjnej](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predykcyjnej konserwacji modelowania przewodnik przy użyciu usług R SQL](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Konserwacji predykcyjnej notesu Python Podręcznik modelowania](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Konserwacji predykcyjnej przy użyciu PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Bezpośrednie uczenia dla konserwacji predykcyjnej](
 https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Kolejne kroki

Brak dostępnych wiele innych przykładowe scenariusze środowiska roboczego uczenia maszynowego Azure, która zademonstrować dodatkowe funkcje produktu. 