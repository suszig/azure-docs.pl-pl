---
title: "Dodawanie niezależnych, za pomocą narzędzi elastycznej bazy danych | Dokumentacja firmy Microsoft"
description: "Aby dodać nowe odłamków do niezależnego fragmentu użyj Interfejsy API elastycznego skalowania zestawu."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 7432bf00aa4d97d8dbc4b92a6a836698ee2b84d7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Dodawanie niezależnych, za pomocą narzędzi elastycznej bazy danych
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Aby dodać niezależnych dla nowego zakresu lub klucza
Aplikacje często konieczne jest po prostu Dodaj nowe odłamków do obsługi danych, która oczekuje się od nowych kluczy lub kluczy zakresów, mapy niezależnego fragmentu, która już istnieje. Na przykład aplikację podzielonej na podstawie Identyfikatora dzierżawcy może być konieczne obsługi administracyjnej nowych niezależnych dla nowej dzierżawy lub co miesiąc podzielonej danych może być konieczne nowy identyfikator niezależnego fragmentu udostępnione przed rozpoczęciem każdego nowego miesiąca. 

Jeśli nowy zakres wartości klucza nie jest już częścią istniejące mapowanie, jest bardzo prosty dodać nowy identyfikator niezależnego fragmentu i Skojarz nowy klucz lub zakresu do tego niezależnego fragmentu. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Przykład: dodanie niezależnego fragmentu i jego zakres do istniejącej mapy niezależnego fragmentu
W przykładzie użyto [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metod i tworzy wystąpienie [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)klasy. W przykładzie poniżej bazy danych o nazwie **sample_shard_2** i wszystkie obiekty niezbędne schematu wewnątrz niej zostały utworzone do przechowywania zakresu [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternatywnie można utworzyć nowego Menedżera mapy niezależnego fragmentu za pomocą programu Powershell. Przykład jest dostępny [tutaj](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Aby dodać niezależnych pustą część istniejącego zakresu
W niektórych sytuacjach może masz już zamapowana zakres niezależnych i częściowo wypełnione z danymi, ale ma teraz nadchodzących danych będą kierowane do różnych niezależnego fragmentu. Na przykład możesz niezależnego fragmentu przez zakres dni i muszą już przydzielony 50 dni niezależnego fragmentu, ale dnia 24 mają przyszłych dane trafić w różnych niezależnego fragmentu. Elastycznej bazy danych [narzędzia do scalania podziału](sql-database-elastic-scale-overview-split-and-merge.md) mogą wykonać tę operację, ale jeśli przenoszenia danych nie jest konieczne (na przykład dane dla zakresu dni [25, 50), tj., dzień 25, włącznie z 50 wyłączności, jeszcze nie istnieje) można wykonać to całkowicie przy użyciu interfejsów API Management mapy niezależnego fragmentu bezpośrednio.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Przykład: podział zakresu i przypisywanie puste części niezależnego fragmentu nowo dodany
Utworzono bazę danych o nazwie "sample_shard_2" oraz wszystkie obiekty niezbędne schematu wewnątrz niej.  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Ważne**: Użyj tej techniki tylko jeśli masz pewność, że zakres dla zaktualizowanej mapowania jest pusta.  Powyżej metod nie sprawdzenie, czy dane dla zakresu przenoszony, dlatego warto uwzględnić kontroli w kodzie.  Jeśli istnieją wiersze w zakresie przenoszony, rzeczywistej dystrybucji danych nie będzie odpowiadała mapy zaktualizowane niezależnego fragmentu. Użyj [narzędzia do scalania podziału](sql-database-elastic-scale-overview-split-and-merge.md) do wykonania tej operacji, zamiast tego w tych przypadkach.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

