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
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 2246dd12b922fcbc2e2b58890b3d56253810849c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="data-dependent-routing"></a>Routing zależny od danych
**Dane zależne routingu** jest możliwość użycia danych w zapytaniu można przekierować żądania do odpowiedniej bazy danych. Podczas pracy z bazy danych podzielonej jest wzorzec podstawowych. Kontekst żądania może również trasy żądania, zwłaszcza, jeśli klucz dzielenia na fragmenty nie jest częścią zapytania. Każdej określonej kwerendy lub transakcji w aplikacji przy użyciu routingu zależnych danych jest ograniczone do uzyskiwania dostępu do pojedynczej bazy danych na żądanie. Dla narzędzi elastycznej bazy danych SQL Azure to routingu jest realizowane za pomocą  **[klasy ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  w aplikacjach ADO.NET.

Aplikacji nie musi śledzić różne parametry połączenia lub skojarzone z inną wycinków danych w środowisku podzielonej lokalizacji bazy danych. Zamiast tego [Menedżera Map niezależnego fragmentu](sql-database-elastic-scale-shard-map-management.md) otwiera połączenia z poprawną baz danych, w razie potrzeby na podstawie danych w planie niezależnego fragmentu i wartość klucza dzielenia na fragmenty, który jest miejscem docelowym żądania aplikacji. Klucz jest zwykle *customer_id*, *tenant_id*, *date_key*, lub niektóre określonym identyfikatorze, który jest parametrem podstawowym żądania bazy danych). 

