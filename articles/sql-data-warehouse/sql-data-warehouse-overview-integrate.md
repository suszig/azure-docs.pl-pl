---
title: "Tworzenie rozwiązań zintegrowanych z usługą Magazyn danych SQL | Dokumentacja firmy Microsoft"
description: "Narzędzia i partnerom rozwiązania, które integrują się z usługą Magazyn danych SQL. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>Korzystać z innych usług z usługą Magazyn danych SQL
Oprócz jego podstawowych funkcji usługi SQL Data Warehouse pozwala użytkownikom korzystać z wielu innych usług na platformie Azure obok.  W szczególności obecnie przekierowaliśmy kroki głębokiej integracji z następującymi:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Usługa Azure Stream Analytics

Pracujemy nad Uzyskuj dostęp do większej liczby usług między ekosystemu platformy Azure.

## <a name="power-bi"></a>Power BI
Integracja z Power BI pozwala na lepsze wykorzystanie mocy obliczeniowej usługi SQL Data Warehouse z raportowaniem dynamiczne i wizualizacja usługi Power BI. Power BI integracji obecnie obejmuje:

* **Bezpośrednie połączenia**: bardziej zaawansowane połączenia z logiczną przekazywanie do magazynu danych SQL.  Zapewnia szybszy analizy na większą skalę.
* **Otwórz w usłudze Power BI**: przycisk "Otwórz w usłudze Power BI" przekazuje informacje o wystąpieniu usługi Power BI, co pozwala na połączenie, aby usprawnić.

Zobacz [integracji z usługą Power BI](sql-data-warehouse-integrate-power-bi.md) lub [dokumentacji usługi Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Aby uzyskać więcej informacji.

## <a name="azure-data-factory"></a>Azure Data Factory
Fabryka danych Azure umożliwia użytkownikom zarządzanych platformy do utworzenia złożonych potoki wyodrębniania obciążenia.  Integracja SQL Data Warehouse z fabryką danych Azure obejmuje następujące funkcje:

* **Procedury składowane**: organizowania wykonywania procedur składowanych na magazyn danych SQL.
* **Kopiuj**: ADF używany do przenoszenia danych do usługi SQL Data Warehouse.  Ta operacja może używają standardowych danych ADF mechanizm lub PolyBase. 

Zobacz [integracji z fabryką danych Azure](sql-data-warehouse-integrate-azure-data-factory.md) lub [dokumentacji fabryki danych Azure](https://azure.microsoft.com/documentation/services/data-factory/) Aby uzyskać więcej informacji.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning to usługi analytics w pełni zarządzana, co pozwala użytkownikom na tworzenie modeli skomplikowanych wykorzystaniu dużego zestawu narzędzi predykcyjnej.  Magazyn danych SQL jest obsługiwany jako źródło i miejsce docelowe dla tych modeli następujące funkcje:

* **Odczyt danych:** dysków modeli na dużą skalę przed SQL Data Warehouse przy użyciu T-SQL.
* **Zapis danych:** zatwierdzania zmieni się z dowolnego modelu do usługi SQL Data Warehouse.

Zobacz [integracji z usługą Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) lub [dokumentacji usługi Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Aby uzyskać więcej informacji.

## <a name="azure-stream-analytics"></a>Usługa Azure Stream Analytics
Usługa Azure Stream Analytics jest złożone, w pełni zarządzana infrastruktura przetwarzania i wykorzystywanie danych zdarzeń generowanych przez Centrum zdarzeń usługi Azure.  Integracja z usługą SQL Data Warehouse umożliwia strumieniowe przesyłanie danych do skutecznie przetwarzania i przechowywania obok włączenie głębiej, bardziej zaawansowane analizy danych relacyjnych.  

* **Dane wyjściowe zadania:** wysyłania danych wyjściowych z zadania usługi analiza strumienia bezpośrednio do usługi SQL Data Warehouse.

Zobacz [integracji z usługą Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) lub [dokumentacji usługi Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) Aby uzyskać więcej informacji.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
