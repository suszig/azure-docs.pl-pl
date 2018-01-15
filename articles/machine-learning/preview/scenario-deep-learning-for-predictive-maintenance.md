---
title: "Głębokie learning konserwacji predykcyjnej rzeczywistych scenariuszach - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o replikacji samouczek dotyczący uczenia głębokie dla konserwacji predykcyjnej z usługi Azure Machine Learning Workbench."
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: a55209256c29fa62cc2da72f9653fbc7fc0e7c54
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Głębokie learning konserwacji predykcyjnej rzeczywistych scenariuszy

Głębokie learning jest jednym z najpopularniejszych trendów w uczeniu maszynowym. Głębokie learning jest używany w wielu pól i aplikacji, w tym driverless samochodów, rozpoznawanie mowy i obrazu robotics i not. Głębokie learning jest zestawem algorytmów jest podstawą przez kształt inteligencji (biologicznych sieci neuronowej) i uczenia maszynowego. Kognitywnych służące zwykle można znaleźć dokładnego learning jako sztuczne sieci neuronowej (ANNs).

Konserwacji predykcyjnej jest również popularnych. W konserwacji predykcyjnej wiele różnych technik zaprojektowano w celu określenia stanu urządzeń i przewidywanie podczas konserwacji powinny być wykonywane. Niektóre typowe zastosowania konserwacji predykcyjnej są prognozowania awarii, diagnozy awarii (Analiza przyczyn), wykrywanie błędów, klasyfikację typu awarii i zalecenie ograniczenie lub konserwacji akcji po awarii.

