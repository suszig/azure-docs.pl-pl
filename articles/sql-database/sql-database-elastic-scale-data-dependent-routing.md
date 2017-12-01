---
title: "Dane zależne routingu w usłudze Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Sposób użycia klasy ShardMapManager w aplikacjach .NET dla danych zależne od routingu, funkcja podzielonej baz danych w bazie danych SQL Azure"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: ddove
ms.openlocfilehash: ef88d6072cfa95842703c31ec8e95ff4664f91ff
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="data-dependent-routing"></a>Routing zależny od danych
**Dane zależne routingu** jest możliwość użycia danych w zapytaniu można przekierować żądania do odpowiedniej bazy danych. Podczas pracy z bazy danych podzielonej jest wzorzec podstawowych. Kontekst żądania może również trasy żądania, zwłaszcza, jeśli klucz dzielenia na fragmenty nie jest częścią zapytania. Każdej określonej kwerendy lub transakcji w aplikacji przy użyciu routingu zależnych danych jest ograniczone do uzyskiwania dostępu do pojedynczej bazy danych na żądanie. Dla narzędzi elastycznej bazy danych SQL Azure to routingu jest realizowane za pomocą **ShardMapManager** ([.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), [Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager)) klasy.

Aplikacji nie musi śledzić różne parametry połączenia lub skojarzone z inną wycinków danych w środowisku podzielonej lokalizacji bazy danych. Zamiast tego [Menedżera Map niezależnego fragmentu](sql-database-elastic-scale-shard-map-management.md) otwiera połączenia z poprawną baz danych, w razie potrzeby na podstawie danych w planie niezależnego fragmentu i wartość klucza dzielenia na fragmenty, który jest miejscem docelowym żądania aplikacji. Klucz jest zwykle *customer_id*, *tenant_id*, *date_key*, lub niektóre określonym identyfikatorze, który jest parametrem podstawowym żądania bazy danych). 

