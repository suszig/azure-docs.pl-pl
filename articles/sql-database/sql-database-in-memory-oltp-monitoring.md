---
title: "Monitorowanie magazynu w pamięci XTP | Dokumentacja firmy Microsoft"
description: "Szacowana i monitorowanie magazynu w pamięci XTP korzystać, pojemności; Napraw błąd pojemności 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jodebrui
ms.openlocfilehash: 613a9ced91d71cc9a65ea67e6ede1a78a03b4bd5
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitor OLTP w pamięci magazynu
Korzystając z [OLTP w pamięci](sql-database-in-memory.md), dane w tabelach zoptymalizowanych pod kątem pamięci i zmiennych tabel znajdują się w magazynie OLTP w pamięci. Każda warstwa usług Premium ma maksymalny rozmiar magazynu OLTP w pamięci, które opisano w [pojedynczy limity zasobów bazy danych](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) i [elastycznej puli zasobów limity](sql-database-resource-limits.md#elastic-pool-change-storage-size). Po przekroczeniu tego limitu insert i zaktualizować operacji mogą kończyć się niepowodzeniem (z powodu błędu 41823). W tym momencie można będzie konieczne usuwania danych w celu odzyskania pamięci, lub Uaktualnij warstwę wydajności bazy danych.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Określić, czy dane będą pasować do zakończenia przechowywania w pamięci
Określ caps magazynu z różnych warstw usługi Premium. Zobacz [pojedyncze bazy danych zasobów limity](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) i [elastycznej puli zasobów limity](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Trwa szacowanie wymagań pamięci dla działania tabel zoptymalizowanych pod kątem pamięci jest taki sam sposób dla programu SQL Server, ponieważ w bazie danych SQL Azure. Potrwać kilka minut, aby zapoznać się z tym tematem na [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Należy pamiętać, że tabela i wiersze do zmiennej tabeli, jak również indeksów, wliczane użytkownika maksymalny rozmiar danych. Instrukcja ALTER TABLE musi ponadto wystarczająco dużo miejsca, aby utworzyć nową wersję całą tabelę i jego indeksy.

## <a name="monitoring-and-alerting"></a>Monitorowanie i zgłaszanie alertów
Możesz monitorować użycie magazynu w pamięci w procentach zakończenia magazynu dla warstwę wydajności w [portalu Azure](https://portal.azure.com/): 

1. W bloku bazy danych odszukaj pole wykorzystania zasobów, a następnie kliknij przycisk Edytuj.
2. Wybierz metrykę, `In-Memory OLTP Storage percentage`.
3. Aby dodać alert, kliknij pole wykorzystania zasobów, aby otworzyć blok metryki, a następnie kliknij pozycję Dodaj alert.

Lub użyj następującego zapytania, aby wyświetlić wykorzystanie magazynu w pamięci:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Popraw sytuacji braku pamięci — błąd 41823
W operacji INSERT, UPDATE i Utwórz się niepowodzeniem z komunikatem o błędzie 41823 uruchomione wyniki braku pamięci.

Komunikat o błędzie 41823 wskazuje, że tabele zoptymalizowane pod kątem pamięci i zmiennych tabel Przekroczono maksymalny rozmiar.

Aby rozwiązać ten problem, albo:

* Usuwanie danych z tabel zoptymalizowanych pod kątem pamięci potencjalnie Odciążanie danych do tabel tradycyjnych, opartej na dysku. lub,
* Uaktualnienie warstwy usług, aby za mało przechowywania w pamięci dla danych, które należy przechowywać w tabelach zoptymalizowanych pod kątem pamięci.

## <a name="next-steps"></a>Następne kroki
Do monitorowania wskazówki, zobacz [monitorowania bazy danych SQL Azure przy użyciu dynamicznych widoków zarządzania](sql-database-monitoring-with-dmvs.md).
