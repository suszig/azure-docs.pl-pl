
# Omówienie
## [Co to jest baza danych DocumentDB?](documentdb-introduction.md)
## [Co to jest usługa DocumentDB: interfejs API dla bazy danych MongoDB?](documentdb-protocol-mongodb.md)
## [Kluczowe pojęcia](documentdb-resources.md)
## [Dystrybucja globalna](documentdb-distribute-data-globally.md)
## [Bezpieczeństwo](documentdb-nosql-database-security.md)
## [Analiza NoSQL całkowitego kosztu posiadania](https://aka.ms/documentdb-tco-paper)
## Scenariusze
### [Typowe przypadki użycia](documentdb-use-cases.md)
### [Baza danych DocumentDB w zastosowaniach społecznościowych](documentdb-social-media-apps.md)

# Wprowadzenie
## Tworzenie pierwszej aplikacji
### [Aplikacja konsoli .NET](documentdb-get-started.md)
### [Aplikacja konsolowa platformy .NET Core](documentdb-dotnetcore-get-started.md)
### [Aplikacja konsolowa środowiska Java](documentdb-java-get-started.md)
### [Aplikacja konsoli Node.js](documentdb-nodejs-get-started.md)
### [Aplikacja konsolowa Node.js dla interfejsu API bazy danych MongoDB](documentdb-mongodb-samples.md)
### [Aplikacja konsolowa języka C++](documentdb-cpp-get-started.md)
## Tworzenie aplikacji sieci Web
### [Aplikacja sieci Web programu .NET](documentdb-dotnet-application.md)
### [Aplikacja sieci Web programu .NET dla interfejsu API bazy danych MongoDB](documentdb-mongodb-application.md)
### [Aplikacja platformy Xamarin](documentdb-mobile-apps-with-xamarin.md)
### [Aplikacja sieci Web środowiska Node.js](documentdb-nodejs-application.md)
### [Aplikacje sieci Web w języku Java](documentdb-java-application.md)
### [Aplikacje sieci Web w języku Python Flask](documentdb-python-application.md)
## [Opracowywanie w środowisku lokalnym](documentdb-nosql-local-emulator.md)
### [Export Emulator Certificates](documentdb-nosql-local-emulator-export-ssl-certificates.md) (Eksportowanie certyfikatów emulatora)
## [Często zadawane pytania](documentdb-faq.md)

# Instrukcje

## Planowanie
### [Partycjonowanie i skalowanie](documentdb-partition-data.md)
### [Spójność](documentdb-consistency-levels.md)
### [NoSQL a SQL](documentdb-nosql-vs-sql.md)
### [Ekonomiczne odczyty i zapisy](documentdb-key-value-store-cost.md)

## Zarządzanie
### [Jednostki żądania](documentdb-request-units.md)
### [Importowanie danych](documentdb-import-data.md)
### [Importowanie do interfejsu API dla bazy danych MongoDB](documentdb-mongodb-migrate.md)
### [Łączenie ze swoim kontem MongoDB](documentdb-connect-mongodb-account.md)
### [Korzystanie z programu MongoChef](documentdb-mongodb-mongochef.md)
### [Korzystanie z narzędzia Robomongo](documentdb-mongodb-robomongo.md)
### [Modelowanie danych](documentdb-modeling-data.md)
### [Automatyczne wygaszanie danych](documentdb-time-to-live.md)
### [Tworzenie kopii zapasowej i przywracanie](documentdb-online-backup-and-restore.md)
### [Praca w trybie failover w regionach](documentdb-regional-failovers.md)
### Automatyzacja
#### [Interfejs wiersza polecenia platformy Azure 2.0](documentdb-automation-resource-manager-cli.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0: tworzenie konta](documentdb-automation-resource-manager-cli-nodejs.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0: dodawanie lub usuwanie regionów](documentdb-automation-region-management.md)
#### [Azure PowerShell](documentdb-manage-account-with-powershell.md)
### Bezpieczeństwo
#### [Zabezpieczanie dostępu do danych](documentdb-secure-access-to-data.md)
#### [Obsługa zapory](documentdb-firewall-support.md)
### [Wycofywanie wersji S1, S2, S3](documentdb-performance-levels.md)

