---
title: "Wstrzymać, wznowić, skalować REST w usłudze Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Zarządzanie moc obliczeniową w usłudze SQL Data Warehouse REST, T-SQL i programu PowerShell."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 21de7337-9356-49bb-a6eb-06c1beeba2c4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 07/25/2017
ms.author: elbutter
ms.openlocfilehash: 24e43205c0c562fca9b1c2c0e5eed4da54e17ed7
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Zarządzanie energią obliczeniowych w magazynie danych SQL Azure (REST)
> [!div class="op_single_selector"]
> * [Omówienie](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [Program PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Moc obliczeniową skali
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić liczbę jednostek dwu, użyj [Tworzenie lub aktualizacja bazy danych] [ Create or Update Database] interfejsu API REST. Poniższy przykład ustawia cel poziomu usługi DW1000 MySQLDW, która jest hostowana na serwerze MyServer bazy danych. Serwer jest w grupie zasobów platformy Azure o nazwie ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, należy użyć [wstrzymać bazy danych] [ Pause Database] interfejsu API REST. W następującym przykładzie wstrzymano bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie zasobów platformy Azure o nazwie ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Wznów obliczeń
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby utworzyć bazę danych, należy użyć [wznowienia bazy danych] [ Resume Database] interfejsu API REST. W następującym przykładzie uruchomiono bazę danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie zasobów platformy Azure o nazwie ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Sprawdź stan bazy danych

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Kolejne kroki
Inne zadania zarządzania, zobacz [omówienie zarządzania][Management overview].

<!--Image references-->

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Resume Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Create or Update Database]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
