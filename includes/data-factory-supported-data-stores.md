Działanie kopiowania w usłudze Data Factory kopiuje dane z magazynu danych źródła do magazynu danych ujścia. Usługa Data Factory obsługuje następujące magazyny danych. Dane z dowolnego źródła można zapisać do dowolnego ujścia. Kliknij magazyn danych, aby dowiedzieć się, jak kopiować dane do i z tego magazynu.

Kategoria | Magazyn danych | Obsługiwany jako źródło | Obsługiwany jako ujście
:------- | :--------- | :------------------ | :-----------------
Azure | [Azure Blob Storage](../articles/data-factory/data-factory-azure-blob-connector.md) <br/> [Azure Data Lake Store](../articles/data-factory/data-factory-azure-datalake-connector.md) <br/> [Azure SQL Database](../articles/data-factory/data-factory-azure-sql-connector.md) <br/> [Azure SQL Data Warehouse](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure Table Storage](../articles/data-factory/data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) <br/> | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓
Bazy danych | [SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)\* <br/> [Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)\* <br/> [MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)\* <br/> [DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)\* <br/> [Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)\* <br/> [PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)\* <br/> [Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)\* <br/>[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)\* <br/>[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)\*<br/>[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓<br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; <br/>&nbsp;
Plik | [System plików](../articles/data-factory/data-factory-onprem-file-system-connector.md)\* <br/> [HDFS](../articles/data-factory/data-factory-hdfs-connector.md)\* <br/> [Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) | ✓ <br/> ✓ <br/> ✓ | ✓ <br/> &nbsp;<br/>&nbsp;
Inne | [SalesForce](../articles/data-factory/data-factory-salesforce-connector.md)<br/> [Ogólne ODBC](../articles/data-factory/data-factory-odbc-connector.md)\* <br/> [Ogólne OData](../articles/data-factory/data-factory-odata-connector.md) <br/> [Tabela sieci Web (tabela z kodu HTML)](../articles/data-factory/data-factory-web-table-connector.md) <br/> [GE Historian](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓  | &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;

> [AZURE.NOTE] Magazyny danych oznaczone znakiem * mogą być konfigurowane lokalnie lub w usłudze Azure IaaS i wymagają zainstalowania [bramy zarządzania danymi](../articles/data-factory/data-factory-data-management-gateway.md) na maszynie lokalnej lub w usłudze Azure IaaS.




<!--HONumber=Sep16_HO4-->


