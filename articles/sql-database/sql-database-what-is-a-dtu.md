---
title: "Usługa SQL Database: Co to jest jednostka DTU? | Microsoft Docs"
description: "Opis jednostki transakcji usługi Azure SQL Database."
keywords: "opcje bazy danych, wydajność bazy danych"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: 89e3e9ce-2eeb-4949-b40f-6fc3bf520538
ms.service: sql-database
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/06/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 91942517a481a252d631af15e532389256b64108


---
# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Wyjaśnienie jednostek DTU (Database Transaction Units) i eDTU (elastic Database Transaction Units)
W tym artykule wyjaśniono jednostki DTU (Database Transaction Unit) i jednostki eDTU (elastic Database Transaction Unit) oraz skutki osiągnięcia maksymalnej liczby jednostek DTU lub eDTU.  

## <a name="what-are-database-transaction-units-dtus"></a>Co to są jednostki DTU (Database Transaction Unit)
Jednostka DTU jest jednostką miary zasobów o gwarantowanej dostępności dla autonomicznej bazy danych SQL platformy Azure na określonym poziomie wydajności w [warstwie usługi autonomicznych baz danych](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). Jednostka DTU stanowi wskaźnik wydajności będący połączeniem mocy procesora CPU, pamięci i wydajności operacji we/wy na danych określany na podstawie porównawczego obciążenia OLTP zaprojektowanego w taki sposób, aby odpowiadało ono typowym, rzeczywistym obciążeniom OLTP. Podwojenie liczby jednostek DTU przez zwiększenie poziomu wydajności bazy danych odpowiada dwukrotnemu zwiększeniu zestawu zasobów dostępnych dla tej bazy danych. Na przykład baza danych Premium P11 z 1750 jednostkami DTU zapewnia 350 razy więcej mocy obliczeniowej DTU niż podstawowa baza danych z 5 jednostkami DTU. Aby zrozumieć metodologię obciążenia porównawczego OLTP używanego w celu określenia połączonego wskaźnika w postaci jednostek DTU, zobacz [Omówienie testu porównawczego usługi SQL Database](sql-database-benchmark-overview.md).

