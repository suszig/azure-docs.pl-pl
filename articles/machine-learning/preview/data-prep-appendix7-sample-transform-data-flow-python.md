---
title: "Przykład przekształcania danych przepływ przekształcenia przy użyciu usługi Azure Machine Learning danych przygotowanie | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera zbiór przykłady przekształcania danych przepływ przekształcenia przy użyciu usługi Azure Machine Learning Przygotowanie danych"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 4a716c1934258e687eb48ecb4077c6be7b269c1f
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Przykładowe dane niestandardowe przepływ przekształcenia (Python) 
Nazwa przekształcenie w menu jest **przekształcenie przepływu danych (skrypt)**. Przed przeczytaniem tego dodatku odczytu [Przegląd rozszerzalności języka Python](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Przekształć ramki
### <a name="create-a-new-column-dynamically"></a>Utwórz nową kolumnę dynamicznie 
Dynamicznie tworzy kolumny (**city2**) i uzgadnia wielu różnych wersji Poznań do jednego z istniejących kolumny miasta.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Dodawanie nowego zagregowanych danych
Tworzy nową ramkę z wartości zagregowanych pierwszy i ostatni obliczanej kolumny wynik. Są one pogrupowane według **risk_category** kolumny.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize kolumny 
Reformulates dane spełniają formułę zmniejszenie wartości oddalone w kolumnie.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Przekształć przepływu danych
### <a name="fill-down"></a>Wypełnij w dół 
Wypełnienie w dół wymaga dwóch transformacji. Przyjęto założenie, danych, która wygląda następująco:


|Stan         |Miasto       |
|--------------|-----------|
|Waszyngton    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Kalifornia    |Los Angeles|
|              |Diego sieci SAN  |
|              |San Jose   |
|Teksas         |Dallas     |
|              |Antonio sieci SAN|
|              |Houston    |

Najpierw należy utworzyć transformacji Dodawanie kolumny (skrypt), zawierający następujący kod:
```python
    row['State'] if len(row['State']) > 0 else None
```
Teraz utworzyć transformacji przekształcenie przepływ danych (skrypt), zawierający następujący kod:
```python
    df = df.fillna( method='pad')
```

Dane teraz wygląda następująco:

|Stan         |Nowy stan         |Miasto       |
|--------------|--------------|-----------|
|Waszyngton    |Waszyngton    |Redmond    |
|              |Waszyngton    |Bellevue   |
|              |Waszyngton    |Issaquah   |
|              |Waszyngton    |Seattle    |
|Kalifornia    |Kalifornia    |Los Angeles|
|              |Kalifornia    |Diego sieci SAN  |
|              |Kalifornia    |San Jose   |
|Teksas         |Teksas         |Dallas     |
|              |Teksas         |Antonio sieci SAN|
|              |Teksas         |Houston    |


### <a name="min-max-normalization"></a>Wartość maksymalna min
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```