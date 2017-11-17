---
title: "Zestawy danych i połączone usługi, w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zestawów danych i usług połączonych w fabryce danych. Połączone usługi magazyny obliczeń/danych łącze do fabryki danych. Zestawy danych reprezentują wejścia/wyjścia danych."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: 
ms.date: 09/05/2017
ms.author: shlo
ms.openlocfilehash: a13e19c7e1a22581b14d1a96e20b8a649c303fc3
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Zestawy danych i usług połączonych w fabryce danych Azure 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-create-datasets.md)
> * [Wersja 2 — wersja zapoznawcza](concepts-datasets-linked-services.md)

W tym artykule opisano, jakie zestawy danych są, jak są definiowane w formacie JSON i w jaki sposób są one używane w wersji 2 fabryki danych Azure potoków. 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [zestawów danych w wersji 1 z fabryki danych](v1/data-factory-create-datasets.md).

Jeśli jesteś nowym użytkownikiem usługi fabryka danych, zobacz [wprowadzenie do fabryki danych Azure](introduction.md) omówienie. 

## <a name="overview"></a>Przegląd
Fabryka danych może obejmować jeden lub wiele potoków. A **potoku** to logiczne grupowanie **działania** który razem wykonania zadania. Działania w potoku definiują akcje do wykonania na danych. Na przykład można użyć działania kopiowania można skopiować danych z lokalnego serwera SQL do magazynu obiektów Blob Azure. Następnie należy użyć działania Hive, które uruchamia skrypt Hive w klastrze usługi HDInsight Azure do przetwarzania danych z magazynu obiektów Blob wygenerowało danych wyjściowych. Ponadto można użyć drugiej działanie kopiowania można skopiować danych wyjściowych do usługi Azure SQL Data Warehouse, na które raportowania są wbudowane rozwiązania analizy biznesowej (BI). Aby uzyskać więcej informacji na temat potoków i działania, zobacz [potoków i działania](concepts-pipelines-activities.md) w fabryce danych Azure.

Teraz **dataset** jest nazwane widoku danych, która po prostu punktów lub odwołuje się do danych mają być używane w sieci **działania** jako danych wejściowych i wyjściowych. Zestawy danych identyfikują dane w różnych magazynach danych, takich jak tabele, pliki, foldery i dokumenty. Na przykład zestaw danych obiektów Blob platformy Azure określa folder i kontener obiektów blob w magazynie obiektów Blob, w którym działanie odczytywane dane.

Przed utworzeniem zestawu danych, należy utworzyć **połączona usługa** do łączenia z magazynu danych z fabryką danych. Połączone usługi działają podobnie do parametrów połączenia, umożliwiając definiowanie informacji wymaganych przez usługę Data Factory do nawiązywania połączeń z zasobami zewnętrznymi. Należy traktować go w ten sposób; zestaw danych reprezentuje struktury danych w ramach połączonych magazynów, a połączona usługa definiuje połączenie ze źródłem danych. Na przykład usługi Azure Storage połączone usługi łączy konta magazynu z fabryką danych. Zestaw danych obiektów Blob platformy Azure reprezentuje kontener obiektów blob oraz folder, w ramach tego konta magazynu platformy Azure, zawierającą wejściowych obiekty BLOB do przetworzenia.

Oto przykładowy scenariusz. Aby skopiować dane z magazynu obiektów Blob do bazy danych SQL, należy utworzyć dwa połączone usługi: Magazyn Azure i bazy danych SQL Azure. Następnie utwórz dwa zestawy danych: zestaw danych obiektów Blob platformy Azure, (czyli połączoną usługą magazynu Azure), a tabeli SQL Azure zestawu danych (odwołuje się do usługi baza danych SQL Azure połączone). Usługi magazynu Azure i połączone bazy danych SQL Azure zawiera parametry połączeń, które korzysta z fabryki danych w czasie wykonywania do nawiązania połączenia z usługi Azure Storage i Azure SQL Database, odpowiednio. Zestaw danych obiektów Blob platformy Azure określa folder obiektów blob, który zawiera wejściowych obiektów blob w magazynie obiektów Blob i kontener obiektów blob. Zestaw danych tabeli SQL Azure określa tabeli SQL w bazie danych SQL, do której ma zostać skopiowany danych.

