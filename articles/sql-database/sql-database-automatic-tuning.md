---
title: "Baza danych SQL Azure — automatycznego dostrajania | Dokumentacja firmy Microsoft"
description: "Baza danych SQL Azure analizuje zapytania SQL i automatycznie dostosowuje się do użytkownika obciążenia."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: drasumic
editor: danimir
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 11/08/2017
ms.author: jovanpop
ms.openlocfilehash: 50d1548637ecc6862f680f73e333e8d7f8d20e39
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatycznego dostrajania w bazie danych SQL Azure

Dostrajanie automatycznego bazy danych SQL Azure zapewnia najwyższą wydajność i stabilna obciążenia za pośrednictwem dostrajania, przy użyciu analizy sztucznego ciągłej wydajności.

Dostrajanie automatycznego jest w pełni zarządzaną usługę, która używa wbudowane narzędzie analizy do ciągłego monitorowania zapytania wykonywane w bazie danych i automatycznie zwiększa ich wydajność. Jest to osiągane przez dynamicznie dostosowania zmiana obciążeń i stosować strojenia zaleceń bazą danych. Automatycznego dostrajania uzyskuje informacje o poziomie od wszystkich baz danych na platformie Azure za pomocą analizy sztucznego i dynamicznie zwiększa jego dostrajania akcje. Im dłużej baza danych SQL Azure działa z automatycznego dostrajania na, lepiej wykonuje.

Dostrajanie automatycznego bazy danych SQL Azure może być jedną z najważniejszych funkcji umożliwiających zapewnienie stabilny i wykonywania obciążenia szczytowego.

## <a name="what-can-automatic-tuning-do-for-you"></a>Co zrobić automatycznego dostrajania dla Ciebie?

- Dostrajanie wydajności automatycznych baz danych SQL Azure
- Automatyczne weryfikacji wzrost wydajności
- Automatyczne wycofywania i samodzielnej korekty
- Dostrajanie Historia dziennika
- Dostrajanie akcji skryptów T-SQL na potrzeby wdrożenia ręczne
- Monitorowanie wydajności aktywnego obciążenia
- Skalowanie w poziomie funkcja na setki tysięcy baz danych
- Pozytywny wpływ na zasoby DevOps i całkowity koszt posiadania

## <a name="safe-reliable-and-proven"></a>Bezpieczne, niezawodne i sprawdzone

Operacje dostrajania stosowany do bazy danych SQL Azure są całkowicie bezpieczne dla wydajność najbardziej wymagających obciążeń. System został zaprojektowany z rozwagą nie, aby zakłócać obciążeń użytkownika. Automatycznego dostrajania zalecenia są stosowane tylko w czasie wykorzystanie niski. System można również tymczasowo wyłączyć automatycznego dostrajania operacje ochrony wydajność obciążenia. W takim przypadku "Wyłączone przez system" wiadomość będzie wyświetlana w portalu Azure. Automatycznego dostrajania zakresie obciążeń o najwyższym priorytecie zasobów.

Automatycznego dostrajania mechanizmów są dojrzałe i został perfected na setki tysięcy baz danych działających na platformie Azure. Automatycznego dostrajania operacji stosowane są automatycznie sprawdza się upewnij się, że dodatnią poprawy wydajności obciążeń. Zalecenia uwzględniona wydajności są wykrywane dynamicznie i szybko przywrócić. Za pomocą dostrajania dziennika historii jest wyczyść śledzenie dostrajanie usprawnienia wprowadzone w każdej bazie danych SQL Azure. 

![Jak działa automatycznego dostrajania](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Dostrajanie automatycznego bazy danych SQL Azure udostępnia swojej logiki core z aparatem automatycznego dostrajania programu SQL Server. Aby uzyskać dodatkowe informacje techniczne na mechanizmie wbudowane narzędzie analizy, zobacz [programu SQL Server automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Użyj automatycznego dostrajania

Automatycznego dostrajania musi zostać włączona ręcznie w ramach subskrypcji. Aby włączyć automatycznego dostrajania przy użyciu portalu Azure, zobacz [Włączanie automatycznego dostrajania](sql-database-automatic-tuning-enable.md).

Automatycznego dostrajania mogą działać autonomicznie przez automatycznego stosowania zaleceń dostrajania, w tym weryfikacji automatyczne zwiększenie wydajności. 

Uzyskać większą kontrolę można wyłączyć automatyczne stosowanie dostrajanie zalecenia i dostrajania zalecenia można ręcznie stosować za pomocą portalu Azure. Istnieje również możliwość użycie rozwiązania do wyświetlania automatycznego dostrajania zalecenia dotyczące tylko i zastosować je ręcznie za pomocą skryptów i narzędzi wybranych przez użytkownika. 

Omówienie sposobu automatycznego dostrajania działania oraz typowe scenariusze użytkowania Zobacz osadzonego wideo:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Opcje automatycznego dostrajania

Dostępne są następujące opcje dostrajania automatycznego dostępnych w bazie danych SQL Azure:
 1. **CREATE INDEX** identyfikującym indeksów, które może poprawić wydajność obciążenia, tworzy indeksy i sprawdza, czy poprawiają wydajność kwerend.
 2. **Instrukcja DROP INDEX** , które identyfikują nadmiarowe i zduplikowane indeksy i indeksów, które nie były używane w długim okresie.
 3. **WYMUŚ ostatniego dobry PLAN** identyfikuje kwerendy SQL, które używają plan wykonania wolniejsze niż poprzednie Dobry plan i korzysta z ostatnim znanym Dobry plan zamiast regressed planu.

Baza danych SQL Azure identyfikuje **CREATE INDEX**, **DROP INDEX**, i **życie ostatniego DOBRE Planowanie** zaleceń, które można zoptymalizować bazę danych i wyświetla je w portalu Azure. Więcej informacji o identyfikacji indeksów, które powinny zostać zmienione w [znaleźć zalecenia dotyczące indeksu w portalu Azure](sql-database-advisor-portal.md). Można ręcznie stosować zalecenia za pomocą portalu lub pozwolić bazy danych SQL Azure, aby automatycznie zastosować zalecenia, monitorowania obciążenia po zmianie i sprawdź, czy zalecenie zwiększona wydajność obciążenia.

Automatyczne opcje dostrajania może być niezależnie włączony lub wyłączony na bazę danych lub mogą być skonfigurowane na serwerze logicznym i zastosować dla każdej bazy danych, która dziedziczy ustawienia z serwera. Konfigurowanie automatycznego dostrajania opcji na serwerze i dziedziczy ustawienia w bazach danych na serwerze jest zalecana metoda Konfigurowanie automatycznego dostrajania, ponieważ takie rozwiązanie upraszcza zarządzanie opcje automatycznego dostrajania na dużą liczbę baz danych.

## <a name="next-steps"></a>Następne kroki

- Aby włączyć automatyczne strojenia w bazie danych SQL Azure do zarządzania obciążenie, zobacz [Włączanie automatycznego dostrajania](sql-database-automatic-tuning-enable.md).
- Aby ręcznie Przejrzyj i Zastosuj automatycznego dostrajania zalecenia, zobacz [Znajdowanie i stosować zalecenia wydajności](sql-database-advisor-portal.md).
- Aby dowiedzieć się, jak używać automatycznego dostrajania wbudowane narzędzie analizy, zobacz [analizy sztucznego Dostraja bazy danych SQL Azure](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Aby dowiedzieć się więcej o sposobie automatycznego dostrajania działania w bazie danych SQL Azure i SQL server 2017, zobacz [programu SQL Server automatycznego dostrajania](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
