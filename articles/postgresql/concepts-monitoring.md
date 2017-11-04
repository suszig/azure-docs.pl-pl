---
title: Monitorowanie bazy danych platformy Azure dla PostgreSQL | Dokumentacja firmy Microsoft
description: "W tym artykule opisano metryki monitorowania oraz alertów dla bazy danych Azure dla PostgreSQL, łącznie z Procesora, limity magazynu i statystyki."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: d48159fbc5e52bf1916a744e3912ca7ceb0ab60f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Monitorowanie bazy danych platformy Azure dla PostgreSQL
Monitorowanie danych dotyczących serwerów ułatwia rozwiązywanie problemów i Optymalizuj dla obciążenia. Bazy danych platformy Azure dla PostgreSQL zapewnia różnych metryk, które zapewniają wgląd w zachowania zasobów, do obsługi serwera PostgreSQL. 

## <a name="metrics"></a>Metryki
Wszystkie metryki Azure ma częstotliwość co minutę, a wszystkie metryki zapewnia 30 dni historii. 

Alerty można skonfigurować na metryki. Aby uzyskać instrukcje krok po kroku, zobacz [jak skonfigurować alerty](howto-alert-on-metric.md). 

Inne zadania obejmują konfigurowanie akcje automatyczne, wykonywanie zaawansowane metody analizy i archiwizowania historii. Aby uzyskać więcej informacji, zobacz [omówienie metryk usługi Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista metryk
Te metryki są dostępne dla bazy danych Azure dla PostgreSQL:

|Metryka|Nazwa wyświetlana metryki|Jednostka|Opis|
|---|---|---|---|---|
|cpu_percent|Procent procesora CPU|Procent|Procent procesora CPU w użyciu.|
|compute_limit|Obliczenia bazy danych limit jednostki|Licznik|Ten serwer maksymalna liczba jednostek obliczeń|
|compute_consumption_percent|Obliczenia bazy danych procent jednostki|Procent|Maksymalne przez procent jednostki obliczeniowe używane poza serwera.|
|memory_percent|Procent pamięci|Procent|Procent użycia pamięci.|
|io_consumption_percent|Procent we/wy|Procent|Procent we/wy w użyciu.|
|storage_percent|Procent użycia magazynu|Procent|Maksymalne przez procent magazynu używane poza serwera.|
|storage_used|Użyty magazyn|Bajty|Ilość pamięci masowej w użyciu. Magazyn używany przez usługę zawiera pliki bazy danych, dzienników transakcji i dzienniki serwera.|
|storage_limit|Limit magazynu|Bajty|Maksymalna przestrzeń magazynowa dla tego serwera.|
|active_connections|Całkowita liczba aktywnych połączeń|Licznik|Liczba aktywnych połączeń z serwerem.|
|connections_failed|Całkowita liczba połączeń nie powiodło się|Licznik|Liczba połączeń z serwerem nie powiodło się.|


> [!NOTE]
> Obliczeń jednostki składa się z pamięci i procesora CPU. Procent obliczeniowe jednostki jest maksymalny (% pamięci, procesora CPU wynoszące %). Sprawdź, czy wykresy pamięci i procesora w celu określenia, która wnosi wkład zmiany procent obliczeniowe jednostki. Aby uzyskać więcej informacji, zobacz [obliczeniowe jednostki](concepts-compute-unit-and-storage.md).

## <a name="next-steps"></a>Następne kroki
- Aby uzyskać instrukcje krok po kroku, zobacz [jak skonfigurować alerty](howto-alert-on-metric.md). 
- Aby uzyskać więcej informacji na temat dostępu i wyeksportować metryki przy użyciu portalu Azure, interfejsu API REST lub interfejsu wiersza polecenia, zobacz [omówienie metryk usługi Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
