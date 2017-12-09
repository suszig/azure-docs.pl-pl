---
title: "Bezpośrednie uczenia dla konserwacji predykcyjnej rzeczywistych scenariuszy - Azure | Dokumentacja firmy Microsoft"
description: "W tym dokumencie opisano sposób replikowania głębokie Learning konserwacji predykcyjnej samouczek z usługi Azure Machine Learning Workbench"
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 8997e38a81ed848c9e052ab08566ffc99560ed86
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Bezpośrednie uczenia dla konserwacji predykcyjnej rzeczywistych scenariuszy

Głębokie learning jest jednym z najpopularniejszych trendów w obecnie uczenia miejsca, a istnieje wiele pól i aplikacje gdzie go oznacza, takich jak driverless samochodów, rozpoznawanie mowy i obrazu, robotics i not. Głębokie learning jest zestawem algorytmów jest podstawą kształt naszych inteligencji (biologicznych sieci neuronowej) i uczenie maszynowe i kognitywnych służące zazwyczaj odwołuje się do niego jako sztuczne Neuronowej sieci (PODS).

Konserwacji predykcyjnej jest również bardzo popularny obszaru, w której wiele różnych technik zostały zaprojektowane w celu określenia stanu sprzętu w celu prognozowania podczas konserwacji powinny być wykonywane. Konserwacji predykcyjnej obejmuje różne tematy, w tym między innymi: prognozowania awarii, diagnozy awarii (analiza głównych przyczyn), wykrywanie błędów, klasyfikację typu awarii i zalecenia ograniczenie lub konserwacji akcji po Wystąpił błąd.

