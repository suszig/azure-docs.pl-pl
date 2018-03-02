---
title: "Usługa Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat warstwy usługi dla pojedynczej i baz danych puli zapewnienie poziomy wydajności i rozmiaru magazynu."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 02/26/2018
ms.author: carlrab
ms.openlocfilehash: b36af32d900f9426424dd08c43946e7dcb5b39b9
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Co to są warstwach usług bazy danych SQL Azure?

[Baza danych SQL Azure](sql-database-technical-overview.md) oferuje **podstawowe**, **standardowe**, i **Premium** warstw usług dla obu [baz danych z jednym](sql-database-single-database-resources.md) i [pule elastyczne](sql-database-elastic-pool.md). Warstwy usług są zróżnicowane głównie przez zakres poziom wydajności i opcje rozmiaru magazynu i cenę.  Wszystkie warstwy usług zapewniają elastyczność w Zmiana wydajności poziom i rozmiaru magazynu.  Pojedyncze bazy danych i pul elastycznych są rozliczane co godzinę na podstawie warstwy usług, poziom wydajności i rozmiar magazynu.   

## <a name="choosing-a-service-tier"></a>Wybieranie warstwy usług

Wybór warstwy usług zależy przede wszystkim ciągłość prowadzenia działalności biznesowej, magazynu i wymagania dotyczące wydajności.
| | **Podstawowa** | **Standardowa** |**Premium**  |
| :-- | --: |--:| --:| --:| 
|Obciążenie docelowego|Projektowanie i produkcji|Projektowanie i produkcji|Projektowanie i produkcji||
|Umowa SLA dotycząca czasu dostępności|99,99%|99,99%|99,99%|N/d znajduje się w wersji zapoznawczej|
|Przechowywanie kopii zapasowych|7 dni|35 dni|35 dni|
|Procesor CPU|Małe|Niski, Średni, wysoki|Średni i wysoki|
|Wydajność We/Wy (przybliżony) |2.5 IOPS dla jednostek dtu w warstwie  | 2.5 IOPS dla jednostek dtu w warstwie | 48 IOPS dla jednostek dtu w warstwie|
|We/Wy, czas oczekiwania (w przybliżeniu)|ms 5 (odczyt), 10 ms (Zapisz)|ms 5 (odczyt), 10 ms (Zapisz)|ms 2 (odczyt/zapis)|
|Indeksowanie magazynu kolumn i OLTP w pamięci|ND|ND|Obsługiwane|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Poziom wydajności i magazynu limity rozmiaru

Poziomy wydajności są wyrażane pod względem jednostki transakcji bazy danych (Dtu) dla pojedynczej bazy danych i jednostek transakcji elastycznej bazy danych (Edtu) dla puli elastycznej. Aby uzyskać więcej informacji na temat jednostek Dtu a Edtu, zobacz [co to są Dtu a Edtu?](sql-database-what-is-a-dtu.md)

### <a name="single-databases"></a>Pojedyncze bazy danych

|  | **Podstawowa** | **Standardowa** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maksymalny rozmiar * | 2 GB | 1 TB | 4 TB  | 
| Maksymalna Dtu | 5 | 3000 | 4000 | |
||||||

### <a name="elastic-pools"></a>Pule elastyczne

| | **Podstawowa** | **Standardowa** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maksymalny rozmiar magazynu na bazie danych *  | 2 GB | 1 TB | 1 TB | 
| Maksymalny rozmiar magazynu na puli * | 156 GB | 4 TB | 4 TB | 
| Maksymalna liczba jednostek Edtu na bazę danych | 5 | 3000 | 4000 | 
| Maksymalna liczba jednostek Edtu na pulę | 1600 | 3000 | 4000 | 
| Maksymalna liczba baz danych dla każdej puli | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \* Magazyn o rozmiarze większym niż ilość miejsca do magazynowania są dostępne w wersji zapoznawczej dodatkowych kosztów za dodatkową opłatą. Szczegóły można znaleźć w [cenniku usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* W warstwie Premium ponad 1 TB miejsca do magazynowania jest obecnie dostępny w następujących regionach: Australia Wschodnia, Australia Południowo-Wschodnia, Brazylia Południowa, Kanada Środkowa, Kanada Wschodnia, Środkowe stany USA, Francja Środkowa, Niemcy Środkowe, Japonia Wschodnia, Japonia Zachodnia, Korea Środkowa, Północno-środkowe stany USA, Europa Północna, Południowo-środkowe stany USA, Azja Południowo-Wschodnia, Południowe Zjednoczone Królestwo, Zachodnie Zjednoczone Królestwo, Wschodnie stany USA 2, Zachodnie stany USA, Administracja USA — Wirginia i Europa Zachodnia. Więcej informacji można znaleźć na stronie [bieżących ograniczeń poziomów P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Aby uzyskać więcej informacji o określonych poziomów wydajności i dostępnych wyborów rozmiar magazynu, zobacz [limity zasobów bazy danych SQL](sql-database-resource-limits.md).


## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o [pojedyncze bazy danych zasobów](sql-database-single-database-resources.md).
- Dowiedz się więcej o pule elastyczne, zobacz [pule elastyczne](sql-database-elastic-pool.md).
- Dowiedz się więcej o [subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../azure-subscription-service-limits.md)
* Dowiedz się więcej o [Dtu a Edtu](sql-database-what-is-a-dtu.md).
* Więcej informacji na temat monitorowania użycie jednostek DTU, zobacz [monitorowania i dostrajania wydajności](sql-database-troubleshoot-performance.md).

