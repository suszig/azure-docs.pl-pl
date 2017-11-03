---
title: Zasoby na potrzeby tworzenia magazynu danych na platformie Azure | Dokumentacja firmy Microsoft
description: "Pojęcia dotyczące programowania, decyzji projektowych i zalecenia dotyczące technik programowania dla usługi SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: b85a4f09e561e429aa5bf46ec680014487fb40c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Decyzje dotyczące projektu i technik programowania dla usługi SQL Data Warehouse
Zapoznaj się za pośrednictwem tych artykułach programowanie, aby lepiej zrozumieć kluczowych decyzji projektowych, zalecenia i technik programowania dla usługi SQL Data Warehouse.

## <a name="key-design-decisions"></a>Kluczowych decyzji projektowych
Następujące artykuły omówiono niektóre z podstawowych pojęć i decyzje dotyczące projektu, które mają być zrozumiały dla rozwoju magazynu danych rozproszonych za pomocą usługi SQL Data Warehouse:

* [połączenia][connections]
* [Współbieżność][concurrency]
* [transakcje][transactions]
* [schematy zdefiniowane przez użytkownika][user-defined schemas]
* [Dystrybucja tabeli][table distribution]
* [indeksy tabel][table indexes]
* [partycje tabeli][table partitions]
* [CTAS][CTAS]
* [statystyki][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Zalecenia dotyczące tworzenia i technik programowania
Te artykuły wyróżnianie określonych technik programowania, wskazówki i zalecenia dotyczące Tworzenie magazynu danych SQL:

* [procedury składowane][stored procedures]
* [etykiety][labels]
* [Widoki][views]
* [tabele tymczasowe][temporary tables]
* [dynamiczne SQL][dynamic SQL]
* [tworzenie pętli][looping]
* [Grupuj według opcje][group by options]
* [Przypisanie zmiennej][variable assignment]

## <a name="next-steps"></a>Następne kroki
Po nastąpiło za pośrednictwem artykuły programowanie zapoznaj się za pośrednictwem [odwołania Transact-SQL] [ Transact-SQL reference] strony, aby uzyskać więcej informacji na obsługiwana składnia SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
