# Omówienie

## [Co to jest Magazyn danych SQL?](sql-data-warehouse-overview-what-is.md)
## [Obciążenie magazynu danych](sql-data-warehouse-overview-workload.md)

# Wprowadzenie

## [Samouczek dla początkujących](sql-data-warehouse-get-started-tutorial.md)
## [Elastyczne zapytanie — samouczek](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)
## [Najlepsze praktyki](sql-data-warehouse-best-practices.md)
## [Zarządzanie](sql-data-warehouse-overview-manage.md)

# Instrukcje

## Funkcje usługi
### [Architektura MPP](massively-parallel-processing-mpp-architecture.md)
### [Warstwy wydajności](performance-tiers.md)
### [Jednostki magazynu danych](what-is-a-data-warehouse-unit-dwu-cdwu.md)
### [Często zadawane pytania](sql-data-warehouse-overview-faq.md)

## Tworzenie kopii zapasowej i przywracanie

### [Omówienie usługi Backup](sql-data-warehouse-backups.md)
### [Omówienie przywracania kopii zapasowych](sql-data-warehouse-restore-database-overview.md)
#### [Witryna Azure Portal](sql-data-warehouse-restore-database-portal.md)
#### [Azure PowerShell](sql-data-warehouse-restore-database-powershell.md)
#### [REST](sql-data-warehouse-restore-database-rest-api.md)

## Połączenie

### [Omówienie](sql-data-warehouse-connect-overview.md)
### [SSMS](sql-data-warehouse-query-ssms.md)
### [Program Visual Studio](sql-data-warehouse-query-visual-studio.md)
### [Instalacja programu Visual Studio](sql-data-warehouse-install-visual-studio.md)
### [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
### [Parametry połączenia](sql-data-warehouse-connection-strings.md)

## Przycisk Utwórz
### [Witryna Azure Portal](sql-data-warehouse-get-started-provision.md)
### [Azure PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
### [T-SQL](sql-data-warehouse-get-started-create-database-tsql.md)

## Programowanie

### [Omówienie](sql-data-warehouse-overview-develop.md)

### Tabele

#### [Omówienie](sql-data-warehouse-tables-overview.md)
#### [CTAS](sql-data-warehouse-develop-ctas.md)
#### [Typy danych](sql-data-warehouse-tables-data-types.md)
#### [Rozproszone tabele](sql-data-warehouse-tables-distribute.md)
#### [Indeksy](sql-data-warehouse-tables-index.md)
#### [Tożsamość](sql-data-warehouse-tables-identity.md)
#### [Partycje](sql-data-warehouse-tables-partition.md)
#### [Replicated tables (Zreplikowane tabele)](design-guidance-for-replicated-tables.md)
#### [Statystyki](sql-data-warehouse-tables-statistics.md)
#### [Tymczasowe](sql-data-warehouse-tables-temporary.md)

### Zapytania

#### [Dynamiczny język SQL](sql-data-warehouse-develop-dynamic-sql.md)
#### [Grupuj według opcji](sql-data-warehouse-develop-group-by-options.md)
#### [Etykiety](sql-data-warehouse-develop-label.md)

### Elementy języka T-SQL

#### [Pętle](sql-data-warehouse-develop-loops.md)
#### [Procedury składowane](sql-data-warehouse-develop-stored-procedures.md)
#### [Transakcje](sql-data-warehouse-develop-transactions.md)
#### [Najlepsze rozwiązania w zakresie transakcji](sql-data-warehouse-develop-best-practices-transactions.md)
#### [Schematy definiowane przez użytkownika](sql-data-warehouse-develop-user-defined-schemas.md)
#### [Przypisywanie zmiennej](sql-data-warehouse-develop-variable-assignment.md)
#### [Widoki](sql-data-warehouse-develop-views.md)

## Integracja

### [Omówienie](sql-data-warehouse-overview-integrate.md)
### [Fabryka danych](sql-data-warehouse-integrate-azure-data-factory.md)
### [Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md)
### [Samouczek usługi Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
### [Power BI](sql-data-warehouse-integrate-power-bi.md)
### [Wizualizacja w usłudze Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
### [Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md)

### [Elastyczne zapytanie — z bazą danych SQL i usługą SQL Data Warehouse](how-to-use-elastic-query-with-sql-data-warehouse.md)

