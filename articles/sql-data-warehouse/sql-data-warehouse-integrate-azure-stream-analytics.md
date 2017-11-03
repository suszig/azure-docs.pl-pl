---
title: "Użyj usługi Azure Stream Analytics z usługą Magazyn danych SQL | Dokumentacja firmy Microsoft"
description: "Porady dotyczące korzystania z usługi Azure Stream Analytics z usługą Magazyn danych SQL Azure związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 14783f0464764a11d7f03a5db1c2d63728a4cb50
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Użyj usługi Azure Stream Analytics z usługą Magazyn danych SQL
Usługa Azure Stream Analytics to w pełni zarządzana usługa dostarczanie przetwarzanie złożonych zdarzeń małe opóźnienia, skalowalne, wysoko dostępne za pośrednictwem przesyłania strumieniowego danych w chmurze. Dowiedz się podstawy odczytując [wprowadzenie do usługi Azure Stream Analytics][Introduction to Azure Stream Analytics]. Następnie Dowiedz się jak utworzyć rozwiązanie end-to-end z usługi Stream Analytics, postępując [rozpocząć korzystanie z usługi Azure Stream Analytics] [ Get started using Azure Stream Analytics] samouczka.

W tym artykule dowiesz się, jak używać bazy danych Azure SQL Data Warehouse ujściem danych wyjściowych dla zadań para analiza.

## <a name="prerequisites"></a>Wymagania wstępne
Najpierw uruchom poniższe kroki w [rozpocząć korzystanie z usługi Azure Stream Analytics] [ Get started using Azure Stream Analytics] samouczka.  

1. Tworzenie Centrum zdarzeń dane wejściowe
2. Skonfiguruj i uruchom aplikację generator zdarzeń
3. Zainicjuj obsługę zadania usługi analiza strumienia
4. Określ dane wejściowe zadania i zapytanie

Następnie utwórz bazę danych magazynu danych SQL Azure

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Określ dane wyjściowe zadania: baza danych Azure SQL Data Warehouse
### <a name="step-1"></a>Krok 1
W przypadku zadania Stream Analytics kliknij **dane wyjściowe** w górnej części strony, a następnie kliknij przycisk **dodać danych wyjściowych**.

### <a name="step-2"></a>Krok 2
Wybierz bazę danych SQL, a następnie kliknij przycisk Dalej.

![][add-output]

### <a name="step-3"></a>Krok 3
Na następnej stronie, wprowadź następujące wartości:

* *Dane wyjściowe Alias*: Wprowadź przyjazną nazwę dla danych wyjściowych tego zadania.
* *Subskrypcja*:
  * Jeśli w bazie danych magazynu danych SQL jest w tej samej subskrypcji co zadanie usługi Stream Analytics, wybierz opcję Użyj bazy danych SQL z bieżącej subskrypcji.
  * Jeśli baza danych jest w innej subskrypcji, wybierz Użyj bazy danych SQL z innej subskrypcji.
* *Baza danych*: Określ nazwę docelowej bazy danych.
* *Nazwa serwera*: Określ nazwę serwera bazy danych, właśnie zostało określone. Aby znaleźć tę wartość, można użyć klasycznego portalu Azure.

![][server-name]

* *Nazwa użytkownika*: Określ nazwę użytkownika konta, które ma uprawnienia do zapisu dla bazy danych.
* *Hasło*: Podaj hasło dla określonego konta użytkownika.
* *Tabela*: Określ nazwę tabeli docelowej w bazie danych.

![][add-database]

### <a name="step-4"></a>Krok 4
Kliknij przycisk wyboru, aby dodać te dane wyjściowe zadania i sprawdzić, czy analiza strumienia może pomyślnie połączyć z bazą danych.

![][test-connection]

Podczas połączenia z bazą danych zakończy się pomyślnie, zostanie wyświetlone powiadomienie w dolnej części portalu. Możesz kliknąć Testuj połączenie na dole, aby przetestować połączenie z bazą danych.

## <a name="next-steps"></a>Następne kroki
Omówienie integracji, zobacz [Omówienie integracji usługi SQL Data Warehouse][SQL Data Warehouse integration overview].

Więcej porad dla deweloperów znajduje się w artykule [Omówienie programowania w usłudze SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
