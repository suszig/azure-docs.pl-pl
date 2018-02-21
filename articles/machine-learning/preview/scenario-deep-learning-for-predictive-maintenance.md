---
title: "Głębokie learning konserwacji predykcyjnej rzeczywistych scenariuszach - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o replikacji samouczek dotyczący uczenia głębokie dla konserwacji predykcyjnej z usługi Azure Machine Learning Workbench."
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Bezpośrednie uczenia dla konserwacji predykcyjnej rzeczywistych scenariuszy.

Głębokie learning jest jednym z najpopularniejszych trendów w uczeniu maszynowym, z aplikacjami do wielu obszarów, w tym:
- driverless samochodów i robotics
- Rozpoznawanie mowy i obrazu
- prognozy finansowe.

Znany również jako bezpośrednich Neuronowej sieci (DNN), te metody są inspirowana przez poszczególne grupy neuronów w inteligencji (biologicznych sieci neuronowej).

Wpływ urządzeń nieplanowanych przestojów mogą być szkodliwe dla dowolnego rodzaju działalności. Warto zachować urządzeń pola do zmaksymalizowania wykorzystania i wydajności i zminimalizować kosztowne, nieplanowanych przestojów. Wczesne identyfikację problemów mogą pomóc przydzielić zasoby obsługi ograniczony w sposób ekonomiczny i zwiększyć jakość i procesy łańcucha dostaw. 

