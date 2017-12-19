---
title: Przezroczystego szyfrowania danych w magazynie danych SQL (T-SQL) | Dokumentacja firmy Microsoft
description: Przezroczystego szyfrowania danych (funkcji TDE) w magazynie danych SQL (T-SQL)
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 74c9032aababdce91ed617cd7a4c628915b42504
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Rozpoczynanie pracy z przezroczystym danych szyfrowania (funkcji TDE)
> [!div class="op_single_selector"]
> * [Przegląd zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
> * [Uwierzytelnianie](sql-data-warehouse-authentication.md)
> * [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Wymagane uprawnienia
Aby włączyć funkcji przezroczystego szyfrowania danych (TDE), musi być administrator lub członek roli dbmanager:.

## <a name="enabling-encryption"></a>Włączenie szyfrowania
Wykonaj następujące kroki, aby włączyć funkcji TDE dla magazynu danych SQL:

1. Połączyć się z *wzorca* bazy danych na serwerze hostującym bazę danych przy użyciu nazwy logowania, która jest administrator lub członek **dbmanager:** roli w bazie danych master
2. Wykonaj następującą instrukcję do szyfrowania bazy danych.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Wyłączenie szyfrowania
Wykonaj następujące kroki, aby wyłączyć funkcji TDE dla magazynu danych SQL:

1. Połączyć się z *wzorca* bazy danych przy użyciu nazwy logowania, która jest administrator lub członek **dbmanager:** roli w bazie danych master
2. Wykonaj następującą instrukcję do szyfrowania bazy danych.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Wstrzymanie magazynu danych SQL musi zostać wznowiony przed wprowadzeniem zmian w ustawieniach funkcji TDE.
> 
> 

## <a name="verifying-encryption"></a>Weryfikowanie szyfrowania
Aby sprawdzić, czy stan szyfrowania magazynu danych SQL, wykonaj następujące czynności:

1. Połączyć się z *wzorca* lub wystąpienie bazy danych przy użyciu nazwy logowania, która jest administrator lub członek **dbmanager:** roli w bazie danych master
2. Wykonaj następującą instrukcję do szyfrowania bazy danych.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

W wyniku ```1``` wskazuje zaszyfrowanej bazy danych, ```0``` wskazuje niezaszyfrowane bazy danych.

## <a name="encryption-dmvs"></a>Szyfrowanie widoków DMV
* [sys.Databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
