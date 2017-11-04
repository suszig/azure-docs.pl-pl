---
title: "Obsługiwane dostępnych źródeł danych Azure Machine Learning danych przygotowań | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera pełną listę obsługiwanych źródeł danych Azure Machine Learning danych przygotowania"
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
ms.date: 09/12/2017
ms.openlocfilehash: 1ef4c5c33d98cfeb566e8fe23bda9e0d3f041781
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="supported-data-sources-for-this-release"></a>Obsługiwanych źródeł danych w tej wersji 
Ten dokument zawiera opis listę aktualnie obsługiwanych źródeł danych Azure Machine Learning danych przygotowania.

Dostępne są następujące obsługiwanych źródeł danych w tej wersji.

## <a name="types"></a>Typy 
### <a name="directory-versus-file"></a>Katalog lub plik
*Plików i katalogów*: Wybierz pojedynczy plik, a następnie go odczytać w przygotowaniu danych. Typ pliku jest analizowany w celu określenia domyślnych parametrów połączenia pliku, który znajduje się na następnym ekranie. Wybierz katalog lub zestaw plików w katalogu (selektora plików jest multiselect). Albo podejście wyniki w plikach odczytywane jako pojedynczy przepływu danych z plikami dołączony do siebie (nagłówki wycięte w razie potrzeby).

Dostępne są następujące typy plików:
- Przecinkami (CSV, .tsv, txt itd.) 
- O stałej szerokości
- Zwykły tekst
- Plik JSON

### <a name="csv-file"></a>Plik CSV
Odczytuje plik CSV z magazynu.

#### <a name="options"></a>Opcje
- Separator
- Komentarz
- Nagłówki
- Decimal — symbol
- Kodowanie pliku
- Wierszy do pominięcia

### <a name="tsv-file"></a>Plik TSV
Odczytuje plik z wartościami rozdzielanymi TSV z magazynu.

#### <a name="options"></a>Opcje
- Komentarz
- Nagłówki
- Kodowanie pliku
- Wierszy do pominięcia

### <a name="excel-xlsxlsx"></a>Program Excel (.xls/.xlsx)
Odczytuje plik programu Excel, jeden arkusz naraz, określając nazwę arkusza lub numer.

#### <a name="options"></a>Opcje
- Numer nazwy arkusza
- Nagłówki
- Wierszy do pominięcia

### <a name="json-file"></a>Plik JSON
Plik JSON do odczytu z magazynu. Należy pamiętać, że plik jest "jako spłaszczone" na odczyt.

#### <a name="options"></a>Opcje
Brak

### <a name="parquet"></a>Parquet
Przeczytaj Parquet zestawu danych, albo pojedynczego pliku lub folderu.

Parquet zgodnie z formatem mogą mieć różne formy w magazynie. W przypadku mniejszych zestawów danych jest czasami używana .parquet pojedynczego pliku. Różne biblioteki Python obsługiwać odczyt lub zapis .parquet pojedynczego pliku. Obecnie Azure Machine Learning Workbench korzysta z biblioteki PyArrow Python do odczytywania Parquet podczas lokalnego interakcyjnej. Obsługuje ona plikami jednej .parquet (o ile zostały one zapisane, nie jako część większy zestaw danych). Obsługuje ona również Parquet zestawów danych. 

Zestaw danych Parquet to zbiór więcej niż jeden plik .parquet, z których każdy reprezentuje mniejszy partycji większy zestaw danych. Zestawy danych zwykle są zawarte w folderze. Są one domyślny format danych wyjściowych Parquet wspólnej platformy, na przykład Spark i Hive.

>[!NOTE]
>Podczas czytania Parquet danych, który znajduje się w folderze z wieloma plikami .parquet, jest możliwie najbezpieczniejszy wybierz katalog do odczytu i znaczników **Parquet Dataset** opcji. Dzięki temu PyArrow odczytu całego folderu zamiast poszczególnych plików. Dzięki temu odczytywanie bardziej skomplikowany sposób przechowywania Parquet na dysku (np. folderu partycjonowania.)

Wykonanie skalowania w poziomie zależy od możliwości odczytu Parquet platforma Spark i obsługuje pojedynczych plików oraz folderów.

#### <a name="options"></a>Opcje
*Zestaw danych parquet*: Ta opcja określa, czy usługi Azure Machine Learning Workbench używa trybu unticked lub ticked. Unticked tryb rozwijane w podanym katalogu i podejmuje próbę odczytania każdy plik w jego pojedynczo. Tryb ticked traktuje katalog jako cały zestaw danych i umożliwia PyArrow określić najlepszy sposób, aby zinterpretować plików.


## <a name="locations"></a>Lokalizacje
### <a name="local"></a>Lokalna
Lokalny dysk twardy lub lokalizacji magazynu mapowanej sieci.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Wymaga subskrypcji platformy Azure.

