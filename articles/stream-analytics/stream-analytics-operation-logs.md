---
title: "Debugowanie przy użyciu operacji i loguje się usługa Stream Analytics | Dokumentacja firmy Microsoft"
description: "Dzienniki operacji sposób użycia usługi analiza strumienia"
keywords: "dzienniki usługi"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2ee0871752dc2a3da345339fb826340d44ae48d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debugowanie przy użyciu dzienników usługi i działania zadania usługi analiza strumienia
Wszystkie usługi Azure, podaj operacyjne rejestrowanie komunikatów użytkowników, aby zapisać szczegóły dotyczące operacji zarządzania. W systemie Azure Stream Analytics te informacje mogą służyć do debugowania, takie jak wyświetlanie stanu zadań, postęp zadania i komunikaty o błędach umożliwia monitorowanie postępu zadania wraz z upływem czasu z rozpoczęcie przetwarzania do danych wyjściowych.

## <a name="find-operation-logs-in-the-azure-portal"></a>Find dzienniki operacji w portalu Azure
Dzienniki operacji można uzyskiwać na dwa sposoby:  

* Pulpit nawigacyjny zadania usługi analiza strumienia  
* Usługi zarządzania w klasycznym portalu Azure  

## <a name="dashboard-of-the-stream-analytics-job"></a>Pulpit nawigacyjny zadania usługi analiza strumienia
Na karcie pulpit nawigacyjny zadanie zostanie wyświetlony link do odpowiednich dzienników zadania usługi analiza strumienia. Po kliknięciu tego łącza ustawia filtry w sposób zawiera najnowsze dzienniki dla określonego zadania.

  ![Wybierz dzienniki usługi zarządzania](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Usługi zarządzania
Aby ręcznie przejdź do dzienników operacji dla usługi analiza strumienia i innych usług w klasycznym portalu Azure:

1. Polecenie **usług zarządzania** w [klasycznego portalu Azure](https://manage.windowsazure.com).
2. Wybierz **Stream Analytics** dla **typu** i nazwę zadania dla **nazwa usługi**.  
   
   ![Wybierz usługi analiza strumienia](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Znajdowanie dzienników inspekcji w portalu Azure
Aby znaleźć operacyjne dzienniki dla zadania Stream Analytics w portalu Azure, kliknij przycisk **Przeglądaj** , a następnie wybierz **dzienniki inspekcji**.

  ![Wybierz usługę Stream Analytics w portalu Azure](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Spowoduje to otwarcie ekranu przedstawiający zdarzenia z ostatnich siedmiu dni dla wszystkich zasobów w ramach subskrypcji.  Można filtrować, aby wyświetlić zdarzenia, określ typ lub przedział czasu, klikając **filtru** polecenia.

  ![Wybierz usługę Stream Analytics w portalu Azure](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Uzyskiwanie szczegółowych informacji dziennika
Można filtrować według stanu, aby wyświetlić dzienniki dla zadania i przedziału czasu.

W portalu Azure kliknij **szczegóły** przycisk w dolnej części okna, aby wyświetlić więcej szczegółów na temat wybranego zdarzenia. 

  ![Wybierz szczegóły](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Polecenie wpisu dziennika, aby wyświetlić szczegółowe zdarzenia w nim w portalu Azure.

  ![Wybierz szczegóły portalu Azure](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Z tego miejsca, możesz otworzyć **szczegółów** wyświetlić, klikając na zdarzenia.

  ![Wybierz szczegóły portalu Azure](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Debugowanie zadanie zakończone niepowodzeniem
W portalu Azure kliknij ikonę wyszukiwania i typu "nie powiodła się". Daje w wyniku wszystkie dzienniki z błędami. 

  ![Debugowanie zadanie zakończone niepowodzeniem](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

W portalu Azure, można filtrować według poziomu komunikat, aby wyświetlić **krytyczny** zdarzenia.

  ![Debugowania portalu Azure](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Zaznacz dowolny błędów, a polecenie **szczegóły** Aby uzyskać więcej informacji na temat błędu usługi.  Niektóre komunikaty o błędach zawierają również informacje dotyczące jak ograniczyć problem. 

  ![Szczegóły operacji](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

W razie potrzeby można skontaktować się z [Obsługa](https://azure.microsoft.com/support/options/) lub podać informacje do zespołu za pomocą [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), w szczególności należy pamiętać szczegóły operacji **identyfikator korelacji**. 

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

