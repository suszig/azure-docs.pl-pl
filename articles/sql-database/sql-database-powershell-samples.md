---
title: "Azure przykłady skryptów środowiska PowerShell dla bazy danych SQL | Dokumentacja firmy Microsoft"
description: "Przykłady ułatwiające tworzenie i zarządzanie nimi serwerów bazy danych SQL Azure, elastyczne pule baz danych i zapór skrypt programu Azure PowerShell."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples, mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: On Demand
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 65f6600f6810024e00eaa50906d7924bd36768d3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Przykładów dla platformy Azure PowerShell bazy danych SQL Azure

Poniższa tabela zawiera linki do przykładowe skrypty programu Azure PowerShell bazy danych SQL Azure.

| |  |
|---|---|
|**Utworzyć pojedynczą bazę danych i elastycznej puli**||
| [Utworzyć pojedynczą bazę danych i skonfigurować reguły zapory](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten skrypt programu PowerShell tworzy pojedynczej bazy danych Azure SQL i konfiguruje regułę zapory poziomu serwera. |
| [Tworzenie elastycznych pul i przeniesienie puli baz danych](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten skrypt programu PowerShell tworzy pule elastyczne bazy danych SQL Azure i przenosi puli baz danych, a następnie zmienia poziomy wydajności.|
|**Konfigurowanie replikacji geograficznej i trybu failover**||
| [Konfigurowanie i pracy awaryjnej pojedynczej bazy danych przy użyciu aktywna replikacja geograficzna](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell konfiguruje aktywna replikacja geograficzna dla pojedynczej bazy danych Azure SQL i awaryjnie do repliki pomocniczej. |
| [Konfigurowanie i pracy awaryjnej puli bazy danych przy użyciu aktywna replikacja geograficzna](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell konfiguruje aktywna replikacja geograficzna dla bazy danych Azure SQL w puli elastycznej SQL i awaryjnie do repliki pomocniczej. |
| [Konfigurowanie i trybu failover, a praca awaryjna grupy dla pojedynczej bazy danych (wersja zapoznawcza)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten skrypt programu PowerShell konfiguruje grupę pracy awaryjnej dla wystąpienia serwera bazy danych SQL Azure, dodaje bazy danych do grupy pracy awaryjnej i awaryjnie na serwer pomocniczy |
|**Skalowania pojedynczej bazy danych i elastycznej puli**||
| [Skalowania pojedynczej bazy danych](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten monitory skryptu PowerShell metryki wydajności bazy danych Azure SQL skaluje go na wyższy poziom wydajności i tworzy regułę alertu na jednym z metryk wydajności. |
| [Skala puli elastycznej](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ta monitory skryptu programu PowerShell metryk wydajności puli elastycznej bazy danych SQL Azure skaluje go na wyższy poziom wydajności i tworzy regułę alertu na jednym z metryk wydajności.  |
| **Inspekcja i wykrywania zagrożeń** |
| [Konfigurowanie inspekcji i wykrywanie zagrożeń](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell konfiguruje zasady wykrywania inspekcji i zagrożeń dla bazy danych Azure SQL. |
| **Przywracanie, kopiowanie i Importowanie bazy danych**||
| [Przywracanie bazy danych](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell przywraca bazę danych Azure SQL z geograficznie nadmiarowej kopii zapasowej i przywrócenie usuniętych bazy danych Azure SQL do najnowszej kopii zapasowej. |
| [Kopiowanie bazy danych do nowego serwera](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell tworzy kopię istniejącej bazy danych Azure SQL w nowy serwer Azure SQL. |
| [Importuj z pliku pliku bacpac bazy danych](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ten skrypt programu PowerShell importuje bazy danych na serwerze Azure SQL z pliku pliku bacpac. |
| **Synchronizowanie danych między bazami danych**||
| [Synchronizowanie danych między bazami danych SQL](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten skrypt programu PowerShell skonfiguruje funkcję synchronizacji danych do synchronizacji między wiele baz danych Azure SQL. |
| [Synchronizowanie danych między bazy danych SQL i programu SQL Server lokalne](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ten skrypt programu PowerShell skonfiguruje funkcję synchronizacji danych do synchronizacji między bazą danych Azure SQL i lokalnej bazy danych programu SQL Server. |
|||
|||
