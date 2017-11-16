---
title: Projektowanie Twojego pierwszego bazy danych SQL Azure - C# | Dokumentacja firmy Microsoft
description: "Dowiedz się więcej o projektowaniu pierwszą bazę danych Azure SQL i nawiązać z nią za pomocą programu C# za pomocą ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 08/25/2017
ms.author: genemi
ms.openlocfilehash: 1e13a9a89873b189e1ea05c0208e7c8646881655
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Projekt bazy danych Azure SQL i Połącz z C & #x23; i ADO.NET

Baza danych SQL Azure to relacyjnej bazy danych — jako a usługa (DBaaS) w Microsoft Cloud (Azure). W tym samouczku Dowiedz się jak używać portalu Azure i ADO.NET z programu Visual Studio: 

> [!div class="checklist"]
> * Utwórz bazę danych w portalu Azure
> * Reguły zapory poziomu serwera w portalu Azure
> * Połączenie z bazą danych ADO.NET i Visual Studio
> * Tworzenie tabel z ADO.NET
> * Wstawianie, aktualizowanie i usuwanie danych za pomocą ADO.NET 
> * Zapytania na danych ADO.NET

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/) przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstalowany program [Visual Studio Community 2017, Visual Studio Professional 2017 lub Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono bazy danych podstawowych zadań takich jak utworzyć bazę danych i tabele, załadować zapytania na danych i Przywróć bazę danych z wcześniejszego punktu w czasie. W tym samouczku omówiono:
> [!div class="checklist"]
> * Tworzenie bazy danych
> * Konfigurowanie reguł zapory
> * Połączenie z bazą danych z [i Visual Studio C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Tworzenie tabel
> * Wstawianie, aktualizowanie i usuwanie danych
> * Zapytania o dane

Przejdź do następnego samouczek, aby dowiedzieć się więcej na temat migracji danych.

> [!div class="nextstepaction"]
>[Migrowanie bazy danych SQL Server do bazy danych SQL Azure](sql-database-migrate-your-sql-server-database.md)