Aby uzyskać więcej informacji, zobacz [skalowanie limit programu SQL Server z routingiem zależnych danych](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Pobierz biblioteki klienta
Aby uzyskać klasy, należy zainstalować [elastycznej bazy danych klienta biblioteki](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Przy użyciu ShardMapManager w danych zależnych aplikacji routingu
Aplikacje powinny wystąpienia **ShardMapManager** podczas inicjowania przy użyciu wywołania fabryki  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. W tym przykładzie zarówno **ShardMapManager** i określony **ShardMap** zawierający są zainicjowane. W tym przykładzie pokazano GetSqlShardMapManager i [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metody.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Użyj najniższy poświadczeń uprawnień możliwe do uzyskania mapy niezależnego fragmentu
Jeśli aplikacja nie jest manipulowanie mapy niezależnego fragmentu, poświadczenia używane w metodzie fabryki powinni mieć uprawnienia tylko do odczytu **globalne mapy niezależnego fragmentu** bazy danych. Te poświadczenia różnią się zwykle z poświadczeniami używanymi do otwarcia połączenia z menedżerem mapy niezależnego fragmentu. Zobacz też [poświadczenia umożliwiające dostęp do biblioteki klienta elastycznej bazy danych](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>Wywołaj metodę OpenConnectionForKey
**[Metody ShardMap.OpenConnectionForKey](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  zwraca gotowe do wydania poleceń odpowiednią bazę danych na podstawie wartości z połączenia ADO.Net **klucza** parametru. Identyfikator niezależnego fragmentu informacje są buforowane w aplikacji przez **ShardMapManager**, więc te żądania nie obejmują zazwyczaj wyszukiwania w bazie danych przed **globalne mapy niezależnego fragmentu** bazy danych. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* **Klucza** parametr jest używany jako klucz wyszukiwania w planie niezależnego fragmentu ustalenie odpowiedniej bazy danych dla żądania. 
* **ConnectionString** używany do przekazywania tylko poświadczenia użytkownika dla żądanego połączenia. Nie nazwy bazy danych lub nazwa serwera znajdują się w tym *connectionString* ponieważ określają metodę bazy danych i serwera przy użyciu **ShardMap**. 
* **[ConnectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  powinien być ustawiony na **ConnectionOptions.Validate** Jeśli środowisku, gdzie mapuje niezależnego fragmentu mogą ulec zmianie, a wiersze mogą przenosić do innych baz danych w wyniku operacji dzielenie i scalanie. Obejmuje to krótkie zapytania do mapy niezależnych lokalnych w celu bazy danych (nie do mapy niezależnych globalne) przed połączenia jest dostarczany do aplikacji. 

W przypadku niepowodzenia weryfikacji względem na mapie lokalnych niezależnego fragmentu (co oznacza, że pamięć podręczna jest nieprawidłowe) Manager mapy niezależnego fragmentu będzie zapytania mapy globalne niezależnych uzyskania nowego poprawną wartość do wyszukiwania, zaktualizuj pamięć podręczną i uzyskać i zwracać połączenie z odpowiednią bazą danych. 

Użyj **ConnectionOptions.None** tylko podczas zmiany mapowania niezależnego fragmentu nie powinny podczas, gdy aplikacja jest w trybie online. W takim przypadku buforowane wartości można założyć, że zawsze być poprawne, a następnie bezpiecznie pominięte wywołania bardzo obustronne weryfikacji do docelowej bazy danych. Która zmniejsza ruch bazy danych. **ConnectionOptions** również mogą zostać ustawione za pomocą wartości, w pliku konfiguracji, aby wskazać, czy zmiany dzielenia na fragmenty są oczekiwane lub nie w okresie czasu.  

W tym przykładzie użyto wartości klucza **CustomerID**za pomocą **ShardMap** obiektu o nazwie **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

**OpenConnectionForKey** metoda zwraca nowe połączenie już otwarty z poprawną bazą danych. Połączenia używane w ten sposób nadal w pełni korzystać z buforowania połączenia ADO.Net. Ile transakcji i żądań można spełnić przez jednego niezależnego fragmentu w czasie, powinno to być modyfikacja tylko niezbędne w aplikacji już za pomocą ADO.Net. 

**[Metody OpenConnectionForKeyAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  jest również dostępna w przypadku aplikacji sprawia, że użycie programowanie asynchroniczne z ADO.Net. Jego zachowanie jest zależne od, routing odpowiednik ADO danych. W sieci  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metody.

## <a name="integrating-with-transient-fault-handling"></a>Integrowanie z obsługi błędu przejściowego
Upewnij się, że błędów przejściowych są przechwytywane przez aplikację i operacje są zwalniane kilka razy przed generowaniem błędu jest najlepszym rozwiązaniem w tworzeniu aplikacji dostęp do danych w chmurze. Wystąpienia błędu przejściowego obsługi dla aplikacji w chmurze została szczegółowo opisana w [obsługi błędów przejściowych](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Obsługa błędu przejściowego mogą współistnieć naturalnie z wzorcem danych zależnych routingu. Decydujące znaczenie jest, aby ponowić próbę, w tym żądania dostępu do danych **przy użyciu** bloku uzyskiwanej połączenia routingu zależne od danych. W powyższym przykładzie można ponownego napisania w następujący sposób (Uwaga wyróżnione zmiany). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Przykład — danych zależnych routingu z obsługą błędu przejściowego
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Pakiety, które należy wykonać, Obsługa błędu przejściowego są pobierane automatycznie podczas tworzenia aplikacji przykładowej elastycznej bazy danych. Pakiety są także dostępne oddzielnie w [Enterprise Library – bloku aplikacji obsługi błędów przejściowych](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Użyj wersji 6.0 lub nowszej. 

## <a name="transactional-consistency"></a>Spójności transakcyjnej
Transakcyjne właściwości dotrą do wszystkich operacji lokalne niezależnego fragmentu. Na przykład transakcji przesłane za pośrednictwem routingu zależne od danych wykonywania w zakresie niezależnych docelowy dla połączenia. W tej chwili jest nie możliwości przewidzianych rejestrowanie wiele połączeń w transakcji, a w związku z tym nie bez gwarancji transakcyjne dla operacji wykonywanych przez odłamków.

## <a name="next-steps"></a>Następne kroki
Aby odłączyć niezależnych lub podłącz niezależnego fragmentu, zobacz [za pomocą klasy RecoveryManager rozwiązywania problemów mapy niezależnego fragmentu](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