## Programowanie
### [Zapytanie SQL](documentdb-sql-query.md)
### [Procedury składowane, wyzwalacze i funkcje definiowane przez użytkownika (UDF)](documentdb-programming.md)
### [Dostosowywanie indeksów](documentdb-indexing-policies.md)
### [Opracowywanie zawartości dla wielu regionów](documentdb-developing-with-multiple-regions.md)
### [Track changes with Change feed](documentdb-change-feed.md) (Śledzenie zmian za pomocą kanału informacyjnego zmian)
### [Używanie danych geoprzestrzennych](documentdb-geospatial.md)
### [Testowanie wydajności](documentdb-performance-testing.md)
### [Porady dotyczące wydajności](documentdb-performance-tips.md)
### Najlepsze praktyki
#### [Konfiguracja wielu wzorców](documentdb-multi-region-writers.md)
#### [Elementy DateTime](documentdb-working-with-dates.md)

## Używanie portalu
### [Tworzenie konta bazy danych](documentdb-create-account.md)
### [Tworzenie kolekcji](documentdb-create-collection.md)
### [Ustawianie przepływności](documentdb-set-throughput.md)
### [Dodawanie replikacji globalnej](documentdb-portal-global-replication.md)
### [Dodawanie i edytowanie dokumentów](documentdb-view-json-document-explorer.md)
### [Wyszukiwanie w dokumentach](documentdb-query-collections-query-explorer.md)
### [Zarządzanie kontem](documentdb-manage-account.md)
### [Monitorowanie konta](documentdb-monitor-accounts.md)
### [Zarządzanie skryptami](documentdb-view-scripts.md)
### [Wskazówki dotyczące rozwiązywania problemów](documentdb-portal-troubleshooting.md)


## Integracja
### [Łączenie z platformą Spark](documentdb-spark-connector.md)
### [Wdrażanie witryny sieci Web za pomocą Azure App Service](documentdb-create-documentdb-website.md)
### [Rejestrowanie aplikacji za pomocą usługi Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
### [Powiązanie z funkcją Azure Functions](../azure-functions/functions-bindings-documentdb.md)
### [Analizowanie danych przy użyciu usługi Hadoop](documentdb-run-hadoop-with-hdinsight.md)
### [Integracja z usługą Azure Search](../search/search-howto-index-documentdb.md)
### [Przenoszenie danych za pomocą usługi Azure Data Factory](../data-factory/data-factory-azure-documentdb-connector.md)
### [Analiza danych czasu rzeczywistego przy użyciu usługi Azure Stream Analytics](../stream-analytics/stream-analytics-define-outputs.md#documentdb)
### [Uzyskiwanie zmienionego rekordu HL7 FHIR przy użyciu usługi Logic Apps](documentdb-change-feed-hl7-fhir-logic-apps.md)
### [Przetwarzanie danych z czujników w czasie rzeczywistym](../hdinsight/hdinsight-storm-iot-eventhub-documentdb.md)
### [Wizualizacja danych przy użyciu usługi Power BI](documentdb-powerbi-visualize.md)
### [Wykorzystywanie sterownika ODBC do wizualizacji danych](documentdb-nosql-odbc-driver.md)


# Dokumentacja
## [Java](documentdb-sdk-java.md)
## [.NET](documentdb-sdk-dotnet.md)
## [.NET Core](documentdb-sdk-dotnet-core.md)
## [Node.js](documentdb-sdk-node.md)
## [Python](documentdb-sdk-python.md)
## [REST](/rest/api/documentdb/)
## [Dostawca zasobów REST](/rest/api/documentdbresourceprovider/)

# Powiązane
## [Przykłady dla platformy .NET](documentdb-dotnet-samples.md)
## [Przykłady dla platformy Node.js](documentdb-nodejs-samples.md)
## [Przykłady w języku Python](documentdb-python-samples.md)
## [Składnia języka SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
## [Gramatyka języka SQL — ściągawka](documentdb-sql-query-cheat-sheet.md)

# Zasoby
## [Cennik](https://azure.microsoft.com/pricing/details/documentdb/)
## [Forum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=documentdb)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=documentdb)
## [Portal społeczności](documentdb-community.md)
## [Zapytania — informacje i testowanie](https://www.documentdb.com/sql/demo)
## [Dokument dotyczący indeksowania niezależnego od schematu](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)
## [Wyjaśnienie pojęcia spójności danych na podstawie baseballu](http://research.microsoft.com/apps/pubs/default.aspx?id=206913)
## [Książka: Using Microsoft Azure DocumentDB in a Node.js Application (Korzystanie z bazy danych Microsoft Azure DocumentDB w aplikacji Node.js)](https://go.microsoft.com/fwlink/?LinkId=828428&clcid=0x409)
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
