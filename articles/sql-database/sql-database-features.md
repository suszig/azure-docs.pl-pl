---
title: "Omówienie funkcji usługi Azure SQL Database | Microsoft Docs"
description: "Ta strona zawiera omówienie serwerów logicznych i baz danych usługi Azure SQL Database, a także opis macierzy przedstawiającej obsługę funkcji wraz z linkami do każdej wymienionej funkcji."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: e052109ff10f21c3a737f336479fe43fb54e500e
ms.lasthandoff: 03/14/2017


---
# <a name="azure-sql-database-features"></a>Funkcje usługi Azure SQL Database
Ten temat zawiera omówienie serwerów logicznych i baz danych usługi Azure SQL Database, a także opis macierzy przedstawiającej obsługę funkcji wraz z linkami do każdej wymienionej funkcji. 

## <a name="what-is-an-azure-sql-database-logical-server"></a>Co to jest serwer logiczny usługi Azure SQL Database?
Serwer logiczny usługi Azure SQL Database działa jako centralny punkt administracyjny dla wielu baz danych. W usłudze SQL Database serwer jest konstrukcją logiczną, inną niż wystąpienie programu SQL Server, z którym można zapoznać się w środowisku lokalnym. W szczególności usługa SQL Database nie udziela żadnych gwarancji dotyczących lokalizacji baz danych w odniesieniu do ich serwerów logicznych i nie uwidacznia dostępu ani żadnych funkcji na poziomie wystąpienia. Aby uzyskać więcej informacji o serwerach logicznych Azure SQL, zobacz [Serwery logiczne](sql-database-server-overview.md). 

## <a name="what-is-an-azure-sql-database"></a>Co to jest baza danych Azure SQL?
Każda baza danych w usłudze Azure SQL Database jest skojarzona z serwerem logicznym. Baza danych może być:

