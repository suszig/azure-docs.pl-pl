---
title: "Baza danych SQL Azure — automatycznego dostrajania | Dokumentacja firmy Microsoft"
description: "Baza danych SQL Azure analizuje zapytania SQL i automatycznie dostosowuje się do użytkownika obciążenia."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 09/19/2017
ms.author: jovanpop
ms.openlocfilehash: 10f8cca8e519b28f0fe29605419b860f73e04a87
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatycznego dostrajania w bazie danych SQL Azure

Baza danych SQL Azure to usługa danych pełni zarządzana monitorująca zapytania, które są wykonywane w bazie danych i automatycznie poprawia wydajność obciążenia bazy danych. Azure SQL Database ma wbudowane [automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) mechanizmu analizy, który można automatycznie dostrojenie i poprawić wydajność kwerend przez dynamicznie dostosowanie bazy danych, aby obciążenie. Automatycznego dostrajania w bazie danych SQL Azure może być jedną z najważniejszych funkcji, możesz włączyć w bazie danych SQL Azure w celu zoptymalizowania wydajności zapytań.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="automatic-tuning-options"></a>Opcje automatycznego dostrajania

[Automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) w bazie danych SQL Azure dostępne opcje to:
 1. **CREATE INDEX** identyfikującym indeksów, które może poprawić wydajność obciążenia, tworzy indeksy i sprawdza, czy poprawiają wydajność kwerend.
 2. **Instrukcja DROP INDEX** , które identyfikują nadmiarowe i zduplikowane indeksy i indeksów, które nie były używane w długim okresie.
 3. **PLAN korekty REGRESJI** identyfikuje kwerendy SQL, które używają plan wykonania wolniejsze niż poprzednie Dobry plan i korzysta z ostatnim znanym Dobry plan zamiast regressed planu.

Baza danych SQL Azure identyfikuje **CREATE INDEX**, **DROP INDEX**, i **PLAN korekty REGRESJI** zaleceń, które można zoptymalizować bazę danych i wyświetla je na platformie Azure Portal. Więcej informacji o identyfikacji indeksów, które powinny zostać zmienione w [znaleźć zalecenia dotyczące indeksu w portalu Azure](sql-database-advisor-portal.md). Można ręcznie stosować zalecenia za pomocą portalu lub pozwolić bazy danych SQL Azure, aby automatycznie zastosować zalecenia, monitorowania obciążenia po zmianie i sprawdź, czy zalecenie zwiększona wydajność obciążenia.

Automatyczne opcje dostrajania może być niezależnie włączony lub wyłączony na bazę danych lub mogą być skonfigurowane na serwerze logicznym i zastosować dla każdej bazy danych, która dziedziczy ustawienia z serwera. Konfigurowanie [automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) opcji na serwerze i dziedziczy ustawienia w bazach danych na serwerze jest zalecana metoda Konfigurowanie automatycznego dostrajania, ponieważ takie rozwiązanie upraszcza zarządzanie opcje automatycznego dostrajania na duża liczba baz danych.

Zobacz ten artykuł, aby uzyskać instrukcje dotyczące [Włączanie automatycznego dostrajania](sql-database-automatic-tuning-enable.md) przy użyciu portalu Azure.

## <a name="next-steps"></a>Następne kroki

- Aby włączyć automatyczne dostrajanie w bazie danych SQL Azure i umożliwić funkcji automatycznego dostrajania obciążenie w pełni zarządzać, zobacz [Włączanie automatycznego dostrajania](sql-database-automatic-tuning-enable.md).
- Umożliwia ręczne dostrajania, możesz przejrzeć [dostrajanie zalecenia w portalu Azure](sql-database-advisor-portal.md) i ręcznie zastosować te, które poprawić wydajność kwerend.
- Przeczytaj więcej na temat wbudowanych analizy, które Dostraja [bazy danych SQL Azure](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Przeczytaj więcej na temat [automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) w bazie danych SQL Azure i 2017 serwera SQL.
