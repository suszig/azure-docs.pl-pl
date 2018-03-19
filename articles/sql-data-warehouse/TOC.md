# [Dokumentacja usługi SQL Data Warehouse](index.md)

# Omówienie

## [Informacje o usłudze Azure SQL Data Warehouse](sql-data-warehouse-overview-what-is.md)
## [Ściągawka](cheat-sheet.md)

# Przewodniki Szybki start

## [Tworzenie i łączenie — portal](create-data-warehouse-portal.md)
## Wstrzymywanie i wznawianie obliczeń
### [Portal](pause-and-resume-compute-portal.md)
### [Program PowerShell](pause-and-resume-compute-powershell.md)
## Skalowanie zasobów obliczeniowych
### [Portal](quickstart-scale-compute-portal.md)
### [Program PowerShell](quickstart-scale-compute-powershell.md)
### [T-SQL](quickstart-scale-compute-tsql.md)


# Samouczki
## [1 — Ładowanie bazy danych WideWorldImporters](load-data-wideworldimportersdw.md)

# Pojęcia
## Funkcje usługi
### [Architektura MPP](massively-parallel-processing-mpp-architecture.md)
### [Warstwy wydajności](performance-tiers.md)
### [Jednostki magazynu danych](what-is-a-data-warehouse-unit-dwu-cdwu.md)
### [Skalowanie w poziomie, wstrzymywanie, wznawianie](sql-data-warehouse-manage-compute-overview.md)
### [Tworzenie kopii zapasowych magazynu danych](sql-data-warehouse-backups.md)
### [Inspekcja](sql-data-warehouse-auditing-overview.md)
### [Limity pojemności](sql-data-warehouse-service-capacity-limits.md)
### [Często zadawane pytania](sql-data-warehouse-overview-faq.md)

## Bezpieczeństwo
### [Omówienie](sql-data-warehouse-overview-manage-security.md)
### [Uwierzytelnianie](sql-data-warehouse-authentication.md)


## Migrowanie do usługi SQL Data Warehouse
### [Omówienie](sql-data-warehouse-overview-migrate.md)
### [Migrowanie schematu](sql-data-warehouse-migrate-schema.md)
### [Migrowanie kodu](sql-data-warehouse-migrate-code.md)
### [Migrowanie danych](sql-data-warehouse-migrate-data.md)

## Ładowanie i przenoszenie danych
### [Omówienie](design-elt-data-loading.md)
### [Najlepsze praktyki](guidance-for-loading-data.md)


## Integracja
### [Omówienie](sql-data-warehouse-overview-integrate.md)
### [Zapytanie elastyczne w usłudze SQL Database](how-to-use-elastic-query-with-sql-data-warehouse.md)


## Wydajność zapytań
### [Klasy zasobów](resource-classes-for-workload-management.md)
### [Kompresja magazynu kolumn](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)

## [Monitorowanie](sql-data-warehouse-manage-monitor.md)


## Programowanie magazynów danych
### [Omówienie](sql-data-warehouse-overview-develop.md)
### [Składniki magazynu danych](sql-data-warehouse-overview-workload.md)

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

## [Rozwiązywanie problemów](sql-data-warehouse-troubleshoot.md)

# Przewodniki z instrukcjami
## Funkcje usługi
### [Przywracanie magazynu danych — portal](sql-data-warehouse-restore-database-portal.md)
### [Przywracanie magazynu danych — program PowerShell](sql-data-warehouse-restore-database-powershell.md)
### [Przywracanie magazynu danych — interfejs API REST](sql-data-warehouse-restore-database-rest-api.md)

## Bezpieczeństwo
### [Włączanie szyfrowania — portal](sql-data-warehouse-encryption-tde.md)
### [Włączanie szyfrowania — język T-SQL](sql-data-warehouse-encryption-tde-tsql.md)
### [Wykrywanie zagrożeń](sql-data-warehouse-security-threat-detection.md)


## Ładowanie i przenoszenie danych
### [Dane nowojorskiej taksówki](load-data-from-azure-blob-storage-using-polybase.md)
### [Contoso public data (Publiczne dane firmy Contoso)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
### [Azure Data Lake Store](sql-data-warehouse-load-from-azure-data-lake-store.md)
### [BCP](sql-data-warehouse-load-with-bcp.md)
### [Fabryka danych](sql-data-warehouse-load-with-data-factory.md)
### [Narzędzie AzCopy](sql-data-warehouse-load-from-sql-server-with-polybase.md)
### [RedGate](sql-data-warehouse-load-with-redgate.md)
### [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)


## Integracja
### [Konfigurowanie zapytania elastycznego z usługi SQL Database](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)
### [Dodawanie zadania usługi Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md)
### [Korzystanie z uczenia maszynowego](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
### [Wizualizowanie przy użyciu usługi Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)

## Monitorowanie i dostrajanie
### [Analizowanie obciążenia](analyze-your-workload.md)

## Skalowanie w poziomie

### [Automatyzowanie poziomów obliczeniowych](manage-compute-with-azure-functions.md)


# Dokumentacja

## T-SQL
### [Pełna dokumentacja](https://docs.microsoft.com/sql/t-sql/language-reference/)
### [Elementy języka usługi SQL DW](sql-data-warehouse-reference-tsql-language-elements.md)
### [Instrukcje usługi SQL DW](sql-data-warehouse-reference-tsql-statements.md)
## [Widoki systemowe](sql-data-warehouse-reference-tsql-system-views.md)
## [Polecenia cmdlet programu PowerShell](sql-data-warehouse-reference-powershell-cmdlets.md)
## [Interfejsy API REST](sql-data-warehouse-manage-compute-rest-api.md)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/?category=databases)
## [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse)
## [Cennik](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Żądania funkcji](https://feedback.azure.com/forums/307516-sql-data-warehouse/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=sql-data-warehouse)
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/)
## [Pomoc techniczna](sql-data-warehouse-get-started-create-support-ticket.md)
## [Wideo](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

## Partnerzy
### [Analiza biznesowa](sql-data-warehouse-partner-business-intelligence.md)
### [Integracja danych](sql-data-warehouse-partner-data-integration.md)
### [Zarządzanie danymi](sql-data-warehouse-partner-data-management.md)
