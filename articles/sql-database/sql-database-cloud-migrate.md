---
title: "Migracja bazy danych programu SQL Server do usługi SQL Database | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej na temat migracji lokalnej bazy danych serwera SQL Server do bazy danych Azure SQL Database w chmurze. Narzędzia migracji bazy danych umożliwiają przetestowanie zgodności przed wykonaniem migracji bazy danych."
keywords: "migracja bazy danych,migracja bazy danych programu sql server,narzędzia migracji bazy danych,migracja bazy danych,migracja bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: d9052bd10693c0b7942c0d90fdf89be37b44842d
ms.openlocfilehash: b0093e48266aedda2b6c88b862c0056ebe3b3114


---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Migracja bazy danych programu SQL Server do usługi SQL Database w chmurze
Ten artykuł zawiera informacje o migracji lokalnej bazy danych programu SQL Server 2005 lub nowszej wersji do bazy danych Azure SQL Database. Proces migracji bazy danych obejmuje migrację schematu i danych z bazy danych programu SQL Server w bieżącym środowisku do usługi SQL Database. Aby migracja zakończyła się sukcesem, istniejąca baza danych musi najpierw przejść test zgodności. W przypadku wersji SQL Database V12 występuje niemal pełna [równowaga między funkcjami](sql-database-features.md), z wyjątkiem kwestii związanych z operacjami realizowanymi na poziomie serwera i między bazami danych. Bazy danych i aplikacje oparte na [częściowo obsługiwanych lub nieobsługiwanych funkcjach](sql-database-transact-sql-information.md) muszą zostać w pewnym stopniu przeprojektowane, aby usunąć niezgodności i umożliwić migrację bazy danych programu SQL Server.

Aby przeprowadzić migrację, należy wykonać następujące kroki:

* **Test zgodności**: sprawdzenie zgodności bazy danych z programem SQL Database. 
* **Rozwiązanie ewentualnych problemów ze zgodnością**: jeśli sprawdzenie poprawności zakończy się niepowodzeniem, należy rozwiązać problemy.  
* **Przeprowadzenie migracji** po zapewnieniu zgodności bazy danych można wykonać migrację przy użyciu jednej lub kilku metod. 

