---
title: "Integrowanie Apache Spark przy użyciu usługi Azure Event Hubs | Dokumentacja firmy Microsoft"
description: "Integracja z Apache Spark przesyłania strumieniowego strukturalnych z usługą Event Hubs"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: shvija
ms.openlocfilehash: 2dcf390b80c119ab21948b982c0812395e45bc01
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Integrowanie Apache Spark przy użyciu usługi Azure Event Hubs

Usługa Azure Event Hubs można bezproblemowo zintegrować z [Apache Spark](https://spark.apache.org/) dokonanie budynku _end-to-end_ dystrybucji przesyłania strumieniowego aplikacji. Integracja [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [przesyłania strumieniowego Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html), [strukturalnych przesyłania strumieniowego](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Łącznik centra zdarzeń platformy Apache Spark jest dostępna w [GitHub](https://github.com/Azure/azure-event-hubs-spark). Ta biblioteka jest również dostępny do użycia w projektach Maven z [Maven centralnym repozytorium](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C)

W tym artykule opisano sposób wprowadzania ciągłego aplikacji w [Azure Databrick](https://azure.microsoft.com/services/databricks/). Gdy w tym artykule wykorzystano [Azure Databricks](https://azure.microsoft.com/services/databricks/), klastry Spark są również dostępne z [HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-overview).

W poniższym przykładzie użyto dwóch notesów Scala, jeden dla strumienia zdarzeń z Centrum zdarzeń i drugi dla wysyłania zdarzeń do niego z powrotem.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Wystąpienie usługi Event Hubs. Jeśli nie masz, [utworzyć](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)
* [Azure Databricks](https://azure.microsoft.com/services/databricks/) wystąpienia. Jeśli nie masz, [utworzyć](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)
* [Utwórz bibliotekę przy użyciu współrzędnych maven](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Umożliwia poniższy fragment kodu w notesie strumienia zdarzeń w Centrum zdarzeń.

```scala
import org.apache.spark.eventhubs._

// To connect to an Event Hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()

// Select the body column and cast it to a string.
val eventhubs = reader
  .select("CAST (body AS STRING)")
  .as[String]
```
Poniższy fragment kodu jest używany do wysyłania zdarzeń do Centrum zdarzeń z interfejsu API partii platforma Spark. Można również napisać zapytanie przesyłania strumieniowego do wysyłania zdarzeń do Centrum zdarzeń.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an Event Hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified Event Hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

Ten artykuł zawiera glimpse działania łącznika centra zdarzeń dla tworzenie w czasie rzeczywistym, odporność na uszkodzenia przesyłania strumieniowego rozwiązań. Dowiedz się więcej o strukturalnych przesyłania strumieniowego i usługi Event Hubs łącznika zintegrowanego, wykonując następujące czynności.

## <a name="next-steps"></a>Kolejne kroki

* [Przesyłanie strumieniowe strukturalnych + przewodnik integracji usługi Azure Event Hubs](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Przesyłanie strumieniowe Spark + przewodnik integracji centra zdarzeń](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
