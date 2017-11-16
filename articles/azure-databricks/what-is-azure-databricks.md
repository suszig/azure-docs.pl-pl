---
title: Co to jest Azure Databricks? | Microsoft Docs
description: "Informacje o nowościach Azure Databricks i jak stwarza Spark na Databricks na platformie Azure. Azure Databricks to platforma Apache Spark na podstawie analizy zoptymalizowane pod kątem platformy usługi w chmurze Microsoft Azure."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: 7ced38cda2669cf03e51f50fbbbeea0344da9277
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="what-is-azure-databricks"></a>Co to jest Azure Databricks?

Azure Databricks to platforma Apache Spark na podstawie analizy zoptymalizowane pod kątem platformy usługi w chmurze Microsoft Azure. Zaprojektowany z założycieli platformy Apache Spark, Databricks jest zintegrowany z platformy Azure w celu zapewnienia Instalatora jednym kliknięciem, prostsze przepływów pracy i interaktywny obszar roboczy, który umożliwia współpracę między analityków danych, inżynierów danych i analityków biznesowych.

![Co to jest Azure Databricks? ] (./media/what-is-azure-databricks/azure-databricks-overview.png "Co to jest Azure Databricks?")

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark na podstawie analizy platformy

Azure Databricks składa się z pełną technologie klastra Apache Spark open source i możliwości. Platforma Spark w usłudze Azure Databricks obejmuje następujące składniki:

![Platforma Apache Spark w Azure Databricks](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Apache Spark w Azure Databricks")

* **Platforma Spark SQL i DataFrames**: Spark SQL jest moduł Spark do pracy z danych strukturalnych. DataFrame jest kolekcją rozproszonej podzielone na nazwane kolumny danych. Odpowiada koncepcyjnie do tabeli w relacyjnej bazie danych lub ramki danych w języku R/Python.

* **Przesyłanie strumieniowe**: przetwarzanie danych w czasie rzeczywistym i analizy aplikacji analitycznych i interaktywne. Integruje się z systemem plików HDFS, Flume i Kafka.

* **MLib**: Biblioteka Machine Learning składające się z wspólnej uczenia algorytmów i narzędzi, tym klasyfikacji, regresji, klaster, filtrowania współpracy, zmniejszenie wymiary, jak również bazowy podstawowych optymalizacji.

* **GraphX**: wykresów i obliczeń wykres szeroki zakres zastosowań z kognitywnych analytics do Eksploracja danych.

* **Platforma Spark Core API**: obsługuje R, SQL, Python, Scala i Java.

## <a name="apache-spark-in-azure-databricks"></a>Platforma Apache Spark w Azure Databricks

Azure Databricks opiera się na możliwości Spark zapewniając platformy zarządzania zero chmury, która obejmuje:

- Klastry Spark pełni zarządzane
- Interaktywny obszar roboczy do eksploracji i wizualizacji
- Platforma do ulubionych aplikacji na podstawie Spark zasilania

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Pełni zarządzane klastry platformy Apache Spark w chmurze

Azure Databricks środowiska produkcyjnego bezpieczny i niezawodny znajdują się w chmurze, zarządzana i obsługiwana przez ekspertów platformy Spark. Możesz:

* Tworzenie klastrów w sekundach.
* Dynamicznie skalowania automatycznego klastrów w górę i w dół, niekorzystającą klastrów, w tym i udostępniać je między zespołami. 
* Za pomocą klastrów programowo przy użyciu interfejsów API REST. 
* Użyj zabezpieczonych danych funkcje integracji opartą na platformie Spark, które umożliwiają ujednolicenie bez centralizacji danych. 
* Daje natychmiastowy dostęp do najnowszych funkcji platformy Apache Spark w poszczególnych wersjach.

### <a name="databricks-runtime"></a>Środowisko uruchomieniowe Databricks
Środowisko uruchomieniowe Databricks bazuje na Apache Spark i natywnie zaprojektowano pod kątem chmury platformy Azure. 

Z **Serverless** opcji Azure Databricks całkowicie abstracts złożoności infrastruktury i potrzebę specjalne wiedzę do instalowania i konfigurowania infrastruktury danych. Opcja Niekorzystającą pomaga dane, które służące szybko przejść w zespole.

Dla inżynierów danych, którzy najważniejsze informacje dotyczące wykonywania zadań produkcji, Azure Databricks udostępnia Spark aparatem, który jest szybsze i wydajności za pośrednictwem różnych optymalizacje na We/Wy i przetwarzania warstwy (Databricks We/Wy).

### <a name="workspace-for-collaboration"></a>Obszar roboczy współpracy

Za pomocą współpracy i zintegrowane środowisko Azure Databricks upraszcza proces eksplorowania danych, prototypowania i uruchamianie opartych na danych aplikacji w łączniku Spark.

* Określenia sposobu używania danych z Eksploracja danych łatwe.
* Następnie należy udokumentować postęp w notesach R, Python, Scala lub SQL.
* Wizualizuj dane za pomocą kilku kliknięć, a za pomocą znanych narzędzi, takich jak Matplotlib, ggoplot lub d3.
* Umożliwia tworzenie dynamicznych raportów interaktywnych pulpitów nawigacyjnych.
* Użyj Spark i interakcję z danymi jednocześnie.

## <a name="enterprise-security"></a>Enterprise zabezpieczeń

Azure Databricks udostępnia korporacyjnej zabezpieczeń platformy Azure, włącznie z integracją usługi Azure Active Directory, oparta na rolach kontroli oraz umów SLA, które zapewniają ochronę danych i firmy.

* Integracja z usługą Azure Active Directory umożliwia uruchamianie kompletnych rozwiązań bazujących na platformie Azure przy użyciu Azure Databricks.
* Azure Databricks dostępu oparte na rolach umożliwia szczegółowe uprawnienia notesów, klastrów, zadania i dane.
* SLA korporacyjnej. 

## <a name="integration-with-azure-services"></a>Integracja z usługami Azure

Azure Databricks ściśle integruje się z bazy danych platformy Azure i magazyny: Magazyn danych SQL, rozwiązania Cosmos DB Data Lake Store i magazynu obiektów Blob. 

## <a name="integration-with-power-bi"></a>Integracja z usługą Power BI
Za pomocą zaawansowanych integracji z usługą Power BI Azure Databricks służy do odnajdywania i udostępniania istotna szczegółowe dane szybkie i łatwe. Można użyć innych narzędzi do analizy Biznesowej oraz, takich jak Tableau oprogramowania za pomocą punktów końcowych klastra JDBC/ODBC.

## <a name="next-steps"></a>Następne kroki

* [Szybki Start: Uruchom zadanie Spark Azure Databricks](quickstart-create-databricks-workspace-portal.md)
* [Praca z klastrami Spark](https://docs.azuredatabricks.net/user-guide/clusters/index.html)
* [Praca z notesów](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)
* [Tworzenie zadań Spark](https://docs.azuredatabricks.net/user-guide/jobs.html)

 









