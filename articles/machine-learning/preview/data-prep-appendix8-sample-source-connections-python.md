---
title: "Przykład dodatkowe źródła danych połączenia przy użyciu usługi Azure Machine Learning Przygotowanie danych | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera zbiór przykłady połączeń ze źródłem danych z usługi Azure Machine Learning Przygotowanie danych"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 9aaf4329b25cb189146949afed89cf15619ba592
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-custom-source-connections-python"></a>Przykładowe Źródło niestandardowe połączeń (Python) 
Przed przeczytaniem tego dodatku odczytu [Przegląd rozszerzalności języka Python](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Ładowanie danych z data.world

### <a name="prerequisites"></a>Wymagania wstępne

#### <a name="register-yourself-at-dataworld"></a>Zarejestruj się w data.world
Należy token interfejsu API z data.world witryny sieci Web.

#### <a name="install-dataworld-library"></a>Zainstaluj biblioteki data.world

Otwórz interfejs wiersza polecenia Azure Machine Learning Workbench wybierając **pliku** > **Otwórz interfejs wiersza polecenia**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Następnie uruchom `dw configure` w wierszu polecenia, który wyświetla monit o podanie token. Po wprowadzeniu token, .dw / katalog jest tworzony w katalogu macierzystym i token jest przechowywane.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Teraz można zaimportować data.world biblioteki.

#### <a name="load-data-into-data-preparation"></a>Ładowanie danych do przygotowywania danych

Utwórz nowy przepływ danych opartych na skryptach. Następnie użyj następującego skryptu do ładowania danych z data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Ładowanie danych z bazy danych Azure rozwiązania Cosmos do przygotowywania danych

Utwórz nowy przepływ danych opartych na skryptach, a następnie użyj następującego skryptu do ładowania danych z bazy danych usługi Azure rozwiązania Cosmos. (Biblioteki trzeba najpierw zainstalować. Aby uzyskać więcej informacji zobacz poprzednie dokument odwołania, który możemy połączyć.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```