## Ładowanie

### Pojęcia
#### [Omówienie](sql-data-warehouse-overview-load.md)
#### [Wskazówki dotyczące technologii PolyBase](sql-data-warehouse-load-polybase-guide.md)

### Samouczki
#### [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)

### Przewodniki z instrukcjami
#### [Dane przykładowe](sql-data-warehouse-load-sample-databases.md)
#### [Azure Data Lake Store](sql-data-warehouse-load-from-azure-data-lake-store.md)
#### [BCP](sql-data-warehouse-load-with-bcp.md)
#### [Fabryka danych](sql-data-warehouse-load-with-data-factory.md)
#### [Program PolyBase z usługi Blob Storage](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
#### [Program PolyBase z programu SQL Server](sql-data-warehouse-load-from-sql-server-with-polybase.md)
#### [RedGate](sql-data-warehouse-load-with-redgate.md)
#### [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)

## Migrate (Migracja)

### [Omówienie](sql-data-warehouse-overview-migrate.md)
### [Narzędzie do migracji](sql-data-warehouse-migrate-migration-utility.md)
### [Migrowanie schematu](sql-data-warehouse-migrate-schema.md)
### [Migrowanie kodu](sql-data-warehouse-migrate-code.md)
### [Migrowanie danych](sql-data-warehouse-migrate-data.md)
### [Migrowanie do usługi Premium Storage](sql-data-warehouse-migrate-to-premium-storage.md)

## Zarządzanie obliczeniami

### [Omówienie](sql-data-warehouse-manage-compute-overview.md)
### [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
### [Program PowerShell](sql-data-warehouse-manage-compute-powershell.md)
### [Interfejs API REST](sql-data-warehouse-manage-compute-rest-api.md)
### [T-SQL](sql-data-warehouse-manage-compute-tsql.md)
### Klasy zasobów
#### [Wytyczne](resource-classes-for-workload-management.md)
#### [Analizowanie obciążenia](analyze-your-workload.md)

## Wydajność

### [Omówienie](sql-data-warehouse-overview-manage-user-queries.md)
### [Kompresja magazynu kolumn](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)
### [Monitorowanie](sql-data-warehouse-manage-monitor.md)

## Bezpieczeństwo

### [Omówienie](sql-data-warehouse-overview-manage-security.md)
### [Inspekcja](sql-data-warehouse-auditing-overview.md)
### [Inspekcja dla klientów niższych poziomów](sql-data-warehouse-auditing-downlevel-clients.md)
### [Uwierzytelnianie](sql-data-warehouse-authentication.md)
### [Szyfrowanie](sql-data-warehouse-encryption-tde.md)
### [Szyfrowanie za pomocą języka T-SQL](sql-data-warehouse-encryption-tde-tsql.md)
### [Wykrywanie zagrożeń](sql-data-warehouse-security-threat-detection.md)

## Rozwiązywanie problemów
### [Rozwiązywanie problemów](sql-data-warehouse-troubleshoot.md)

# Dokumentacja

## [Limity pojemności](sql-data-warehouse-service-capacity-limits.md)
## T-SQL
### [Pełna dokumentacja](https://docs.microsoft.com/sql/t-sql/language-reference/)
### [Elementy języka usługi SQL DW](sql-data-warehouse-reference-tsql-language-elements.md)
### [Instrukcje usługi SQL DW](sql-data-warehouse-reference-tsql-statements.md)
## [Widoki systemowe](sql-data-warehouse-reference-tsql-system-views.md)
## [Polecenia cmdlet programu PowerShell](sql-data-warehouse-reference-powershell-cmdlets.md)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/?category=databases)
## [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse)
## [Cennik](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Żądania funkcji](https://feedback.azure.com/forums/307516-sql-data-warehouse/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=sql-data-warehouse)
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/)
## [Pomoc techniczna](sql-data-warehouse-get-started-create-support-ticket.md)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

## Partnerzy
### [Analiza biznesowa](sql-data-warehouse-partner-business-intelligence.md)
### [Integracja danych](sql-data-warehouse-partner-data-integration.md)
### [Zarządzanie danymi](sql-data-warehouse-partner-data-management.md)