Na poniższym diagramie przedstawiono relacje między potoku, działania, zestawu danych i połączonej usługi z fabryki danych:

![Relacja między potoku, działania, zestaw danych, połączone usługi](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON dla zestawu danych
Zestaw danych z fabryki danych jest zdefiniowany w formacie JSON w następujący sposób:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                 "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}

```
W poniższej tabeli opisano właściwości w powyższym JSON:

Właściwość | Opis | Wymagane | Domyślny
-------- | ----------- | -------- | -------
name | Nazwa zestawu danych. | Zobacz [fabryki danych Azure - reguły nazewnictwa](naming-rules.md). | Tak | Nie dotyczy
type | Typ zestawu danych. | Określ jeden z typów obsługiwanych przez fabrykę danych (na przykład: AzureBlob, AzureSqlTable). <br/><br/>Aby uzyskać więcej informacji, zobacz [zestawu danych typów](#dataset-types). | Tak | Nie dotyczy
Struktura | Schemat zestawu danych. | Aby uzyskać więcej informacji, zobacz [struktury zestawu danych](#dataset-structure). | Nie | Nie dotyczy
typeProperties | Właściwości typu są różne dla każdego typu (na przykład: obiektów Blob platformy Azure, tabeli Azure SQL). Aby uzyskać szczegółowe informacje o obsługiwanych typów i ich właściwości, zobacz [typ zestawu](#dataset-type). | Tak | Nie dotyczy

## <a name="dataset-example"></a>Przykład zestawu danych
W poniższym przykładzie zestawu danych reprezentuje tabeli o nazwie MyTable w bazie danych SQL.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                 "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Pamiętaj o następujących kwestiach:

- Typ jest ustawiony na AzureSqlTable.
- Typ właściwości tableName (specyficzne dla typu AzureSqlTable) ma ustawioną MyTable.
- linkedServiceName odwołuje się do połączonej usługi typu AzureSqlDatabase, który został zdefiniowany w następnym fragment kodu JSON.

## <a name="linked-service-example"></a>Przykład połączonej usługi
AzureSqlLinkedService jest zdefiniowane w następujący sposób:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```
W poprzednim fragment kodu JSON:

- **Typ** ma ustawioną wartość AzureSqlDatabase.
- **connectionString** właściwość type określa informacje do połączenia z bazą danych SQL.

Jak widać, połączonej usługi definiuje sposób nawiązywania połączenia z bazą danych SQL. Zestaw danych definiuje, jakie tabeli jest używane jako dane wejściowe i wyjściowe działania w potoku.

## <a name="dataset-type"></a>Typ zestawu danych
Istnieje wiele różnych typów zestawów danych, w zależności od źródła danych, którego używasz. Zobacz poniższą tabelę listę magazynów danych obsługiwane przez fabryki danych. Kliknij magazyn danych, aby dowiedzieć się, jak utworzyć połączonej usługi i zestawu danych dla tego magazynu danych.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

W tym przykładzie w poprzedniej sekcji ustawiono typ zestawu danych **AzureSqlTable**. Podobnie dla zestawu danych obiektów Blob platformy Azure, typ zestawu danych ma ustawioną wartość **AzureBlob**, jak pokazano w poniższych JSON:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                 "type": "LinkedServiceReference",
        }, 
 
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```
## <a name="dataset-structure"></a>Struktura zestawu danych
**Struktury** sekcja jest opcjonalna. Definiuje schemat zestawu danych przez zawierający kolekcję nazwy i typy danych kolumn. Sekcja struktury umożliwia Podaj informacje o typie, który służy do konwersji typów i mapowanie kolumn ze źródła do miejsca docelowego. W poniższym przykładzie element dataset zawiera trzy kolumny: timestamp, projectname i pageviews. Są one typu String, typ String i dziesiętnych, odpowiednio.

```json
[
    { "name": "timestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Każda kolumna w strukturze zawiera następujące właściwości:

Właściwość | Opis | Wymagane
-------- | ----------- | --------
name | Nazwa kolumny. | Tak
type | Typ danych kolumny. | Nie
Kultury | . Kulturę opartą na sieci do użycia, gdy typem jest typ architektury .NET: `Datetime` lub `Datetimeoffset`. Wartość domyślna to `en-us`. | Nie
Format | Ciąg do użycia, gdy typem jest typ architektury .NET formatu: `Datetime` lub `Datetimeoffset`. | Nie

Poniższe wskazówki pomocne w określeniu, kiedy obejmują informacje o strukturze i elementów do uwzględnienia w **struktury** sekcji.

- **Dla źródeł danych strukturalnych**, określ sekcji struktury tylko wtedy, gdy ma mapować kolumny źródłowe do zbiornika, kolumn i ich nazwy nie są takie same. Tego rodzaju źródła danych strukturalnych przechowuje informacje o schematu i typu danych oraz samych danych. Przykładami źródeł danych strukturalnych programu SQL Server, Oracle i bazy danych SQL Azure.<br/><br/>Ponieważ informacje o typie jest już dostępne dla źródeł danych strukturalnych, nie może zawierać informacje o typie zawierają sekcji struktury.
- **Do schematu w źródłach danych odczytu (w szczególności magazynu obiektów Blob)**, można wybrać do przechowywania danych bez przechowywania żadnych informacji schematu lub typ z danymi. W przypadku tych typów źródeł danych mają strukturę, aby mapować kolumny źródłowe do zbiornika kolumn. Gdy zestaw danych jest wartością wejściową dla działania kopiowania i typy danych zestawu źródła danych powinny być konwertowane na typy natywne dla obiekt sink także struktury.<br/><br/> Fabryka danych obsługuje następujące wartości udostępnienie informacji o typie w strukturze: `Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan`. 

Dowiedz się więcej informacji na temat sposobu fabryki danych mapowania źródła danych do zbiornika z [schematu i mapowanie typu]( copy-activity-schema-and-type-mapping.md) i kiedy należy określić informacje o strukturze.

## <a name="create-datasets"></a>Tworzenie zestawów danych
Zestawy danych można utworzyć za pomocą jednej z tych narzędzi i zestawy SDK: [interfejs API .NET](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [interfejsu API REST](quickstart-create-data-factory-rest-api.md), szablon Menedżera zasobów Azure i portalu Azure

## <a name="v1-vs-v2-datasets"></a>V1 vs. Zestawy danych w wersji 2

Poniżej przedstawiono niektóre różnice między fabryki danych v1 i v2 zestawów danych: 

- Właściwość zewnętrznych nie jest obsługiwana w wersji 2. Został zastąpiony przez [wyzwalacza](concepts-pipeline-execution-triggers.md).
- Właściwości zasad i dostępności nie są obsługiwane w wersji 2. Czas rozpoczęcia dla potoku jest zależna od [wyzwalaczy](concepts-pipeline-execution-triggers.md).
- Zakresami zestawów danych (zestawy danych zdefiniowany w potoku) nie są obsługiwane w wersji 2. 

## <a name="next-steps"></a>Następne kroki
Zobacz samouczek następujące instrukcje krok po kroku do tworzenia potoki i zestawów danych przy użyciu jednej z tych narzędzi i zestawy SDK. 

- [Quickstart: create a data factory using .NET (Szybki start: tworzenie fabryki danych przy użyciu platformy .NET)](quickstart-create-data-factory-dot-net.md)
- [Szybki Start: tworzenie fabryki danych przy użyciu programu PowerShell](quickstart-create-data-factory-powershell.md)
- [Szybki Start: tworzenie fabryki danych przy użyciu interfejsu API REST](quickstart-create-data-factory-rest-api.md)
- Szybki Start: tworzenie fabryki danych przy użyciu portalu Azure
