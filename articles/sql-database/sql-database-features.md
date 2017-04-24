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
ms.custom: features
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a1ede93b5aacf0d8a5bcf83f208f72be328ee72f
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-features"></a>Funkcje usługi Azure SQL Database

W poniższych tabelach zawarto główne funkcje usługi Azure SQL Database oraz równoważne funkcje serwera SQL Server — a także informacje o obsłudze poszczególnych funkcji oraz linki do bardziej szczegółowych informacji o funkcjach na każdej platformie. W przypadku różnic w języku Transact-SQL do rozważenia przed migrowaniem istniejącego rozwiązania z zakresu bazy danych, zobacz [Rozwiązywanie różnic w języku Transact-SQL podczas migracji do usługi SQL Database](sql-database-transact-sql-information.md).

W dalszym ciągu dodajemy funkcje do usługi Azure SQL Database. Zachęcamy zatem do odwiedzania naszej strony poświęconej aktualizacjom usług dla platformy Azure i korzystania z dostępnych tam filtrów:

* Włączony filtr [usługi SQL Database](https://azure.microsoft.com/updates/?service=sql-database).
* Włączony filtr [ogłoszeń dotyczących ogólnej dostępności](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) funkcji usługi SQL Database.

| **Funkcja** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Aktywna replikacja geograficzna | Nieobsługiwane — zobacz [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) (Zawsze włączone grupy dostępności) | [Obsługiwane](sql-database-geo-replication-overview.md)
| Zawsze szyfrowane | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Obsługiwane — zobacz [Magazyn certyfikatów](sql-database-always-encrypted.md) i [Magazyn kluczy](sql-database-always-encrypted-azure-key-vault.md)|
| Zawsze włączone grupy dostępności | [Obsługiwane](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Dołączanie bazy danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nieobsługiwane |
| Role aplikacji | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) |
| Automatyczne skalowanie | Nieobsługiwane | Obsługiwane — zobacz [Warstwy usług](sql-database-service-tiers.md) |
| Usługa Azure Active Directory | Nieobsługiwane | [Obsługiwane](sql-database-aad-authentication.md) |
| Azure Data Factory | [Obsługiwane](../data-factory/data-factory-introduction.md) | [Obsługiwane](../data-factory/data-factory-introduction.md) |
| Inspekcja | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Obsługiwane](sql-database-auditing.md) |
| Plik BACPAC (eksportowanie) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | [Obsługiwane](sql-database-export.md) |
| Plik BACPAC (importowanie) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | [Obsługiwane](sql-database-import.md) |
| KOPIA ZAPASOWA | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nieobsługiwane |
| Funkcje wbudowane | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/functions/functions) | Większość — zobacz [Pojedyncze funkcje] (https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Przechwytywanie danych dotyczących zmian | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nieobsługiwane |
| Śledzenie zmian | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) |
| Instrukcje sortowania | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/collations) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/collations) |
| Indeksy magazynu kolumn | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | [Tylko w wersji Premium](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| Środowisko uruchomieniowe języka wspólnego (CLR) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nieobsługiwane |
| Zawarte bazy danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) |
| Zawarci użytkownicy | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | [Obsługiwane](sql-database-manage-logins.md#non-administrator-users) |
| Słowa kluczowe języka sterowania przepływem | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) |
| Zapytania obejmujące wiele baz danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Częściowe — zobacz [Zapytania elastyczne](sql-database-elastic-query-overview.md) |
| Kursory | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | 
| Kompresja danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) |
| Kopie zapasowe bazy danych | [Zarządzane przez użytkownika](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) | [Zarządzane przez usługę SQL Database](sql-database-automated-backups.md) |
| Poczta bazy danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nieobsługiwane |
| Dublowanie bazy danych | [Obsługiwane](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nieobsługiwane |
| Ustawienia konfiguracji bazy danych | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Usługi Data Quality Services (DQS) | [Obsługiwane](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nieobsługiwane |
| Migawki baz danych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nieobsługiwane |
| Typy danych | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) |  
| Instrukcje DBCC | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Większość — zobacz [Pojedyncze instrukcje](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) |
| Instrukcje języka DDL | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/statements) | Większość — zobacz [Pojedyncze instrukcje](https://docs.microsoft.com/sql/t-sql/statements/statements)
| Wyzwalacze języka DDL | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | [Tylko dla bazy danych](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) |
| Transakcje rozproszone | [Narzędzie MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Tylko ograniczone scenariusze wewnątrz usługi SQL Database |
| Instrukcje języka DML | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/queries/queries) | Większość — zobacz [Pojedyncze instrukcje]](https://docs.microsoft.com/sql/t-sql/queries/queries) |
| Wyzwalacze języka DML | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| Dynamiczne widoki zarządzania | [Wszystkie](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Niektóre — zobacz [Pojedyncze dynamiczne widoki zarządzania](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) |
| Pule elastyczne | Nieobsługiwane | [Obsługiwane](sql-database-elastic-pool.md) |
| Zadania elastyczne | Nieobsługiwane — zobacz temat [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | [Obsługiwane](sql-database-elastic-jobs-getting-started.md) | 
| Zapytania elastyczne | Nieobsługiwane — zobacz temat [Cross-database queries](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) (Zapytania obejmujące wiele baz danych) | [Obsługiwane](sql-database-elastic-query-overview.md) |
| Powiadomienia o zdarzeniach | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | [Obsługiwane](sql-database-insights-alerts-portal.md) |
| Wyrażenia | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |
| Zdarzenia rozszerzone | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Niektóre — zobacz [Pojedyncze zdarzenia](sql-database-xevent-db-diff-from-svr.md) |
| Rozszerzone procedury składowane | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nieobsługiwane |
| Pliki i grupy plików | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | [Tylko podstawowe](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) |
| Strumień pliku | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nieobsługiwane |
| Wyszukiwanie pełnotekstowe | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | [Moduły dzielenia wyrazów innych firm są nieobsługiwane](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |
| Funkcje | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/functions/functions) | Większość — zobacz [Pojedyncze funkcje](https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Optymalizacja w pamięci | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | [Tylko w wersji Premium](sql-database-in-memory.md) |
| Zadania | Zobacz [Program SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Zobacz [Zadania elastyczne](sql-database-elastic-jobs-getting-started.md) |
| Obsługa danych JSON | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Obsługiwane](sql-database-json-features.md) |
| Elementy języka | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Większość — zobacz [Pojedyncze elementy](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) |  
| Serwery połączone | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nieobsługiwane — zobacz temat [Elastic query](sql-database-elastic-query-horizontal-partitioning.md) (Zapytanie elastyczne) |
| Wysyłanie dziennika | [Obsługiwane](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Usługi Master Data Services (MDS) | [Obsługiwane](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nieobsługiwane |
| Rejestrowanie minimalne w imporcie zbiorczym | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nieobsługiwane |
| Modyfikowanie danych systemowych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nieobsługiwane |
| Operacje indeksowania w trybie online | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | [Obsługiwane — rozmiar transakcji ograniczony przez warstwę usług](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) |
| Operatory | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Większość — zobacz [Pojedyncze operatory](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) |
| Przywracania bazy danych do punktu w czasie | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | [Obsługiwane](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Program Polybase | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nieobsługiwane
| Zarządzanie oparte na zasadach | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nieobsługiwane |
| Predykaty | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Większość — zobacz [Pojedyncze predykaty](https://docs.microsoft.com/sql/t-sql/queries/predicates)
| Usługi języka R | [Obsługiwane](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services)
| Zarządca zasobów | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nieobsługiwane |
| Instrukcje RESTORE | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nieobsługiwane | 
| Przywracanie bazy danych z kopii zapasowej | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | [Tylko z wbudowanych kopii zapasowych](sql-database-recovery-using-backups.md) |
| Zabezpieczenia na poziomie wiersza | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) |
| Wyszukiwanie semantyczne | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nieobsługiwane |
| Numery sekwencyjne | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) |
| Usługa Service Broker | [Obsługiwane](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nieobsługiwane |
| Ustawienia konfiguracji serwera | [Obsługiwane](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nieobsługiwane — zobacz temat [Database configuration options](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) (Opcje konfiguracji bazy danych) |
| Instrukcje SET | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Większość — zobacz [Pojedyncze instrukcje](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) 
| Przestrzenne | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) |
| Program SQL Server Agent | [Obsługiwane](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nieobsługiwane — zobacz temat [Elastic jobs](sql-database-elastic-jobs-getting-started.md) (Zadania elastyczne) |
| Usługi SQL Server Analysis Services (SSAS) | [Obsługiwane](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nieobsługiwane — zobacz [stronę usług Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| Usługi SQL Server Integration Services (SSIS) | [Obsługiwane](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Nieobsługiwane — zobacz [stronę usługi Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| Program PowerShell dla oprogramowania SQL Server | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) |
| Narzędzie SQL Server Profiler | [Obsługiwane](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nieobsługiwane — zobacz temat [Extended events](sql-database-xevent-db-diff-from-svr.md) (Zdarzenia rozszerzone) |
| Replikacja programu SQL Server | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Tylko dla subskrybentów replikacji transakcyjnej i replikacji migawki](sql-database-cloud-migrate.md) |
| Usługi SQL Server Reporting Services (SSRS) | [Obsługiwane](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nieobsługiwane |
| Procedury składowane | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) |
| Systemowe funkcje składowane | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Większość — zobacz [Pojedyncza funkcja](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) |
| Systemowe procedury składowane | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Niektóre — zobacz [Pojedyncza procedura składowana](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Tabele systemowe | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Niektóre — zobacz [Pojedyncza tabela](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Widoki wykazu systemu | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Niektóre — zobacz [Pojedyncze widoki](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)
| Partycjonowanie tabel | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Obsługiwane — [Tylko podstawowa grupa plików](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) |
| Tabele tymczasowe | [Lokalne i globalne](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | [Tylko lokalne](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) |
| Tabele danych czasowych | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Obsługiwane](sql-database-temporal-tables.md) |
| Transakcje | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) |
| Zmienne | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | 
| Transparent Data Encryption (TDE)  | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | [Obsługiwane](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Usługa Windows Server Failover Clustering | [Obsługiwane](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Nieobsługiwane — zobacz temat [Active Geo-Replication](sql-database-geo-replication-overview.md) (Aktywna replikacja geograficzna) |
| Indeksy XML | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | [Obsługiwane](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) |

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat usługi Azure SQL Database, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md)
- Informacje o obsłudze i różnicach języka Transact-SQL można znaleźć w temacie [Rozwiązywanie różnic w języku Transact-SQL podczas migracji do usługi SQL Database](sql-database-transact-sql-information.md).

