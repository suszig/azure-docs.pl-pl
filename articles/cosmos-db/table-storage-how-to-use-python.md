---
title: "Jak używać magazynu tabel Azure języka Python | Dokumentacja firmy Microsoft"
description: "Przechowywanie danych strukturalnych w chmurze za pomocą Magazynu tabel Azure, magazyn danych NoSQL."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: a786f82d94a1a0039ed65a618670f872ffa3e3c2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-with-python"></a>Jak używać magazynu tabel Azure języka Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

W tym przewodniku opisano sposób wykonywania typowych scenariuszy magazynu tabel Azure przy użyciu języka Python [Microsoft Azure magazynu SDK dla języka Python](https://github.com/Azure/azure-storage-python). Omówione scenariusze obejmują tworzenie i usuwania tabeli, wstawianie i badania jednostek.

Podczas pracy nad scenariusze w tym samouczku, możesz odwoływać się do [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a>Zainstalować magazynu Azure SDK dla języka Python

Po utworzeniu konta magazynu, następnym krokiem jest zainstalowanie [Microsoft Azure magazynu SDK dla języka Python](https://github.com/Azure/azure-storage-python). Aby uzyskać więcej informacji na temat instalowania zestawu SDK, zapoznaj się [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) pliku w zestawie SDK magazynu dla repozytorium Python w witrynie GitHub.

## <a name="create-a-table"></a>Tworzenie tabeli

Aby pracować z usługi Azure tabel w języku Python, należy zaimportować [TableService] [ py_TableService] modułu. Ponieważ będzie działać z jednostek tabeli, należy również [jednostki] [ py_Entity] klasy. Dodaj ten kod u góry pliku Python do zaimportowania zarówno:

```python
from azure.storage.table import TableService, Entity
```

Utwórz [TableService] [ py_TableService] obiektu, przekazując klucz konta magazynu nazwy i konta. Zastąp `myaccount` i `mykey` z nazwą konta i klucz i wywołanie [create_table] [ py_create_table] do utworzenia tabeli w usłudze Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli

Aby dodać jednostkę, najpierw utwórz obiekt, który reprezentuje jednostki, a następnie przekaż obiekt do [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metody. Obiekt jednostki można słownika lub typu obiektu [jednostki][py_Entity]i definiuje użytkownika jednostki nazwy i wartości właściwości. Każdy podmiot musi zawierać wymaganych [PartitionKey i RowKey](#partitionkey-and-rowkey) właściwości, oprócz innych właściwości, należy zdefiniować dla jednostki.

Ten przykład tworzy obiekt słownik reprezentujący jednostkę, następnie przekazuje ją do [insert_entity] [ py_insert_entity] metody, aby dodać go do tabeli:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Ten przykład tworzy [jednostki] [ py_Entity] obiekt, a następnie przekazuje go do [insert_entity] [ py_insert_entity] metody, aby dodać go do tabeli:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey i RowKey

Należy określić zarówno **PartitionKey** i **RowKey** właściwości dla każdej jednostki. Jak razem są unikatowe identyfikatory jednostki, tworzą one klucza podstawowego jednostki. Można badać przy użyciu następujących wartości szybciej, niż można zapytania innych właściwości jednostki, ponieważ tylko te właściwości są indeksowane.

Używane przez usługę tabeli **PartitionKey** do inteligentnie rozpowszechniają jednostek tabeli węzłów magazynu. Jednostki, które mają taki sam **PartitionKey** są przechowywane w tym samym węźle. **RowKey** jest unikatowy identyfikator w partycji, należy do jednostki.

## <a name="update-an-entity"></a>Aktualizuj jednostkę

Aby zaktualizować wszystkie wartości właściwości jednostki, należy wywołać [update_entity] [ py_update_entity] metody. W tym przykładzie pokazano, jak zastąpić istniejącą jednostkę zaktualizowanej wersji:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Jeśli jednostka, która jest aktualizowana już nie istnieje, następnie operacji aktualizacji zakończy się niepowodzeniem. Jeśli chcesz przechowywać jednostki tego, czy istnieje lub nie, użyj [insert_or_replace_entity][py_insert_or_replace_entity]. W poniższym przykładzie pierwsze wywołanie spowoduje zastąpienie istniejącej jednostki. Drugie wywołanie będzie wstawić nową jednostkę, ponieważ brak jednostki z określonym PartitionKey i RowKey nie istnieje w tabeli.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> [Update_entity] [ py_update_entity] metoda zastępuje wszystkie właściwości i wartości istniejącej jednostki, która umożliwia także usunąć właściwości z istniejącej jednostki. Można użyć [merge_entity] [ py_merge_entity] metody do zaktualizowania istniejącej jednostki z wartościami właściwości nowe lub zmodyfikowane bez całkowicie zastępowania jednostki.

## <a name="modify-multiple-entities"></a>Modyfikowanie wiele jednostek

W celu zapewnienia atomic przetwarzania żądania przez usługę tabeli, możesz przesłać wiele operacji ze sobą w partii. Najpierw użyj [TableBatch] [ py_TableBatch] klasy, aby dodać wiele operacji do pojedynczej partii. Następnie wywołaj [TableService][py_TableService].[ commit_batch] [ py_commit_batch] przesłać operacje w niepodzielną operację. Wszystkie jednostki można zmodyfikować w partii musi należeć do tej samej partycji.

W tym przykładzie dodaje dwie jednostki w partii:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Partie można również przy użyciu składni Menedżera kontekstu:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Zapytanie dla jednostki

Aby wyszukać jednostkę w tabeli, należy przekazać jego PartitionKey i RowKey do [TableService][py_TableService].[ get_entity] [ py_get_entity] metody.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Zapytanie zestawu jednostek

Można wyszukać zestawu jednostek, podając ciąg filtru z **filtru** parametru. W tym przykładzie znajduje wszystkie zadania w Seattle, stosując filtr na PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Tworzenie zapytania do podzbioru właściwości jednostki

Można również ograniczyć, właściwości, które są zwracane dla każdej jednostki w zapytaniu. Ta technika, zwana *projekcji*, redukuje przepustowość i może poprawiać wydajność zapytań, zwłaszcza w przypadku dużych jednostek lub zestawów wyników. Użyj **wybierz** parametru i przekazać nazwy właściwości, które jest zwracana do klienta.

Zapytanie w poniższym kodzie zwraca tylko opisy jednostek w tabeli.

> [!NOTE]
> Poniższy fragment działa tylko w usłudze Azure Storage. Nie jest obsługiwana przez emulator magazynu.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Usuwanie jednostki

Usuwanie jednostki przez przekazanie jej PartitionKey i RowKey do [delete_entity] [ py_delete_entity] metody.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Usuwanie tabeli

Jeśli nie są już potrzebne tabeli lub dowolnej jednostki w niej wywołanie [delete_table] [ py_delete_table] metodę, aby trwale usunąć tabelę z usługi Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Kolejne kroki

* [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Magazyn Azure SDK dla języka Python](https://github.com/Azure/azure-storage-python)
* [Centrum deweloperów języka Python](https://azure.microsoft.com/develop/python/)
* [Eksplorator magazynu Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md): wolne, i platform aplikacji do pracy wizualnie z danymi usługi Azure Storage w systemie Windows, macOS i Linux.

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch