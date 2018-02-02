---
title: "Jak używać magazynu tabel platformy Azure z Ruby | Dokumentacja firmy Microsoft"
description: "Przechowywanie danych strukturalnych w chmurze za pomocą Magazynu tabel Azure, magazyn danych NoSQL."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: decc6ffb38a4358d3593642f9cedb59d08f6bfef
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-use-azure-table-storage-with-ruby"></a>Jak używać magazynu tabel Azure z Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Przegląd
W tym przewodniku przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi tabel Azure. Przykłady są napisane przy użyciu interfejsu API Ruby. Omówione scenariusze obejmują **tworzenie i usuwanie tabeli, wstawianie i badania jednostek w tabeli**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji Ruby
Aby uzyskać instrukcje tworzenia aplikacji dopisków fonetycznych, zobacz [dopisków fonetycznych w aplikacji sieci Web szyny na maszynie Wirtualnej platformy Azure](../virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji dostęp do magazynu
Aby korzystać z usługi Azure Storage, konieczne pobranie i użycie dopisków fonetycznych azure pakiet, który zawiera zestaw wygody bibliotek, które komunikują się z usługami REST magazynu.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **terminali** (Mac), lub **Bash** (Unix).
2. Typ **azure instalacji gem** w oknie wiersza polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu
Użyj edytora tekstu, Dodaj następujący element do góry pliku dopisków fonetycznych, których zamierzasz używać magazynu:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia z magazynem Azure
Moduł azure odczyta zmiennych środowiskowych **AZURE\_MAGAZYNU\_konta** i **AZURE\_MAGAZYNU\_dostępu\_klucza**informacje wymagane do łączenia się z kontem magazynu platformy Azure. Jeśli te zmienne środowiskowe nie są skonfigurowane, należy określić informacje o koncie przed użyciem **Azure::TableService** następującym kodem:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

Aby uzyskać te wartości z klasyczny lub Menedżera zasobów konta magazynu w portalu Azure:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Przejdź do konta magazynu, którego chcesz użyć.
3. W bloku ustawienia po prawej stronie, kliknij przycisk **klucze dostępu**.
4. W bloku klucze dostępu pojawia się zostanie wyświetlony klucz dostępu 1 i klucz dostępu 2. Można użyć jednego z tych.
5. Kliknij ikonę kopiowania, aby skopiować klucz do Schowka.

## <a name="create-a-table"></a>Tworzenie tabeli
**Azure::TableService** obiektu umożliwia pracę z tabel i jednostek. Aby utworzyć tabelę, użyj **utworzyć\_table()** metody. Poniższy przykład tworzy tabelę lub drukuje ten błąd, jeśli istnieje.

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli
Aby dodać jednostkę, należy najpierw utworzyć obiektu skrótu, który definiuje właściwości jednostki. Należy zauważyć, że dla każdej jednostki można określić **PartitionKey** i **RowKey**. Są to unikatowe identyfikatory jednostek które są wartości, które można wykonać zapytania znacznie szybciej niż inne właściwości. Usługa Azure Storage korzysta **PartitionKey** to automatyczną dystrybucję jednostek tabeli przez wiele węzłów magazynu. Jednostek o takim samym **PartitionKey** są przechowywane w tym samym węźle. **RowKey** jest unikatowy identyfikator w partycji, należy do jednostki.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Aktualizuj jednostkę
Dostępnych jest kilka metod do zaktualizowania istniejącej jednostki:

* **Zaktualizuj\_entity():** zaktualizowania istniejącej jednostki poprzez zastąpienie jej.
* **Scalanie\_entity():** aktualizuje istniejącą jednostkę przez scalenie nowej wartości właściwości istniejącej jednostki.
* **Wstaw\_lub\_scalania\_entity():** aktualizacji przez zastąpienie istniejącej jednostki. Jeśli jednostka nie istnieje, zostanie wstawiony nowy:
* **Wstaw\_lub\_Zastąp\_entity():** aktualizuje istniejącą jednostkę przez scalenie nowej wartości właściwości istniejącej jednostki. Jeśli jednostka nie istnieje, zostanie wstawiony nowy.

W poniższym przykładzie pokazano, aktualizowanie jednostki przy użyciu **aktualizacji\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

Z **aktualizacji\_entity()** i **scalania\_entity()**, jeśli jednostka, która aktualizujesz nie istnieje, operacja aktualizacji zakończy się niepowodzeniem. W związku z tym jeśli chcesz przechowywać jednostki niezależnie od tego, czy już istnieje, należy zamiast tego użyć **Wstaw\_lub\_Zastąp\_entity()** lub **Wstaw\_lub \_scalania\_entity()**.

## <a name="work-with-groups-of-entities"></a>Praca z grupami jednostek
Czasami warto przesłać wiele operacji ze sobą w partii zapewnienie atomic przetwarzania przez serwer. Celu, który należy najpierw utworzyć **partii** obiekt, a następnie użyj **wykonania\_batch()** metoda **TableService**. W poniższym przykładzie pokazano, przesyłanie dwie jednostki z RowKey 2 i 3 w partii. Należy zauważyć, że działa ona tylko dla jednostek o tej samej PartitionKey.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Zapytanie dla jednostki
Aby sprawdzić jednostkę w tabeli, użyj **uzyskać\_entity()** metody, przekazując nazwy tabeli **PartitionKey** i **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Zapytanie zestawu jednostek
Aby odpytać zestawu jednostek w tabeli, utworzyć obiektu skrótu zapytania i użyj **zapytania\_entities()** metody. W poniższym przykładzie pokazano pobieranie wszystkich jednostek o takim samym **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Jeśli zestaw wyników jest za duża dla pojedynczego zapytania do zwrócenia, token kontynuacji zostaną zwrócone służącego do pobierania stron sieci Web.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Tworzenie zapytania do podzbioru właściwości jednostki
Zapytanie do tabeli może pobrać kilka właściwości jednostki. Ta technika, zwana "projekcji", redukuje przepustowość i może poprawiać wydajność zapytań, zwłaszcza w przypadku dużych jednostek. Użyj klauzuli select i przekazywać nazwy właściwości, które chcesz przełączyć za pośrednictwem do klienta.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Usuwanie jednostki
Aby usunąć jednostkę, użyj **usunąć\_entity()** metody. Należy przekazać nazwy tabeli, która zawiera obiekt PartitionKey i RowKey jednostki.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Usuwanie tabeli
Aby usunąć tabelę, użyj **usunąć\_table()** — metoda i przekazać nazwy tabeli, które chcesz usunąć.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Kolejne kroki

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.
* [Zestaw Azure SDK dla środowiska Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repozytorium w witrynie GitHub

