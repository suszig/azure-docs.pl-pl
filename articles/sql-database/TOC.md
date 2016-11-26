# Omówienie
## [Co to jest SQL Database?](sql-database-technical-overview.md)
### [Warstwy usług](sql-database-service-tiers.md)
### [Jednostki DTU (Database Transaction Unit)](sql-database-what-is-a-dtu.md)
### [Omówienie testu porównawczego jednostek DTU](sql-database-benchmark-overview.md)
### [Limity zasobów](sql-database-resource-limits.md)
### [Funkcje](sql-database-features.md)
### [SQL Database — często zadawane pytania](sql-database-faq.md)
## Korzyści
### [Uczy się i dostosowuje](sql-database-learn-and-adapt.md)
### [Skaluje się na bieżąco](sql-database-scale-on-the-fly.md)
### [Tworzy aplikacje wielodostępne](sql-database-build-multi-tenant-apps.md)
### [Zabezpiecza i chroni](sql-database-helps-secures-and-protects.md)
### [Działa w Twoim środowisku](sql-database-works-in-your-environment.md)
## Porównania
### [Usługa SQL Database a baza danych SQL na maszynie wirtualnej](sql-database-paas-vs-sql-server-iaas.md)
### [Różnice w języku T-SQL](sql-database-transact-sql-information.md)
### [SQL a NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [Narzędzia usługi SQL Database](sql-database-manage-overview.md)
## [Samouczki dotyczące usługi SQL Database](sql-database-explore-tutorials.md)
## [Przewodniki szybkiego startu dla rozwiązań](sql-database-solution-quick-starts.md)
## Bezpieczeństwo
### [Usługa Azure Security Center dla usługi Azure SQL Database](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [Centrum zabezpieczeń SQL](https://msdn.microsoft.com/library/azure/bb510589)
# Wprowadzenie
## Bazy danych i serwery
### Pojedyncze bazy danych
#### Informacje
##### [Samouczek dotyczący witryny Azure Portal: tworzenie bazy danych Azure SQL Database przy użyciu witryny Azure Portal](sql-database-get-started.md)
#### Zalecenia
##### [Samouczek dotyczący witryny Azure Portal: tworzenie bazy danych Azure SQL Database przy użyciu witryny Azure Portal](sql-database-get-started.md)
##### [Samouczek dotyczący witryny Azure Portal: tworzenie bazy danych Azure SQL Database przy użyciu programu PowerShell](sql-database-get-started-powershell.md)
##### [Samouczek dotyczący witryny Azure Portal: tworzenie bazy danych Azure SQL Database przy użyciu języka C#](sql-database-get-started-csharp.md)
### Elastyczne pule baz danych
#### Informacje
##### [Elastyczne pule baz danych](sql-database-elastic-pool.md)
##### [Kiedy korzystać z elastycznej puli baz danych](sql-database-elastic-pool-guidance.md)
##### [Ceny puli elastycznej](sql-database-elastic-pool-price.md)
#### Zalecenia
##### [Tworzenie za pomocą witryny Azure Portal](sql-database-elastic-pool-create-portal.md)
##### [Tworzenie za pomocą programu PowerShell](sql-database-elastic-pool-create-powershell.md)
##### [Tworzenie za pomocą języka C#](sql-database-elastic-pool-create-csharp.md)
### Skalowanie w poziomie
#### Informacje
##### [Skalowanie w poziomie](sql-database-elastic-scale-introduction.md)
##### [Elastyczne skalowanie](sql-database-elastic-scale-get-started.md)
##### [Tworzenie skalowalnych baz danych w chmurze](sql-database-elastic-database-client-library.md)
##### [Zadania obejmujące wiele baz danych](sql-database-elastic-jobs-overview.md)
##### [Narzędzia elastyczne — często zadawane pytania](sql-database-elastic-scale-faq.md)
##### [Słownik narzędzi elastycznych baz danych](sql-database-elastic-scale-glossary.md)
##### [Przenoszenie danych między skalowanymi bazami danych w chmurze](sql-database-elastic-scale-overview-split-and-merge.md)
#### Zalecenia
##### [Zadania elastyczne](sql-database-elastic-jobs-getting-started.md)
##### [Raporty obejmujące wiele baz danych](sql-database-elastic-query-getting-started.md)
##### [Zapytania obejmujące wiele baz danych](sql-database-elastic-query-getting-started-vertical.md)
##### [Odinstalowywanie zadań elastycznych](sql-database-elastic-jobs-uninstall.md)
## Migrowanie i przenoszenie danych
### Informacje
#### [Migrowanie bazy danych](sql-database-cloud-migrate.md)
#### [Replikacja transakcyjna](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
#### [Synchronizacja danych](sql-database-get-started-sql-data-sync.md)
#### [Kopiowanie bazy danych SQL](sql-database-copy.md)
### Zalecenia
#### Określanie zgodności bazy danych
##### [Narzędzie SQL Package](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
#### Rozwiązywanie problemów ze zgodnością bazy danych
##### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
##### [Kreator migracji usług SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md)
##### [Kreator migracji programu SQL Server Management Studio](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
#### Kopiowanie bazy danych
##### [Kopiowanie za pomocą witryny Azure Portal](sql-database-copy-portal.md)
##### [Kopiowanie za pomocą programu PowerShell](sql-database-copy-powershell.md)
##### [Kopiowanie za pomocą języka T-SQL](sql-database-copy-transact-sql.md)
#### Eksportowanie bazy danych do pliku BACPAC
##### [Azure Portal](sql-database-export.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
##### [Narzędzie SQL Package](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
##### [Program PowerShell](sql-database-export-powershell.md)
#### Importowanie bazy danych z pliku BACPAC
##### [Azure Portal](sql-database-import.md)
##### [Program PowerShell](sql-database-import-powershell.md)
##### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
##### [Narzędzie SQL Package](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
#### [Ładowanie z pliku CSV przy użyciu narzędzia BCP](sql-database-load-from-csv-with-bcp.md)
## Uwierzytelnianie i udzielanie dostępu
### Informacje
#### [Omówienie](sql-database-security.md)
#### [Zalecenia dotyczące zabezpieczeń](sql-database-security-guidelines.md)
#### [Zarządzanie danymi logowania](sql-database-manage-logins.md)
### Zalecenia
### [Samouczek: uwierzytelnianie i dostęp SQL](sql-database-get-started-security.md)
## Zabezpieczanie i ochrona danych
### Informacje
#### Inspekcja
##### [Inspekcja](sql-database-auditing-get-started.md)
##### [Obsługa klientów z obniżonym poziomem i zmiany punktu końcowego adresu IP na potrzeby inspekcji](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Wykrywanie zagrożeń](sql-database-threat-detection-get-started.md)
#### Szyfrowanie danych
##### [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
##### [Magazyn certyfikatów systemu Windows](sql-database-always-encrypted.md)
##### [Niewidoczne szyfrowanie danych](https://msdn.microsoft.com/library/azure/dn948096)
##### [Szyfrowanie kolumn](https://msdn.microsoft.com/library/azure/ms179331)
#### Maskowanie danych
##### Dynamiczne maskowanie danych
###### [Azure Portal](sql-database-dynamic-data-masking-get-started.md)
### Zalecenia
#### [Dynamiczne maskowanie danych za pomocą witryny Azure Portal](sql-database-dynamic-data-masking-get-started.md)
## Ciągłość działalności biznesowej
### Informacje
#### [Omówienie](sql-database-business-continuity.md)
#### [Kopie zapasowe bazy danych](sql-database-automated-backups.md)
#### [Długoterminowe przechowywanie](sql-database-long-term-retention.md) 
#### [Odzyskiwanie bazy danych przy użyciu kopii zapasowych](sql-database-recovery-using-backups.md)
#### [Wymagania dotyczące uwierzytelniania dla odzyskiwania po awarii](sql-database-geo-replication-security-config.md)
#### [Ciągłość działania — scenariusze dotyczące projektów](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategie odzyskiwania po awarii z pulami elastycznymi](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Uaktualnienia równoległe](sql-database-manage-application-rolling-upgrade.md)
### Zalecenia
#### Przywracanie usuniętej bazy danych
##### [Azure Portal](sql-database-restore-deleted-database-portal.md)
##### [Program PowerShell](sql-database-restore-deleted-database-powershell.md)
#### Przywracanie do określonego momentu
##### [Azure Portal](sql-database-point-in-time-restore-portal.md)
##### [Program PowerShell](sql-database-point-in-time-restore-powershell.md)
#### Przywracanie geograficzne baz danych
##### [Azure Portal](sql-database-geo-restore-portal.md)
##### [Program PowerShell](sql-database-geo-restore-powershell.md)
#### [Pojedyncza tabela](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Odzyskiwanie sprawności po awarii centrum danych](sql-database-disaster-recovery.md)
#### [Przeprowadzanie próbnego odzyskiwania po awarii](sql-database-disaster-recovery-drills.md)
### Replikowanie baz danych
#### [Omówienie aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md)
#### Konfigurowanie aktywnej replikacji geograficznej
##### [Azure Portal](sql-database-geo-replication-portal.md)
##### [Program PowerShell](sql-database-geo-replication-powershell.md)
##### [T-SQL](sql-database-geo-replication-transact-sql.md)
#### Przełączanie w tryb failover przy użyciu aktywnej replikacji geograficznej
##### [Azure Portal](sql-database-geo-replication-failover-portal.md)
##### [Program PowerShell](sql-database-geo-replication-failover-powershell.md)
##### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## Opracowywanie aplikacji
### Informacje
#### [Omówienie tworzenia baz danych i aplikacji](sql-database-develop-overview.md)
#### [Biblioteki łączności](sql-database-libraries.md)
#### [Wielodostępne aplikacje SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md)
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
### Implementacje oprogramowania klientów jako usługi
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
#### [W pamięci](sql-database-in-memory.md)
### Zalecenia
#### [Opracowywanie rozwiązań dla programu SQL Server](https://msdn.microsoft.com/library/ms179422.aspx)
## Monitorowanie i dostosowywanie
### Informacje
#### [Pojedyncze bazy danych](sql-database-single-database-monitor.md)
#### [Omówienie funkcji SQL Database Advisor](sql-database-advisor.md)
#### [Wytyczne dotyczące pojedynczej bazy danych](sql-database-performance-guidance.md)
#### [Szczegółowe informacje o obciążeniach w witrynie Azure Portal](sql-database-performance.md)
#### [Korzystanie z dzielenia na partie](sql-database-use-batching-to-improve-performance.md)
## SQL Database V11
### [Wycofanie wersji Web i Business](sql-database-web-business-sunset-faq.md)
### [Service Tier Advisor](sql-database-service-tier-advisor.md)
### [Narzędzie do oceny puli elastycznej](sql-database-elastic-pool-database-assessment-powershell.md)
### [Uaktualnianie do wersji V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Uaktualnianie za pomocą witryny Azure Portal](sql-database-upgrade-server-portal.md)
#### [Uaktualnianie za pomocą programu PowerShell](sql-database-upgrade-server-powershell.md)
#### [Zalecenia dotyczące warstw cenowych](sql-database-service-tier-advisor.md)
# Instrukcje
## Tworzenie i zarządzanie
### Serwery i bazy danych
#### [Pojedyncze bazy danych](sql-database-manage-portal.md)
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [Program PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Elastyczne pule baz danych
#### [Tworzenie za pomocą witryny Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [Tworzenie za pomocą programu PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [Tworzenie za pomocą języka C#](sql-database-elastic-pool-create-csharp.md)
#### [Zarządzanie za pomocą witryny Azure Portal](sql-database-elastic-pool-manage-portal.md)
#### [Zarządzanie za pomocą programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [Zarządzanie za pomocą języka C#](sql-database-elastic-pool-manage-csharp.md)
#### [Zarządzanie za pomocą języka T-SQL](sql-database-elastic-pool-manage-tsql.md)
### Bazy danych podzielone na fragmenty
#### [Korzystanie z menedżera map fragmentów](sql-database-elastic-scale-shard-map-management.md)
#### [Konfiguracja zabezpieczeń z dzieleniem i scalaniem](sql-database-elastic-scale-split-merge-security-configuration.md)
#### [Praca z programem Dapper](sql-database-elastic-scale-working-with-dapper.md)
#### [Korzystanie z programu Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Routing zależny od danych](sql-database-elastic-scale-data-dependent-routing.md)
#### [Zabezpieczenia na poziomie wierszy dla wielu dzierżaw](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Zarządzanie poświadczeniami](sql-database-elastic-scale-manage-credentials.md)
#### [Wdrażanie usługi z podziałem i scalaniem](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Dodawanie fragmentu](sql-database-elastic-scale-add-a-shard.md)
#### [Używanie klasy RecoveryManager do rozwiązywanie problemów z mapą fragmentów](sql-database-elastic-database-recovery-manager.md)
###  Authentication
#### [Uwierzytelnianie w usłudze Azure AD](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Reguły zapory
#### [Azure portal](sql-database-configure-firewall-settings.md)
#### [Program PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [Interfejs API REST](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Zadania i automatyzacja
#### [Instalacja usługi](sql-database-elastic-jobs-service-installation.md)
#### [Tworzenie zadań elastycznych w witrynie Azure Portal i zarządzanie nimi](sql-database-elastic-jobs-create-and-manage.md)
#### [Tworzenie zadań elastycznych i zarządzanie nimi za pomocą programu PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Uaktualnianie biblioteki klienckiej](sql-database-elastic-scale-upgrade-client-library.md)
#### [Zarządzanie bazami danych SQL przy użyciu usługi Azure Automation](sql-database-manage-automation.md)
### [Zapora](sql-database-firewall-configure.md)
## Szyfrowanie danych
### [Zawsze zaszyfrowane — omówienie](sql-database-always-encrypted.md)
### [Niewidoczne szyfrowanie danych](https://msdn.microsoft.com/library/azure/dn948096)
### [Szyfrowanie kolumn](https://msdn.microsoft.com/library/azure/ms179331)
## Migrate (Migracja)
### Określanie zgodności
#### [Narzędzie SQL Package](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Rozwiązywanie problemów ze zgodnością
#### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [Kreator migracji usług SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [Korzystanie z kreatora migracji programu SQL Server Management Studio](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
### [Korzystanie z replikacji transakcyjnej](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
### [Migrowanie istniejących baz danych skalowanych w poziomie w celu skalowania w poziomie](sql-database-elastic-convert-to-use-elastic-tools.md)
## Monitorowanie i dostrajanie
### [Szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [Dynamiczne widoki zarządzania](sql-database-monitoring-with-dmvs.md)
### [Poziomy zgodności](sql-database-compatibility-level-query-performance-130.md)
### [Liczniki wydajności dla menedżera map fragmentów](sql-database-elastic-database-perf-counters.md)
### [Porady dotyczące dostrajania wydajności](sql-database-troubleshoot-performance.md)
### Zmienianie warstw usług i poziomów wydajności
#### [Korzystanie z witryny Azure Portal](sql-database-scale-up.md)
#### [Korzystanie z programu PowerShell](sql-database-scale-up-powershell.md)
### [Tworzenie alertów](sql-database-insights-alerts-portal.md)
### Przetwarzanie OLTP w pamięci
#### [Wdrażanie przetwarzania OLTP w pamięci](sql-database-in-memory-oltp-migration.md)
#### [Monitorowanie magazynu OLTP w pamięci](sql-database-in-memory-oltp-monitoring.md)
### Magazyn zapytań
#### [Monitorowanie wydajności za pomocą magazynu zapytań](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Scenariusze użycia magazynu zapytań](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Obsługa magazynu zapytań](sql-database-operate-query-store.md)
### Zdarzenia rozszerzone
#### [Zdarzenia rozszerzone](sql-database-xevent-db-diff-from-svr.md)
#### [Kod docelowy pliku zdarzenia](sql-database-xevent-code-event-file.md)
#### [Kod docelowy buforu cyklicznego](sql-database-xevent-code-ring-buffer.md)
## Przenoszenie danych
### [Kopiowanie bazy danych SQL](sql-database-copy.md)
#### [Azure Portal](sql-database-copy-portal.md)
#### [Program PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Eksportowanie bazy danych do pliku BACPAC
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [Narzędzie SQL Package](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [Program PowerShell](sql-database-export-powershell.md)
### Importowanie bazy danych z pliku BACPAC
#### [Azure Portal](sql-database-import.md)
#### [Program PowerShell](sql-database-import-powershell.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [Narzędzie SQL Package](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Ładowanie z pliku CSV przy użyciu narzędzia BCP](sql-database-load-from-csv-with-bcp.md)
### [Przenoszenie danych między skalowanymi bazami danych w chmurze](sql-database-elastic-scale-overview-split-and-merge.md)
## Zapytanie
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Zapytania wielofragmentowe](sql-database-elastic-scale-multishard-querying.md)
### Zapytania obejmujące wiele baz danych
#### [Omówienie](sql-database-elastic-query-overview.md)
#### [Zapytania obejmujące wiele baz danych z różnymi schematami](sql-database-elastic-query-vertical-partitioning.md)
#### [Raportowanie obejmujące wiele baz danych](sql-database-elastic-query-horizontal-partitioning.md)
#### [Transakcje rozproszone w bazach danych w chmurze](sql-database-elastic-transactions-overview.md)
## Przywracanie
### Przywracanie usuniętej bazy danych
#### [Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [Program PowerShell](sql-database-restore-deleted-database-powershell.md)
### Przywracanie do punktu w czasie
#### [Azure Portal](sql-database-point-in-time-restore-portal.md)
#### [Program PowerShell](sql-database-point-in-time-restore-powershell.md)
### Przywracanie geograficzne
#### [Azure Portal](sql-database-geo-restore-portal.md)
#### [Program PowerShell](sql-database-geo-restore-powershell.md)
#### [Pojedyncza tabela](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
### [Odzyskiwanie sprawności po awarii centrum danych](sql-database-disaster-recovery.md)
### [Przeprowadzanie próbnego odzyskiwania po awarii](sql-database-disaster-recovery-drills.md)
## Replikowanie
### [Omówienie aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md)
### Konfigurowanie
#### [Program PowerShell](sql-database-geo-replication-powershell.md)
#### [T-SQL](sql-database-geo-replication-transact-sql.md)
### Tryb failover
#### [Azure Portal](sql-database-geo-replication-failover-portal.md)
#### [Program PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)

## Rozwiązywanie problemów
### [Problemy z połączeniem](sql-database-troubleshoot-common-connection-issues.md)
### [Przejściowy błąd połączenia](sql-database-troubleshoot-connection.md)
### [Diagnozowanie i zapobieganie](sql-database-connectivity-issues.md)
### [Uprawnienia](sql-database-troubleshoot-permissions.md)
### [Przenoszenie baz danych](sql-database-troubleshoot-moving-data.md)


# Dokumentacja
## [Program PowerShell](/powershell/azureps-cmdlets-docs/)
## [PowerShell — model klasyczny](/powershell/servicemanagement/)
## [Java](/java/api/)
## [.NET](/dotnet/api/)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [Polecenia cmdlet usługi Azure SQL Database](/powershell/resourcemanager/AzureRM.Sql/v1.0.12/AzureRM.Sql)
## [Polecenia cmdlet programu SQL Server](https://msdn.microsoft.com/library/mt740629.aspx)
## [REST](/rest/api/sql/)

## Biblioteka zarządzania usługi SQL Database
### [Dokumentacja biblioteki zarządzania usługi SQL Database](https://msdn.microsoft.com/library/azure/mt349017.aspx)
### [Pobieranie pakietu biblioteki zarządzania usługi SQL Database](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [Sterowniki programu SQL Server](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)

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




<!--HONumber=Nov16_HO4-->


