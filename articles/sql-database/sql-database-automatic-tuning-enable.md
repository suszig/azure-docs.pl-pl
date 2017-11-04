---
title: "Włączanie automatycznego dostrajania dla usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Można włączyć automatycznego dostrajania bazy danych SQL Azure łatwe."
services: sql-database
documentationcenter: 
author: veljko-msft
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/19/2016
ms.author: veljko-msft
ms.openlocfilehash: 82db5996c1ba1f224593e4eaa5b3b0067755db49
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="enable-automatic-tuning"></a>Włączanie automatycznego dostrajania

Baza danych SQL Azure jest usługą automatycznie zarządzanych danych, która stale monitoruje zapytań i określa działania, które można wykonać, aby poprawić wydajność obciążenia. Możesz przejrzeć zalecenia i ręcznie zastosować je lub pozwól bazy danych SQL Azure automatyczne stosowanie działania naprawcze — jest to nazywane **automatycznego dostrajania tryb**. Dostrajanie automatycznej można włączyć na poziomie bazy danych lub serwera.

## <a name="enable-automatic-tuning-on-server"></a>Włączanie automatycznego dostrajania na serwerze

Aby włączyć automatyczne dostrajanie na serwerze bazy danych SQL Azure, przejdź do serwera w portalu Azure, a następnie wybierz **automatycznego dostrajania** w menu. Wybierz opcje automatycznego dostrajania chcesz włączyć, a następnie wybierz **Zastosuj**:

![Serwer](./media/sql-database-automatic-tuning-enable/server.png)

Opcje automatycznego dostrajania na serwerze są stosowane do wszystkich baz danych na serwerze. Domyślnie wszystkie bazy danych dziedziczy konfiguracji z ich nadrzędnego serwera, ale to zastąpione i określić osobno dla każdej bazy danych.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurowanie automatycznego dostrajania dla bazy danych

Azure portal umożliwia niezależnie Określ automatycznego dostrajania konfiguracji w każdej bazie danych.

> [!NOTE]
> Ogólne zaleca się zarządzanie automatycznego dostrajania konfiguracji na poziomie serwera, dlatego te same ustawienia konfiguracji mogą być automatycznie stosowane w każdej bazie danych. Konfigurowanie automatycznego dostrajania na poszczególne bazy danych, jeśli baza danych jest inny tego inne, na tym samym serwerze.
>

Aby włączyć automatycznego dostrajania dla jednej bazy danych, przejdź do bazy danych w portalu Azure wybierz a następnie **automatycznego dostrajania**. Można skonfigurować pojedynczy bazy danych, aby odziedziczyć ustawienia z serwera, wybierając opcję lub konfiguracji bazy danych można określić osobno.

![Database (Baza danych)](./media/sql-database-automatic-tuning-enable/database.png)

Po wybraniu odpowiedniej konfiguracji, kliknij przycisk **Zastosuj**.

## <a name="next-steps"></a>Następne kroki
* Odczyt [automatycznego dostrajania artykułu](sql-database-automatic-tuning.md) Aby dowiedzieć się więcej na temat automatycznego dostrajania i jak go w celu poprawy wydajności.
* Zobacz [zaleceń](sql-database-advisor.md) omówienie zaleceń wydajności bazy danych SQL Azure.
* Zobacz [szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md) Aby dowiedzieć się więcej o wyświetlaniu wpływu na wydajność kwerend top.
