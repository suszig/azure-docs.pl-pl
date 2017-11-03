---
title: "Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (Azure portal) | Dokumentacja firmy Microsoft"
description: "Azure zadania portalu do zarządzania mocy obliczeniowej. Zasoby obliczeniowe skali przez dostosowanie wartości dwu. Wstrzymać lub wznowić zasobów obliczeniowych w celu ograniczenia kosztów."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (Azure portal)
> [!div class="op_single_selector"]
> * [Omówienie](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Moc obliczeniową skali
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić zasoby obliczeniowe:

1. Otwórz [portalu Azure][Azure portal], otwórz bazę danych i kliknij przycisk **skali**.

    ![Kliknij polecenie Skaluj][1]
2. W bloku skali Przesuń suwak w lewo lub w prawo zmienić ustawienie wartości DWU.

    ![Przesuń suwak][2]
3. Kliknij pozycję **Zapisz**. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** o potwierdzenie lub **nie** do anulowania.

    ![Klikanie pozycji Zapisz.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych:

1. Otwórz [portalu Azure] [ Azure portal] i otworzyć bazy danych. Należy zauważyć, że stan jest **Online**.

    ![Stan online][6]
2. Wstrzymania zasobów obliczeniowych i pamięć, kliknij przycisk **Wstrzymaj**, a następnie zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** o potwierdzenie lub **nie** do anulowania.

    ![Upewnij się, Wstrzymaj][7]
3. Podczas uruchamiania bazy danych SQL Data Warehouse, stan jest **wstrzymywanie**.
4. Jeśli stan jest **wstrzymana**, odbywa się operację wstrzymywania i nie zawierają dla jednostek dwu.

    ![Stan wstrzymania][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Wznów obliczeń
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby wznowić bazy danych:

1. Otwórz [portalu Azure] [ Azure portal] i otworzyć bazy danych. Należy zauważyć, że stan jest **Paused**.

    ![Wstrzymaj bazy danych][4]
2. Wznowienie kliknij bazy danych **Start**, a następnie zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** o potwierdzenie lub **nie** do anulowania.

    ![Upewnij się, Wznów][5]
3. Podczas uruchamiania bazy danych SQL Data Warehouse, stanu jest "Wznawianie".
4. Jeśli stan jest **online**, baza danych jest gotowy.

    ![Stan online][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, zobacz [omówienie zarządzania][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
