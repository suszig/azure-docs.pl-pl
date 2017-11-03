---
title: "Jak wyświetlać dane dotyczące zasobów w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób wyświetlania danych powiązane zasoby trwałego wybranych danych w wykazie danych Azure."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d45f2cabe712a7982f99a9d280fed4494fc4d377
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Jak wyświetlać dane dotyczące zasobów w usłudze Azure Data Catalog?
Wykaz danych Azure służy do wyświetlania zasobów danych związanych z wybranych danych zasobów i widoku relacji między nimi. 

## <a name="supported-data-sources"></a>Obsługiwane źródła danych 
Podczas rejestrowania zasobów danych z następujących źródeł danych usługi Azure Data Catalog automatycznie rejestruje metadane dotyczące relacji sprzężenia między wybranych zasobów danych. 

- Oprogramowanie SQL Server
- Usługa Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Wyświetlanie powiązanych danych zasobów
Aby wyświetlić zasoby danych, które są powiązane z wybrany zestaw danych, użyj **relacje** karcie, jak pokazano na poniższej ilustracji: 

![Azure Data Catalog — wyświetlanie powiązanych zasobów danych](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

W tym przykładzie są dwie relacje dla wybranego **ProductSubcategory** zasobu danych: 

- Kolumna ProductSubcategoryID tabeli produktu ma relacji klucza obcego z kolumną ProductSubcategoryID ProductSubcategory wybranej tabeli. 
- Kolumna tabeli ProductSubCategory ProductCategoryID ma relacji klucza obcego z kolumną ProductCategoryID wybranej tabeli ProductCategory.

> [!NOTE]
> Zwróć uwagę, kierunek strzałki w widoku drzewa relacji.  

Aby zobaczyć więcej szczegółów, takich jak w pełni kwalifikowana nazwa kolumny, przesuń wskaźnik myszy nad i zobacz menu podręcznego podobnie do poniższej ilustracji: 

![Azure Data Catalog — podręczny relacji](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

Aby uwzględnić relacje między zasoby, które zostały już zarejestrowane, Zarejestruj ponownie tych zasobów.

## <a name="next-steps"></a>Następne kroki
- [Jak zarządzać zasobami danych](data-catalog-how-to-manage.md)