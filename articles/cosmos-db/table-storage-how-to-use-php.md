---
title: "Jak używać magazynu tabel w języku PHP | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi tabel w języku PHP do tworzenia i usuwania tabeli i wstawianie, usuwanie i zapytanie tabeli."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 7fa82875ba823f1a4a9a886d4f699ca6757c3a3b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-use-azure-table-storage-from-php"></a>Jak używać magazynu tabel Azure za pomocą języka PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Przegląd
W tym przewodniku przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi tabel Azure. Przykłady są napisane w PHP i użyj [zestaw Azure SDK for PHP][download]. Omówione scenariusze obejmują **tworzenia i usuwania tabeli i wstawianie, usuwanie i badania jednostek w tabeli**. Aby uzyskać więcej informacji w usłudze tabel Azure, zobacz [następne kroki](#next-steps) sekcji.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP
Jedynym wymaganiem służący do tworzenia aplikacji PHP, który uzyskuje dostęp do usługi Azure Table jest odwoływanie się do klasy w zestawie SDK Azure dla programu PHP z w kodzie. Narzędzia do programowania służy do tworzenia aplikacji, łącznie z Notatnika.

W tym przewodniku korzystanie z funkcji usługi tabeli, które mogą być wywoływane w ramach aplikacji PHP lokalnie lub w kodzie działających w roli sieci web platformy Azure, roli procesu roboczego lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Pobierz bibliotek klienta platformy Azure
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurowanie aplikacji do uzyskania dostępu do usługi tabeli
Aby korzystać z usługi tabel Azure interfejsów API, musisz:

1. Odwołanie przy użyciu pliku automatycznej ładowarki [require_once] [ require_once] instrukcji, i
2. Odwoływać się do wszystkich klas, których może używać.

Poniższy przykład pokazuje, jak dołączyć plik automatycznej ładowarki i odwołanie **ServicesBuilder** klasy.

> [!NOTE]
> Przykłady w tym artykule przyjęto założenie, że zainstalowano bibliotek klienckich PHP na platformie Azure za pośrednictwem Composer. Jeśli zainstalowano bibliotek ręcznie, należy odwoływać się do <code>WindowsAzure.php</code> automatycznej ładowarki pliku.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

W poniższych przykładach `require_once` instrukcji jest zawsze widoczne, ale odwołuje się tylko do klas, które są konieczne na przykład do wykonania.

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Można utworzyć klienta usługi tabel Azure, najpierw musi mieć prawidłowe parametry połączenia. Format dla parametrów połączenia usługi tabeli jest:

Aby uzyskać dostęp do usługi na żywo:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Aby uzyskać dostęp do emulatora magazynu:

```php
UseDevelopmentStorage=true
```

Aby utworzyć dowolnego klienta usługi Azure, musisz użyć **ServicesBuilder** klasy. Możesz:

* Przekaż parametry połączenia do niego bezpośrednio lub
* Użyj **CloudConfigurationManager (CCM)** do sprawdzenia wiele źródeł zewnętrznych ciągu połączenia:
  * Domyślnie pochodzi z obsługą jednego źródła zewnętrznego — zmienne środowiskowe
  * można dodać nowego źródła rozszerzając **ConnectionStringSource** — klasa

Przykłady przedstawione w tym miejscu parametry połączenia zostaną przekazane bezpośrednio.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Tworzenie tabeli
A **TableRestProxy** obiektu umożliwia tworzenie tabeli z **createTable** metody. Podczas tworzenia tabeli, można ustawić limitu czasu usługi tabeli. (Aby uzyskać więcej informacji na temat limitu czasu usługi tabeli, zobacz [ustawienie przekroczeń limitu czasu dla operacji usługi tabeli][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

Aby uzyskać informacji o ograniczeniach dotyczących nazw tabeli, zobacz [opis modelu danych usługi tabel][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli
Aby dodać jednostkę do tabeli, Utwórz nową **jednostki** obiektu i przekaż go do **TableRestProxy -> insertEntity**. Należy pamiętać, że podczas tworzenia obiektu, należy określić `PartitionKey` i `RowKey`. Są to unikatowych identyfikatorów dla jednostek które mają wartości, które można wykonać zapytania znacznie szybciej niż inne właściwości jednostki. System używa `PartitionKey` to automatyczną dystrybucję jednostek tabeli przez wiele węzłów magazynu. Jednostek o takim samym `PartitionKey` są przechowywane w tym samym węźle. (Wykonywania operacji na wiele jednostek przechowywanych w tym samym węźle lepszym rozwiązaniem niż na jednostek przechowywanych w różnych węzłach.) `RowKey` Jest unikatowy identyfikator jednostek w partycji.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Informacje dotyczące właściwości i typów, zobacz [opis modelu danych usługi tabel][table-data-model].

**TableRestProxy** klasy oferuje dwie alternatywne metody Wstawianie jednostek: **insertOrMergeEntity** i **insertOrReplaceEntity**. Aby korzystać z tych metod, Utwórz nową **jednostki** i przekaż go jako parametr do każdej z metod. Każda metoda powoduje wstawienie jednostki, jeśli nie istnieje. Jeśli ta jednostka już istnieje, **insertOrMergeEntity** aktualizacji wartości właściwości, jeśli właściwości już istnieje i dodaje nowe właściwości nie są dostępne, podczas gdy **insertOrReplaceEntity** całkowicie zastępuje istniejącej jednostki. Poniższy przykład przedstawia użycie **insertOrMergeEntity**. Jeśli jednostka z `PartitionKey` "tasksSeattle" i `RowKey` "1" nie istnieje, zostanie on włożony. Jednak jeśli wcześniej wstawione (jak pokazano w powyższym przykładzie), `DueDate` zostanie zaktualizowana i `Status` właściwości zostanie dodany. `Description` i `Location` są również zaktualizować właściwości, ale z wartościami które skutecznie pozostaw je bez zmian. Jeśli te dwie właściwości to drugie nie zostały dodane, jak pokazano w przykładzie, ale znajdował się na obiekcie docelowym, ich istniejące wartości pozostanie bez zmian.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Pobieranie pojedynczej jednostki
**TableRestProxy -> getEntity** metoda pozwala na pobranie jednej jednostki przez wykonanie zapytania dotyczącego jego `PartitionKey` i `RowKey`. W przykładzie poniżej klucz partycji `tasksSeattle` i klucz wiersza `1` są przekazywane do **getEntity** metody.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Pobieranie wszystkich jednostek w partycji
Zapytania jednostki są konstruowane przy użyciu filtrów (Aby uzyskać więcej informacji, zobacz [badania tabel i jednostek][filters]). Aby uzyskać dostęp do wszystkich jednostek w partycji, użyj filtru "PartitionKey eq *nazwa_partycji*". Poniższy przykład przedstawia sposób pobieranie wszystkich jednostek w `tasksSeattle` partycji, przekazując filtr, aby **queryEntities** metody.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Pobrać podzbiór jednostek w partycji
Tego samego wzorca użyty w poprzednim przykładzie może służyć do pobierania dowolny podzbiór jednostek w partycji. Podzbiór pobierania jednostek są określane przez filtr używasz (Aby uzyskać więcej informacji, zobacz [badania tabel i jednostek][filters]). Poniższy przykład przedstawia sposób Użyj filtru w celu pobrania wszystkich jednostek z określonym `Location` i `DueDate` mniej niż określona data.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Pobrać podzbioru właściwości jednostki
Zapytania można pobrać podzbioru właściwości jednostki. Ta technika, zwana *projekcji*, redukuje przepustowość i może poprawiać wydajność zapytań, zwłaszcza w przypadku dużych jednostek. Aby określić właściwości mają zostać pobrane, należy przekazać nazwę właściwości do **zapytania -> addSelectField** metody. Tę metodę można wywołać wielokrotnie można dodać więcej właściwości. Po wykonaniu **TableRestProxy -> queryEntities**, zwrócone jednostki będzie miał tylko wybranych właściwości. (Jeśli chcesz przywrócić podzbiór jednostek tabeli, jak pokazano powyżej zapytań Użyj filtru.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Aktualizuj jednostkę
Istniejącej jednostki można aktualizować za pomocą **jednostki -> setProperty** i **jednostki -> addProperty** w jednostki, a następnie wywołania metod **TableRestProxy -> updateEntity**. Poniższy przykład pobiera jednostki, modyfikuje jedną właściwość usuwa innej właściwości i dodaje nową właściwość. Usunięcie właściwości ustawiając wartość **null**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Usuwanie jednostki
Aby usunąć jednostkę, przekaż nazwę tabeli i jednostki `PartitionKey` i `RowKey` do **TableRestProxy -> deleteEntity** metody.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Należy pamiętać, że kontrolach współbieżności, można ustawić Etag obiektu do usunięcia za pomocą **DeleteEntityOptions -> setEtag** — metoda i przekazywanie **DeleteEntityOptions** do obiektu **deleteEntity** jako czwartego parametru.

## <a name="batch-table-operations"></a>Operacje tabeli partii
**TableRestProxy -> partii** metoda umożliwia wykonywanie wielu operacji w ramach pojedynczego żądania. Wzorzec tutaj obejmuje dodawanie działań do **BatchRequest** obiektu, a następnie przekazywanie **BatchRequest** do obiektu **TableRestProxy -> partii** metody. Aby dodać operację do **BatchRequest** obiektu, można wywołać żadnego z następujących metod wiele razy:

* **addInsertEntity** (dodaje operację insertEntity)
* **addUpdateEntity** (dodaje operację updateEntity)
* **addMergeEntity** (dodaje operacji mergeEntity)
* **addInsertOrReplaceEntity** (dodaje operację insertOrReplaceEntity)
* **addInsertOrMergeEntity** (dodaje operację insertOrMergeEntity)
* **addDeleteEntity** (dodaje operacji deleteEntity)

Poniższy przykład przedstawia sposób wykonania **insertEntity** i **deleteEntity** operacji w ramach pojedynczego żądania:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Aby uzyskać więcej informacji na temat przetwarzania wsadowego operacje tabeli, zobacz [wykonywanie transakcji grupy jednostek][entity-group-transactions].

## <a name="delete-a-table"></a>Usuwanie tabeli
Na koniec można usunąć tabeli, Przekaż nazwy tabeli, aby **TableRestProxy -> deleteTable** metody.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Kolejne kroki
Teraz, kiedy znasz już podstawy usługi Azure tabel, skorzystaj z poniższych linków, aby dowiedzieć się więcej o bardziej skomplikowanych zadaniach magazynu.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.

* [Centrum deweloperów języka PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