Aby uzyskać więcej informacji, zobacz [skalowanie limit programu SQL Server z routingiem zależnych danych](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Pobierz biblioteki klienta
Aby pobrać:
* .NET w wersji biblioteki, zobacz [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).
* wersja języka Java biblioteki, zobacz [Maven centralnym repozytorium](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Przy użyciu ShardMapManager w danych zależnych aplikacji routingu
Aplikacje powinny wystąpienia **ShardMapManager** podczas inicjowania przy użyciu wywołania fabryki **GetSQLShardMapManager** ([.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx), [Java ](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.getsqlshardmapmanager)). W tym przykładzie zarówno **ShardMapManager** i określony **ShardMap** zawierający są zainicjowane. W tym przykładzie pokazano GetSqlShardMapManager i GetRangeShardMap ([.NET](https://msdn.microsoft.com/library/azure/dn824173.aspx), [Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getrangeshardmap)) metod.

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Użyj najniższy poświadczeń uprawnień możliwe do uzyskania mapy niezależnego fragmentu
Jeśli aplikacja nie jest manipulowanie mapy niezależnego fragmentu, poświadczenia używane w metodzie fabryki powinni mieć uprawnienia tylko do odczytu **globalne mapy niezależnego fragmentu** bazy danych. Te poświadczenia różnią się zwykle z poświadczeniami używanymi do otwarcia połączenia z menedżerem mapy niezależnego fragmentu. Zobacz też [poświadczenia umożliwiające dostęp do biblioteki klienta elastycznej bazy danych](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>Wywołaj metodę OpenConnectionForKey
**Metody ShardMap.OpenConnectionForKey** ([.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx), [Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._list_shard_mapper.openconnectionforkey)) zwraca gotowe do wydania poleceń odpowiednią bazę danych na podstawie wartości z połączeniem **klucza** parametru. Identyfikator niezależnego fragmentu informacje są buforowane w aplikacji przez **ShardMapManager**, więc te żądania nie obejmują zazwyczaj wyszukiwania w bazie danych przed **globalne mapy niezależnego fragmentu** bazy danych. 

```csharp
// Syntax: 
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```

* **Klucza** parametr jest używany jako klucz wyszukiwania w planie niezależnego fragmentu ustalenie odpowiedniej bazy danych dla żądania. 
* **ConnectionString** używany do przekazywania tylko poświadczenia użytkownika dla żądanego połączenia. Nie nazwy bazy danych lub nazwa serwera znajdują się w tym *connectionString* ponieważ określają metodę bazy danych i serwera przy użyciu **ShardMap**. 
* **ConnectionOptions** ([.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx), [Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._connection_options)) powinien być ustawiony na **ConnectionOptions.Validate** Jeśli środowisku, gdzie niezależnego fragmentu mapuje maja zmiany i wiersze mogą przenosić do innych baz danych w wyniku operacji dzielenie i scalanie. Obejmuje to krótkie zapytania do mapy niezależnych lokalnych w celu bazy danych (nie do mapy niezależnych globalne) przed połączenia jest dostarczany do aplikacji. 

W przypadku niepowodzenia weryfikacji względem na mapie lokalnych niezależnego fragmentu (co oznacza, że pamięć podręczna jest nieprawidłowe) Manager mapy niezależnego fragmentu będzie zapytania mapy globalne niezależnych uzyskania nowego poprawną wartość do wyszukiwania, zaktualizuj pamięć podręczną i uzyskać i zwracać połączenie z odpowiednią bazą danych. 

Użyj **ConnectionOptions.None** tylko podczas zmiany mapowania niezależnego fragmentu nie powinny podczas, gdy aplikacja jest w trybie online. W takim przypadku buforowane wartości można założyć, że zawsze być poprawne, a następnie bezpiecznie pominięte wywołania bardzo obustronne weryfikacji do docelowej bazy danych. Która zmniejsza ruch bazy danych. **ConnectionOptions** również mogą zostać ustawione za pomocą wartości, w pliku konfiguracji, aby wskazać, czy zmiany dzielenia na fragmenty są oczekiwane lub nie w okresie czasu.  

W tym przykładzie użyto wartości klucza **CustomerID**za pomocą **ShardMap** obiektu o nazwie **customerShardMap**.  

```csharp
int customerId = 12345; 
int newPersonId = 4321; 

// Connect to the shard for that customer ID. No need to call a SqlConnection 
// constructor followed by the Open method.
using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
{ 
    // Execute a simple command. 
    SqlCommand cmd = conn.CreateCommand(); 
    cmd.CommandText = @"UPDATE Sales.Customer 
                        SET PersonID = @newPersonID WHERE CustomerID = @customerID"; 

    cmd.Parameters.AddWithValue("@customerID", customerId);cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
    cmd.ExecuteNonQuery(); 
}  
```

**OpenConnectionForKey** metoda zwraca nowe połączenie już otwarty z poprawną bazą danych. Połączenia używane w ten sposób nadal w pełni korzystać z puli połączeń. 

**Metody OpenConnectionForKeyAsync** ([.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx), [Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._list_shard_mapper.openconnectionforkeyasync)) jest również dostępna w przypadku aplikacji sprawia, że użycie programowanie asynchroniczne z ADO.Net.

## <a name="integrating-with-transient-fault-handling"></a>Integrowanie z obsługi błędu przejściowego
Upewnij się, że błędów przejściowych są przechwytywane przez aplikację i operacje są zwalniane kilka razy przed generowaniem błędu jest najlepszym rozwiązaniem w tworzeniu aplikacji dostęp do danych w chmurze. Wystąpienia błędu przejściowego obsługi dla aplikacji w chmurze została szczegółowo opisana w obsługi błędów przejściowych ([.NET](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx), [Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling)). 

Obsługa błędu przejściowego mogą współistnieć naturalnie z wzorcem danych zależnych routingu. Decydujące znaczenie jest, aby ponowić próbę, w tym żądania dostępu do danych **przy użyciu** bloku uzyskiwanej połączenia routingu zależne od danych. W powyższym przykładzie można ponownego napisania w następujący sposób (Uwaga wyróżnione zmiany). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Przykład — danych zależnych routingu z obsługą błędu przejściowego
```csharp
int customerId = 12345; 
int newPersonId = 4321; 

Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; 
{
    // Connect to the shard for a customer ID. 
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command 
        SqlCommand cmd = conn.CreateCommand(); 

        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 

        Console.WriteLine("Update completed"); 
    } 
}); 
```


Pakiety, które należy wykonać, Obsługa błędu przejściowego są pobierane automatycznie podczas tworzenia aplikacji przykładowej elastycznej bazy danych. 

## <a name="transactional-consistency"></a>Spójności transakcyjnej
Transakcyjne właściwości dotrą do wszystkich operacji lokalne niezależnego fragmentu. Na przykład transakcji przesłane za pośrednictwem routingu zależne od danych wykonywania w zakresie niezależnych docelowy dla połączenia. W tej chwili jest nie możliwości przewidzianych rejestrowanie wiele połączeń w transakcji, a w związku z tym nie bez gwarancji transakcyjne dla operacji wykonywanych przez odłamków.

## <a name="next-steps"></a>Następne kroki
Aby odłączyć niezależnych lub podłącz niezależnego fragmentu, zobacz [za pomocą klasy RecoveryManager rozwiązywania problemów mapy niezależnego fragmentu](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