- pojedynczą bazą danych z [własnym zestawem zasobów](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (jednostki DTU).
- częścią [puli baz danych](sql-database-elastic-pool.md), która [współużytkuje zestaw zasobów](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (jednostki eDTU).
- częścią [skalowanego zestawu baz danych podzielonych na fragmenty](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), które mogą być pojedynczymi bazami danych lub pulami baz danych.
- częścią zestawu baz danych uczestniczącego we [wzorcu projektowym wielodostępnych aplikacji SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md), którego bazy danych mogą być pojedynczymi bazami danych i/lub pulami baz danych. 

Aby uzyskać więcej informacji o bazach danych SQL Azure, zobacz temat [Bazy danych SQL](sql-database-overview.md).

## <a name="what-features-are-supported"></a>Jakie funkcje są obsługiwane?

W poniższych tabelach przedstawiono główne funkcje usługi Azure SQL Database i programu SQL Server. Określono możliwość ich obsługi, a także zawarto linki umożliwiające uzyskanie dodatkowych informacji o danej funkcji na każdej platformie. Aby uzyskać informacje o funkcjach języka Transact-SQL, skorzystaj z umieszczonego w tabeli linku dla kategorii odpowiadającej danej funkcji. Zobacz też [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md), aby uzyskać więcej informacji na temat przyczyn braku obsługi niektórych typów funkcji.

W dalszym ciągu dodajemy funkcje do wersji V12. Zachęcamy zatem do odwiedzania naszej strony poświęconej aktualizacjom usług dla platformy Azure i korzystania z dostępnych tam filtrów:

* Włączony filtr [usługi SQL Database](https://azure.microsoft.com/updates/?service=sql-database).
* Włączony filtr [ogłoszeń dotyczących ogólnej dostępności](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) funkcji usługi SQL Database.

> [!TIP]
> Aby przetestować istniejącą bazę danych pod kątem zgodności z usługą Azure SQL Database, zobacz temat [Migrate a SQL Server database to Azure](sql-database-cloud-migrate.md) (Migrowanie bazy danych programu SQL Server na platformę Azure).
>

| **Funkcja** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Aktywna replikacja geograficzna | Nieobsługiwane — zobacz temat [AlwaysOn Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) (Zawsze włączone grupy dostępności) | [Obsługiwane](sql-database-geo-replication-overview.md)
| Zawsze szyfrowane | [Obsługiwane](https://msdn.microsoft.com/library/mt163865.aspx) | [Obsługiwane](sql-database-always-encrypted.md) |
| Zawsze włączone grupy dostępności | [Obsługiwane](https://msdn.microsoft.com/library/hh510230.aspx) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Dołączanie bazy danych | [Obsługiwane](https://msdn.microsoft.com/library/ms190209.aspx) | Nieobsługiwane |
| Role aplikacji | [Obsługiwane](https://msdn.microsoft.com/library/ms190998.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms190998.aspx) |
| Automatyczne skalowanie | Nieobsługiwane | [Obsługiwane](sql-database-service-tiers.md) |
| Usługa Azure Active Directory | Nieobsługiwane | [Obsługiwane](sql-database-aad-authentication.md) |
| Azure Data Factory | [Obsługiwane](../data-factory/data-factory-introduction.md) | [Obsługiwane](../data-factory/data-factory-introduction.md) |
| Inspekcja | [Obsługiwane](https://msdn.microsoft.com/library/cc280386.aspx) | [Obsługiwane](sql-database-auditing.md) |
| Plik BACPAC (eksportowanie) | [Obsługiwane](https://msdn.microsoft.com/library/hh213241.aspx) | [Obsługiwane](sql-database-export.md) |
| Plik BACPAC (importowanie) | [Obsługiwane](https://msdn.microsoft.com/library/hh710052.aspx) | [Obsługiwane](sql-database-import-portal.md) |
| Instrukcje BACKUP i RESTORE | [Obsługiwane](https://msdn.microsoft.com/library/ff848768.aspx) | Nieobsługiwane |
| Funkcje wbudowane | [Obsługiwane](https://msdn.microsoft.com/library/ms174318.aspx) | [Większość](https://msdn.microsoft.com/library/ms174318.aspx) |
| Przechwytywanie danych dotyczących zmian | [Obsługiwane](https://msdn.microsoft.com/library/cc645937.aspx) | Nieobsługiwane |
| Śledzenie zmian | [Obsługiwane](https://msdn.microsoft.com/library/bb933875.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/bb933875.aspx) |
| Instrukcje sortowania | [Obsługiwane](https://msdn.microsoft.com/library/ff848763.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ff848763.aspx) |
| Indeksy magazynu kolumn | [Obsługiwane](https://msdn.microsoft.com/library/gg492088.aspx) | [Tylko w wersji Premium](https://msdn.microsoft.com/library/gg492088.aspx) |
| Środowisko uruchomieniowe języka wspólnego (CLR) | [Obsługiwane](https://msdn.microsoft.com/library/ms131102.aspx) | Nieobsługiwane |
| Zawarte bazy danych | [Obsługiwane](https://msdn.microsoft.com/library/ff929071.aspx) | Wbudowane |
| Zawarci użytkownicy | [Obsługiwane](https://msdn.microsoft.com/library/ff929188.aspx) | [Obsługiwane](sql-database-manage-logins.md#non-administrator-users) |
| Słowa kluczowe języka sterowania przepływem | [Obsługiwane](https://msdn.microsoft.com/library/ms174290.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms174290.aspx) |
| Zapytania obejmujące wiele baz danych | [Obsługiwane](https://msdn.microsoft.com/library/dn584627.aspx) | [Zapytania elastyczne](sql-database-elastic-query-overview.md) |
| Kursory | [Obsługiwane](https://msdn.microsoft.com/library/ms181441.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms181441.aspx) | 
| Kompresja danych | [Obsługiwane](https://msdn.microsoft.com/library/cc280449.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/cc280449.aspx) |
| Kopie zapasowe bazy danych | [Udostępnione dla użytkowników](https://msdn.microsoft.com/library/ms187048.aspx) | [Wbudowane](sql-database-automated-backups.md) |
| Poczta bazy danych | [Obsługiwane](https://msdn.microsoft.com/library/ms189635.aspx) | Nieobsługiwane |
| Dublowanie bazy danych | [Obsługiwane](https://msdn.microsoft.com/library/ms189852.aspx) | Nieobsługiwane |
| Opcje konfiguracji bazy danych | [Obsługiwane](https://msdn.microsoft.com/library/mt629158.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/mt629158.aspx) |
| Usługi Data Quality Services (DQS) | [Obsługiwane](https://msdn.microsoft.com/library/ff877925.aspx) | Nieobsługiwane |
| Migawki baz danych | [Obsługiwane](https://msdn.microsoft.com/library/ms175158.aspx) | Nieobsługiwane |
| Typy danych | [Obsługiwane](https://msdn.microsoft.com/library/ms187752.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms187752.aspx) |  
| Instrukcje DBCC | [Wszystkie](https://msdn.microsoft.com/library/ms188796.aspx) | [Niektóre](https://msdn.microsoft.com/library/ms188796.aspx) |
| Instrukcje języka DDL | [Obsługiwane](https://msdn.microsoft.com/library/ff848799.aspx) | [Większość](https://msdn.microsoft.com/library/ff848799.aspx)
| Wyzwalacze języka DDL | [Obsługiwane](https://msdn.microsoft.com/library/ms175941.aspx) | [Tylko dla bazy danych](https://msdn.microsoft.com/library/ms175941.aspx) |
| Transakcje rozproszone | [Narzędzie MS DTC](https://msdn.microsoft.com/library/ms131665.aspx) | Tylko ograniczone scenariusze wewnątrz usługi SQL Database |
| Instrukcje języka DML | [Obsługiwane](https://msdn.microsoft.com/library/ff848766.aspx) | [Większość](https://msdn.microsoft.com/library/ff848766.aspx) |
| Wyzwalacze języka DML | [Obsługiwane](https://msdn.microsoft.com/library/ms178110.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms178110.aspx) |
| Dynamiczne widoki zarządzania | [Wszystkie](https://msdn.microsoft.com/library/ms188754.aspx) | [Niektóre](https://msdn.microsoft.com/library/ms188754.aspx) |
| pule elastyczne | Nieobsługiwane | [Obsługiwane](sql-database-elastic-pool.md) |
| Zadania elastyczne | Nieobsługiwane — zobacz temat [SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Obsługiwane](sql-database-elastic-jobs-getting-started.md) | 
| Zapytania elastyczne | Nieobsługiwane — zobacz temat [Cross-database queries](https://msdn.microsoft.com/library/dn584627.aspx) (Zapytania obejmujące wiele baz danych) | [Obsługiwane](sql-database-elastic-query-overview.md) |
| Powiadomienia o zdarzeniach | [Obsługiwane](https://msdn.microsoft.com/library/ms186376.aspx) | [Obsługiwane](sql-database-insights-alerts-portal.md) |
| Wyrażenia | [Obsługiwane](https://msdn.microsoft.com/library/ms190286.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms190286.aspx) |
| Zdarzenia rozszerzone | [Obsługiwane](https://msdn.microsoft.com/library/bb630282.aspx) | [Niektóre](sql-database-xevent-db-diff-from-svr.md) |
| Rozszerzone procedury składowane | [Obsługiwane](https://msdn.microsoft.com/library/ms164627.aspx) | Nieobsługiwane |
| Grupy plików | [Obsługiwane](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) | [Tylko podstawowe](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) |
| Strumień pliku | [Obsługiwane](https://msdn.microsoft.com/library/gg471497.aspx) | Nieobsługiwane |
| Wyszukiwanie pełnotekstowe | [Obsługiwane](https://msdn.microsoft.com/library/ms142571.aspx) | [Nieobsługiwane moduły dzielenia wyrazów innych firm](https://msdn.microsoft.com/library/ms142571.aspx) |
| Funkcje | [Obsługiwane](https://msdn.microsoft.com/library/ms174318.aspx) | [Większość](https://msdn.microsoft.com/library/ms174318.aspx) |
| Optymalizacja w pamięci | [Obsługiwane](https://msdn.microsoft.com/library/dn133186.aspx) | [Tylko w wersji Premium](https://msdn.microsoft.com/library/dn133186.aspx) |
| Zadania | [Program SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Obsługiwane](sql-database-elastic-jobs-getting-started.md) |
| Obsługa danych JSON | [Obsługiwane](https://msdn.microsoft.com/library/dn921897.aspx) | [Obsługiwane](sql-database-json-features.md) |
| Elementy języka | [Obsługiwane](https://msdn.microsoft.com/library/ff848807.aspx) | [Większość](https://msdn.microsoft.com/library/ff848807.aspx) |  
| Serwery połączone | [Obsługiwane](https://msdn.microsoft.com/library/ms188279.aspx) | Nieobsługiwane — zobacz temat [Elastic query](sql-database-elastic-query-horizontal-partitioning.md) (Zapytanie elastyczne) |
| Wysyłanie dziennika | [Obsługiwane](https://msdn.microsoft.com/library/ms187103.aspx) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Polecenia zarządzania | [Obsługiwane](https://msdn.microsoft.com/library/ms190286.aspx)| [Nieobsługiwane](https://msdn.microsoft.com/library/ms190286.aspx) |
| Usługi Master Data Services (MDS) | [Obsługiwane](https://msdn.microsoft.com/library/ff487003.aspx) | Nieobsługiwane |
| Rejestrowanie minimalne w imporcie zbiorczym | [Obsługiwane](https://msdn.microsoft.com/library/ms190422.aspx) | Nieobsługiwane |
| Modyfikowanie danych systemowych | [Obsługiwane](https://msdn.microsoft.com/library/ms178028.aspx) | Nieobsługiwane |
| Operacje indeksowania w trybie online | [Obsługiwane](https://msdn.microsoft.com/library/ms177442.aspx) | [Rozmiar transakcji ograniczony przez warstwę usług](https://msdn.microsoft.com/library/ms177442.aspx) |
| Operatory | [Obsługiwane](https://msdn.microsoft.com/library/ms174986.aspx) | [Większość](https://msdn.microsoft.com/library/ms174986.aspx) |
| Przywracania bazy danych do punktu w czasie | [Obsługiwane](https://msdn.microsoft.com/library/ms179451.aspx) | [Obsługiwane](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Program Polybase | [Obsługiwane](https://msdn.microsoft.com/library/mt143171.aspx) | [Nieobsługiwane]
| Zarządzanie oparte na zasadach | [Obsługiwane](https://msdn.microsoft.com/library/bb510667.aspx) | Nieobsługiwane |
| Predykaty | [Obsługiwane](https://msdn.microsoft.com/library/ms189523.aspx) | [Większość](https://msdn.microsoft.com/library/ms189523.aspx)
| Usługi języka R | [Obsługiwane](https://msdn.microsoft.com/library/mt604845.aspx)
| Zarządca zasobów | [Obsługiwane](https://msdn.microsoft.com/library/bb933866.aspx) | Nieobsługiwane |
| Przywracanie bazy danych z kopii zapasowej | [Obsługiwane](https://msdn.microsoft.com/library/ms187048.aspx#anchor_6) | [Tylko z wbudowanych kopii zapasowych](sql-database-recovery-using-backups.md) |
| Zabezpieczenia na poziomie wiersza | [Obsługiwane](https://msdn.microsoft.com/library/dn765131.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/dn765131.aspx) |
| Instrukcje zabezpieczeń | [Obsługiwane](https://msdn.microsoft.com/library/ff848791.aspx) | [Niektóre](https://msdn.microsoft.com/library/ff848791.aspx) |
| Wyszukiwanie semantyczne | [Obsługiwane](https://msdn.microsoft.com/library/gg492075.aspx) | Nieobsługiwane |
| Numery sekwencyjne | [Obsługiwane](https://msdn.microsoft.com/library/ff878058.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ff878058.aspx) |
| Usługa Service Broker | [Obsługiwane](https://msdn.microsoft.com/library/bb522893.aspx) | Nieobsługiwane |
| Opcje konfiguracji serwera | [Obsługiwane](https://msdn.microsoft.com/library/ms189631.aspx) | Nieobsługiwane — zobacz temat [Database configuration options](https://msdn.microsoft.com/library/mt629158.aspx) (Opcje konfiguracji bazy danych) |
| Instrukcje SET | [Obsługiwane](https://msdn.microsoft.com/library/ms190356.aspx) | [Większość](https://msdn.microsoft.com/library/ms190356.aspx) 
| Przestrzenne | [Obsługiwane](https://msdn.microsoft.com/library/bb933790.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/bb933790.aspx) |
| Program SQL Server Agent | [Obsługiwane](https://msdn.microsoft.com/library/ms189237.aspx) | Nieobsługiwane — zobacz temat [Elastic jobs](sql-database-elastic-jobs-getting-started.md) (Zadania elastyczne) |
| Usługi SQL Server Analysis Services (SSAS) | [Obsługiwane](https://msdn.microsoft.com/library/bb522607.aspx) | Nieobsługiwane — zobacz [stronę usług Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| Usługi SQL Server Integration Services (SSIS) | [Obsługiwane](https://msdn.microsoft.com/library/ms141026.aspx) | Nieobsługiwane — zobacz [stronę usługi Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| Program PowerShell dla oprogramowania SQL Server | [Obsługiwane](https://msdn.microsoft.com/library/hh245198.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/hh245198.aspx) |
| Narzędzie SQL Server Profiler | [Obsługiwane](https://msdn.microsoft.com/library/ms181091.aspx) | Nieobsługiwane — zobacz temat [Extended events](https://msdn.microsoft.com/library/ms181091.aspx) (Zdarzenia rozszerzone) |
| Replikacja programu SQL Server | [Obsługiwane](https://msdn.microsoft.com/library/ms151198.aspx) | [Tylko dla subskrybentów replikacji transakcyjnej i replikacji migawki](sql-database-cloud-migrate.md) |
| Usługi SQL Server Reporting Services (SSRS) | [Obsługiwane](https://msdn.microsoft.com/library/ms159106.aspx) | Nieobsługiwane |
| Procedury składowane | [Obsługiwane](https://msdn.microsoft.com/library/ms190782.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms190782.aspx) |
| Systemowe funkcje składowane | [Obsługiwane](https://msdn.microsoft.com/library/ff848780.aspx) | [Niektóre](https://msdn.microsoft.com/library/ff848780.aspx) |
| Systemowe procedury składowane | [Obsługiwane](https://msdn.microsoft.com/library/ms187961.aspx) | [Niektóre](https://msdn.microsoft.com/library/ms187961.aspx) |
| Tabele systemowe | [Obsługiwane](https://msdn.microsoft.com/library/ms179932.aspx) | [Niektóre](https://msdn.microsoft.com/library/ms179932.aspx) |
| Widoki systemowe | [Obsługiwane](https://msdn.microsoft.com/library/ms177862.aspx) | [Niektóre](https://msdn.microsoft.com/library/ms177862.aspx)
| Partycjonowanie tabel | [Obsługiwane](https://msdn.microsoft.com/library/ms190787.aspx) | [Tylko podstawowa grupa plików](https://msdn.microsoft.com/library/ms190787.aspx) |
| Tabele tymczasowe | [Lokalne i globalne](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) | [Tylko lokalne](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) |
| Tabele danych czasowych | [Obsługiwane](https://msdn.microsoft.com/library/dn935015.aspx) | [Obsługiwane](sql-database-temporal-tables.md) |
| Instrukcje transakcji | [Obsługiwane](https://msdn.microsoft.com/library/ms174377.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ms174377.aspx) |
| Zmienne | [Obsługiwane](https://msdn.microsoft.com/library/ff848809.aspx) | | [Obsługiwane](https://msdn.microsoft.com/library/ff848809.aspx) | 
| Transparent Data Encryption (TDE)  | [Obsługiwane](https://msdn.microsoft.com/library/bb934049.aspx) | [Obsługiwane](https://msdn.microsoft.com/dn948096.aspx) |
| Usługa Windows Server Failover Clustering | [Obsługiwane](https://msdn.microsoft.com/library/hh270278.aspx) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Indeksy XML | [Obsługiwane](http://msdn.microsoft.com/library/bb934097.aspx) | [Obsługiwane](http://msdn.microsoft.com/library/bb934097.aspx) |
| Instrukcje języka XML | [Obsługiwane](https://msdn.microsoft.com/library/ff848798.aspx) | [Obsługiwane](https://msdn.microsoft.com/library/ff848798.aspx) |

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat usługi Azure SQL Database, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md)
- Omówienie serwerów logicznych Azure SQL można znaleźć w temacie [Omówienie serwerów logicznych usługi SQL Database](sql-database-server-overview.md).
- Omówienie baz danych Azure SQL znajduje się w temacie [Omówienie usługi SQL Database](sql-database-overview.md)
- Informacje o obsłudze i różnicach języka Transact-SQL można znaleźć w temacie [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md).
- Informacje o konkretnych limitach przydziału i ograniczeniach zasobów na podstawie określonej **warstwy usługi**. Omówienie warstw usługi można znaleźć w temacie [Warstwy usługi w usłudze SQL Database](sql-database-service-tiers.md).
- Aby zapoznać się z przeglądem zabezpieczeń, zobacz [przegląd zabezpieczeń usługi Azure SQL Database](sql-database-security-overview.md).
- Aby uzyskać informacje o dostępności sterowników i obsłudze usługi SQL Database, zobacz [biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).