Program SQL Server udostępnia kilka metod wykonania każdego z tych zadań. Ten artykuł zawiera omówienie dostępnych metod realizacji poszczególnych zadań. Na poniższym diagramie przedstawiono kroki i metody.

  ![Diagram migracji VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> Aby wykonać migrację bazy danych innej niż baza danych programu SQL Server, w tym bazy danych Microsoft Access, Sybase, MySQL Oracle i DB2, do usługi Azure SQL Database, zobacz temat [Asystent migracji programu SQL Server](http://blogs.msdn.com/b/ssma/).
> 
> 

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Narzędzia migracji bazy danych umożliwiają przetestowanie zgodności bazy danych programu SQL Server z bazą danych usługi SQL Database
Aby sprawdzić problemy ze zgodnością bazy danych usługi SQL Database przed rozpoczęciem procesu migracji bazy danych, użyj jednej z następujących metod:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [SQL Server Data Tools dla programu Visual Studio („SSDT”)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): narzędzie SSDT korzysta z najnowszych reguł zgodności do wykrywania niezgodności bazy danych SQL Database V12. W przypadku wykrycia niezgodności można naprawić znalezione błędy bezpośrednio w tym narzędziu. Ta metoda jest zalecaną metodą testowania i rozwiązywania problemów ze zgodnością bazy danych SQL Database V12. 
* [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage to narzędzie wiersza polecenia, które wykrywa problemy ze zgodnością i generuje raport zawierający wykryte niezgodności. Jeśli używasz tego narzędzia, upewnij się, że używana jest najnowsza wersja korzystająca z najnowszych zasad zgodności. W przypadku wykrycia błędów należy użyć innego narzędzia, aby naprawić wszelkie wykryte problemy ze zgodnością. Zaleca się korzystanie z narzędzia SSDT.  
* [Kreator aplikacji warstwy danych eksportu w programie SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): ten kreator wykrywa błędy i wyświetla je na ekranie. Jeśli nie wykryto błędów, można kontynuować i przeprowadzić migrację do bazy danych usługi SQL Database. W przypadku wykrycia błędów należy użyć innego narzędzia, aby naprawić wszelkie wykryte problemy ze zgodnością. Zaleca się korzystanie z narzędzia SSDT.
* [Kreator migracji SQL Azure („SAMW”)](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW to narzędzie codeplex wykorzystujące reguły zgodności usługi Azure SQL Database V11 w celu wykrywania problemów ze zgodnością w wersji Azure SQL Database V12. W przypadku wykrycia niezgodności można naprawić niektóre znalezione błędy bezpośrednio w tym narzędziu. To narzędzie może wykrywać niezgodności, które nie wymagają naprawy. Jest to pierwsze z udostępnionych narzędzi ułatwiających migrację do bazy danych Azure SQL Database, które jest aktywnie wspierane przez społeczność użytkowników programu SQL Server. To narzędzie umożliwia przeprowadzenie migracji z jego poziomu. 

## <a name="fix-database-migration-compatibility-issues"></a>Rozwiązywanie problemów ze zgodnością migracji bazy danych
Jeśli zostaną wykryte problemy ze zgodnością, należy je naprawić przed przystąpieniem do migracji bazy danych programu SQL Server. Może wystąpić wiele różnych problemów ze zgodnością, w zależności od wersji programu SQL Server w źródłowej bazie danych oraz złożoności migrowanej bazy danych. Starsze wersje programu SQL Server mają więcej problemów ze zgodnością. Oprócz ukierunkowanego wyszukiwania w Internecie za pomocą preferowanej wyszukiwarki użyj następujących zasobów:

* [Funkcje bazy danych programu SQL Server nieobsługiwane przez usługę Azure SQL Database](sql-database-transact-sql-information.md)
* [Nieobsługiwane funkcje aparatu bazy danych w programie SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Nieobsługiwane funkcje aparatu bazy danych w programie SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Nieobsługiwane funkcje aparatu bazy danych w programie SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Nieobsługiwane funkcje aparatu bazy danych w programie SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Nieobsługiwane funkcje aparatu bazy danych w programie SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Oprócz wyszukiwania w Internecie i użycia wymienionych zasobów, możesz skorzystać z [forów społeczności programu SQL Server w sieci MSDN](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) lub witryny [StackOverflow](http://stackoverflow.com/).

Aby rozwiązać wykryte problemy, użyj jednego z następujących narzędzi do migracji baz danych:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* Korzystanie z narzędzi [SQL Server Data Tools dla programu Visual Studio („SSDT”)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): aby użyć narzędzi SSDT, zaimportuj schemat bazy danych do programu SQL Server Data Tools dla programu Visual Studio („SSDT”) i skompiluj projekt wdrożenia bazy danych SQL Database V12. Następnie rozwiąż wszystkie wykryte problemy ze zgodnością przy użyciu narzędzi SSDT. Po zakończeniu zsynchronizuj zmiany ze źródłową bazą danych (lub kopią źródłowej bazy danych). SSDT jest obecnie zalecaną metodą testowania i rozwiązywania problemów ze zgodnością bazy danych SQL Database V12. Użyj linku, aby zapoznać się z [omówieniem narzędzi SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
* Korzystanie z programu [SQL Server Management Studio („SSMS”)](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): używając programu SSMS, wykonaj polecenia języka Transact-SQL, aby naprawić błędy wykryte przy użyciu innego narzędzia. Ta metoda jest przeznaczona przede wszystkim dla użytkowników zaawansowanych, którzy chcą zmodyfikować schemat bazy danych bezpośrednio w źródłowej bazie danych. 
* Korzystanie z [Kreatora migracji SQL Azure („SAMW”)](sql-database-cloud-migrate-fix-compatibility-issues.md): aby użyć narzędzia SAMW, wygeneruj skrypt języka Transact-SQL ze źródłowej bazy danych. Kreator przekształca skrypt, jeśli jest to możliwe, aby zapewnić zgodność schematu z usługą SQL Database V12. Po zakończeniu kreator SAMW może nawiązać połączenie z usługą SQL Database V12, aby wykonać skrypt. To narzędzie analizuje również pliki śledzenia w celu określenia problemów ze zgodnością. Skrypt można wygenerować przy użyciu samego schematu lub można dołączyć dane w formacie BCP.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migracja zgodnej bazy danych programu SQL Server do usługi SQL Database
Aby wykonać migrację zgodnej bazy danych programu SQL Server, Microsoft zapewnia kilka metod migracji dla różnych scenariuszy. Wybór metody zależy od tolerancji na przestoje, wielkości i złożoności bazy danych programu SQL Server oraz łączności z chmurą Microsoft Azure.  

> [!div class="op_single_selector"]
> * [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Eksport do pliku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Import z pliku BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Replikacja transakcyjna](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

Aby wybrać metodę migracji, należy najpierw zadać sobie pytanie, czy można pozwolić sobie na wyłączenie tej bazy z systemu produkcyjnego na czas migracji. Migracja bazy danych w czasie, gdy są realizowane aktywne transakcje, może spowodować niespójności bazy danych, a nawet jej uszkodzenie. Istnieje wiele metod przełączenia bazy danych w stan spoczynku: od wyłączenia łączności klientów po utworzenie [migawki bazy danych](https://msdn.microsoft.com/library/ms175876.aspx).

Aby wykonać migrację z minimalnym przestojem, wykonaj [replikację transakcji programu SQL Server](sql-database-cloud-migrate-compatible-using-transactional-replication.md), o ile baza danych spełnia wymagania replikacji transakcyjnej. Jeżeli przestój jest dopuszczalny lub wykonywana jest testowa migracja produkcyjnej bazy danych do celów późniejszej migracji, można rozważyć użycie jednej z poniższych trzech metod:

* [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): w przypadku małych i średnich baz danych zgodnych z programem SQL Server 2005 lub nowszą wersją wystarczy użyć [Kreatora wdrożenia bazy danych w usłudze Microsoft Azure Database](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) w programie SQL Server Management Studio.
* [Eksport do pliku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md), a następnie [import z pliku BACPAC File](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): jeśli występują problemy z łącznością (brak połączenia, niska przepustowość lub problemy z upływem limitu czasu) w przypadku średnich i dużych baz danych, użyj pliku [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4). Ta metoda umożliwia eksport schematu programu SQL Server i danych do pliku BACPAC. Następnie należy zaimportować plik BACPAC do bazy danych SQL Database za pomocą Kreatora aplikacji warstwy danych eksportu w programie SQL Server Management Studio lub narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx).
* Łączne użycie metod BACPAC i BCP: w przypadku znacznie większych baz danych można użyć pliku [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) i [BCP](https://msdn.microsoft.com/library/ms162802.aspx), aby uzyskać wyższy stopień równoległości i zwiększyć wydajność, jednak przy większej złożoności zadania. W przypadku tej metody należy wykonać oddzielne migracje schematu i danych.
  
  * [Wyeksportuj sam schemat do pliku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
  * [Zaimportuj tylko schemat z pliku BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) do bazy danych SQL Database.
  * Użyj [BCP](https://msdn.microsoft.com/library/ms162802.aspx) do wyodrębnienia danych do plików prostych i [załaduj równolegle](https://technet.microsoft.com/library/dd425070.aspx) te pliki do bazy danych Azure SQL Database.
    
     ![Migracja bazy danych programu SQL Server — migracja bazy danych SQL do chmury.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Następne kroki
* [Najnowsza wersja narzędzi SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Funkcje programu SQL Database](sql-database-features.md)
  [Częściowo obsługiwane lub nieobsługiwane funkcje języka Transact-SQL](sql-database-transact-sql-information.md)
* [Migracja baz danych innych niż bazy danych programu SQL Server przy użyciu Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Dec16_HO5-->


