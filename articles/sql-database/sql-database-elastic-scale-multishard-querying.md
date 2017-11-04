---
title: Zapytanie bazy danych Azure SQL podzielonej | Dokumentacja firmy Microsoft
description: "Uruchamianie zapytań między odłamków za pomocą biblioteki klienta elastycznej bazy danych."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: b4827fafdfd2f8b094c4f541a7511d6233dd4e6f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="multi-shard-querying"></a>Wiele niezależnych zapytań
## <a name="overview"></a>Omówienie
Z [narzędzi elastycznej bazy danych](sql-database-elastic-scale-introduction.md), można utworzyć bazy danych podzielonej rozwiązania. **Wiele niezależnych badania** służy do zadań, takich jak kolekcja/raportowania danych wymagających uruchomienia zapytania, który rozciąga się na kilka fragmentów. (Natomiast aby [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md), które wykonuje całą pracę na jednego niezależnego fragmentu.) 

1. Pobierz [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) lub [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) przy użyciu [ **TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), lub [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metody. Zobacz [ **konstruowania ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) i [ **uzyskać RangeShardMap lub ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Utwórz  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  obiektu.
3. Utwórz  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Ustaw  **[Właściwość CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  polecenia T-SQL.
5. Wykonaj polecenie wywołując  **[metodę ExecuteReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Wyświetl wyniki za pomocą  **[klasy MultiShardDataReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Przykład
Poniższy kod przedstawia użycie wielu niezależnych zapytań za pomocą danego **ShardMap** o nazwie *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                 } 
           } 
    } 


Najważniejsza różnica polega na konstrukcji niezależnych wielu połączeń. Gdzie **SqlConnection** działa na pojedynczej bazy danych **MultiShardConnection** przyjmuje ***kolekcji odłamków*** jako dane wejściowe. Wypełnianie kolekcji odłamków z mapy niezależnego fragmentu. Następnie wykonać zapytania w kolekcji przy użyciu fragmentów **UNION ALL** semantyki można złożyć jeden wynik ogólny. Opcjonalnie można dodać nazwę niezależnego fragmentu, z której pochodzi wiersz do danych wyjściowych przy użyciu **ExecutionOptions** właściwość polecenia. 

Należy pamiętać, wywołanie **myShardMap.GetShards()**. Ta metoda pobiera wszystkie odłamków z mapy niezależnego fragmentu i zapewnia prosty sposób do uruchamiania kwerendy dla wszystkich odpowiednich baz danych. Kolekcji odłamków dla wielu niezależnych zapytanie może być precyzyjnych dalsze przez wykonanie zapytania LINQ w kolekcji zwróconej z wywołania do **myShardMap.GetShards()**. W połączeniu z zasadami wyniki częściowe bieżących możliwości wielu niezależnych badania został zaprojektowany do pracy oraz dziesiątki maksymalnie setki fragmentów.

To ograniczenie z wielu niezależnych zapytań jest obecnie braku weryfikacji odłamków i shardlets, które będą pytani. Podczas routingu zależne od danych sprawdza, czy dany identyfikator niezależnego fragmentu mapy niezależnego fragmentu w czasie wykonywania zapytania, zapytania wielu niezależnego fragmentu nie wykonuj tego wyboru. Może to prowadzić do zapytań wielu niezależnych wykonywanych w bazach danych, które zostały usunięte z mapy niezależnego fragmentu.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Wiele niezależnych zapytań i operacji scalania podziału
Wiele niezależnych zapytania nie weryfikują czy shardlets w bazie danych, którego dotyczy kwerenda uczestniczą w trwających operacji scalania podziału. (Zobacz [odbierającej za pomocą narzędzia do scalania podziału elastycznej bazy danych](sql-database-elastic-scale-overview-split-and-merge.md).) Może to prowadzić do niespójności gdzie wierszy z tej samej shardlet Pokaż dla wielu baz danych w jednym zapytaniu wielu niezależnego fragmentu. Należy pamiętać o tych ograniczeniach i podczas wykonywania zapytania wielu niezależnych należy wziąć pod uwagę opróżniania trwających operacji scalania podziału i zmiany do mapy niezależnego fragmentu.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Zobacz też
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  klasy i metody.

Zarządzanie za pomocą odłamków [biblioteki klienta elastycznej bazy danych](sql-database-elastic-database-client-library.md). Zawiera obszar nazw o nazwie [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) zapewnia możliwość wielu odłamków przy użyciu pojedynczego zapytania i wyników zapytania. Zapewnia on podczas badania abstrakcji przez kolekcję fragmentów. Umożliwia także zasad alternatywnych wykonywania, w szczególności częściowe wyniki, na wypadek awarii podczas wykonywania zapytania w wielu fragmentów.  