W scenariuszach konserwacji predykcyjnej dane są zbierane wraz z upływem czasu, aby monitorować stan urządzeń w celu końcowego wyszukiwanie wzorców do prognozowania błędów. Wśród bezpośrednich uczenia metod [długi krótki okres pamięci (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sieci są szczególnie atrakcyjne w domenie konserwacji predykcyjnej na fakt, że są one bardzo dobrze sobie radzi pod sekwencji. Ten fakt jest przydatna do aplikacji przy użyciu danych serii czasu umożliwiając wstecz przez dłuższy czas do wykrywania awarii wzorce.

W tym samouczku budujemy sieci LSTM dla zestawu danych oraz scenariusz opisany w [konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) do prognozowania pozostałych użytkowania aparatów powietrznego. Podsumowując szablon używa wartości czujnik symulowane powietrznego do przewidzenia, gdy aparat powietrznego zakończy się niepowodzeniem w przyszłości, a więc konserwacji, które mogą zostać zaplanowane z wyprzedzeniem.

W tym samouczku używana [keras](https://keras.io/) biblioteki głębokie learning kognitywnych narzędzi Microsoft [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) jako wewnętrznej bazy danych.

## <a name="link-to-the-gallery-github-repository"></a>Połącz z repozytorium GitHub galerii 

Poniżej znajduje się łącze do publicznego repozytorium GitHub: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)

## <a name="use-case-overview"></a>Omówienie przypadków użycia

W tym samouczku użyto przykładu zdarzenia Uruchom — błąd aparatu symulowane powietrznego aby zademonstrować konserwacji predykcyjnej modelowania procesu. Niejawne założeń modelowania danych, jak wykonać poniżej jest zasobów zainteresowania przetwarzania wzorca degradacji, której w pomiarów czujnik elementu zawartości. Sprawdzając wartości czujnik zasobów w czasie, algorytmu uczenia maszynowego można znaleźć relacji między czujnik wartości i zmiany w wartości czujnika do historycznych błędów w celu prognozowania błędów w przyszłości. Sugerujemy, sprawdzając format danych i przechodzenia przez wszystkie trzy kroki szablonu przed wymianą z własnych danych.

## <a name="prerequisites"></a>Wymagania wstępne

- [Konta Azure](https://azure.microsoft.com/free/) (bezpłatnych wersji próbnych są dostępne).
- Zainstalowana kopia programu Azure Machine Learning Workbench z utworzono obszar roboczy.
- Dla modelu operationalization: Azure Machine Learning Operationalization z konfiguracją środowiska lokalnego wdrożenia i [modelu konto zarządzania](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Utwórz nowy projekt workbench

Utwórz nowy projekt za pomocą tego przykładu jako szablon:

1. Otwórz Azure Machine Learning Workbench
2. Na stronie Projekty, kliknij pozycję + Zaloguj się i wybierz nowy projekt
3. W okienku utwórz nowy projekt wprowadź informacje dla nowego projektu
4. W polu wyszukiwania szablony projektów wyszukiwania wpisz "Konserwacji predykcyjnej" i wybierz szablon
5. Kliknięcie pozycji Utwórz

## <a name="prepare-the-notebook-server-computation-target"></a>Przygotowanie docelowy obliczeniowym serwera notesu

Uruchamianie na komputerze lokalnym, z AML Workbench `File` menu, wybierz opcję `Open Command Prompt` lub `Open PowerShell` interfejsu wiersza polecenia. W ramach interfejsu wiersza polecenia systemu windows, wykonaj następujące polecenia:

`az ml experiment prepare --target docker --run-configuration docker`

 Zaleca się uruchamiania na standardzie DS4_V2 [danych maszyny wirtualnej nauki dla systemu Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Po skonfigurowaniu DSVM, należy uruchomić dwa poniższe polecenia:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Przy użyciu obrazów docker _przygotowane_, otwórz serwera notesu jupyter w *AML Workbench* notesów karcie lub uruchamiania serwera opartego na przeglądarce, uruchom:`az ml notebook start`

- Notesów są przechowywane w `Code` odnaleźć katalogu w środowisku Jupyter. Przeprowadzana te notesy sekwencyjnie jako numerowane, począwszy od dnia (`Code\1_data_ingestionand_and_preparation.ipynb`).

- Wybierz jądra zgodne z _Template [Project_Name] [Desired_Connection_Name], a następnie kliknij przycisk Ustaw jądra

## <a name="data-description"></a>Opis elementu danych

Szablon ma trzy zestawy danych jako dane wejściowe: "PM_train.txt", "PM_test.txt" i "PM_truth.txt"
1. Dane pociągu: są to dane Uruchom — błąd aparatu powietrznego. Dane pociągu ("PM_train.txt") składa się z wielu szeregach czasowych wielu zmiennych z "cyklu" jako jednostkę czasu, wraz z 21 odczyty czujników dla każdego cyklu. Jako są generowane z inny aparat tego samego typu można założyć każdy szeregów czasowych. Przyjęto, że każdy aparat rozpoczynać się od różnych stopni początkowej zużycia i produkcji zmiany, a tych informacji jest nieznany dla użytkownika. W tym symulowane danych aparat przyjęto, że działają normalnie na początku każdego szeregów czasowych. Rozpoczyna się zmniejszyć w pewnym momencie podczas serii cykli operacyjnych. Degradacji postępu i rozwoju wielkości. Po osiągnięciu progu wstępnie zdefiniowanych następnie aparat są uważane za niebezpieczne dla dalszego działania. Innymi słowy można uznać za punktu awarii odpowiedniego silnika ostatniego cyklu w każdym szeregów czasowych.

2. Dane testowe: to aparat powietrznego operacyjnego danych bez rejestrowane zdarzenia błędów. Dane testowe ("PM_test.txt") ma ten sam schemat danych jako dane szkoleniowe. Jedyną różnicą jest to, że dane nie wskazuje po wystąpieniu błędu (innymi słowy, w ostatnim okresie nie reprezentuje punkt awarii). Nie wiadomo, ile więcej cykli ten aparat może trwać przed jej nie powiedzie się.

3. Dane prawdy: zawiera informacje cykli pozostałych wartość true dla każdego aparatu danych testowych. Dane prawdy podstaw zawiera liczby cykli pracy pozostałej dla aparatów danych testowych.

## <a name="scenario-structure"></a>Scenariusz — struktura

Zawartość dla tego scenariusza jest dostępna w [repozytorium GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

W repozytorium, Brak pliku [Readme] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md), który opisano procesy z przygotowywanie danych do tworzenia i operacyjnych model. Trzy notesów Jupyter są dostępne w folderze [kod] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) w repozytorium. 

Następnie opisano krok po kroku scenariusza przepływu pracy:

### <a name="task-1-data-ingestion--preparation"></a>Zadanie 1: Wprowadzanie danych i przygotowanie

Notesu Jupyter wprowadzanie danych w `Code/1_data_ingestion_and_preparation.ipnyb` obciążenia trzech zestawów danych w danych wejściowych `Pandas` dataframe format przygotowuje dane do części modelowania i jest niektórych wizualizację wstępnych danych. Następnie przekształceniu danych `PySpark` formatowania i przechowywane w kontenera magazynu obiektów Blob platformy Azure w ramach subskrypcji do użycia w kolejnym zadaniem modelowania.

### <a name="task-2-model-building--evaluation"></a>Zadanie 2: Konstruowania modelu & oceny

Notesu Jupyter tworzenia modelu w `Code/2_model_building_and_evaluation.ipnyb` które odczytuje `PySpark` nauczenia i przetestowania zestawów danych z magazynu obiektów blob. Następnie sieci LSTM jest zbudowany z zestawami danych szkoleniowych. W zestawie testów mierzona jest wydajność modelu. Modelu wynikowego są serializowane i przechowywane w kontekście lokalnym do użycia w zadaniu operationalization.

### <a name="task-3-operationalization"></a>Zadanie 3: Operationalization

Operationalization notesu Jupyter w `Code/3_operationalization.ipnyb` ma przechowywanych modelu i tworzy wymagane funkcje i schematu dla wywoływania modelu dla platformy Azure hostowanej usługi sieci web. Notesu testów funkcje i zips zasoby operationalization w pliku zip, który także jest przechowywany w sieci kontenera magazynu obiektów Blob platformy Azure.
Plik zip zawiera:

- `service_schema.json`Plik definicji schematu dla wdrożenia. 
- `lstmscore.py`Init() i run() funkcje wymagane przez usługę sieci web platformy Azure
- `lstmfull.model`Katalog definicji modelu.

Notesu testy funkcji z definicji modelu przed spakowaniem zasoby operationalization dla wdrożenia. Instrukcje dotyczące wdrażania znajdują się na końcu notesu.


## <a name="conclusion"></a>Podsumowanie

W tym samouczku służy jako przewodnik dla początkujących chcą zastosować głębokie learning w domenie konserwacji predykcyjnej w środowisku notesu Jupyter w *Azure Machine Learning Workbench*. W tym samouczku używana jest prosty scenariusz, w którym tylko jedno źródło danych (czujnik wartości) służy do tworzenia prognoz. Bardziej zaawansowanych scenariuszy konserwacji predykcyjnej takim jak [konserwacji predykcyjnej modelowania przewodnik](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-R-Notebook-1), istnieje wiele innych źródeł danych (tj. historycznych konserwacji rekordów, dzienniki błędów, maszyny i operatora funkcji itp.) który mogą wymagać różnych rodzajów działania mające na celu można użyć w dokładnego uczenia sieci. Ponieważ konserwacji predykcyjnej nie jest typowe domeny głębokie szkoleniowe, jego aplikacji jest pusty obszar badań.

## <a name="references"></a>Dokumentacja

- [Szablon rozwiązania konserwacji predykcyjnej](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1)
- [Konserwacji predykcyjnej notesu Python Podręcznik modelowania](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-Python-Notebook-1)
- [Konserwacji predykcyjnej przy użyciu PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="future-directions-and-improvements"></a>Przyszłe kierunki i ulepszenia

Ten samouczek obejmuje podstawy za pomocą bezpośrednich learning w konserwacji predykcyjnej; wiele problemów konserwacji predykcyjnej zwykle wymagają różnych źródeł danych, które należy wziąć pod uwagę, stosując głębokie learning w tej domenie. Ponadto jest ważne dostroić modele do uwzględnienia w prawo parametry, takie jak rozmiar okna. Czytnik można edytować odpowiednich części w tym scenariuszu i spróbuj scenariusza inny problem, takie jak te opisane w [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1) którym są związane z wielu źródeł danych.
