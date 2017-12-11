---
title: "Przyrostowe kopiowanie danych przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "W tych samouczkach przedstawiono sposób przyrostowego kopiowania danych z magazynu danych źródłowych do docelowego magazynu danych. W pierwszym z nich opisane jest kopiowanie danych z jednej tabeli. "
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/05/2017
ms.author: shlo
ms.openlocfilehash: 2ae6cb42685dfb227bd75f83e73dfdf646ab909f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Przyrostowe ładowanie danych ze źródłowego do docelowego magazynu danych

W rozwiązaniu integracji danych przyrostowe (lub różnicowe) ładowanie danych po początkowym pełnym ładowaniu danych to powszechnie używany scenariusz. W samouczkach w tej sekcji opisano różne sposoby przyrostowego ładowania danych przy użyciu usługi Azure Data Factory w wersji 2.

## <a name="delta-data-loading-using-a-watermark"></a>Ładowanie danych różnicowych przy użyciu limitu
W takim przypadku należy zdefiniować limit w źródłowej bazie danych. Limit to kolumna, która zawiera znacznik czasu ostatniej aktualizacji lub klucz o zwiększającej się wartości. Rozwiązanie ładowania różnicowego służy do ładowania zmienionych danych między starą a nową wartością limitu. Na poniższym diagramie przedstawiono przepływ pracy dla tej metody: 

![Przepływ pracy dotyczący używania limitu](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Aby uzyskać instrukcje krok po kroku, zobacz następujące samouczki: 

- [Przyrostowe kopiowanie danych z jednej tabeli w bazie danych Azure SQL Database do magazynu Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Przyrostowe kopiowanie danych z wielu tabel na lokalnym serwerze SQL Server do bazy danych Azure SQL Database](tutorial-incremental-copy-multiple-tables-powershell.md)


## <a name="delta-data-loading-using-the-change-tracking-technology"></a>Ładowanie danych różnicowych przy użyciu technologii Change Tracking
Technologia Change Tracking to rozwiązanie o małych wymaganiach programu SQL Server i bazy danych Azure SQL Database, które zapewnia wydajny mechanizm śledzenia zmian na potrzeby aplikacji. Dzięki temu w aplikacji można łatwo zidentyfikować dane, który zostały wstawione, zaktualizowane lub usunięte. 

Na poniższym diagramie przedstawiono przepływ pracy dla tej metody:

![Przepływ pracy dotyczący używania technologii Change Tracking](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Aby uzyskać instrukcje krok po kroku, zobacz następujące samouczki: <br/>
[Przyrostowe kopiowanie danych z bazy danych Azure SQL Database do magazynu Azure Blob Storage z użyciem technologii Change Tracking](tutorial-incremental-copy-change-tracking-feature-powershell.md)


## <a name="next-steps"></a>Następne kroki
Przejdź do następującego samouczka: 

> [!div class="nextstepaction"]
>[Przyrostowe kopiowanie danych z jednej tabeli w bazie danych Azure SQL Database do magazynu Azure Blob Storage](tutorial-incremental-copy-powershell.md)
