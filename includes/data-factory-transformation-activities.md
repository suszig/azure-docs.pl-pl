Usługa Azure Data Factory obsługuje następujące działania przekształcania, które można dodawać indywidualnie do potoków lub łączyć je z innymi działaniami.

Działanie przekształcania danych |  Środowisko obliczeniowe 
:----------------------- | :--------------------
[Hive](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Pig](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Połączenia strumieniowe usługi Hadoop](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Działania usługi Machine Learning: wykonywanie wsadowe i aktualizacja zasobów](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Maszyna wirtualna platformy Azure 
[Procedura składowana](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL Data Warehouse lub SQL Server |
[Data Lake Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure Data Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | Usługa HDInsight [Hadoop] lub usługa Azure Batch
   
> [AZURE.NOTE] 
> Możesz użyć działania MapReduce, aby uruchomić programy platformy Spark w klastrze usługi HDInsight Spark. Zobacz [Wywoływanie programów platformy Spark z usługi Azure Data Factory](../articles/data-factory/data-factory-spark.md).
> Możesz utworzyć niestandardowe działanie, aby uruchamiać skrypty w klastrze usługi HDInsight z zainstalowanym językiem R. Zobacz [Uruchamianie skryptów języka R przy użyciu usługi Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).

<!--HONumber=Sep16_HO4-->


