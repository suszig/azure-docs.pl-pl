---
title: "Odwołanie do usługi Azure Machine Learning Model danych kolekcji API | Dokumentacja firmy Microsoft"
description: "Odwołanie do usługi Azure Machine Learning Model danych kolekcji API."
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 7a0fda8a44d13bcaba84b4124d9b693c05874154
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Odwołanie do usługi Azure Machine Learning Model danych kolekcji API

Zbieranie danych modelu pozwala archiwum wejść modelu i prognozowanie z usługą sieci web uczenia maszynowego. Zobacz [modelu danych kolekcji przewodnik](how-to-use-model-data-collection.md) zrozumienie, jak zainstalować `azureml.datacollector` na komputerze z systemem Windows i Linux.

W niniejszym przewodniku interfejsu API używamy podejście krok po kroku na temat sposobu zbierania danych wejściowych modelu i prognozowanie z usługi sieci web uczenia maszynowego.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Włącz zbieranie danych modelu w środowisku Azure ML Workbench

 Wyszukaj conda\_dependencies.yml pliku w projekcie w folderze aml_config i mieć conda Twojego\_dependencies.yml plik zawiera moduł azureml.datacollector sekcji pip, jak pokazano poniżej. Należy pamiętać, że to tylko podsekcji pełne conda\_dependencies.yml pliku:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Obecnie można użyć modułu modułu zbierającego dane w konsoli usługi Azure ML Workbench przez uruchomienie w trybie docker. Tryb lokalny może nie działać we wszystkich środowiskach.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Włącz zbieranie danych modelu w pliku wyników

W pliku wyników jest używany przez operationalization Zaimportuj moduł zbierający dane i ModelDataCollector klasy:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Tworzenie wystąpienia modułu zbierającego dane modelu
Utwórz wystąpienie nowe wystąpienie klasy ModelDataCollector:

DC = ModelDataCollector (nazwa_modelu, identyfikator = "default", feature_names = Brak model_management_account_id = "Nieznany" webservice_name = "Nieznany" model_id = "Nieznany" model_version = "Nieznany")

Zobacz szczegóły klasy i parametr:

### <a name="class"></a>Klasy
| Nazwa | Opis |
|--------------------|--------------------|
| ModelDataCollector | Klasa w azureml.datacollector przestrzeni nazw. Wystąpienie tej klasy będzie służyć do zbierania danych modelu. Pojedynczy plik oceniania może zawierać wiele ModelDataCollectors. Każde wystąpienie powinna być używana do zbierania danych w jednej lokalizacji odrębny w pliku wyników, dzięki czemu schematu zebranych danych pozostaje spójna (to, że dane wejściowe i prognozowanie)|


### <a name="parameters"></a>Parametry

| Nazwa | Typ | Opis |
|-------------|------------|-------------------------|
| nazwa_modelu | Ciąg | Nazwa modelu zbieranych danych na |
| Identyfikator | Ciąg | Lokalizacja w kodzie, który identyfikuje te dane, np. "RawInput" lub "Prognozowania" |
| feature_names | Lista ciągów | Lista nazw funkcji, które stają się nagłówek csv, gdy podane |
| model_management_account_id | Ciąg | Identyfikator modelu konto zarządzania przechowywania tego modelu. To jest wypełniana automatycznie podczas modeli są operationalized za pośrednictwem AML |
| webservice_name | Ciąg | Nazwa usługi sieci Web, na którym ten model jest obecnie wdrożona. To jest wypełniana automatycznie podczas modeli są operationalized za pośrednictwem AML |
| model_id | Ciąg | Unikatowy identyfikator dla tego modelu w kontekście konta zarządzania modelu. to jest wypełniana automatycznie podczas modeli są operationalized za pośrednictwem AML |
| model_version | Ciąg | numer wersji tego modelu w kontekście konta zarządzania modelu. To jest wypełniana automatycznie podczas modeli są operationalized za pośrednictwem AML |



 

## <a name="collecting-the-model-data"></a>Zbieranie danych modelu

Można zbierać dane modelu za pomocą wystąpienia ModelDataCollector utworzone powyżej.

    dc.collect(input_data, user_correlation_id="")

Szczegółowe informacje można znaleźć metody i parametr:

### <a name="method"></a>Metoda
| Nazwa | Opis |
|--------------------|--------------------|
| zbieranie | Używany do gromadzenia danych dla modelu danych wejściowych lub prognozowania|


### <a name="parameters"></a>Parametry

| Nazwa | Typ | Opis |
|-------------|------------|-------------------------|
| input_data | wiele typów | dane do zebrania (obecnie akceptuje listy typów, numpy.array, pandas. DataFrame, pyspark.sql.DataFrame). Dla typów dataframe Jeśli istnieje nagłówek o nazwach funkcji tych informacji znajduje się w miejsce docelowe danych (bez konieczności jawnego przesłania nazwy funkcji w Konstruktorze ModelDataCollector) |
| user_correlation_id | Ciąg | Identyfikator korelacji opcjonalne, który może być udostępniane przez użytkownika służące do skorelowania tego prognozowania |