Strategii konserwacji predykcyjnej (PM) używa machine learning metod, aby określić stan urządzenia preemptively przeprowadzania konserwacji w celu uniknięcia wydajność niekorzystny maszyny. W PM dane pobierane w czasie, aby monitorować stan komputera, jest analizy, aby znaleźć wzorców, które mogą służyć do prognozowania błędów. [Długie krótki okres pamięci (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) sieci są atrakcyjny dla tego ustawienia, ponieważ są one przeznaczone do poznania sekwencji danych.

## <a name="link-to-the-gallery-github-repository"></a>Połącz z repozytorium GitHub galerii

Poniżej znajduje się łącze do publicznego repozytorium GitHub raporty problem i udziały: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Omówienie przypadków użycia

W tym samouczku użyto przykładu zdarzenia Uruchom — błąd aparatu symulowane powietrznego aby zademonstrować konserwacji predykcyjnej modelowania procesu. Scenariusz jest opisany w [konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3)

Głównych założeń, w tym ustawieniu jest aparatem stopniowo ze spadkiem swojego okresu istnienia. Degradacji można wykryć w aparat czujnik pomiarów. PM próbuje modelowania relacji między zmiany w wartościach tych czujników i historycznych błędów. Model można następnie prognozowania, aparatu i kiedy może zakończyć się niepowodzeniem w przyszłości, na podstawie bieżącego stanu czujnik pomiarów.

Ten scenariusz tworzy sieć LSTM do prognozowania pozostałych użytkowania (RUL) aparatów powietrznego przy użyciu wartości czujnik historycznych. Przy użyciu [Keras](https://keras.io/) z [Tensorflow](https://www.tensorflow.org/) głębokie learning framework jako aparat obliczeń scenariusza przygotowuje LSTM z jednego zestawu aparaty i test sieci na podstawie zestawu niewidocznym aparatu.

## <a name="prerequisites"></a>Wymagania wstępne
- [Konta Azure](https://azure.microsoft.com/free/) (bezpłatnych wersji próbnych są dostępne).
- Azure Machine Learning Workbench z utworzono obszar roboczy.
- Dla modelu operationalization: Azure Operationalization Learning maszyny, ze środowiska lokalnego wdrożenia, konfigurowanie i [konta Azure Machine Learning Model zarządzania](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Utwórz nowy projekt Workbench

Utwórz nowy projekt, korzystając z tego przykładu jako szablon:

1. Otwórz Workbench uczenia maszynowego.
2. Na **projekty** wybierz pozycję  **+** , a następnie wybierz **nowy projekt**.
3. W **Utwórz nowy projekt** okienku, wprowadź informacje dotyczące nowego projektu.
4. W **szablony projektów wyszukiwania** polu wyszukiwania wpisz "Konserwacji predykcyjnej", wybierz **głębokie Learning w scenariuszu konserwacji predykcyjnej** szablonu.
5. Kliknij przycisk **Utwórz** przycisku

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

Szablon używa trzech zestawów danych jako dane wejściowe w plikach **PM_train.txt**, **PM_test.txt**, i **PM_truth.txt**. 

-  **Dane pociągu**: powietrznego aparat danych uruchom do awarii. Dane pociągu (PM_train.txt) składa się z wieloma szeregów czasowych wielu zmiennych, *cykl* jako jednostki czasu. Obejmuje on 21 odczyty czujników dla każdego cyklu. 

    - Każdy szeregu czasowego jest generowany na podstawie inny aparat tego samego typu. Każdy aparat rozpoczyna się od różnych stopni początkowej zużycia i różnice unikatowy produkcyjnym. Te informacje są nieznane dla użytkownika. 

    - W tym symulowane danych aparat przyjęto, że działają normalnie na początku każdego szeregów czasowych. Rozpoczyna się zmniejszyć w pewnym momencie podczas serii cykli operacyjnych. Degradacji postępu i rozwoju wielkości. 

    - Po osiągnięciu progu wstępnie zdefiniowanych przez aparat są uważane za niebezpieczne dla dalszego działania. Ostatniego cyklu każdego szeregu czasowego jest punktem awarii tego aparatu.

-   **Dane testowe**: powietrznego aparat dane operacyjne bez rejestrowane zdarzenia błędów. Dane testowe (PM_test.txt) ma ten sam schemat danych jako dane szkoleniowe. Jedyną różnicą jest to, że dane nie wskazuje po wystąpieniu błędu (jest ostatnim okresie *nie* reprezentuje punkt awarii). Nie wiadomo, ile więcej cykli ten aparat może trwać przed jej nie powiedzie się.

- **Dane prawdy**: informacje o pozostałych true cykli dla każdego aparatu danych testowych. Dane prawdy podstaw zawiera liczby cykli pracy pozostałej dla aparatów danych testowych.

## <a name="scenario-structure"></a>Scenariusz — struktura

Przepływ pracy scenariusza jest podzielone na trzy kroki, każdy wykonywane w notesach Jupyter. Każdy notesu tworzy artefaktów danych, które są trwałe lokalnie do użycia w notesach następujące: 

### <a name="task-1-data-ingestion-and-preparation"></a>Zadanie 1: Wprowadzanie danych i przygotowanie

Notesu Jupyter wprowadzanie danych w `Code/1_data_ingestion_and_preparation.ipnyb` ładuje trzy zestawy danych wejściowych do formatu ramek danych Pandas. Następnie przygotowuje dane do modelowania i jest niektórych wizualizację wstępnych danych. Zestawy danych są przechowywane w lokalnych do kontekstu obliczeń do użycia w notesie Kompilowanie modelu.

### <a name="task-2-model-building-and-evaluation"></a>Zadanie 2: Konstruowania modelu i ocena

Notesu Jupyter tworzenia modelu w `Code/2_model_building_and_evaluation.ipnyb` odczytuje pociągu i testowania zestawów danych z dysku i tworzy sieć LSTM szkoleniowy zestaw danych. W zestawie testów mierzona jest wydajność modelu. Modelu wynikowego są serializowane i przechowywane w kontekście lokalnym do użycia w zadaniu operationalization.

### <a name="task-3-operationalization"></a>Zadanie 3: Operationalization

Operationalization notesu Jupyter w `Code/3_operationalization.ipnyb` korzysta z przechowywanych modelu Tworzenie funkcji i schematu wywoływania modelu usługi hostowanej platformy Azure w sieci web. Notesu testów funkcje i kompresuje zasoby do `LSTM_o16n.zip` pliku, który jest ładowany do Twojej kontenera magazynu systemu Azure dla wdrożenia.

`LSTM_o16n.zip` Plik wdrożenia zawiera następujące artefakty:

- `webservices_conda.yaml` Definiuje pakietów języka python, wymagane do uruchomienia LSTM modelu w celu wdrożenia.  
- `service_schema.json` Definiuje schemat danych oczekiwany przez LSTM model.     
- `lstmscore.py` Definiuje funkcje, których cel wdrożenia jest uruchomiona do oceny nowych danych.    
- `modellstm.json` Definiuje architekturę LSTM. Funkcje lstmscore.py odczytać architektury i wagi zainicjować modelu.
- `modellstm.h5` Definiuje wag na poziomie modelu.
- `test_service.py` Skrypt testu, który wywołuje punkt końcowy wdrożenia z testem rekordów danych. 
- `PM_test_files.pkl` `test_service.py` Odczytuje dane historyczne aparatu z `PM_test_files.pkl` pliku i wysyła wystarczającej liczby cykli LSTM do zwrócenia prawdopodobieństwo błąd aparatu usługi sieci Web.

Notesu testy funkcji przy użyciu definicji modelu, przed jego pakietów zasobów operationalization dla wdrożenia. Instrukcje dotyczące konfigurowania i testowania usługi sieci Web znajdują się na końcu `Code/3_operationalization.ipnyb` notesu.

## <a name="conclusion"></a>Podsumowanie

W tym samouczku korzysta z prostego scenariusza, używając wartości z czujnika do tworzenia prognoz. Więcej zaawansowanych scenariuszy konserwacji predykcyjnej, takie jak [predykcyjnej konserwacji modelowania przewodnik R notesu](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), można użyć wielu źródeł danych, takich jak rekordy historyczne konserwacji, dzienniki błędów i funkcje komputera. Dodatkowych źródeł danych może wymagać różnych procesów do użycia z learning głębokie.


## <a name="references"></a>Dokumentacja

Dostępne są inne przykłady przypadków użycia konserwacji predykcyjnej na różnych platformach:

* [Szablon rozwiązania konserwacji predykcyjnej](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Podręcznik modelowania konserwacji predykcyjnej](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Predykcyjnej konserwacji modelowania przewodnik przy użyciu usług R SQL](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Konserwacji predykcyjnej notesu Python Podręcznik modelowania](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Konserwacji predykcyjnej przy użyciu PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Konserwacji predykcyjnej rzeczywistych scenariuszy](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Kolejne kroki

Brak dostępnych wiele innych przykładowe scenariusze środowiska roboczego uczenia maszynowego Azure, która zademonstrować dodatkowe funkcje produktu. 