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
ms.author: shvija;sethm
ms.openlocfilehash: 3b44c7e7387eceb2d9ea0b2c214b13a82869110f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Integrowanie Apache Spark przy użyciu usługi Azure Event Hubs

Usługa Azure Event Hubs można bezproblemowo zintegrować z [Apache Spark](https://spark.apache.org/) dokonanie budynku end-to-end dystrybucji przesyłania strumieniowego aplikacji. Integracja [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [przesyłania strumieniowego Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html), [strukturalnych przesyłania strumieniowego](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Łącznik usługi Event Hubs platformy Apache Spark jest dostępna w [GitHub](https://github.com/Azure/azure-event-hubs-spark). Ta biblioteka jest również dostępny do użycia w projektach Maven z [Maven centralnym repozytorium](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

W tym artykule przedstawiono sposób ciągły aplikacja w [Azure Databrick](https://azure.microsoft.com/services/databricks/). Gdy w tym artykule wykorzystano [Azure Databricks](https://azure.microsoft.com/services/databricks/), klastry Spark są również dostępne z [HDInsight](../hdinsight/spark/apache-spark-overview.md).

W poniższym przykładzie użyto dwóch notesów Scala: jeden dla strumienia zdarzeń z Centrum zdarzeń, a druga do wysyłania zdarzeń do niego z powrotem.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Wystąpienie usługi Event Hubs. Jeśli nie masz, [utworzyć](event-hubs-create.md)
* [Azure Databricks](https://azure.microsoft.com/services/databricks/) wystąpienia. Jeśli nie masz, [utworzyć](../azure-databricks/quickstart-create-databricks-workspace-portal.md)
* [Utwórz bibliotekę przy użyciu współrzędnych maven](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Użyć poniższego kodu w notesie strumienia zdarzeń w Centrum zdarzeń.

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
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
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
Poniższy przykładowy kod wysyła zdarzenia do Centrum zdarzeń z interfejsów API partii Spark. Można również napisać zapytanie przesyłania strumieniowego do wysyłania zdarzeń do Centrum zdarzeń.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule pokazano, jak działa łącznik usługi Event Hubs tworzenie w czasie rzeczywistym, odporność na uszkodzenia przesyłania strumieniowego rozwiązań. Dowiedz się więcej o strukturalnych przesyłania strumieniowego i usługi Event Hubs łącznika zintegrowanego, wykonując te linki:

* [Przesyłanie strumieniowe strukturalnych + przewodnik integracji usługi Azure Event Hubs](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Przesyłanie strumieniowe Spark + przewodnik integracji centra zdarzeń](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
