---
title: "Dodaj odporność na uszkodzenia w przypadku działania kopiowania fabryki danych Azure przez pominięcie niezgodne wierszy | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać odporność na uszkodzenia w przypadku działania kopiowania fabryki danych Azure przez pominięcie niezgodne wierszy podczas kopiowania"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 5c32d4ac2c1179a83a82bd5deb41047b82e43b7e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Dodaj odporność na uszkodzenia w przypadku działania kopiowania przez pominięcie niezgodne wierszy
> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi Data Factory, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi fabryka danych, która jest w wersji zapoznawczej, zobacz [odporność na uszkodzenia w przypadku działania kopiowania fabryki danych w wersji 2](../copy-activity-fault-tolerance.md).

Fabryka danych Azure [działanie kopiowania](data-factory-data-movement-activities.md) oferuje obsługi niezgodne wierszy, gdy kopiowanie danych między źródłowy i odbiorczy magazynów danych na dwa sposoby:

- Możesz przerwać i niepowodzenie kopiowania działanie w przypadku niezgodnych danych napotkało (domyślnie).
- Możesz skopiować wszystkie dane przez dodanie odporność na uszkodzenia i pomijanie niezgodne dane wierszy. Ponadto możesz zalogować się niezgodne wiersze magazynu obiektów Blob platformy Azure. Następnie można sprawdzić dziennik Dowiedz się przyczynę błędu, Usuń dane w źródle danych, a następnie ponów działanie kopiowania.

## <a name="supported-scenarios"></a>Obsługiwane scenariusze
Działanie kopiowania obsługuje trzy scenariusze wykrywanie, pomijanie i rejestrowanie danych niezgodne:

- **Niezgodność między typem źródła danych i typ macierzysty ujścia**

    Na przykład: kopiowanie danych z pliku CSV w magazynie obiektów Blob do bazy danych SQL z definicji schematu, która zawiera trzy **INT** kolumny o typie. Wiersze pliku CSV, które zawierają dane liczbowe, takie jak `123,456,789` są pomyślnie skopiowane do ujścia magazynu. Jednak wiersze zawierające wartości innych niż alfanumeryczne, takie jak `123,456,abc` zostały wykryte jako niezgodna ale są pomijane.

- **Niezgodność liczby kolumn między serwerem źródłowym a sink**

    Na przykład: kopiowanie danych z pliku CSV w magazynie obiektów Blob do bazy danych SQL z definicji schematu, który zawiera sześć kolumn. Wiersze pliku CSV, które zawierają sześć kolumn są została pomyślnie skopiowana do ujścia magazynu. Wiersze pliku CSV, które zawierają więcej lub mniej niż sześć kolumn są wykryte jako niezgodne, są pomijane.

- **Naruszenia dotyczącego klucza podstawowego podczas zapisywania relacyjnej bazy danych**

    Na przykład: kopiowanie danych z programu SQL server z bazą danych SQL. W bazie danych SQL zbiornika jest zdefiniowany klucz podstawowy, ale taki klucz podstawowy jest zdefiniowany w programie SQL server źródła. Zduplikowane wiersze, które istnieją w źródle nie można skopiować do ujścia. Działanie kopiowania kopiuje tylko pierwszy wiersz źródła danych do ujścia. Wiersze kolejnych źródła, które zawierają zduplikowane wartości klucza podstawowego są wykrywane niezgodne i są pomijane.

## <a name="configuration"></a>Konfiguracja
W poniższym przykładzie przedstawiono definicji JSON, aby skonfigurować pomijanie niezgodne wierszy w przypadku działania kopiowania:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Właściwość | Opis | Dozwolone wartości | Wymagane |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Włącz pomijanie niezgodne wierszy podczas kopiowania lub nie. | True<br/>Wartość FAŁSZ (ustawienie domyślne) | Nie |
| **redirectIncompatibleRowSettings** | Grupy właściwości, które można określić, kiedy mają być rejestrowane niezgodne wierszy. | &nbsp; | Nie |
| **linkedServiceName** | Połączonej usługi magazynu Azure do przechowywania dziennika, który zawiera wiersze zostało pominięte. | Nazwa [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) lub [element AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) połączonej usługi, która odwołuje się do wystąpienia magazynu, który ma być używany do przechowywania plików dziennika. | Nie |
| **Ścieżka** | Ścieżka pliku dziennika, który zawiera wiersze zostało pominięte. | Określ ścieżki do magazynu obiektów Blob, który ma być używany do rejestrowania danych niezgodne. Jeśli ścieżka nie zostanie określona, usługa tworzy kontener. | Nie |

## <a name="monitoring"></a>Monitorowanie
Po zakończeniu uruchamiania działania kopiowania, można wyświetlić Liczba pominiętych wierszy w sekcji monitorowania:

![Monitor pominięte niezgodne wierszy](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Jeśli konfigurujesz dziennika niezgodne wiersze, można znaleźć pliku dziennika na tej ścieżce: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` w pliku dziennika, możesz wyświetlać wiersze, które zostały pominięte i przyczynę niezgodności.

Zarówno oryginalnych danych, jak i odpowiednie błąd są rejestrowane w pliku. Oto przykład zawartości pliku dziennika jest następujący:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat działania kopiowania fabryki danych Azure, zobacz [przenoszenia danych za pomocą działania kopiowania](data-factory-data-movement-activities.md).