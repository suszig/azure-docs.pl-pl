---
title: "Zarządzanie poświadczeniami w biblioteki klienta elastycznej bazy danych | Dokumentacja firmy Microsoft"
description: "Jak ustawić odpowiedni poziom poświadczeń administratora tylko do odczytu, dla elastycznej bazy danych aplikacji"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 72e0edaf-795e-4856-84a5-6594f735fb7e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: be041a2c1095452efa722a29f0b7a25aee1bc464
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Poświadczenia używane do dostępu biblioteki klienta elastycznej bazy danych
[Biblioteki klienta elastycznej bazy danych](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) używa trzech różnych rodzajów poświadczenia dostępu do [menedżera map niezależnego fragmentu](sql-database-elastic-scale-shard-map-management.md). Zależności od potrzeb za pomocą poświadczeń najniższego poziomu dostępu, które są możliwe.

* **Poświadczenia zarządzania**: do tworzenia lub manipulowanie menedżera map niezależnego fragmentu. (Zobacz [słownik](sql-database-elastic-scale-glossary.md).) 
* **Dostęp do poświadczeń**: do istniejącego menedżera mapy niezależnego fragmentu Aby uzyskać informacje o liczbie fragmentów.
* **Poświadczenia połączenia**: nawiązać połączenia z fragmentów. 

Zobacz też [Zarządzanie bazami danych i logowaniami w bazie danych SQL Azure](sql-database-manage-logins.md). 

## <a name="about-management-credentials"></a>Informacji o poświadczeniach zarządzania
Poświadczenia zarządzania są używane do tworzenia [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) obiektu dla aplikacji, które manipulowania niezależnego fragmentu mapowania. (Na przykład, zobacz [Dodawanie niezależnych, za pomocą narzędzi elastycznej bazy danych](sql-database-elastic-scale-add-a-shard.md) i [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md)). Użytkownik biblioteki klienta elastycznej skali tworzy SQL użytkowników i kont logowania SQL i upewnia się, że każdy otrzymuje uprawnienia odczytu/zapisu w bazie danych globalnych niezależnego fragmentu mapy i wszystkie również niezależnego fragmentu bazy danych. Te poświadczenia są używane do obsługi mapy niezależnych globalne i map niezależnych lokalnych podczas zmiany do mapy niezależnego fragmentu są wykonywane. Na przykład użyć poświadczeń zarządzania można utworzyć obiektu Menedżera mapy niezależnego fragmentu (przy użyciu [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Zmienna **smmAdminConnectionString** ciąg połączenia, który zawiera poświadczenia zarządzania. Identyfikator użytkownika i hasło zapewniają dostęp odczytu/zapisu do bazy danych mapy niezależnego fragmentu i poszczególnych fragmentów. Parametry połączenia zarządzania obejmuje również nazwę serwera i nazwę bazy danych, aby zidentyfikować bazy danych globalnych niezależnych mapy. Poniżej przedstawiono typowe parametry, w tym celu:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Nie używaj wartości w postaci "username@server" — zamiast tego użyć wartości "nazwa_użytkownika".  Jest to spowodowane poświadczeń, należy skontaktować się wobec bazy danych Menedżera mapy niezależnego fragmentu i poszczególnych fragmentów, które mogą znajdować się na różnych serwerach.

## <a name="access-credentials"></a>Poświadczenia dostępu
Podczas tworzenia niezależnych Menedżera mapy w aplikacji, która nie administrować niezależnego fragmentu mapy, Użyj poświadczeń z uprawnieniami tylko do odczytu na mapie globalne niezależnego fragmentu. Informacje uzyskane z globalnego niezależnych mapy w ramach tych poświadczeń jest używany dla [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md) i w celu wypełnienia pamięci podręcznej mapy niezależnego fragmentu na kliencie. Poświadczenia są realizowane za pośrednictwem tego samego wzorca wywołania do **GetSqlShardMapManager**: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Zwróć uwagę na użycie **smmReadOnlyConnectionString** do uwzględnienia użycia różnych poświadczeń dla tego dostępu dla **bez uprawnień administratora** użytkowników: te poświadczenia nie może dostarczyć uprawnienia do zapisu w globalny identyfikator niezależnego fragmentu mapy. 

## <a name="connection-credentials"></a>Poświadczenia połączenia
Dodatkowe poświadczenia są wymagane w przypadku korzystania [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metodę dostępu niezależnych, skojarzone z kluczem dzielenia na fragmenty. Wymagane są poświadczenia zapewniające uprawnienia dostępu tylko do odczytu do tabel Mapa lokalnych niezależnego fragmentu znajdującej się na niezależnego fragmentu. Jest to niezbędne do przeprowadzenia weryfikacji połączenia zależne od danych routingu na niezależnego fragmentu. Następujący fragment kodu umożliwia dostęp do danych w kontekście routingu zależne od danych: 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

W tym przykładzie **smmUserConnectionString** zawiera parametry połączenia dla poświadczeń użytkownika. Dla bazy danych SQL Azure w tym miejscu jest ciąg połączenia typowe dla poświadczeń użytkownika: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Zgodnie z poświadczeniami administratora, nie używaj wartości w postaci "username@server". Zamiast tego można użyć "nazwa_użytkownika".  Należy również zauważyć, że parametry połączenia nie zawiera nazwę serwera i nazwę bazy danych. Jest to spowodowane tym **OpenConnectionForKey** wywołania automatycznie kieruje połączenie poprawny identyfikator niezależnego fragmentu opartą na kluczu. W związku z tym nazwy bazy danych i nazwy serwera nie są dostarczane. 

## <a name="see-also"></a>Zobacz też
[Zarządzanie bazami danych i nazwami logowania w usłudze Azure SQL Database](sql-database-manage-logins.md)

[Zabezpieczanie bazy danych SQL](sql-database-security-overview.md)

[Wprowadzenie zadania elastycznej bazy danych](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