W scenariuszach konserwacji predykcyjnej dane są zbierane wraz z upływem czasu, aby monitorować stan urządzeń. Celem jest uzyskanie wzorców, które mogą pomóc przewidzieć i ostatecznie zapobiec błędom. Przy użyciu [długi krótki okres pamięci (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sieci jest bezpośrednie, learning metodę, która jest szczególnie atrakcyjne w konserwacji predykcyjnej. LSTM sieci są dobrze sobie radzi pod sekwencji. Czas serii danych można wstecz na dłuższy czas, aby wykrywać wzorce awarii.

W tym samouczku budujemy sieć LSTM zestawu danych i scenariusz, w którym są opisane w [konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3). Używamy sieci na potrzeby prognozowania pozostałych użytkowania aparatów powietrznego. Szablon używa wartości czujnik symulowane powietrznego do prognozowania, gdy aparat powietrznego zakończy się niepowodzeniem w przyszłości. Za pomocą tego prognozowania, konserwacji mogą zostać zaplanowane z wyprzedzeniem, aby uniknąć awarii.

W tym samouczku używana [Keras](https://keras.io/) głębokie szkoleniowe dotyczące biblioteki i kognitywnych zestaw narzędzi firmy Microsoft [CNTK](https://docs.microsoft.com/cognitive-toolkit/Using-CNTK-with-Keras) jako wewnętrznej.

Publicznego repozytorium GitHub, zawierający przykłady w tym samouczku jest na [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Omówienie przypadków użycia

W tym samouczku użyto przykładu zdarzenia Uruchom — błąd aparatu symulowane powietrznego aby zademonstrować konserwacji predykcyjnej modelowania procesu. 

Niejawne założeń danych modelowania opisanych tutaj się, że zasób ma przetwarzania wzorca degradacji. Wzorzec jest widoczny w pomiarów czujnik elementu zawartości. Przez sprawdzenie wartości czujnik zasobów w czasie, algorytmu uczenia maszynowego można znaleźć relacji między wartościami czujnika, zmiany w wartościach czujnik i historycznych błędów. Ta relacja służy do prognozowania błędów w przyszłości. 

Zalecamy Sprawdź format danych, a następnie wykonać wszystkie trzy kroki szablonu Aby zamienić przykładowe dane z własnych datą.

## <a name="prerequisites"></a>Wymagania wstępne

- [Konta Azure](https://azure.microsoft.com/free/) (bezpłatnych wersji próbnych są dostępne).
- Azure Machine Learning Workbench z utworzono obszar roboczy.
- Dla modelu operationalization: Azure Operationalization Learning maszyny, ze środowiska lokalnego wdrożenia, konfigurowanie i [konta Azure Machine Learning Model zarządzania](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview).

## <a name="create-a-new-workbench-project"></a>Utwórz nowy projekt Workbench

Utwórz nowy projekt, korzystając z tego przykładu jako szablon:

1. Otwórz Workbench uczenia maszynowego.
2. Na **projekty** wybierz pozycję  **+** , a następnie wybierz **nowy projekt**.
3. W **Utwórz nowy projekt** okienku, wprowadź informacje dotyczące nowego projektu.
4. W **szablony projektów wyszukiwania** polu wyszukiwania, wprowadź **konserwacji predykcyjnej**, a następnie wybierz szablon.
5. Wybierz pozycję **Utwórz**.

## <a name="prepare-the-notebook-server-computation-target"></a>Przygotowanie docelowy obliczeniowym serwera notesu

Na komputerze lokalnym, na Machine Learning Workbench **pliku** menu, wybierz opcję **Otwórz okno wiersza polecenia** lub **Otwórz program PowerShell**. W oknie wiersza polecenia wybranej opcji wykonaj następujące polecenia:

`az ml experiment prepare --target docker --run-configuration docker`

Zaleca się uruchamiania serwera notesu na standardzie DS4_V2 [danych nauki maszyny wirtualnej (DSVM) dla systemu Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Po DSVM jest skonfigurowana, uruchom następujące polecenia w celu przygotowania obrazów Docker:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Przygotowane obrazy usługi Docker Otwórz serwera notesu Jupyter. Aby otworzyć serwera notesu Jupyter, albo przejść do Machine Learning Workbench **notesów** karcie lub uruchamiania serwera opartego na przeglądarce:`az ml notebook start`

Notesów są przechowywane w katalogu kodu w środowisku Jupyter. Uruchom te notesy sekwencyjnie jako numerowane, począwszy od Code\1_data_ingestion_and_preparation.ipynb.

Wybierz jądra zgodne z _Template [project_name] [connection_name], a następnie wybierz **ustawić jądra**.

## <a name="data-description"></a>Opis elementu danych

Szablon używa trzech zestawów danych jako dane wejściowe w plikach PM_train.txt, PM_test.txt i PM_truth.txt.

-  **Dane pociągu**: powietrznego aparat danych uruchom do awarii. Dane pociągu (PM_train.txt) składa się z wieloma szeregów czasowych wielu zmiennych, *cykl* jako jednostki czasu. Obejmuje on 21 odczyty czujników dla każdego cyklu. 

    Można założyć, że każdy szeregu czasowego można wygenerować na podstawie inny aparat tego samego typu. Każdy aparat zakłada, że chcesz rozpocząć od różnych stopni początkowej zużycia i produkcji odmiany. Te informacje są nieznane dla użytkownika. 

    W tym symulowane danych aparat przyjęto, że działają normalnie na początku każdego szeregów czasowych. Rozpoczyna się zmniejszyć w pewnym momencie podczas serii cykli operacyjnych. Degradacji postępu i rozwoju wielkości. 

    Po osiągnięciu progu wstępnie zdefiniowanych przez aparat są uważane za niebezpieczne dla dalszego działania. Ostatniego cyklu w każdym szeregu czasowego jest uznawana za punktu awarii odpowiedniego aparatu.

-   **Dane testowe**: powietrznego aparat dane operacyjne bez rejestrowane zdarzenia błędów. Dane testowe (PM_test.txt) ma ten sam schemat danych jako dane szkoleniowe. Jedyną różnicą jest to, że dane nie wskazuje po wystąpieniu błędu (jest ostatnim okresie *nie* reprezentuje punkt awarii). Nie wiadomo, ile więcej cykli ten aparat może trwać przed jej nie powiedzie się.

- **Dane prawdy**: informacje o pozostałych true cykli dla każdego aparatu danych testowych. Dane prawdy podstaw zawiera liczby cykli pracy pozostałej dla aparatów danych testowych.

## <a name="scenario-structure"></a>Scenariusz — struktura

Zawartość dla tego scenariusza jest dostępna w [repozytorium GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance). 

W repozytorium plik [readme] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md) opisano procesy, od przygotowania dane, do tworzenia i operacyjnych modelu. Trzy notesów Jupyter są dostępne w folderze [kod] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) w repozytorium. 

Następnie opisano krok po kroku scenariusza przepływ pracy.

### <a name="task-1-data-ingestion-and-preparation"></a>Zadanie 1: Wprowadzanie danych i przygotowanie

Notesu Jupyter wprowadzanie danych w Code/1_data_ingestion_and_preparation.ipnyb ładuje trzy zestawy danych wejściowych do formatu dataframe Pandas. Następnie przygotowuje dane do modelowania i jest niektórych wizualizację wstępnych danych. Dane są następnie przekształcone w formacie PySpark i przechowywane w kontenera magazynu obiektów Blob platformy Azure w ramach subskrypcji platformy Azure do użycia w kolejnym zadaniem modelowania.

### <a name="task-2-model-building-and-evaluation"></a>Zadanie 2: Konstruowania modelu i ocena

Notesu Jupyter tworzenia modelu w odczyty Code/2_model_building_and_evaluation.ipnyb PySpark nauczenia i przetestowania zestawów danych z magazynu obiektów Blob. Następnie sieci LSTM jest zbudowane z zestawami danych szkoleniowych. W zestawie testów mierzona jest wydajność modelu. Modelu wynikowego są serializowane i przechowywane w kontekście lokalnym do użycia w zadaniu operationalization.

### <a name="task-3-operationalization"></a>Zadanie 3: Operationalization

Operationalization notesu Jupyter w Code/3_operationalization.ipnyb korzysta z przechowywanych modelu Tworzenie funkcji i schematu, które są wymagane do wywoływania modelu dla usługi hostowanej Azure sieci web. Notesu testów funkcje, a następnie zips pliku (kompresuje) zasoby operationalization na zip.

Plik zip zawiera następujące pliki:

- **modellstm.JSON**: plik definicji schematu dla wdrożenia. 
- **lstmscore.PY**: **init()** i **run()** funkcji, które są wymagane przez usługę sieci web platformy Azure.
- **lstm.model**: katalog definicji modelu.

Notesu testy funkcji za pomocą definicji modelu, przed jego pakietów zasobów operationalization dla wdrożenia. Instrukcje dotyczące wdrażania znajdują się na końcu notesu.


## <a name="conclusion"></a>Podsumowanie

W tym samouczku korzysta z prostego scenariusza danych tylko jedno źródło (czujnik wartości) służy do tworzenia prognoz. Bardziej zaawansowanych scenariuszy konserwacji predykcyjnej, takie jak [predykcyjnej konserwacji modelowania przewodnik R notesu](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), mogą używać wielu źródeł danych. Inne źródła danych mogą obejmować rekordy historyczne konserwacji, dzienniki błędów i funkcje maszyny i operatora. Dodatkowych źródeł danych może wymagać różnego rodzaju działania do użycia w sieciach learning bezpośrednich. Należy również Dostrój modeli prawo parametrów, takich jak rozmiar okna. 

Możesz edytować odpowiednich części w tym scenariuszu i spróbuj problem różnych scenariuszy, takich jak te opisane w [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1), który obejmuje wiele źródeł danych.


## <a name="references"></a>Dokumentacja

- [Szablon rozwiązania konserwacji predykcyjnej](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Konserwacji predykcyjnej notesu Python Podręcznik modelowania](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [Konserwacji predykcyjnej przy użyciu PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

