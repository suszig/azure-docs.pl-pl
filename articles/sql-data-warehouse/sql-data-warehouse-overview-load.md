---
title: "Ładowanie danych do usługi Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Dowiedz się typowe scenariusze dotyczące danych ładowania do usługi SQL Data Warehouse. Obejmują one przy użyciu programu PolyBase, magazynu obiektów blob platformy Azure, plików prostych i wysyłania dysku. Można także użyć narzędzi innych firm."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Ładowanie danych do usługi Azure SQL Data Warehouse
Podsumowanie opcji scenariusza i zalecenia dotyczące ładowania danych do usługi SQL Data Warehouse.

Najtrudniejsze część podczas ładowania danych jest zwykle Przygotowanie danych obciążenia. Ładowanie upraszcza Azure przy użyciu magazynu obiektów blob platformy Azure jako magazynu danych wspólne dla wielu usług i fabryki danych Azure do organizowania przepływu danych i komunikacji między usługami Azure. Te procesy są zintegrowane z technologii PolyBase, który używa masowego przetwarzania równoległego (MPP) do ładowania danych równolegle z magazynu obiektów blob platformy Azure do usługi SQL Data Warehouse. 

Samouczki, które ładują przykładowe bazy danych, zobacz [załadować przykładowe bazy danych][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Ładowanie z magazynu obiektów blob platformy Azure
Jest to najszybszy sposób importowania danych do usługi SQL Data Warehouse ładowanie danych z magazynu obiektów blob platformy Azure przy użyciu programu PolyBase. Program PolyBase używa projektu masowego przetwarzania równoległego (MPP) magazynu danych SQL do ładowania danych równolegle z magazynu obiektów blob platformy Azure. Aby użyć programu PolyBase, można użyć polecenia T-SQL lub potoku fabryki danych Azure.

### <a name="1-use-polybase-and-t-sql"></a>1. Użyj programu PolyBase i T-SQL
Podsumowanie procesu ładowania:

1. Przenoszenie danych do magazynu obiektów blob platformy Azure lub usługi Azure Data Lake Store i zapisze go w plikach tekstowych.
2. Skonfiguruj obiekty zewnętrzne w magazynie danych SQL, aby określić lokalizację i format danych
3. Uruchom polecenie T-SQL, aby załadować dane jednocześnie do nowej tabeli bazy danych.

<!-- 5. Schedule and run a loading job. --> 

Samouczek, zobacz [ładowanie danych z magazynu obiektów blob platformy Azure SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Używanie usługi Azure Data Factory
W prostszy sposób Użyj programu PolyBase można utworzyć potok fabryki danych Azure, który używa programu PolyBase do ładowania danych z magazynu obiektów blob platformy Azure do usługi SQL Data Warehouse. Jest to szybkie do skonfigurowania, ponieważ nie ma potrzeby definiowania obiektów T-SQL. Jeśli potrzebujesz kwerendy danych zewnętrznych nie jest importowany, użyj T-SQL. 

Podsumowanie procesu ładowania:

1. Przenoszenie danych do magazynu obiektów blob platformy Azure i zapisze go w plikach tekstowych. Fabryka danych Azure nie obsługuje obecnie łączności ADLS przy użyciu programu PolyBase).
2. Utworzyć potok fabryki danych Azure pozyskiwanie danych. Opcja PolyBase.
4. Planowanie i uruchamianie potoku.

Samouczek, zobacz [ładowanie danych z magazynu obiektów blob platformy Azure SQL Data Warehouse (fabryka danych Azure)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Ładowanie z programu SQL Server
Aby załadować dane z programu SQL Server do usługi SQL Data Warehouse można użyć Integration Services (SSIS), transfer plików prostych lub dysków dostawy do firmy Microsoft. Odczyt do zawiera podsumowanie różnic ładowania procesy i łącza do samouczki.

Aby zaplanować migracji danych z programu SQL Server do usługi SQL Data Warehouse, zobacz [Omówienie migracji][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Użyj usług Integration Services (SSIS)
Jeśli korzystasz już z pakietów Integration Services (SSIS) do załadowania do serwera SQL Server, należy zaktualizować pakiety używać programu SQL Server jako źródła i magazynu danych SQL jako miejsca docelowego. Jest to szybki i łatwy w celu i jest dobrym rozwiązaniem, jeśli nie chcesz migrować procesu ładowania, aby użyć danych już w chmurze. Zależności to obciążenie będzie przebiegać wolniej niż przy użyciu programu PolyBase, ponieważ ta SSIS nie przeprowadza obciążenia równolegle.

Podsumowanie procesu ładowania:

1. Sprawdź, czy pakietu usług Integration Services, aby wskazywała wystąpienie serwera SQL dla źródła i bazy danych magazynu danych SQL dla miejsca docelowego.
2. Migrację schematu SQL Data Warehouse, jeśli nie jest już.
3. Zmień mapowanie w pakietach Użyj tylko typy danych, które są obsługiwane przez usługi SQL Data Warehouse.
4. Planowanie i uruchamianie pakietu.

Samouczek, zobacz [ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Przy użyciu programu AZCopy (zalecane dla < 10 TB danych)
Jeśli rozmiar danych jest < 10 TB, można wyeksportować dane z programu SQL Server do plików prostych, skopiuj pliki do magazynu obiektów blob platformy Azure i następnie użyj programu PolyBase, aby załadować dane do magazynu danych SQL

Podsumowanie procesu ładowania:

1. Użyj narzędzia wiersza polecenia bcp do eksportowania danych z programu SQL Server do plików prostych.
2. Użyj narzędzia wiersza polecenia AZCopy do kopiowania danych z plików prostych do magazynu obiektów blob platformy Azure.
3. Użyj programu PolyBase, aby załadować do usługi SQL Data Warehouse.

Samouczek, zobacz [ładowanie danych z magazynu obiektów blob platformy Azure SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Użyj narzędzia bcp
Jeśli masz niewielką ilość danych można użyć bcp, aby załadować bezpośrednio do usługi Azure SQL Data Warehouse.

Podsumowanie procesu ładowania:

1. Użyj narzędzia wiersza polecenia bcp do eksportowania danych z programu SQL Server do plików prostych.
2. Użyj narzędzia bcp do ładowania danych z plików prostych bezpośrednio do usługi SQL Data Warehouse.

Samouczek, zobacz [ładowania danych z programu SQL Server do usługi Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Użyj importu/eksportu (zalecane dla > 10 TB danych)
Jeśli rozmiar danych jest > 10 TB i chcesz przenieść ją do platformy Azure, zalecane jest użycie naszych dysku wysyłania usługi [importu/eksportu][Import/Export]. 

Podsumowanie procesu ładowania

1. Użyj narzędzia wiersza polecenia bcp do eksportowania danych z programu SQL Server do plików prostych możliwej dysków.
2. Należy najpierw wydać dysków do firmy Microsoft.
3. Microsoft ładuje dane do magazynu danych SQL

## <a name="load-from-hdinsight"></a>Ładowanie z usługi HDInsight
Magazyn danych SQL obsługuje ładowanie danych z usługi HDInsight przy użyciu programu PolyBase. Proces jest taki sam, jak podczas ładowania danych z magazynu obiektów Blob platformy Azure — przy użyciu programu PolyBase do nawiązania połączenia usługi HDInsight można załadować danych. 

### <a name="1-use-polybase-and-t-sql"></a>1. Użyj programu PolyBase i T-SQL
Podsumowanie procesu ładowania:

1. Przenoszenie danych do usługi HDInsight i zapisze go w plikach tekstowych ORC lub Parquet format.
2. Skonfiguruj obiekty zewnętrzne w magazynie danych SQL, aby określić lokalizację i format danych.
3. Uruchom polecenie T-SQL, aby załadować dane jednocześnie do nowej tabeli bazy danych.

Samouczek, zobacz [ładowanie danych z magazynu obiektów blob platformy Azure SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Zalecenia
Wiele naszych partnerów ma ładowania rozwiązania. Aby dowiedzieć się więcej, zobacz listę naszych [rozwiązania partnerów][solution partners]. 

Dane pochodzące ze źródłem danych nierelacyjnych i chcesz załadować go do magazynu danych SQL należy przekształcić w wiersze i kolumny, przed rozpoczęciem ładowania. Przekształcone dane nie mają być przechowywane w bazie danych, mogą być przechowywane w plikach tekstowych.

Tworzenie statystyk na nowo załadowanych danych. Usługa Azure SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk.  Aby uzyskać najlepszą wydajność zapytań, ważne jest, aby utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu lub występują znaczne zmiany w danych.  Aby uzyskać więcej informacji, zobacz [statystyki][Statistics].

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [omówienie tworzenia][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
