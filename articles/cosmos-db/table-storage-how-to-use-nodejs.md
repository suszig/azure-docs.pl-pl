---
title: "Jak używać magazynu tabel Azure w oprogramowaniu Node.js | Dokumentacja firmy Microsoft"
description: "Przechowywanie danych strukturalnych w chmurze za pomocą Magazynu tabel Azure, magazyn danych NoSQL."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 03cd5088b55e37b2ada78e0f531702705d45a4c0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Jak używać magazynu tabel Azure w oprogramowaniu Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Omówienie
W tym temacie przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi Azure tabel w aplikacji Node.js.

Przykłady kodu, w tym temacie założono, że masz już aplikację Node.js. Aby uzyskać informacje o sposobie tworzenia aplikacji Node.js na platformie Azure, zobacz Tematy te:

* [Tworzenie aplikacji sieci web Node.js w usłudze Azure App Service](../app-service/app-service-web-get-started-nodejs.md)
* [Tworzenie i wdrażanie aplikacji Node.js do usługi w chmurze platformy Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (przy użyciu programu Windows PowerShell)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurowanie aplikacji dostęp do magazynu Azure
Korzystanie z usługi Azure Storage, wymaga zestawu SDK usługi Magazyn Azure dla środowiska Node.js, w tym zestaw wygody bibliotek, które komunikują się z magazynu usługi REST.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Należy zainstalować pakiet węzeł Menedżera pakietów (NPM)
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **Terminal** (Mac), lub **Bash** (Unix) i przejdź do folderu, w którym utworzono aplikację.
2. Typ **magazyn azure instalacji narzędzia npm** w oknie wiersza polecenia. Dane wyjściowe polecenia jest podobny do poniższego przykładu.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Możesz ręcznie uruchomić **ls** polecenie, aby sprawdzić, czy **węzła\_modułów** folder został utworzony. Wewnątrz tego folderu znajdują się **magazyn azure** pakiet, który zawiera biblioteki muszą uzyskać dostęp do magazynu.

### <a name="import-the-package"></a>Importowanie pakietu
Dodaj następujący kod do góry **server.js** plik w aplikacji:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Moduł azure odczyta zmiennych środowiskowych AZURE\_MAGAZYNU\_konto i AZURE\_MAGAZYNU\_dostępu\_klucz lub AZURE\_MAGAZYNU\_połączenia\_ciąg informacje wymagane do łączenia się z kontem magazynu platformy Azure. Jeśli te zmienne środowiskowe nie są skonfigurowane, należy określić informacje o koncie podczas wywoływania metody **TableService**.

## <a name="create-a-table"></a>Tworzenie tabeli
Poniższy kod tworzy **TableService** obiektu i używa jej do utworzenia nowej tabeli. Dodaj następujący kod w górnej części **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

Wywołanie **createTableIfNotExists** spowoduje utworzenie nowej tabeli o określonej nazwie, jeśli jeszcze nie istnieje. Poniższy przykład tworzy nową tabelę o nazwie "mytable", jeśli jeszcze nie istnieje:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

`result.created` Będzie `true` Jeśli nowa tabela została utworzona, oraz `false` Jeśli tabela już istnieje. `response` Będzie zawierać informacje o żądaniu.

### <a name="filters"></a>Filtry
Opcjonalne operacjach filtrowania może odnosić się do operacji wykonywanych przy użyciu **TableService**. Filtrowanie operacje mogą obejmować rejestrowania, Automatyczne ponawianie próby itp. Obiekty, które implementują metodę o sygnaturze są następujące filtry:

```nodejs
function handle (requestOptions, next)
```

Po wykonaniu przetwarzanie wstępne opcje żądania, metoda musi wywołać "dalej", przekazywanie wywołania zwrotnego z następującą sygnaturą:

```nodejs
function (returnObject, finalCallback, next)
```

W tym wywołania zwrotnego, a po przetworzeniu returnObject (odpowiedź z żądania do serwera) wywołania zwrotnego musi wywołać obok, jeśli istnieje kontynuować przetwarzanie inne filtry lub po prostu Wywołaj finalCallback inaczej na koniec wywołania usługi.

Dwa filtry, które implementują logikę ponawiania wchodzą w skład zestawu Azure SDK dla środowiska Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. Tworzy następujące **TableService** obiekt, który używa **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli
Aby dodać jednostkę, należy najpierw utworzyć obiekt, który definiuje właściwości jednostki. Musi zawierać wszystkie jednostki **PartitionKey** i **RowKey**, które są unikatowe identyfikatory dla jednostki.

* **PartitionKey** — Określa jednostki przechowywanego w partycji
* **RowKey** — unikatowo identyfikuje jednostek w partycji

Zarówno **PartitionKey** i **RowKey** muszą być ciągami. Aby uzyskać więcej informacji, zobacz [opis modelu danych usługi tabel](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Oto przykład Definiowanie jednostki. Należy pamiętać, że **Data ukończenia** jest zdefiniowana jako typ **Edm.DateTime**. Określenie typu jest opcjonalny i typy będzie można wywnioskować, jeśli nie została określona.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Istnieje również **sygnatury czasowej** pola dla każdego rekordu, który jest ustawiony przez platformę Azure, gdy jednostki są wstawiane lub aktualizowane.
>
>

Można również użyć **entityGenerator** do tworzenia jednostek. Poniższy przykład tworzy w tej samej zadań jednostki przy użyciu **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

Aby dodać jednostkę do tabeli, należy przekazać do obiektu jednostki **insertEntity** metody.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Jeśli operacja się powiodła, `result` będzie zawierać [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) wstawionego rekordu i `response` będzie zawierać informacje na temat operacji.

Przykład odpowiedzi:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Domyślnie **insertEntity** nie zwraca jednostek wstawiony jako część `response` informacji. Jeśli planowane jest wykonywanie innych operacji w tej jednostce lub chcesz buforowanie tych informacji mogą być przydatne jest zwracane jako część `result`. Można to zrobić przez włączenie **echoContent** w następujący sposób:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Aktualizuj jednostkę
Dostępnych jest kilka metod do zaktualizowania istniejącej jednostki:

* **replaceEntity** -aktualizacji przez zastąpienie istniejącej jednostki
* **mergeEntity** -aktualizuje istniejącą jednostkę przez scalenie nowej wartości właściwości istniejącej jednostki
* **insertOrReplaceEntity** -aktualizacji przez zastąpienie istniejącej jednostki. Jeśli jednostka nie istnieje, zostanie wstawiony nowy
* **insertOrMergeEntity** — aktualizuje istniejące jednostki przez scalenie nowej wartości właściwości istniejącej. Jeśli jednostka nie istnieje, zostanie wstawiony nowy

W poniższym przykładzie pokazano, aktualizowanie jednostki przy użyciu **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Domyślnie aktualizowania jednostki nie Sprawdź, czy dane aktualizowana wcześniej zostały zmodyfikowane przez inny proces. Do obsługi współbieżnych aktualizacji:
>
> 1. Pobierz element ETag aktualizowany obiekt. Ten błąd jest zwracany jako część `response` do żadnej operacji powiązanych jednostek i mogą zostać pobrane za pośrednictwem `response['.metadata'].etag`.
> 2. Podczas wykonywania operacji update na jednostkę, Dodaj informacje ETag wcześniej pobrane do nowego obiektu. Na przykład:
>
>       entity2 .etag [.metadata] = currentEtag;
> 3. Wykonaj operację aktualizacji. Jeśli jednostka została zmodyfikowana od czasu pobrania wartość ETag, takich jak inne wystąpienie aplikacji, `error` zostaną zwrócone z informacją nie był spełniony warunek aktualizacji określony w żądaniu.
>
>

Z **replaceEntity** i **mergeEntity**, jeśli jednostka, która jest aktualizowana nie istnieje, operacja aktualizacji zakończy się niepowodzeniem. Dlatego jeśli chcesz przechowywać jednostki, niezależnie od tego, czy już istnieje, użyj **insertOrReplaceEntity** lub **insertOrMergeEntity**.

`result` Dla pomyślnej aktualizacji będzie zawierać operacje **Etag** zaktualizowane jednostki.

## <a name="work-with-groups-of-entities"></a>Praca z grupami jednostek
Czasami warto przesłać wiele operacji ze sobą w partii zapewnienie atomic przetwarzania przez serwer. Celu, który należy użyć **TableBatch** klasa do tworzenia partii, a następnie użyj **executeBatch** metody **TableService** w celu wykonania operacji wsadowej.

 W poniższym przykładzie pokazano, przesyłanie dwie jednostki w partii:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

Dla operacji wsadowych pomyślne `result` będzie zawierać informacje dla każdej operacji w partii.

### <a name="work-with-batched-operations"></a>Praca z operacji wsadowych
Dodane do partii operacje mogą być kontrolowane przez wyświetlanie `operations` właściwości. Można także użyć następujących metod do pracy z operacjami:

* **Wyczyść** — usuwa wszystkie operacje z partii
* **getOperations** -pobiera operację z partii
* **hasOperations** — zwraca wartość true, jeśli partia zawiera operacje
* **removeOperations** — usuwa operacji
* **rozmiar** — zwraca liczbę operacji w partii

## <a name="retrieve-an-entity-by-key"></a>Pobrać jednostek według klucza
Aby powrócić do określonej jednostki na podstawie **PartitionKey** i **RowKey**, użyj **retrieveEntity** metody.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

Po zakończeniu tej operacji `result` będzie zawierać jednostki.

## <a name="query-a-set-of-entities"></a>Zapytanie zestawu jednostek
Aby sprawdzić tabelę, użyj **TableQuery** obiekt do zbudowania przy użyciu następujących klauzule wyrażenia zapytania:

* **Wybierz** -pola, które mają być zwracane z kwerendy
* **gdzie** -where — klauzula

  * **i** — `and` warunek where
  * **lub** — `or` warunek where
* **TOP** -liczba elementów do pobrania

Poniższy przykład tworzy kwerendę, która zwraca pierwsze pięć elementy z PartitionKey "hometasks".

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Ponieważ **wybierz** nie jest używany, zostaną zwrócone wszystkie pola. Aby wykonać zapytania dotyczącego tabeli, należy użyć **queryEntities**. W poniższym przykładzie użyto tego zapytania do zwrócenia jednostek z "mytable".

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

W przypadku powodzenia `result.entries` będzie zawierać tablicę jednostki, które pasują do zapytania. Jeśli zapytanie nie może zwracać wszystkie jednostki `result.continuationToken` będzie inną niż*null* i mogą być używane jako trzeci parametr funkcji **queryEntities** można pobrać więcej wyników. Początkowego zapytania można użyć *null* trzeci parametr.

### <a name="query-a-subset-of-entity-properties"></a>Tworzenie zapytania do podzbioru właściwości jednostki
Zapytanie do tabeli może pobrać kilka pól jednostki.
To redukuje przepustowość i może poprawiać wydajność zapytań, zwłaszcza w przypadku dużych jednostek. Użyj **wybierz** klauzuli i przekazywać nazwy pól, które ma zostać zwrócona. Na przykład poniższe zapytanie zwróci tylko **opis** i **Data ukończenia** pola.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Usuwanie jednostki
Można usunąć jednostki przy użyciu jego kluczy partycji i wiersza. W tym przykładzie **task1** zawiera obiekt **RowKey** i **PartitionKey** wartości jednostki do usunięcia. Następnie obiekt został przekazany do **deleteEntity** metody.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Należy rozważyć użycie elementy etag podczas usuwania elementów, aby upewnić się, że element nie została zmodyfikowana przez inny proces. Zobacz [Aktualizuj jednostkę](#update-an-entity) informacji na temat używania elementy ETag.
>
>

## <a name="delete-a-table"></a>Usuwanie tabeli
Poniższy kod usuwa tabelę z konta magazynu.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Jeśli masz pewności, czy tabela istnieje, użyj **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Używaj tokenów kontynuacji
Podczas wykonywania zapytań dotyczących tabel w przypadku dużych ilości wyników, należy wyszukać kontynuacji tokenów. Mogą być duże ilości danych dostępne dla Twojego zapytania, które mogą nie okazuje się, jeśli nie Kompiluj rozpoznanie token kontynuacji jest obecny.

Obiekt wyniki zwrócone podczas wykonywania zapytania zestawów jednostek `continuationToken` właściwości, gdy obecny jest takie tokenu. Następnie można to podczas wykonywania zapytania w dalszym ciągu porusza się na partycji i tabela jednostek.

Podczas wykonywania zapytania, między wystąpieniem obiektu zapytania i funkcja wywołania zwrotnego można podać parametru continuationToken:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Jeśli sprawdzenie `continuationToken` obiektu, można znaleźć właściwości takich jak `nextPartitionKey`, `nextRowKey` i `targetLocation`, które mogą służyć do iterowania po wszystkich wyników.

Istnieje również kontynuacji próbki w repozytorium Node.js magazynu Azure w serwisie GitHub. Wyszukaj `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Praca z sygnatury dostępu współdzielonego
Sygnatury dostępu współdzielonego (SAS) to bezpieczny sposób zapewnienia szczegółowej dostępu do tabel bez podawania Twojej nazwy konta magazynu i klucze. Skojarzenia zabezpieczeń są często używane do udzielany ograniczony dostęp do danych, na przykład pozwala aplikacji mobilnej, aby w rekordach zapytań.

Generuje zaufanych aplikacji, takich jak jest usługą opartą na chmurze przy użyciu sygnatury dostępu Współdzielonego **generateSharedAccessSignature** z **TableService**i udostępnia go do niezaufanych lub częściowo zaufanych aplikacji takich jak aplikacji mobilnej. Sygnatury dostępu Współdzielonego jest generowany przy użyciu zasad, opisujący daty rozpoczęcia i zakończenia, w których sygnatury dostępu Współdzielonego jest prawidłowy, a także poziom dostępu przyznane posiadacz sygnatury dostępu Współdzielonego.

Poniższy przykład generuje nowe zasady dostępu współdzielonego, które umożliwią SAS właścicielowi zapytania ("r") tabeli i wygasa 100 minut po jego utworzeniu.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Należy pamiętać, że informacje hosta należy podać również, jak jest wymagany, gdy właściciel SAS próbuje uzyskać dostęp w tabeli.

Następnie aplikacja kliencka używa SAS z **TableServiceWithSAS** wykonywanie operacji względem tabeli. Poniższy przykład łączy do tabeli i wykonuje zapytania.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

Ponieważ sygnatury dostępu Współdzielonego został wygenerowany z zapytania dostęp tylko, jeśli zostały podjęta próba Wstawianie, aktualizowanie lub usuwanie jednostek, czy zwracany błąd.

### <a name="access-control-lists"></a>Listy kontroli dostępu
Listy kontroli dostępu (ACL) umożliwia także ustawić zasady dostępu dla sygnatury dostępu Współdzielonego. Jest to przydatne, jeśli chcesz umożliwić wielu klientów dostępu do tabeli, ale zawiera różne zasady dostępu dla każdego klienta.

Listy ACL jest zaimplementowana przy użyciu tablicy zasad dostępu w usłudze identyfikator skojarzony z każdej zasady. W poniższym przykładzie zdefiniowano dwie zasady, jeden dla "Użytkownik1" i jeden dla "uzytkownik2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Poniższy przykład pobiera bieżącej listy ACL dla **hometasks** tabeli, a następnie dodanie nowych zasad za pomocą **setTableAcl**. Takie podejście umożliwia:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Po ustawieniu listy ACL następnie można utworzyć na podstawie Identyfikatora zasady sygnatury dostępu Współdzielonego. Poniższy przykład tworzy nowe sygnatury dostępu Współdzielonego dla "uzytkownik2":

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji zobacz następujące zasoby.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.
* [Zestaw Azure SDK magazynu dla węzła](https://github.com/Azure/azure-storage-node) repozytorium w witrynie GitHub.
* [Centrum deweloperów środowiska Node.js](/develop/nodejs/)
* [Tworzenie i wdrażanie aplikacji Node.js do witryny sieci Web platformy Azure](../app-service/app-service-web-get-started-nodejs.md)