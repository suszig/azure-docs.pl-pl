---
title: "Azure Machine Learning wdrażanie usługi sieci Web zarządzania modelu | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera opis czynności związanych z wdrażaniem modelu uczenia maszynowego przy użyciu usługi Azure Machine Learning model zarządzania."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: c5942bed5e171c23864ef3ee10665273e04640a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Wdrażanie modelu uczenia maszynowego jako usługi sieci web

Azure Machine Learning modelu zarządzania zawiera interfejsy w celu wdrażania modeli jako konteneryzowanych usług sieci web opartych na Docker. Można wdrożyć modele utworzone za pomocą platform, takich jak Spark, kognitywnych zestawu narzędzi firmy Microsoft (CNTK), Keras, Tensorflow i Python. 

W tym dokumencie opisano procedurę wdrażania modeli jako usługi sieci web przy użyciu interfejsu wiersza polecenia (CLI) Azure Machine Learning Model zarządzania.

## <a name="deploying-web-services"></a>Wdrażanie usług sieci web
CLIs można wdrożyć usługi sieci web do uruchamiania na komputerze lokalnym lub w klastrze.

Firma Microsoft zaleca, począwszy od lokalnego wdrożenia. Zweryfikuj czy model i kod działają, następnie wdrażanie usługi sieci web do klastra do użytku produkcyjnego skali. Aby uzyskać więcej informacji na temat konfigurowania środowiska do wdrożenia klastra, zobacz [konfiguracji zarządzania modelu](deployment-setup-configuration.md). 

Poniżej przedstawiono kroki wdrażania:
1. Użyj modelu uczenia maszynowego zapisane, przeszkolone,
2. Tworzenie schematu dla usługi sieci web dane wejściowe i danych wyjściowych
3. Tworzenie kontenera Docker na podstawie obrazu
4. Tworzenie i wdrażanie usługi sieci web

### <a name="1-save-your-model"></a>1. Zapisz model
Uruchom plik zapisany trenowanego modelu, na przykład mymodel.pkl. Rozszerzenie pliku zależy od używanej do nauczenia i Zapisz model platformy. 

Na przykład można użyć języka Python Pickle biblioteki do zapisania trenowanego modelu w pliku. Oto [przykład](http://scikit-learn.org/stable/modules/model_persistence.html) przy użyciu zestawu danych Iris:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf = linear_model.LogisticRegression()
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Utwórz plik schema.json
Ten krok jest opcjonalny. 

Tworzenie schematu, aby automatycznie zatwierdzać wejście i wyjście usługi sieci web. CLIs również użycie schematu w celu generowania dokumentu Swagger dla usługi sieci web.

Aby utworzyć schemat, należy zaimportować następujące biblioteki:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Następnie określ zmienne wejściowe, takie jak Spark dataframe, Pandas dataframe lub NumPy tablicy. W poniższym przykładzie użyto tablicy Numpy:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```
W poniższym przykładzie użyto Spark dataframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

W poniższym przykładzie użyto PANDAS dataframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Utwórz plik score.py
Musisz podać w pliku score.py, który ładuje modelu i zwraca wynik(-i) prognozowania przy użyciu modelu.

Plik musi zawierać dwie funkcje: init i uruchom.

Dodaj następujący kod w górnej części pliku score.py, aby włączyć funkcję kolekcji danych, która pozwala zbierać dane wejściowe i prognozowania modelu

```python
from azureml.datacollector import ModelDataCollector
```

Sprawdź [modelu zbierania danych](how-to-use-model-data-collection.md) sekcji, aby uzyskać więcej informacji na temat korzystania z tej funkcji.

#### <a name="init-function"></a>Init — funkcja
Aby załadować zapisany model, należy użyć funkcji inicjowania.

Przykład funkcji init proste ładowania modelu:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Uruchamianie funkcji
Uruchom funkcję używa modelu i danych wejściowych do zwrócenia Prognozowanie.

Przykładem prostej uruchamiania funkcji przetwarzania danych wejściowych i zwrócenia wyniku prognozowania:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Zarejestruj modelu
Następujące polecenie służy do rejestrowania utworzonego w kroku 1, model

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Tworzenie manifestu
Następujące polecenie ułatwia tworzenie manifestu dla modelu

```
az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]
```
Można dodać wcześniej zarejestrowanego modelu do manifestu za pomocą argumentu `--model-id` lub `-i` w powyższym poleceniu. Można określić wiele modeli z dodatkowymi -i argumentów.

### <a name="6-create-an-image"></a>6. Tworzenie obrazu 
Z możliwością wytworzeniu swoim manifeście przed można utworzyć obrazu. 

```
az ml image create -n [image name] -manifest-id [the manifest ID]
```

Lub można utworzyć manifestu i obrazu za pomocą jednego polecenia. 

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Więcej szczegółów na parametry polecenia wpisz -h na końcu polecenia, na przykład obraz ml az utworzyć -h.


### <a name="7-create-and-deploy-the-web-service"></a>7. Tworzenie i wdrażanie usługi sieci web
Wdróż usługę za pomocą następującego polecenia:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>Aby wykonać poprzednie kroki 4 umożliwia także jednego polecenia. Użyj -h z usługą Utwórz polecenie, aby uzyskać więcej informacji.

### <a name="8-test-the-service"></a>8. Testowanie usługi
Aby uzyskać informacje na temat do wywołania tej usługi, użyj następującego polecenia:

```
az ml service usage realtime -i <service id>
```

Wywołania tej usługi przy użyciu funkcji wykonywania w wierszu polecenia:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Poniższy przykład wywołuje próbkę Iris usługi sieci web:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Kolejne kroki
Teraz, gdy zostały przetestowane usługi sieci web, aby uruchomić lokalnie, można wdrożyć klaster do użytku na dużą skalę. Aby uzyskać więcej informacji na temat konfigurowania klastra dla wdrożenia usługi sieci web, zobacz [konfiguracji zarządzania modelu](deployment-setup-configuration.md). 
