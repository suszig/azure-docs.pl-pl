# Omówienie
## [Co to jest SQL Database?](sql-database-technical-overview.md)
### [Warstwy usług](sql-database-service-tiers.md)
### [Jednostki DTU i eDTU](sql-database-what-is-a-dtu.md)
### [Omówienie testu porównawczego jednostek DTU](sql-database-benchmark-overview.md)
### [Limity zasobów](sql-database-resource-limits.md)
### [Funkcje](sql-database-features.md)
### [SQL Database — często zadawane pytania](sql-database-faq.md)
## Porównania
### [Usługa SQL Database a baza danych SQL na maszynie wirtualnej](sql-database-paas-vs-sql-server-iaas.md)
### [Różnice w języku T-SQL](sql-database-transact-sql-information.md)
### [SQL a NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [Narzędzia usługi SQL Database](sql-database-manage-overview.md)
## [Samouczki dotyczące usługi SQL Database](sql-database-explore-tutorials.md)
## Bezpieczeństwo
### [Omówienie zabezpieczeń](sql-database-security-overview.md)
### [Usługa Azure Security Center dla usługi Azure SQL Database](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [Centrum zabezpieczeń SQL](https://msdn.microsoft.com/library/azure/bb510589)
# Wprowadzenie
## Bazy danych i serwery
### Informacje
#### [Serwery](sql-database-server-overview.md)
#### [Pojedyncze bazy danych](sql-database-overview.md)
#### [Wiele baz danych](sql-database-elastic-scale-introduction.md)
##### Mapowanie dzierżaw
###### [Elastyczna biblioteka kliencka](sql-database-elastic-database-client-library.md)
###### [Menedżer map fragmentów](sql-database-elastic-scale-shard-map-management.md)
###### [Routing zależny od danych](sql-database-elastic-scale-data-dependent-routing.md)
###### [Zarządzanie poświadczeniami](sql-database-elastic-scale-manage-credentials.md)
###### [Zapytania wielofragmentowe](sql-database-elastic-scale-multishard-querying.md)
##### Pule elastyczne (pule zasobów)
###### [Co to jest pula elastyczna?](sql-database-elastic-pool.md)
###### [Kiedy korzystać z puli elastycznej](sql-database-elastic-pool-guidance.md)
###### [Ceny puli elastycznej](sql-database-elastic-pool-price.md)
##### Bazy danych podzielone na fragmenty
###### [Narzędzia elastyczne — słownik](sql-database-elastic-scale-glossary.md)
###### [Przenoszenie danych między fragmentami](sql-database-elastic-scale-overview-split-and-merge.md)
###### [Narzędzia elastyczne — często zadawane pytania](sql-database-elastic-scale-faq.md)
##### Zapytanie elastyczne (zapytania obejmujące wiele baz danych)
###### [Co to jest zapytanie elastyczne?](sql-database-elastic-query-overview.md)
##### Transakcje elastyczne (transakcje rozproszone)
###### [Transakcje w bazach danych w chmurze](sql-database-elastic-transactions-overview.md)
##### Zadania elastyczne (zadania obejmujące wiele baz danych)
###### [Co to jest zadanie elastyczne?](sql-database-elastic-jobs-overview.md)
#### [Nawiązywanie połączenia z usługą SQL Database za pomocą usługi Azure RemoteApp](sql-database-ssms-remoteapp.md)
#### [Zarządzanie bazami danych SQL przy użyciu usługi Azure Automation](sql-database-manage-automation.md)
### Zalecenia
#### [Tworzenie pojedynczej bazy danych za pomocą witryny Azure Portal](sql-database-get-started.md)
#### [Tworzenie pojedynczej bazy danych za pomocą programu PowerShell](sql-database-get-started-powershell.md)
#### [Tworzenie pojedynczej bazy danych za pomocą języka C#](sql-database-get-started-csharp.md)
#### [Tworzenie aplikacji podzielonej na fragmenty](sql-database-elastic-scale-get-started.md)
#### [Przenoszenie danych między fragmentami](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Wprowadzenie do zadań elastycznych](sql-database-elastic-jobs-getting-started.md)
#### [Wprowadzenie do zapytań elastycznych](sql-database-elastic-query-getting-started-vertical.md)
#### [Tworzenie raportów przy użyciu zapytania elastycznego](sql-database-elastic-query-getting-started.md)
#### [Wykonywanie zapytań w bazach danych z różnymi schematami](sql-database-elastic-query-vertical-partitioning.md)
#### [Raportowanie w bazach danych skalowanych w poziomie](sql-database-elastic-query-horizontal-partitioning.md)
## Migrowanie i przenoszenie danych
### Informacje
#### [Migrowanie bazy danych](sql-database-cloud-migrate.md)
#### [Replikacja transakcyjna](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
#### [Synchronizacja danych](sql-database-get-started-sql-data-sync.md)
#### [Kopiowanie bazy danych SQL](sql-database-copy.md)
## Reguły zapory, uwierzytelnianie i autoryzacja
### Informacje
#### [Kontrola dostępu](sql-database-control-access.md)
#### [Zapora](sql-database-firewall-configure.md)
#### [Zarządzanie danymi logowania](sql-database-manage-logins.md)
### Zalecenia
#### [Uwierzytelnianie i autoryzacja SQL](sql-database-control-access-sql-authentication-get-started.md)
#### [Uwierzytelnianie i autoryzacja w usłudze Azure AD](sql-database-control-access-aad-authentication-get-started.md)
## Zabezpieczanie i ochrona danych
### Informacje
#### Inspekcja
##### [Inspekcja](sql-database-auditing-get-started.md)
##### [Obsługa klientów z obniżonym poziomem i zmiany punktu końcowego adresu IP na potrzeby inspekcji](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Wykrywanie zagrożeń](sql-database-threat-detection-get-started.md)
#### Szyfrowanie danych
##### [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
##### [Niewidoczne szyfrowanie danych](https://msdn.microsoft.com/library/azure/dn948096)
##### [Szyfrowanie kolumn](https://msdn.microsoft.com/library/azure/ms179331)
#### Maskowanie danych
##### Dynamiczne maskowanie danych
###### [Azure Portal](sql-database-dynamic-data-masking-get-started.md)
### Zalecenia
#### [Dynamiczne maskowanie danych za pomocą witryny Azure Portal](sql-database-dynamic-data-masking-get-started.md)
##### [Korzystanie z funkcji Zawsze zaszyfrowane za pomocą magazynu certyfikatów systemu Windows](sql-database-always-encrypted.md)
## Ciągłość działalności biznesowej
### Informacje
#### [Omówienie](sql-database-business-continuity.md)
#### [Kopie zapasowe bazy danych](sql-database-automated-backups.md)
#### [Długoterminowe przechowywanie](sql-database-long-term-retention.md)
#### [Odzyskiwanie bazy danych przy użyciu kopii zapasowych](sql-database-recovery-using-backups.md)
#### [Odzyskiwanie pojedynczej tabeli](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Odzyskiwanie sprawności po awarii centrum danych](sql-database-disaster-recovery.md)
#### [Wymagania dotyczące uwierzytelniania dla odzyskiwania po awarii](sql-database-geo-replication-security-config.md)
#### [Ciągłość działania — scenariusze dotyczące projektów](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategie odzyskiwania po awarii z pulami elastycznymi](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Uaktualnienia równoległe](sql-database-manage-application-rolling-upgrade.md)
#### [Przeprowadzanie próbnego odzyskiwania po awarii](sql-database-disaster-recovery-drills.md)
#### [Omówienie aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md)
### Zalecenia
#### [Azure Portal: tworzenie kopii zapasowych i ich przywracanie](sql-database-get-started-backup-recovery.md)
#### [PowerShell: tworzenie kopii zapasowych i ich przywracanie](sql-database-get-started-backup-recovery-powershell.md)
## Opracowywanie aplikacji
### Informacje
#### [Omówienie tworzenia baz danych i aplikacji](sql-database-develop-overview.md)
#### [Biblioteki łączności](sql-database-libraries.md)
#### [Wielodostępne aplikacje SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Skalowanie wielodostępnych aplikacji SaaS z zabezpieczeniami na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Używanie portów wyższych niż 1433 dla platformy ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Pobieranie wymaganych wartości do uwierzytelniania aplikacji](sql-database-client-id-keys.md)
### Zalecenia
#### Łączenie aplikacji
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [C i C++](sql-database-develop-cplusplus-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Łączenie z programem Visual Studio](sql-database-connect-query.md)
#### [Tworzenie aplikacji klienckiej](https://www.microsoft.com/sql-server/developer-get-started)
#### [Praca z komunikatami o błędach](sql-database-develop-error-messages.md)
#### [Korzystanie z programu Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Używanie biblioteki klienckiej z programem Dapper](sql-database-elastic-scale-working-with-dapper.md)
### Implementacje klientów
#### [Oprogramowanie Daxko/CSI](sql-database-implementation-daxko.md)
#### [GEP](sql-database-implementation-gep.md)
#### [SnelStart](sql-database-implementation-snelstart.md)
#### [Umbraco](sql-database-implementation-umbraco.md)
## Tworzenie baz danych
### Informacje
#### Tabele danych czasowych
##### [Tabele danych czasowych](sql-database-temporal-tables.md)
##### [Zasady przechowywania](sql-database-temporal-tables-retention-policy.md)
#### [Dane JSON](sql-database-json-features.md)
#### [Optymalizacja w pamięci](sql-database-in-memory.md)
### Zalecenia
#### [Opracowywanie rozwiązań dla programu SQL Server](https://msdn.microsoft.com/library/ms179422.aspx)
#### [Wdrażanie przetwarzania OLTP w pamięci](sql-database-in-memory-oltp-migration.md)
## Monitorowanie i dostosowywanie
### Informacje
#### [Pojedyncze bazy danych](sql-database-single-database-monitor.md)
#### [Omówienie funkcji SQL Database Advisor](sql-database-advisor.md)
#### [Wytyczne dotyczące pojedynczej bazy danych](sql-database-performance-guidance.md)
#### [Szczegółowe informacje o wydajności: Azure Portal](sql-database-performance.md)
#### [Korzystanie z dzielenia na partie](sql-database-use-batching-to-improve-performance.md)
#### [Zdarzenia rozszerzone](sql-database-xevent-db-diff-from-svr.md)
## SQL Database V11
### [Wycofanie wersji Web i Business](sql-database-web-business-sunset-faq.md)
### [Service Tier Advisor](sql-database-service-tier-advisor.md)
### [Narzędzie do oceny puli elastycznej](sql-database-elastic-pool-database-assessment-powershell.md)
### [Uaktualnianie do wersji V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Uaktualnianie za pomocą witryny Azure Portal](sql-database-upgrade-server-portal.md)
#### [Uaktualnianie za pomocą programu PowerShell](sql-database-upgrade-server-powershell.md)
# Instrukcje
## Tworzenie i zarządzanie
### [Zarządzanie usługą SQL Database przy użyciu witryny Azure Portal](sql-database-manage-portal.md)
### [Zarządzanie usługą SQL Database za pomocą programu PowerShell](sql-database-manage-powershell.md)
### [Zarządzanie usługą SQL Database za pomocą programu SSMS](sql-database-manage-azure-ssms.md)
### Serwery
#### [Tworzenie serwerów](sql-database-create-servers.md)
#### [Wyświetlanie i aktualizowanie ustawień serwera](sql-database-view-update-server-settings.md)
### Pojedyncze bazy danych
#### [Tworzenie pojedynczych baz danych](sql-database-create-databases.md)
#### [Wyświetlanie i aktualizowanie ustawień bazy danych](sql-database-view-update-database-settings.md)
### Reguły zapory
#### [Tworzenie reguł zapory za pomocą witryny Azure Portal](sql-database-configure-firewall-settings.md)
#### [Tworzenie reguł zapory przy użyciu programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [Tworzenie reguł zapory przy użyciu interfejsu API REST](sql-database-configure-firewall-settings-rest.md)
#### [Tworzenie reguł zapory przy użyciu języka T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Wiele baz danych
#### [Uaktualnianie biblioteki klienckiej w aplikacjach klienckich](sql-database-elastic-scale-upgrade-client-library.md)
#### Bazy danych podzielone na fragmenty
##### [Konfiguracja zabezpieczeń](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Dodawanie fragmentu](sql-database-elastic-scale-add-a-shard.md)
##### [Rozwiązywanie problemów z mapami fragmentów](sql-database-elastic-database-recovery-manager.md)
##### [Migrowanie istniejących baz danych skalowanych w poziomie do baz danych podzielonych na fragmenty](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Tworzenie liczników wydajności dla menedżera map fragmentów](sql-database-elastic-database-perf-counters.md)
#### Zadania elastyczne
##### [Jak zainstalować usługę zadań elastycznych?](sql-database-elastic-jobs-service-installation.md)
##### [Tworzenie zadań elastycznych i zarządzanie nimi za pomocą programu PowerShell](sql-database-elastic-jobs-powershell.md) 
##### [Tworzenie zadań elastycznych w witrynie Azure Portal i zarządzanie nimi](sql-database-elastic-jobs-create-and-manage.md)
##### [Jak odinstalować zadania elastyczne?](sql-database-elastic-jobs-uninstall.md)
#### Pule elastyczne
##### [Tworzenie za pomocą witryny Azure Portal](sql-database-elastic-pool-create-portal.md)
##### [Tworzenie za pomocą programu PowerShell](sql-database-elastic-pool-create-powershell.md)
##### [Tworzenie za pomocą języka C#](sql-database-elastic-pool-create-csharp.md)
##### [Zarządzanie za pomocą witryny Azure Portal](sql-database-elastic-pool-manage-portal.md)
##### [Zarządzanie za pomocą programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
##### [Zarządzanie za pomocą języka C#](sql-database-elastic-pool-manage-csharp.md)
##### [Zarządzanie za pomocą języka T-SQL](sql-database-elastic-pool-manage-tsql.md)
##  Uwierzytelnianie i autoryzacja
### [Uwierzytelnianie w usłudze Azure AD](sql-database-aad-authentication.md)
### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
## Szyfrowanie danych
### [Niewidoczne szyfrowanie danych](https://msdn.microsoft.com/library/azure/dn948096)
### [Szyfrowanie kolumn](https://msdn.microsoft.com/library/azure/ms179331)
## Migrowanie baz danych
### Określanie zgodności
#### [Określanie zgodności za pomocą narzędzia pakietu SQL](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [Określanie zgodności za pomocą programu SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Rozwiązywanie problemów ze zgodnością
#### [Rozwiązywanie problemów ze zgodnością za pomocą narzędzi SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [Rozwiązywanie problemów ze zgodnością za pomocą programu SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [Rozwiązywanie problemów ze zgodnością za pomocą kreatora SMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [Migrowanie przy użyciu Kreatora migracji programu SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
## Monitorowanie i dostrajanie
### [Szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [Dynamiczne widoki zarządzania](sql-database-monitoring-with-dmvs.md)
### [Poziomy zgodności](sql-database-compatibility-level-query-performance-130.md)
### [Porady dotyczące dostrajania wydajności](sql-database-troubleshoot-performance.md)
### Zmienianie warstw usług i poziomów wydajności
#### [Zmienianie warstw usług przy użyciu witryny Azure Portal](sql-database-scale-up.md)
#### [Zmienianie warstw usług przy użyciu programu PowerShell](sql-database-scale-up-powershell.md)
### [Tworzenie alertów](sql-database-insights-alerts-portal.md)
#### [Monitorowanie magazynu OLTP w pamięci](sql-database-in-memory-oltp-monitoring.md)
### Magazyn zapytań
#### [Monitorowanie wydajności za pomocą magazynu zapytań](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Scenariusze użycia magazynu zapytań](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Obsługa magazynu zapytań](sql-database-operate-query-store.md)
### Zdarzenia rozszerzone
#### [Kod docelowy pliku zdarzenia](sql-database-xevent-code-event-file.md)
#### [Kod docelowy buforu cyklicznego](sql-database-xevent-code-ring-buffer.md)
## Przenoszenie danych
### Kopiowanie bazy danych SQL
#### [Kopiowanie za pomocą witryny Azure Portal](sql-database-copy-portal.md)
#### [Kopiowanie za pomocą programu PowerShell](sql-database-copy-powershell.md)
#### [Kopiowanie za pomocą języka T-SQL](sql-database-copy-transact-sql.md)
### Eksportowanie bazy danych do pliku BACPAC
#### [Eksportowanie za pomocą witryny Azure Portal](sql-database-export.md)
#### [Eksportowanie przy użyciu programu SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [Eksportowanie za pomocą narzędzia pakietu SQL](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [Eksportowanie przy użyciu programu PowerShell](sql-database-export-powershell.md)
### Importowanie bazy danych z pliku BACPAC
#### [Importowanie za pomocą witryny Azure Portal](sql-database-import.md)
#### [Importowanie przy użyciu programu PowerShell](sql-database-import-powershell.md)
#### [Importowanie przy użyciu programu SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [Importowanie za pomocą narzędzia pakietu SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Ładowanie z pliku CSV przy użyciu narzędzia BCP](sql-database-load-from-csv-with-bcp.md)
## Zapytanie
### [Wykonywanie zapytań przy użyciu programu SSMS](sql-database-connect-query-ssms.md)
## Wykonywanie kopii zapasowych i przywracanie
### Długoterminowe przechowywanie kopii zapasowych
#### [Konfigurowanie długoterminowego przechowywania kopii zapasowych](sql-database-configure-long-term-retention.md)
#### [Wyświetlanie kopii zapasowych w magazynie usługi Recovery Services](sql-database-view-backups-in-vault.md)
#### [Przywracanie po długoterminowym przechowywaniu kopii zapasowych](sql-database-restore-from-long-term-retention.md)
#### [Usuwanie z długoterminowego przechowywania kopii zapasowych](sql-database-long-term-retention-delete.md)
### Przywracanie usuniętej bazy danych
#### [Przywracanie usuniętych danych za pomocą witryny Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [Przywracanie usuniętych danych przy użyciu programu PowerShell](sql-database-restore-deleted-database-powershell.md)
### Przywracanie do punktu w czasie
#### [Przywracanie do punktu w czasie](sql-database-point-in-time-restore.md)
#### [Wyświetlanie najstarszego punktu przywracania](sql-database-view-oldest-restore-point.md)
### [Przywracanie z kopii zapasowej nadmiarowej geograficznie](sql-database-geo-restore.md)
## Aktywna replikacja geograficzna
### [Konfigurowanie przy użyciu witryny Azure Portal](sql-database-geo-replication-portal.md)
### [Konfigurowanie przy użyciu programu PowerShell](sql-database-geo-replication-powershell.md)
### [Konfigurowanie przy użyciu języka T-SQL](sql-database-geo-replication-transact-sql.md)
### [Przełączanie w tryb failover za pomocą witryny Azure Portal](sql-database-geo-replication-failover-portal.md)
### [Przełączanie w tryb failover za pomocą programu PowerShell](sql-database-geo-replication-failover-powershell.md)
### [Przełączanie w tryb failover przy użyciu języka T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## Rozwiązywanie problemów
### [Problemy z połączeniem](sql-database-troubleshoot-common-connection-issues.md)
### [Przejściowy błąd połączenia](sql-database-troubleshoot-connection.md)
### [Diagnozowanie i zapobieganie](sql-database-connectivity-issues.md)
### [Uprawnienia](sql-database-troubleshoot-permissions.md)
### [Przenoszenie baz danych](sql-database-troubleshoot-moving-data.md)
# Dokumentacja
## [Program PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (elastyczna baza danych)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [Interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/appservice)
## [.NET](/dotnet/api/microsoft.azure.management.sql.models)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/bb510741.aspx)
## [REST](https://msdn.microsoft.com/library/azure/mt163571.aspx)

# Powiązane
## Biblioteka zarządzania usługi SQL Database
### [Pobieranie pakietu biblioteki zarządzania usługi SQL Database](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [Sterowniki programu SQL Server](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)

# Zasoby
## [Cennik](https://azure.microsoft.com/pricing/details/sql-database/)
## [Forum MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-azure)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?service=sql-database)
## [Narzędzia programu SQL Server](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