![Wprowadzenie do usługi SQL Database: jednostki DTU z jedną bazą danych według warstwy i poziomu.](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Możesz [zmienić warstwy usług](sql-database-scale-up.md) w dowolnym momencie przy minimalnych przestojach w działaniu aplikacji (zwykle poniżej czterech sekund). W przypadku wielu firm i aplikacji możliwość tworzenia baz danych i dostosowywania wydajności pojedynczej aplikacji na żądanie jest wystarczająca, zwłaszcza jeśli wzorce użycia są względnie przewidywalne. Jednak w przypadku nieprzewidywalnych wzorców zarządzanie kosztami i modelem biznesowym może być trudne. W tym scenariuszu należy użyć puli elastycznej z określoną liczbą jednostek eDTU.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Co to są jednostki eDTU (elastic Database Transaction Unit)
Jednostka eDTU jest jednostką miary zestawu zasobów (DTU), które mogą być współdzielone między bazami danych na serwerze SQL platformy Azure, nazywanego [pulą elastyczną](sql-database-elastic-pool.png). Pule elastyczne zapewniają proste i ekonomiczne rozwiązanie umożliwiające zarządzanie celami wydajności dla wielu baz danych o znacznie zróżnicowanych i nieprzewidywalnych wzorcach użycia. Aby uzyskać więcej informacji, zobacz [pule elastyczne i warstwy usług](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus).

![Wprowadzenie do usługi SQL Database: jednostki eDTU według warstwy i poziomu](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Puli jest przydzielana określona liczba jednostek eDTU za określoną cenę. Poszczególne bazy danych w ramach puli mają możliwość elastycznego skalowania automatycznego w określonym zakresie parametrów. W przypadku dużego obciążenia baza danych może wykorzystywać więcej jednostek eDTU w odpowiedzi na zapotrzebowanie. Bazy danych pod niskim obciążeniem wykorzystują mniej jednostek eDTU, a bazy danych bez obciążenia nie zużywają jednostek eDTU. Aprowizacja zasobów dla całej puli zamiast pojedynczych baz danych upraszcza zadania związane z zarządzaniem. Ponadto pule mają przewidywalny budżet.

Dodatkowe jednostki eDTU można dodać do istniejącej puli bez przerwy w działaniu baz danych ani wpływu na bazy danych w puli elastycznej. Podobnie, jeśli dodatkowe jednostki eDTU nie są już potrzebne, można je usunąć z istniejącej puli w dowolnej chwili. Można dodawać bazy danych do puli lub odejmować je z niej. Jeśli baza danych przewidywalnie niewystarczająco wykorzystuje zasoby, należy ją przenieść.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Jak określić wymaganą liczbę jednostek DTU dla mojego obciążenia?
Jeśli chcesz zmigrować istniejące obciążenie lokalne lub obciążenie maszyny wirtualnej programu SQL Server do usługi Azure SQL Database, możesz skorzystać z [Kalkulatora jednostek DTU ](http://dtucalculator.azurewebsites.net/), aby obliczyć przybliżoną liczbę wymaganych jednostek DTU. Dla istniejącego obciążenia usługi Azure SQL Database można użyć narzędzia [Szczegółowe informacje o wydajności zapytań usługi SQL Database](sql-database-query-performance.md), aby poznać użycie zasobów bazy danych (jednostek DTU) w celu uzyskania bardziej szczegółowych informacji umożliwiających zoptymalizowanie obciążenia. Informacje o użyciu zasobów w ostatniej godzinie można również uzyskać przy użyciu dynamicznego widoku zarządzania [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx). Alternatywnie można przesłać zapytanie widoku katalogu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx), aby uzyskać te dane dla ostatnich 14 dni, chociaż o mniejszej dokładności wynoszącej średnią z pięciu minut.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Jak sprawdzić, czy elastyczna pula zasobów jest dla mnie korzystnym rozwiązaniem?
Pule są odpowiednie dla wielu baz danych o określonych wzorcach użycia. Dla danej bazy danych ten wzorzec charakteryzuje się niskim średnim wykorzystaniem oraz stosunkowo rzadkimi okresami zwiększonego użycia. Usługa SQL Database automatycznie ocenia historyczne użycie zasobów przez bazy danych na istniejącym serwerze usługi SQL Database i zaleca odpowiednią konfigurację puli w witrynie Azure Portal. Aby uzyskać więcej informacji, zobacz [kiedy należy użyć elastycznej puli baz danych](sql-database-elastic-pool-guidance.md).

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Co się stanie po osiągnięciu limitu jednostek DTU?
Poziomy wydajności są kalibrowane i określane w celu zapewnienia zasobów wymaganych do uruchomienia obciążenia baz danych do maksymalnego limitu dozwolonego dla wybranej warstwy usługi/poziomu wydajności. Jeśli obciążenie powoduje osiągnięcie limitu procesora CPU, operacji we/wy danych lub operacji we/wy dziennika, nadal zapewniane będą zasoby na maksymalnym dozwolonym poziomie, ale mogą wystąpić opóźnienia zapytań. Te limity nie powodują występowania błędów, ale raczej spowalniają działanie obciążenia, o ile obciążenie nie stanie się tak duże, by powodować przekroczenie limitu czasu zapytań. W przypadku osiągnięcia limitów maksymalnej dozwolonej liczby równoczesnych sesji/żądań użytkowników (wątków roboczych) występują jawne błędy. Zobacz [Limity zasobów usługi Azure SQL Database](sql-database-resource-limits.md), aby uzyskać informacje na temat limitów zasobów innych niż procesor CPU, pamięć, operacje we/wy danych oraz operacje we/wy dziennika transakcji.

## <a name="next-steps"></a>Następne kroki
* Zobacz [Warstwy usługi](sql-database-service-tiers.md), aby uzyskać informacje na temat jednostek DTU i eDTU dostępnych dla autonomicznych baz danych i pul elastycznych.
* Zobacz [Limity zasobów usługi Azure SQL Database](sql-database-resource-limits.md), aby uzyskać informacje na temat limitów zasobów innych niż procesor CPU, pamięć, operacje we/wy danych oraz operacje we/wy dziennika transakcji.
* Zobacz [Szczegółowe informacje o wydajności zapytań usługi SQL Database](sql-database-query-performance.md), aby zrozumieć wykorzystanie jednostek DTU.
* Zobacz [Omówienie testu porównawczego usługi SQL Database](sql-database-benchmark-overview.md), aby zrozumieć metodologię obciążenia porównawczego OLTP używanego w celu określenia połączonego wskaźnika w postaci jednostek DTU.




<!--HONumber=Nov16_HO2-->


